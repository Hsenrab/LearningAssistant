# questions/

This directory contains **question banks** — one JSON file per topic.

## Purpose

Each question is linked to a specific concept in the corresponding learning plan (via `concept_id`). Questions carry metadata that the adaptive scheduler uses to decide what to ask next:

- **Difficulty** — how hard the question is (1 = easiest, 5 = hardest).
- **Times asked / times correct** — used to calculate accuracy per question.
- **Last asked date** — used for spaced-repetition scheduling.
- **Tags** — optional labels for filtering or grouping questions.

## File Naming

Files are named `topic-<slug>.json` to match the corresponding plan in `plans/` and progress file in `progress/`.

**Example:** `topic-kubernetes-basics.json`

## Schema

See [`_schema-question.json`](./_schema-question.json) for the full annotated schema.

## Files in This Directory

| File | Description |
|------|-------------|
| `_schema-question.json` | Annotated schema / template |
| `topic-kubernetes-basics.json` | Sample question bank for Kubernetes Basics |

## How Copilot Uses This

- **Generating questions:** When asked to create questions for a topic or concept, Copilot appends new question objects to the appropriate file.
- **Selecting the next question:** Copilot reads this file to find questions eligible for the current session based on the config (repetition weight, novelty weight, difficulty) and the user's progress history.
- **Updating after a response:** After you answer a question, Copilot updates `times_asked`, `times_correct`, and `last_asked_at` in place.
