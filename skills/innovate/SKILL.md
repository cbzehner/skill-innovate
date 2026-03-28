---
name: innovate
description: Ask frontier models for the single smartest, most radically innovative
  addition to your plan or project. Based on Jeffrey Emanuel's "one prompt" technique.
  Use after finishing a plan, completing a feature, or when you want a creative push.
argument-hint: "[plan file or project description]"
allowed-tools: Bash, Read, Glob, Grep, Agent, AskUserQuestion
---

# Innovate: Radical Innovation Prompt

Surface the single most impactful addition to a plan or project by querying
multiple frontier models in parallel and synthesizing their best ideas.

## Origin

Based on Jeffrey Emanuel's technique: after you think you're done with a plan,
ask frontier models "What's the single smartest and most radically innovative
and accretive and useful and compelling addition you could make?"

## When to Use

- After writing or finalizing an implementation plan
- After completing a major feature — to find what you missed
- When a project feels "done" but you suspect there's a high-leverage addition
- When the user says "innovate", "what am I missing", or "what's the one thing"

## Workflow

### Step 1: Gather Context

If `$ARGUMENTS` points to a file, read it. Otherwise, read the most relevant
plan or project files in the current directory (PLAN.md, README.md, etc.).

Build a concise summary of what exists — goals, architecture, current state.

### Step 2: Query Advisors in Parallel

Run three advisors in parallel using the Agent tool (one message, three calls):

**Prompt template for each advisor:**

```
Here is the current state of a project/plan:

---
{context}
---

What is the single smartest and most radically innovative and accretive and
useful and compelling addition you could make to this {plan|project} at this
point?

Rules:
- Propose exactly ONE addition, not a list
- Explain WHY it's high-leverage — what does it unlock?
- Be specific and concrete, not vague ("add AI" is not an answer)
- Consider what's missing, not what's already there
- Think about 10x improvements, not 10% improvements
```

Run these in parallel:
1. **Claude advisor** (Agent, model: opus) — with the prompt above
2. **Gemini advisor** (Bash) — `gemini -p "{prompt}" --model gemini-3.1-pro-preview --sandbox -o text`
3. **Codex advisor** (Bash) — `codex exec --sandbox read-only --skip-git-repo-check -- "{prompt}"`

If Gemini or Codex fail (permission denied, rate limit, not installed), proceed
with whichever advisors succeed. At minimum, the Claude advisor always runs.

### Step 3: Synthesize

Present results as:

```markdown
## Innovation Proposals

### Gemini
{proposal + reasoning}

### Codex
{proposal + reasoning}

### Claude
{proposal + reasoning}

## Synthesis
{Which proposal is strongest and why. Are there common themes?
 If proposals complement each other, note that.}

## Recommended Addition
{The single best addition, refined from all inputs.
 Include concrete next steps if the user wants to proceed.}
```

### Step 4: Ask

Use AskUserQuestion to ask:
> Want to incorporate this into your plan/project, explore it further, or skip?
