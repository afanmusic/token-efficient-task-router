# token-efficient-task-router Guide

## What This Skill Does

`token-efficient-task-router` is a control skill that helps an agent decide how to work before it starts spending tokens or editing files.

It is designed to answer questions such as:

- Is this request clear enough to execute?
- Should the agent diagnose, plan, or act?
- Is the task low-risk or does it need confirmation?
- Does the user need a full result, or just the next useful step?

## Why It Exists

Many agent failures come from the same pattern: acting too broadly before the real task has been scoped.

This skill reduces that problem by routing the request through four decision lenses:

- clarity
- complexity
- risk
- expected token cost

## The Four Modes

### Ask Mode

Use when the user wants explanation, diagnosis, feasibility judgment, risk analysis, or next-step advice.

### Plan Mode

Use when the task is multi-step, file-sensitive, batch-oriented, or expensive enough that scope should be confirmed before execution.

### Craft Mode

Use when the task is clear, low-risk, and can be completed in one safe pass.

### Expert Mode

Use when repeated attempts are failing, the task needs deeper specialization, or further trial-and-error would be a poor trade.

## Interactive Routing

The skill can ask the user to confirm:

- task complexity
- preferred working mode
- output depth

This is especially useful when the request is ambiguous, file-related, batch-oriented, or likely to consume more tokens than necessary.

## Progressive Delivery

The skill is built to prefer the smallest useful output first.

Typical progression:

1. decision or diagnosis
2. sample or pilot output
3. staged execution
4. full batch delivery

This makes the skill a better fit for real-world work where the right scope is often discovered during execution.

## File Safety

The skill is intentionally conservative around:

- core file overwrites
- deletion
- batch rename or move operations
- deployment-impacting changes
- database-impacting changes

For these categories, confirmation gates are part of the expected behavior.

## Platform Behavior

### WorkBuddy

Prefers scope-first file handling, read-only analysis, and sample-batch execution before broad folder changes.

### iMA Copilot

Prefers narrow retrieval, short evidence-based outputs, and material grouping before long-form expansion.

### CodeBuddy

Prefers limited code reading, minimal diffs, and focused validation instead of broad scan-and-rewrite behavior.

## Example Prompts

```text
Please route this task into Ask, Plan, Craft, or Expert mode.
```

```text
Use Plan Mode first. Do not modify files yet.
```

```text
This is a small low-risk task. Use Craft Mode directly.
```

```text
Diagnose this issue first and avoid changing code until the cause is clearer.
```

```text
Use the most token-efficient approach and only give me the next useful step.
```

## What This Skill Does Not Try To Do

It does not try to replace domain skills. It does not try to make every task slower. It does not try to produce maximum detail by default.

Its purpose is narrower: make agent execution safer, cheaper, and more aligned with the real need behind the request.
