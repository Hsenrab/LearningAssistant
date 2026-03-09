# Copilot Usage Guide

A practical guide for using GitHub Copilot Chat with the LearningAssistant workspace in VS Code.

---

## Prerequisites

1. This repository is open in VS Code.
2. The **GitHub Copilot** and **GitHub Copilot Chat** extensions are installed and active.
3. You are signed in to GitHub Copilot.

The `.github/copilot-instructions.md` file in this repo provides Copilot with workspace-specific context automatically — you don't need to paste instructions into every chat message.

---

## Common Tasks and How to Request Them

### Start a Learning Session

```
I want to learn [topic]. Let's start a session.
```

**Example:**
> *"I want to learn Kubernetes basics. Let's start a session."*

Copilot will:
- Load (or create) the learning plan and question bank.
- Ask you questions one at a time.
- Give feedback and explanations after each answer.
- Save your progress at the end.

---

### Create a Learning Plan for a New Topic

```
Create a learning plan for [topic].
```

**Example:**
> *"Create a learning plan for Docker fundamentals."*

Copilot will:
- Generate a `plans/topic-docker-fundamentals.json` with a 3-level hierarchy (Module → Lesson → Concept).
- Ask you to review and confirm before saving (optional).

---

### Generate Questions for a Topic or Concept

```
Generate [N] questions for [topic / concept].
```

**Examples:**
> *"Generate 5 questions for the kube-scheduler concept in Kubernetes basics."*
> *"Generate a full question set for the Docker fundamentals topic."*

Copilot will:
- Create new question objects following the schema in `questions/_schema-question.json`.
- Append them to `questions/topic-<slug>.json`.
- Cover the specified concept(s) with a mix of difficulties and question types.

---

### Check if More Questions Are Needed

```
Do I have enough questions for [topic]?
```

**Example:**
> *"Do I have enough questions for Kubernetes basics?"*

Copilot will:
- Compare the number of questions per concept against a recommended minimum (default: 3 per concept).
- List concepts that have fewer questions than recommended.
- Offer to generate additional questions for those concepts.

---

### Analyse Your Learning Progress

```
Analyse my progress on [topic].
```

**Example:**
> *"Analyse my progress on Kubernetes basics."*

Copilot will:
- Read `progress/topic-kubernetes-basics.json`.
- Summarise mastery scores per concept and per module.
- Highlight strong areas (mastery ≥ threshold) and weak areas (mastery < threshold).
- Show your accuracy trend across recent sessions.
- Recommend which concepts to focus on next.

---

### Get the Next Question

```
Give me the next question.
```

or

```
Continue my Kubernetes basics session.
```

Copilot will:
- Apply the adaptive selection algorithm (see `docs/how-it-works.md`).
- Present the next question based on your mastery scores and the config settings.

---

### Suggest the Next Topic

```
What should I learn next?
```

Copilot will:
- Review your overall mastery across all topics in `progress/`.
- Suggest a next topic based on what you have mastered vs what is still new, and natural topic dependencies where known.

---

### Adjust Learning Settings

```
Change the repetition weight to [value] for [topic / all topics].
```

**Example:**
> *"Set the repetition weight to 0.7 for all topics. I have an exam next week."*

Copilot will update `config/default.json` (or a topic-specific config file) accordingly.

---

### Add a Custom Question

```
Add a question about [topic/concept]: "[question text]"
```

**Example:**
> *"Add a question about kube-proxy: What networking technology does kube-proxy use by default on Linux?"*

Copilot will create a properly formatted question object and append it to the correct question bank file.

---

### View a Summary of a Learning Plan

```
Show me the learning plan for [topic].
```

Copilot will summarise the plan structure in a readable format, listing modules, lessons, and concepts.

---

## Tips

- **Be specific about topics and concepts.** The more specific you are, the better Copilot can target questions.
- **Review generated files.** Copilot-generated plans and questions may contain errors. Use your domain knowledge to review and correct them before relying on them for learning.
- **Use Git to track changes.** All progress data is committed to the repo. You can view your history with `git log` and revert changes if needed.
- **Customise the config.** Adjust `config/default.json` to tune difficulty progression, session length, and the repetition/novelty balance to suit your learning style.
