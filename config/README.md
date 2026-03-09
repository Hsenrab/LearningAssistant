# config/

This directory contains **configuration profiles** for tuning the adaptive learning system.

## Purpose

The config file controls how the scheduler selects questions. Key parameters:

| Parameter | Description |
|-----------|-------------|
| `repetition_weight` | 0.0–1.0 — how much to favour revisiting questions the user has answered before |
| `novelty_weight` | 0.0–1.0 — how much to favour introducing new questions (auto-derived as `1 - repetition_weight`) |
| `min_interval_days` | Minimum number of days before a question can be repeated (spaced repetition floor) |
| `mastery_threshold` | Mastery score (0.0–1.0) above which a concept is considered "mastered" |
| `session_length` | Default number of questions per learning session |
| `difficulty_adjustment` | Whether to auto-adjust question difficulty based on recent performance |
| `difficulty_ramp_rate` | How quickly difficulty increases/decreases after correct/incorrect answers |

## File Naming

`default.json` is the baseline config loaded when no topic-specific override exists.
You can create `<topic-slug>.json` (e.g. `kubernetes-basics.json`) to override settings for a specific topic.

## Schema

See [`_schema-config.json`](./_schema-config.json) for the full annotated schema.

## Files in This Directory

| File | Description |
|------|-------------|
| `_schema-config.json` | Annotated schema / template |
| `default.json` | Default configuration profile |
