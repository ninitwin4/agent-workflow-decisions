# agent-workflow-decisions

Decision records for a multi-agent AI development workflow I'm building
for my personal projects, starting with
[MatchingEngine](https://github.com/ninitwin4/matching-engine) Work in progress.

## Why this repo exists

I want to dedicate each agent to a single task instead of letting one agent wear multiple hats. 
I'd rather spend tokens on the work, not on context the task doesn't need. With subagents, I can talk to 
just the one scoped to my task, rather than loading everything into a single conversation. 
I also don't want to push a one-line fix through four stages, so small tasks skip the pipeline entirely.

## How to read it

Each file in `decisions/` is an Architecture Decision Record (ADR):
context, the decision, alternatives I rejected, and consequences.
Records are numbered and append-only. If I change my mind, I write a
new record that supersedes the old one rather than editing it.

## Decisions so far

| # | Decision | Status |
|---|----------|--------|
| 000 | [Keep AGENTS.md lean](decisions/000-lean-agents-md.md) | Accepted |
| 001 | [Route tasks by first unknown](decisions/001-tier-routing.md) | Draft |

## Planned

- Subagents (scoped, to control token cost)
- The handoff artifact (so each stage can start cold)




