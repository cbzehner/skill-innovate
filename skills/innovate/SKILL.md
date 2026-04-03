---
name: innovate
description: Queries frontier models in parallel for the single highest-leverage
  addition to a plan or project. Use when brainstorming, seeking creative push,
  or asking "what am I missing?"
argument-hint: "[plan file or project description]"
effort: high
allowed-tools: Bash, Read, Glob, Grep, Agent, AskUserQuestion
---

# Innovate

Surface the single most impactful addition to a plan or project by querying
multiple frontier models in parallel and synthesizing their best ideas.

Based on Jeffrey Emanuel's technique: after you think you're done, ask
"What's the single smartest and most radically innovative addition you
could make?"

## When to Use

- After writing or finalizing an implementation plan
- After completing a major feature — to find what you missed
- When a project feels "done" but you suspect there's a high-leverage addition
- When the user says "innovate", "what am I missing", or "what's the one thing"

## When NOT to Use

- **Bug fixing or debugging** — use systematic-debugging instead
- **Execution tasks** — if the user wants implementation, not ideation
- **Incomplete plans** — innovate on something finished, not half-baked
- **Scope-constrained work** — if the user explicitly wants MVP or minimal scope
- **Routine refactoring** — not for standard code cleanup

## Workflow

### Step 1: Gather Context

Read `$ARGUMENTS` if it points to a file. Otherwise, search for plan or project
files in the current directory (PLAN.md, README.md, AGENTS.md, CLAUDE.md, src/, etc.).

**Why:** Models produce generic suggestions without concrete context. The more
specific the input, the more specific (and useful) the proposal.

Build a concise summary: goals, architecture, current state, constraints.

### Step 2: Query Advisors in Parallel

Run three advisors in parallel (one message, three Agent/Bash calls):

See [references/advisor-prompt.md](references/advisor-prompt.md) for the prompt template and advisor specifications (host-native, Gemini, Codex).

### Step 3: Synthesize

**Why synthesize:** Raw advisor output overwhelms. Your job is to critically
evaluate, find themes, and elevate the most practical high-impact idea.

Present results using the format in [references/synthesis-template.md](references/synthesis-template.md).

### Step 4: Ask

Use AskUserQuestion:
> Want to incorporate this into your plan/project, explore it further, or skip?

## Error Handling

| Situation | Action |
|-----------|--------|
| Tool not installed | Note "Unavailable: not installed" in output, proceed with others |
| Rate limit / 429 | Retry once after 60s, then mark unavailable |
| Permission denied | Note in output; suggest allowlisting `gemini` / `codex` in the host agent settings |
| Thin context | Ask user for more context before querying — garbage in, garbage out |
| All proposals generic | Tighten the prompt with more specific context and re-query one advisor |
| No genuinely novel idea | Say so honestly — don't force a recommendation that isn't there |

## Examples

**Trigger:** `/innovate PLAN.md` after finishing an implementation plan
**Context gathered:** Plan for a CLI tool with parsing, transformation, output phases
**Output:** Three proposals (e.g., structural diff, plugin system, watch mode) →
synthesized recommendation with concrete next steps.

**Trigger:** `/innovate` in an existing project directory
**Context gathered:** README, src/ structure, existing features
**Output:** One high-leverage addition the project is missing.

**Anti-trigger:** "Fix this OAuth callback bug" → do NOT trigger innovate.
This is a debugging task, not an ideation task.
