# Innovate

Ask frontier models for the single smartest, most radically innovative addition to your plan or project.

> *"What's the single smartest and most radically innovative and accretive and useful and compelling addition you could make to the plan at this point?"*
>
> — [Jeffrey Emanuel](https://x.com/doodlestein)

## Why Use This?

When you finish a plan or project, you're often too close to see the highest-leverage addition. This skill queries multiple frontier models in parallel — each brings different training data and reasoning styles — then synthesizes their proposals into a single recommendation.

## Prerequisites

For multi-model queries, install the Gemini and Codex CLIs:

```bash
# Gemini CLI
npm install -g @google/gemini-cli
gemini --login

# Codex CLI
npm install -g @openai/codex
codex login
```

The skill degrades gracefully — if only Claude is available, it still works.

## Installation

### Manual Installation

```bash
cd ~/.claude/skills/
ln -s ~/Developer/Personal/skill-innovate/skills/innovate innovate
```

## Usage

```
/innovate                  # Auto-detect plan/project in current directory
/innovate PLAN.md          # Target a specific file
/innovate "my project"     # Describe what to innovate on
```

### When to Use

- After finalizing an implementation plan
- After completing a major feature
- When a project feels "done" but you want one more high-leverage push
- When you say "what am I missing?" or "what's the one thing?"

### Example

```
You: /innovate PLAN.md

Claude: [Reads plan, queries 3 models in parallel]

## Innovation Proposals

### Gemini
Add a feedback loop that automatically captures user behavior...

### Codex
Implement a shadow-mode deployment that runs both old and new...

### Claude
Add property-based testing that generates edge cases from your types...

## Recommended Addition
The shadow-mode deployment (Codex's proposal) is the highest-leverage
because it de-risks the entire rollout...

Want to incorporate this into your plan, explore it further, or skip?
```

## How It Works

```
/innovate PLAN.md
     |
     +-- Read context (plan file, README, etc.)
     |
     +-- Query in parallel:
     |       +-- Claude (Agent, Opus)
     |       +-- Gemini CLI
     |       +-- Codex CLI
     |
     +-- Synthesize proposals
     |
     +-- Present recommendation + ask user
```

## Files

```
skill-innovate/
├── .claude-plugin/
│   ├── plugin.json         # Plugin metadata
│   └── marketplace.json    # Marketplace listing
├── skills/
│   └── innovate/
│       └── SKILL.md        # Skill definition
├── examples/
│   └── plan-innovation.md  # Example session
├── README.md               # This file
└── LICENSE                 # MIT
```

## Privacy & Cost

Your plan/project context is sent to Google (Gemini), OpenAI (Codex), and Anthropic (Claude) APIs. Each invocation queries up to 3 models.

## License

MIT
