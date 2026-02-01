AI Judge – Rock Paper Scissors Plus
Overview

This solution implements a prompt-driven AI Judge that evaluates user moves in a rule-based game while also acting as the opposing player. The focus is on instruction design, determinism, constraint enforcement, and explainability, not on code-heavy logic.

Why this prompt structure

The LLM is explicitly instructed to act as a judge, not a chatbot.

All game rules and constraints are declared inside the prompt, ensuring decision-making is driven by instructions rather than hardcoded logic.

The prompt enforces clear separation of concerns:

Intent understanding (interpreting free-text input)

Game logic (rule validation, state enforcement, winner determination)

Response generation (structured, explainable output)

State handling

Minimal state is maintained:

bomb_used tracks whether the user has already used the bomb.

This demonstrates constraint enforcement without over-engineering or external storage.

Handling ambiguity and edge cases

Ambiguous or noisy inputs are handled deterministically.

The system allows only one clarification attempt to avoid infinite loops.

Reusing the bomb after it has been used is marked invalid.

Invalid or unclear moves waste the turn, ensuring predictable outcomes.

The bot’s move is selected only after the user’s move is confirmed and is never revealed prematurely.

Why this approach

Avoids if/else-heavy logic hidden in code.

Keeps behavior observable, auditable, and easy to reason about.

Mirrors real-world agent design where correctness and control matter more than conversational polish.

What could be improved next

Confidence scoring for intent interpretation.

Multi-round score tracking.

Metrics to analyze ambiguity frequency and invalid move patterns.

