# 002 — Move sometimes-needed knowledge into on-demand skills

**Status:** Draft
**Date:** 2026-09-29

> **Takeaway:** If only some tasks need it, make it a skill. The agent
> sees a one-line description and loads the rest only when relevant.

## Context

001 keeps AGENTS.md to what an agent would get wrong, because every line
loads into every session. But some knowledge is essential only for
certain tasks. In MatchingEngine, running and reading the AI-bonus eval
is the clearest example: it costs money, needs an API key, and has
results that are easy to misread. Most tasks never touch it.

## Decision

Knowledge needed only for certain tasks goes in a skill, not AGENTS.md.
A skill's description is always visible to the agent, but its body only
loads when the task matches.

- AGENTS.md keeps a one-line pointer (e.g. the eval command); the skill
  holds the how-to and how-to-read.
- The description names concrete triggers (files, commands, phrases) and
  what the skill is *not* for, so it doesn't load on unrelated work.
- First skill: [`run-evals`](https://github.com/ninitwin4/matching-engine/blob/main/.claude/skills/run-evals/SKILL.md). It covers running the eval, cost, and how to
  read each check. One trap it covers: green tests don't prove the AI layer
  works, because every test uses a fake client.

## Alternatives I rejected

- **Put it in AGENTS.md.** Every session pays for knowledge that most
  sessions don't use. This is what 001 set out to avoid.
- **A plain doc in `docs/`.** Free until read, but nothing tells the
  agent when to read it. A skill's description does.

## Consequences

- Each skill description is a small fixed cost. Vague descriptions
  either load the skill too often or miss the task entirely.
- Knowledge is now in two places. When code changes, I check both.
- Leads to tiers: once context is split by task, the next step is
  splitting the *work* by task size.

## Evidence

Partially tested in a fresh session, without naming the skill.

- **Loads when it should: pass.** Prompt: "The last AI-bonus eval came
  back 7/8 with ai-006 failing direction. What should I do?" The skill
  loaded first. The agent applied rules that exist only in the skill:
  re-run once before calling it real (ADR-002), read each run's rationale,
  and treat negative-direction cases as fragile. It also used "suspect the
  pin before the case": it found the `anthropic` pin had changed since the
  last run and gave that as a reason to re-run. It recommended the plain
  run over `--escalate` because the plain run is what users get.
- **Not yet tested:** that it stays unloaded on unrelated work (a
  frontend task, and a Tier 1 pytest task that the description
  excludes), and a full run of a new eval case.
