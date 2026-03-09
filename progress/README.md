# progress/

This directory contains **user progress and session history** — one JSON file per topic.

## Purpose

Progress files serve two roles:

1. **Session log** — a chronological record of every question asked, the user's answer, whether it was correct, and how long it took.
2. **Mastery map** — a per-concept summary of mastery score (0.0–1.0) derived from session history. This drives the repetition vs novelty balance.

## File Naming

Files are named `topic-<slug>.json` to match the corresponding plan in `plans/` and question bank in `questions/`.

**Example:** `topic-kubernetes-basics.json`

## Schema

See [`_schema-progress.json`](./_schema-progress.json) for the full annotated schema.

## Files in This Directory

| File | Description |
|------|-------------|
| `_schema-progress.json` | Annotated schema / template |
| `topic-kubernetes-basics.json` | Sample progress file for Kubernetes Basics |

## How Copilot Uses This

- **Before a session:** Reads mastery scores to choose which concepts need reinforcement vs which are new.
- **After each answer:** Appends the result to the current session's `questions_asked` array.
- **After a session:** Recalculates mastery scores for all concepts touched in the session.
- **For analysis:** When you ask Copilot to analyse your learning, it reads this file to identify weak areas and trends.
