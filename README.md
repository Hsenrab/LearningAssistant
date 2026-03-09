# LearningAssistant

An **adaptive learning workspace** designed to be used with **GitHub Copilot in VS Code** as the primary interface. You interact with this repo by opening it in VS Code and chatting with Copilot — no separate app or UI needed.

---

## How It Works

1. Open this repository in VS Code.
2. Open GitHub Copilot Chat.
3. Tell Copilot a topic you want to learn (e.g. *"I want to learn Kubernetes basics"*).
4. Copilot will:
   - Create or load a **learning plan** for that topic.
   - Generate or load **questions** from the question bank.
   - Track your **progress** and adapt future questions based on your history.
   - Suggest the next topic or concept when you're ready.

All data is stored as plain JSON files in this repository. Copilot reads and writes these files during your learning sessions.

---

## Directory Structure

```
LearningAssistant/
├── plans/        # Learning plans — one file per topic
├── questions/    # Question banks — one file per topic
├── progress/     # User progress & session history — one file per topic
├── config/       # Tuning parameters (repetition vs novelty, scheduling)
├── docs/         # Documentation and usage guides
└── .github/
    └── copilot-instructions.md   # Workspace-level Copilot instructions
```

### `plans/`
Stores **structured learning plans** organised as a 3-level hierarchy:
`Topic → Module → Lesson → Concept`

Each plan file is named `topic-<slug>.json` (e.g. `topic-kubernetes-basics.json`).
See [`plans/README.md`](plans/README.md) for details and the schema.

### `questions/`
Stores **question banks** keyed to the concepts defined in the matching plan file.
Questions carry metadata for adaptive scheduling: difficulty, times asked, times correct, last asked date.
See [`questions/README.md`](questions/README.md) for details and the schema.

### `progress/`
Stores **session history and mastery scores** per topic.
Each session records which questions were asked, the user's answers, and whether they were correct.
Mastery scores per concept are derived from this history and used by the scheduler.
See [`progress/README.md`](progress/README.md) for details and the schema.

### `config/`
Stores **tuning parameters** that control how questions are selected:
- Repetition vs novelty balance
- Spaced-repetition scheduling intervals
- Session length and difficulty adjustment
See [`config/README.md`](config/README.md) for details and the schema.

### `docs/`
Contains human-readable documentation:
- [`docs/how-it-works.md`](docs/how-it-works.md) — end-to-end flow explanation
- [`docs/copilot-usage-guide.md`](docs/copilot-usage-guide.md) — practical guide for using Copilot with this repo

### `.github/copilot-instructions.md`
Workspace-level instructions that tell GitHub Copilot how to operate within this repo.
These are automatically picked up by Copilot Chat when you open the workspace.

---

## File Naming Conventions

| Type | Pattern | Example |
|------|---------|---------|
| Learning plan | `topic-<slug>.json` | `topic-kubernetes-basics.json` |
| Question bank | `topic-<slug>.json` | `topic-kubernetes-basics.json` |
| Progress file | `topic-<slug>.json` | `topic-kubernetes-basics.json` |
| Config | `default.json` or `<profile>.json` | `default.json` |
| Schema docs | `_schema-<type>.json` | `_schema-learning-plan.json` |

The `<slug>` should be lowercase, hyphen-separated, and match across all three data directories for the same topic.

---

## Getting Started

1. Clone or open this repository in VS Code.
2. Ensure the **GitHub Copilot** extension is installed and you are signed in.
3. Open Copilot Chat (`Ctrl+Shift+I` / `Cmd+Shift+I`).
4. Type something like:

   > *"I want to start learning Kubernetes basics. Create a learning plan and generate some questions."*

5. Copilot will guide you through the rest.

See [`docs/copilot-usage-guide.md`](docs/copilot-usage-guide.md) for a full walkthrough.
