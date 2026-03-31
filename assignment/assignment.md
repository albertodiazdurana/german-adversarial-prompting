# Assessment: GenAI Conversation Design

The goal of this assessment is to evaluate the ability to work with Generative AI systems to design high-quality prompts, manage multi-turn conversations, and understand model limitations in a language of expertise or in English.

## Objective

Demonstrate a three-turn conversation with GPT-4o on OpenRouter centered on a coding task. You may use the free chat interface, or the API (e.g. within a Jupyter Notebook).

The conversation should demonstrate:

1. **Design of effective prompts**
   In the early and middle turns, guide the model to produce strong, correct outputs.

2. **Discovery of model limitations**
   In the final turn, intentionally push the interaction so the model fails to produce the desired outcome correctly, despite a sophisticated and well-constructed prompt.

## Guidelines

- Do not spend more than 60 minutes on this task.
- An example of a coding task is: "Write a compilable program in C that computes the fibonacci element for input n and outputs an integer, or overflows."
- You may write the prompts and interactions in English or in your language of expertise. If you break the model in another language, please provide a reference English translation for the prompt and conversation, even if the model solves the task successfully in English.

## Deliverable

Please provide:

- An exported .JSON chat file from the OpenRouter-based full interaction
- A short written note that explains:
  - The logic behind the task
  - Why earlier turns were expected to succeed
  - What changed in the final turn
  - What limitations of the model were exposed in the last step
