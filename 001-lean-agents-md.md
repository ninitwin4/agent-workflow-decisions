# 000 — Keep AGENTS.md lean: only what an agent would get wrong

## Context

I use coding agents on MatchingEngine. The common advice is to give
agents a thorough context file: directory tree, tech stack, conventions.

"Evaluating AGENTS.md" (Gloaguen et al., ETH Zurich, 2026) tested this.
LLM-generated context files lowered task success and raised inference
cost by over 20%. Human-written files helped slightly, at a similar
cost increase. The authors recommend limiting these files to details
an agent can't infer from the repo.

Every line in AGENTS.md is loaded into every session, so every line
has a recurring token cost.

## Decision

AGENTS.md contains only what an agent would get **wrong** by reading
the code. Nothing inferable: no directory trees, no tech-stack lists.

- The file is hand-written, not generated.
- It lives at the repo root (563 words as of this record).
- CLAUDE.md is one line, `@AGENTS.md`, so there is a single source of
  truth across tools.
- Anything needed only sometimes goes in an on-demand skill, not here.

Two examples from MatchingEngine. Healthcare has no AI step at all, only housing
does, and nothing in the code makes that obvious. The AI adjustment can be negative,
even though every name in the code says "bonus." An agent reading only the code
would treat a negative value as a bug.

## Alternatives I rejected

- **Generate it with /init.** Fast, but this is the case the paper
  found harmful: it restates what the agent can already read.
- **No context file.** Cheapest, but my repo has traps that are not
  visible from the code alone. For example, on a cache miss, scoring
  silently falls back to base scores. Tests still pass, so an agent would call it done.
- **A comprehensive hand-written file.** The small success gain does
  not justify paying for it on every session.

## Consequences

- Lower fixed token cost per session.
- I have to maintain the file by hand and prune it when code changes
  make a line inferable.
- Sometimes-needed knowledge needs another home. That leads to the
  skills layer (run-evals is the first).

## Evidence

Verified working on a fresh machine.
Result: pass.
- Tests: 92 passed on a fresh clone.
- Rule 1 (signed adjustment): followed without being told.
- Rule 2 (verify before claiming done): the agent ran the cache-invalidation check instead of stopping at green tests.
- Not tested: the healthcare rule. The task didn't touch healthcare.