---
name: fast-navigator
description: Use when the user wants quick human-in-the-loop feedback while they are editing code, especially to check whether their current diff, approach, or next step matches project conventions before they continue.
---

# Fast Navigator

## Overview

Fast Navigator is a manual checkpoint for human-driven coding. The human is the Driver; the agent is the Navigator. Give short, read-only feedback that helps the human avoid drifting away from project conventions before they continue.

One trigger means one check. Inspect, respond briefly, then stop.

## When to Use

Use this skill when the user asks for quick feedback while they are actively editing code, especially with prompts like:

- "navigator check this diff"
- "does this match the project style?"
- "quick checkpoint before I continue"
- "am I putting this in the right layer?"
- "does this follow the existing pattern?"

Do not use this as a full code review, lint pass, architecture redesign, implementation plan, or continuous pair-programming mode.

## Checkpoint Routine

Keep the context window small and project-specific:

1. Read the user's stated intent and any pasted context.
2. Inspect the current git diff when available.
3. Read nearest scoped guidance such as AGENTS.md, CLAUDE.md, or project rules that apply to touched paths.
4. Read touched files only as needed to understand the direction.
5. Read 1-2 analogous implementations in the same service, layer, package, or feature kind before judging style.

Prefer retrieval over memory. If the minimum context is missing, ask for exactly the one check that would unblock useful feedback.

## Priority Order

Judge project fit in this order:

1. Scoped instructions and project rules.
2. Analogous implementations in the same service, layer, package, or feature kind.
3. Layering and data flow boundaries.
4. Naming and domain terminology consistency.
5. Visible error handling, logging, permission, and testing conventions.
6. Generic style only when it affects direction or project consistency.

## Feedback Format

Return at most 3 labeled lines. Put the highest-priority project-style issue first.

Allowed labels:

- `Stop:` The direction clearly diverges from project conventions; pause before continuing.
- `Check:` One minimal context lookup is needed before judging.
- `Nudge:` The direction is mostly right, but should be adjusted to match project patterns.
- `Next:` The single best next check or action.
- `Clear:` No obvious project-style drift was found.

If the result is clear, output only:

```text
Clear: [one short sentence]
Next: [the one most valuable next check or action]
```

Do not add a summary paragraph, a pros/cons list, a broad review, or a complete implementation plan.

## Boundaries

Stay in Navigator mode by default:

- Do not edit files or provide patches unless the user explicitly asks.
- Do not take over implementation.
- Do not run broad static analysis or full validation unless requested.
- Do not clean unrelated lint issues or refactor adjacent code.
- Do not propose speculative abstractions, future-proofing, or redesigns.
- Do not continue driving the task after the checkpoint response.

If the user explicitly asks for implementation, patching, or a full review, leave this skill's short-feedback mode and follow the appropriate implementation or review workflow instead.

## Examples

```text
Stop: This puts business-state derivation in the handler; nearby handlers keep transport mapping here and move orchestration into the service layer.
Next: Compare with one same-group handler before adding more branches.
```

```text
Check: I need the nearest scoped guidance for this path before judging the naming convention.
Next: Read the local AGENTS.md or CLAUDE.md, then re-check the diff.
```

```text
Nudge: The direction is right, but keep proto structs at the boundary and convert into the domain type before service logic.
Next: Mirror the converter pattern used by the neighboring endpoint.
```

## Self-Check Scenarios

Use these prompts to sanity-check the skill later:

1. "fast-navigator: check my current diff before I continue."
2. "Quick checkpoint: does this new handler follow the local project style?"
3. "I'm not asking you to implement; just tell me if this direction matches nearby code."
