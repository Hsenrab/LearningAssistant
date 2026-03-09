# How It Works

This document explains the end-to-end flow of the LearningAssistant adaptive learning system.

---

## Overview

The system is entirely file-based. There is no running server or database. GitHub Copilot acts as the "runtime" — it reads and writes JSON files in this repository in response to your chat messages.

```
User (VS Code Copilot Chat)
        │
        │  natural language requests
        ▼
  GitHub Copilot
        │
        │  reads / writes
        ▼
 JSON files in this repo
 ┌────────────┐  ┌───────────────┐  ┌────────────┐  ┌────────────┐
 │  plans/    │  │  questions/   │  │  progress/ │  │  config/   │
 └────────────┘  └───────────────┘  └────────────┘  └────────────┘
```

---

## Data Flow for a Learning Session

### Step 1 — Topic Selection

You tell Copilot a topic (e.g., *"I want to learn Kubernetes basics"*).

Copilot:
1. Derives a `topic_slug` (e.g., `kubernetes-basics`).
2. Looks for `plans/topic-kubernetes-basics.json`.
   - If it **exists**, loads the plan.
   - If it **doesn't exist**, generates a new one based on its training knowledge and saves it.

### Step 2 — Session Initialisation

Copilot:
1. Loads `progress/topic-kubernetes-basics.json` (or creates it if absent).
2. Reads `config/default.json` (or a topic-specific override if present).
3. Builds a list of candidate questions from `questions/topic-kubernetes-basics.json`.

### Step 3 — Question Selection (Adaptive)

For each question slot in the session, Copilot selects the next question using the following weighted criteria:

| Factor | Effect |
|--------|--------|
| **Novelty weight** (`1 - repetition_weight`) | Biases towards questions never asked before |
| **Repetition weight** | Biases towards questions already seen |
| **Spaced-repetition interval** (`min_interval_days`) | Suppresses questions asked too recently |
| **Mastery score** (`low_mastery_boost`) | Increases probability for concepts the user struggles with |
| **Difficulty** (`difficulty_adjustment`) | Adjusts difficulty band based on recent performance |

### Step 4 — Question & Answer

Copilot presents the question. You answer it in chat.

For `multiple_choice` and `true_false`: Copilot checks your answer against `correct_answer` automatically.

For `free_text`: Copilot evaluates your answer heuristically, comparing it to the `correct_answer` and `explanation` fields.

### Step 5 — Progress Update

After each answer, Copilot:
1. Appends the result to the current session's `questions_asked` array in `progress/topic-<slug>.json`.
2. Updates `times_asked` and `times_correct` on the question in `questions/topic-<slug>.json`.
3. Updates `last_asked_at` on the question.

### Step 6 — Session End and Mastery Recalculation

At the end of the session (or when you say you're done):
1. Copilot sets `ended_at` and computes `session_score` in the progress file.
2. Copilot recalculates `concept_mastery` scores for all concepts touched in the session.
3. Copilot recalculates `overall_mastery` for the topic.
4. Copilot saves all changes.

---

## Mastery Score Calculation

Mastery for a concept is a **weighted accuracy** that favours recent performance:

```
mastery(concept) = (recent_correct / recent_asked) * recency_weight
                 + (all_time_correct / all_time_asked) * (1 - recency_weight)
```

- `recency_weight` is fixed at 0.7 (recent performance matters more than historical).
- "Recent" = questions asked in the last 3 sessions.
- Score of 0.0 means never seen or always wrong; 1.0 means always correct.

---

## The Repetition vs Novelty Balance

Controlled by `repetition_weight` in `config/default.json`:

- **0.0** — every session introduces only questions you've never seen. Good for first-pass exploration.
- **0.5** — balanced mix of new and review questions.
- **1.0** — every session only revisits existing questions. Good for exam preparation.

The `low_mastery_boost` parameter further amplifies concepts below the `mastery_threshold`, ensuring weak areas are prioritised regardless of the repetition/novelty split.

---

## Adding a New Topic

1. Ask Copilot to create a learning plan for the new topic.
2. Copilot generates `plans/topic-<slug>.json`, `questions/topic-<slug>.json`, and `progress/topic-<slug>.json`.
3. Start a learning session for the new topic.
