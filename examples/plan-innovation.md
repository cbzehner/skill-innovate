# Example: Innovating on an Implementation Plan

## Scenario

You've just finished writing an implementation plan for a CLI tool that processes markdown files. The plan covers parsing, transformation, and output — but you want to check if there's a high-leverage addition you're missing.

## Session

```
You: /innovate PLAN.md

Claude: Reading PLAN.md... Found a CLI tool plan with 4 phases:
parsing, AST transformation, output rendering, and testing.

Querying 3 advisors in parallel...

## Innovation Proposals

### Gemini
**Add an incremental processing mode with file-watching.**
Instead of processing all files on every run, hash each input file and
skip unchanged ones. Combined with `--watch`, this turns a batch CLI into
a live development tool. High-leverage because it changes the usage
pattern from "run occasionally" to "always running" — which means users
discover bugs and formatting issues instantly instead of in CI.

### Codex
**Add a plugin system for custom AST transformers.**
Let users drop `.js` files into a `plugins/` directory that export a
`transform(ast) -> ast` function. This is high-leverage because it turns
your specific tool into a platform — every user's unique formatting
needs become solvable without forking.

### Claude
**Add structural diff output that shows what changed semantically.**
Instead of just outputting the transformed markdown, show a tree-diff of
the AST before/after transformation. This is high-leverage because it
makes the tool debuggable — users can verify transformations are doing
what they expect, which builds trust and adoption.

## Synthesis
All three proposals target the same gap: the plan builds a tool that
*works* but doesn't help users *understand and extend* it. Gemini's
watch mode improves the feedback loop, Codex's plugins improve
extensibility, and Claude's diff improves transparency.

## Recommended Addition
**Structural diff output** (Claude's proposal) — it's the smallest
implementation cost with the highest trust-building impact. Watch mode
and plugins are valuable but can be added later; debuggability should
be built in from the start.

Want to incorporate this into your plan, explore it further, or skip?

You: Incorporate it — add it as a new phase between transformation and testing.

Claude: [Updates PLAN.md with Phase 3.5: Structural Diff Output]
```
