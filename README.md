# german-adversarial-prompting

Can GPT-4o write Python that silently destroys German text?

This project explores linguistically-grounded adversarial prompting, designing conversations that expose genuine LLM limitations through German orthography.

## The core insight: ß is a one-way function

German has a well-known orthographic quirk: the letter ß (Eszett) becomes SS when uppercased.

```python
"Straße".upper()          # → "STRASSE"
"STRASSE".lower()         # → "strasse"
# But "strasse" ≠ "Straße" — the ß is gone.
```

This means `lowercase(uppercase(text)) ≠ text` whenever ß is present. The information is destroyed during uppercasing, and no algorithm can recover it without a dictionary. Even with a dictionary, ambiguous cases exist: "MASSE" could be "Masse" (mass) or "Maße" (measurements).

The Unicode Consortium confirms this is permanent: "Case mapping loses information and thus does not allow for a round trip." Their Stability Policy guarantees ß and SS can never become a reversible case pair.

## Three-turn conversation design

All prompts in German. The conversation builds a trap through legitimate, useful code:

**Turn 1 — Uppercase conversion (succeeds).** Write `to_uppercase()` handling ß → SS. Standard, well-documented. Builds confidence.

**Turn 2 — Lowercase + test suite (succeeds).** Add `to_lowercase()` with test cases using words like Straße, STRASSE, Maße, MASSE. The model confirms `"straße".upper() == "STRASSE"` on record, a prediction it cannot reconcile in Turn 3.

**Turn 3 — Round-trip normalization (fails).** Write `normalize_case()` that uppercases for comparison, then restores the original. Requirements: `normalize_case(text) == text` for all German input, no external libraries, no dictionaries, purely algorithmic. Prove correctness.

The prompt is well-constructed and impossible to satisfy. The constraint "rein algorithmisch" eliminates dictionary lookup, the only viable workaround.

## Results

GPT-4o produced a function that stores the original text before uppercasing, performs a theatrical normalization, and copies the original back. The "proof" is circular: it preserves the input because it never loses it. In any real pipeline where already-uppercased text arrives, the approach fails.

The model never identified the task as impossible, despite having confirmed one turn earlier that uppercasing destroys ß.

See [`results/analysis-note.md`](results/analysis-note.md) for the full analysis and [`notebooks/assessment.ipynb`](notebooks/assessment.ipynb) for the conversation notebook.

## References

- [Unicode FAQ: Case mappings](https://unicode.org/faq/casemap_charprop.html) — "Case mapping loses information and thus does not allow for a round trip"
- [Unicode Stability Policy](https://unicode.org/policies/stability_policy.html) — guarantees ß/SS mapping is permanent
- [SpecialCasing.txt](https://unicode.org/Public/UCD/latest/ucd/SpecialCasing.txt) — the actual Unicode case mapping data
- [Python `str.upper()` documentation](https://docs.python.org/3/library/stdtypes.html#str.upper)
- [Rat für deutsche Rechtschreibung](https://www.rechtschreibrat.com/) — German orthography authority, adopted ẞ as optional uppercase form in 2017

## Project structure

```
assignment/         — Public version of the original prompt/assignment
notebooks/          — Jupyter notebook with 3-turn GPT-4o conversation
results/            — Analysis note and exported conversation JSON
```
