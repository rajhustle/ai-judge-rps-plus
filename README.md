# AI Judge — Rock Paper Scissors Plus

**Prompt-driven AI Judge with deterministic constraint enforcement and explainable outputs**

A prompt engineering project demonstrating production-safe LLM control — deterministic behavior, constraint enforcement, and explainable outputs without heavy code logic.

---

## What This Is

This project implements a **prompt-driven AI Judge** that evaluates user moves in a rule-based game while simultaneously acting as the opposing player.

The focus is not on code-heavy logic. The focus is on **instruction design, determinism, constraint enforcement, and explainability** — the same principles that make production AI agents reliable in real-world deployments.

---

---

## How to Play

This is a **prompt-only project** — no installation, no server, no code to run.

**Step 1:** Open any LLM interface — Claude (claude.ai), ChatGPT, or Gemini

**Step 2:** Paste the full contents of `prompt.md` as the system prompt
- In Claude: start a new chat → paste prompt.md content as your first message labelled "System:"
- In ChatGPT: use the System field in the API Playground or paste at top of conversation
- In Gemini: paste at the start before your first move

**Step 3:** Start playing — type your move naturally:

```
rock
I'll throw scissors
bomb
paper
quit
```

**What to test:**
- Use bomb once — then try using it again (watch it get blocked as INVALID)
- Type something ambiguous like "the strong one" — it asks once, then wastes your turn
- Type "stop" or "quit" to end the game gracefully

No pip install. No API key needed. Just paste and play.


## Why This Matters

Most LLM applications fail in production not because the model is wrong but because:
- Rules and constraints aren't enforced deterministically
- Ambiguous inputs are handled inconsistently
- The agent's decision process is a black box

This project solves all three — using only a structured prompt.

---

## Game Rules

| Move | Beats |
|------|-------|
| Rock | Scissors |
| Scissors | Paper |
| Paper | Rock |
| Bomb | Everything (once only) |
| Bomb vs Bomb | Draw |

- User may use **bomb only once** per game
- Ambiguous or unclear moves **waste the turn** — no free retries
- Bot move is **never revealed** before the user's move is confirmed

---

## Prompt Architecture — 6-Step Decision Process

The entire game logic lives inside a single structured prompt:

```
Step 1: User Intent Understanding
        → Interpret free-text input to determine intended move
        → Detect game-ending phrases (stop / quit / end)

Step 2: Ambiguity Handling
        → Ask for clarification once if move is unclear
        → If still unclear after one attempt → mark UNCLEAR, waste turn
        → No infinite clarification loops

Step 3: Validation
        → Check bomb_used state before allowing bomb
        → Mark INVALID if bomb reused
        → Lock move once submitted — cannot be changed

Step 4: Bot Move Selection
        → Select bot move randomly AFTER user move is confirmed
        → Never reveal bot move prematurely

Step 5: Outcome Determination
        → Apply game rules to determine result
        → USER_WINS / BOT_WINS / DRAW / TURN_WASTED

Step 6: Response Generation
        → Structured, factual explanation of outcome
        → No unnecessary conversation or internal instruction leakage
```

---

## State Management

Minimal state tracked across rounds:

```
bomb_used: true / false
```

Demonstrates that **constraint enforcement doesn't require complex infrastructure** — a single boolean, tracked in context, is enough to enforce a game-critical rule across an entire session.

---

## Output Format (Every Round)

```
Round number
User move        (or null if INVALID / UNCLEAR)
Bot move         (or null if turn wasted)
Status           VALID / INVALID / UNCLEAR
Reason           Plain explanation of the decision
Round result     USER_WINS / BOT_WINS / DRAW / TURN_WASTED
State update     bomb_used: true / false
```

---

## Design Decisions

**Why prompt-driven instead of code-driven?**

Traditional approach: encode every rule as `if/else` in Python.  
Problem: logic is hidden, hard to audit, breaks on edge cases silently.

This approach: encode all rules as **explicit instructions** in the prompt.  
Result: behavior is observable, auditable, and easy to reason about — exactly how production AI agents should be built.

**Why waste the turn on ambiguity instead of asking again?**

Infinite clarification loops are a real production failure mode. One attempt, then move on — deterministic behavior over polite helpfulness.

**Why lock the move once submitted?**

Prevents users from gaming the system by changing their move after seeing the bot's response pattern. Constraint enforcement over conversational flexibility.

---

## What This Demonstrates

This project is a minimal but complete example of:

- **Instruction design** — rules declared explicitly, not implied
- **Deterministic LLM behavior** — same input always produces same structured output
- **Constraint enforcement** — bomb usage tracked and blocked via state
- **Explainability** — every decision comes with a reason
- **Edge case handling** — ambiguity, invalid reuse, game exit all handled

These are the same principles used in production Voice AI and Conversational AI systems where reliability matters more than conversational polish.

The same constraint enforcement and ambiguity handling patterns in this prompt are what I applied in production Voice AI systems for legal intake calls — handling real clients at a New York law firm where a misrouted call had real compliance consequences. This project is the minimal proof of concept. The production system was the real thing.

---

## Files

| File | Description |
|------|-------------|
| `prompt.md` | The complete system prompt — this is the core of the project |
| `README.md` | This file |

---

## Author

**Kaushal Raj** — Production Voice & Agentic AI Engineer  
[GitHub](https://github.com/rajhustle) · [LinkedIn](https://linkedin.com/in/kaushal-raj-a83603380)
