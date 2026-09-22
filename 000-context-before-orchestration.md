# 000 — Build the context layer before any orchestration

**Status:** Accepted
**Date:** 2026-09-21

## Context

I'm building a multi-agent workflow (research / plan / build / review)
for MatchingEngine. The tempting first step is the agents themselves.

But every agent in the pipeline reads the same project context file.
If that file states a wrong invariant, each agent inherits the error,
and each handoff passes it forward. One mistake, multiplied by every
stage.

Before writing any agent, I had Claude Code check every claim in my old context 
file against the code. One line was wrong. My old `CLAUDE.md` described a value 
as a fixed constant in the code, when it's actually configured per domain. That
taught me the context file is my job to get right, not the agent's. An agent
only works correctly on top of correct context. 

## Decision

Get the shared context right first. Build and verify AGENTS.md on a
real task before writing any subagent or handoff format.

Order: context file → skills (on-demand) → tiers → subagents → handoffs.

## Alternatives I rejected

- **Agents first, context later.** Faster to demo, but I'd be debugging
  agent behavior caused by a bad context file, without knowing which
  layer was wrong.
- **Both at once.** Two moving parts; a failure can't be attributed.

## Consequences

- Slower start. The first visible "agent" arrives later.
- Every later record assumes AGENTS.md is correct. If it changes, I
  re-check the agents that depend on it.
- Leads directly to 001 (what goes in the file).