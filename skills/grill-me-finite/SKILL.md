---
name: grill-me-finite
description: Interview the user relentlessly about a plan, design, architecture, or rollout until there is shared understanding. Use when the user asks to stress-test a plan, get grilled, resolve ambiguity, or walk a decision tree before implementation.
version: 1.0.0
metadata:
  hermes:
    tags: [planning, design-review, architecture, questions, finite]
    related_skills: []
---

# Grill Me

Use this skill when the user wants a plan, product decision, architecture, or
implementation approach stress-tested before work begins.

## Workflow

1. Restate the decision or plan being evaluated in one or two sentences.
2. Identify the next unresolved branch of the design tree.
3. Ask exactly one question at a time.
4. Include your recommended answer with each question.
5. If the question can be answered by inspecting the codebase, docs, logs, or
   runtime state, inspect first and avoid asking the user.
6. After each answer, update the shared understanding and move to the next
   dependency.
7. Stop when the remaining ambiguity is low enough to implement safely, or when
   the user asks to stop.

## Question Style

- Prefer concrete tradeoffs over abstract preferences.
- Ask the question that unlocks the most downstream decisions.
- Avoid asking the user to restate information already present in the repo or
  conversation.
- Keep the question short enough to answer quickly.
- When the user shows decision fatigue, make the simplest conservative choice
  yourself and flag only serious unresolved ambiguity.

## Output Style

For each turn, use this shape:

```text
Recommended answer: ...

Question: ...
```

When the interview is complete, summarize:

- decisions made;
- assumptions still in force;
- risks or rollback points;
- the next implementation step.

## Attribution

Adapted from Matt Pocock's `grill-me` skill:
https://github.com/mattpocock/skills/blob/733d312884b3878a9a9cff693c5886943753a741/skills/productivity/grill-me/SKILL.md

Original copyright (c) 2026 Matt Pocock. Used under the MIT License.
