# plans/

This directory contains **learning plans** — one JSON file per topic.

## Purpose

A learning plan defines the full structure of a topic broken into a 3-level hierarchy:

```
Topic
  └── Module          (broad area within the topic)
        └── Lesson    (focused subject within the module)
              └── Concept  (the smallest teachable unit — maps to questions)
```

Concepts are the atoms of the learning system. Each question in `questions/` is linked to a specific concept by `concept_id`.

## File Naming

Files are named `topic-<slug>.json` where `<slug>` is a lowercase, hyphen-separated identifier that must match the corresponding files in `questions/` and `progress/`.

**Example:** `topic-kubernetes-basics.json`

## Schema

See [`_schema-learning-plan.json`](./_schema-learning-plan.json) for the full annotated schema.

## Files in This Directory

| File | Description |
|------|-------------|
| `_schema-learning-plan.json` | Annotated schema / template |
| `topic-kubernetes-basics.json` | Sample learning plan for Kubernetes Basics |

## How Copilot Uses This

When you ask Copilot to start a learning session on a new topic, it will:
1. Check whether a `topic-<slug>.json` file already exists here.
2. If not, generate one and save it to this directory.
3. Use the plan structure to organise questions and track progress.
