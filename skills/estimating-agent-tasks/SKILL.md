---
name: estimating-agent-tasks
description: Use when planning features, estimating tasks, scoping sprints, or budgeting costs for projects built with AI coding agents. Triggers on estimation, story points, task sizing, sprint planning, complexity scoring, model selection, token budgeting, or timeline planning for agent-assisted development.
---

# Estimating Agent Tasks — ACR Framework

## Overview

**Agent Complexity Rating (ACR)** replaces story points for AI-agent-assisted development. Story points measure human effort to BUILD. ACR measures the real bottleneck: how hard it is to VALIDATE, REVIEW, and INTEGRATE agent output.

## When to Use

- Planning features, sprints, or roadmaps for AI-agent development
- Estimating time, cost, model, or tokens for a set of tasks
- Replacing story points in project planning documents
- **NOT for:** pure human development, non-coding tasks, single trivial changes

## ACR = E + R + I (range 3-15)

Three axes, each scored 1-5:

### E — Scope (Escopo)

| Score | Description | Example |
|-------|-------------|---------|
| 1 | 1 file, isolated change | Config tweak, typo fix |
| 2 | 2-5 files, single feature | New component + route |
| 3 | 6-15 files, cross-cutting | Endpoint + frontend + DB schema |
| 4 | 15+ files, multi-domain | Auth migration across codebase |
| 5 | Entire architecture | Full system redesign |

### R — Review (Revisao)

R = pure human review time spent reading and evaluating agent output. Does NOT include time waiting for agent iterations (that's captured in I).

| Score | Description | Human time |
|-------|-------------|------------|
| 1 | Auto-verifiable (tests pass = done) | <10 min |
| 2 | Quick diff scan | 10-20 min |
| 3 | Careful review (logic, edge cases) | 20-45 min |
| 4 | Deep review (security, architecture) | 45min-1.5h |
| 5 | Expert review (legal, UX, compliance) | >1.5h |

### I — Iteration (Iteracao)

Each cycle = one round-trip: human reviews output → gives feedback → agent adjusts. NOT agent-internal retries.

| Score | Description | Round-trips |
|-------|-------------|-------------|
| 1 | First-shot (agent nails it) | 1 |
| 2 | Minor fixes | 2 |
| 3 | Moderate back-and-forth | 3-4 |
| 4 | Complex integration, many adjustments | 5-7 |
| 5 | Exploratory, trial-and-error | 8+ |

## ACR to Time Mapping

| ACR | Size | Wall clock (solo dev + agent) |
|-----|------|-------------------------------|
| 3-4 | **XS** | 30min - 1h |
| 5-6 | **S** | 1-3h |
| 7-9 | **M** | 3-8h (half day to full day) |
| 10-12 | **L** | 1-2 days |
| 13-15 | **XL** | 2-4 days |

## Model Selection

| Condition | Model | Why |
|-----------|-------|-----|
| ACR >= 10 **or** security/auth critical | **Opus** | Deep reasoning, edge cases, architectural decisions |
| ACR 7-9 (default) | **Sonnet** | Best cost-quality balance for standard implementation |
| ACR <= 6 **and** low risk | **Haiku** | Fast and cheap: boilerplate, static pages, config |

**Security override:** Security/auth tasks always use Opus regardless of ACR score. A simple auth config change (ACR 5) still needs Opus because security mistakes are costly and hard to detect in review.

**Hybrid strategy within a phase:** Opus as architect/lead, Sonnet subagents for implementation, Haiku for repetitive sub-tasks.

## Token and Cost Estimation

| Size | Turns | Tokens ~ | Opus ~$ | Sonnet ~$ | Haiku ~$ |
|------|-------|----------|---------|-----------|----------|
| XS | 5-10 | 100-250K | $3-8 | $0.60-1.50 | $0.15-0.40 |
| S | 10-20 | 250-500K | $8-15 | $1.50-3 | $0.40-0.80 |
| M | 20-40 | 500K-1.2M | $15-36 | $3-7 | $0.80-2 |
| L | 40-80 | 1-2.5M | $30-75 | $6-15 | $1.50-4 |
| XL | 80-150 | 2-5M | $60-150 | $12-30 | $3-8 |

> Blended rates (3:1 input:output): Opus ~$30/M, Sonnet ~$6/M, Haiku ~$1.60/M.
> On Max plan ($100-200/mo), use token estimates for quota planning instead of cost.
> **Range heuristic:** Use lower bound for greenfield projects (less context to read). Use upper bound for legacy codebases (many files to read/understand before changing).

## Parallelism

When tasks can run in parallel (subagents/teams), total time = **critical path** (longest sequential chain), not sum of all tasks.

**How to calculate:**
1. List all tasks with dependencies (use `→` for "blocks": `Task A → Task B` means B depends on A)
2. Draw the dependency graph
3. Find the longest path — that's your real timeline
4. Parallel tasks within that path add zero extra time

**Example:** Tasks A, B, C where A → C and B → C (C depends on both):
- Sequential time: A + B + C
- Parallel time: max(A, B) + C

## Output Format

When estimating a project, produce a table like this:

| Task | E | R | I | ACR | Size | Model | Tokens ~ | Cost ~ | Time ~ |
|------|---|---|---|-----|------|-------|----------|--------|--------|
| Task name | X | X | X | XX | S/M/L | Opus/Sonnet/Haiku | XXK-XXM | $XX | Xh/Xd |

Then calculate:
- **Total tokens** (sum)
- **Total cost** (sum, considering model per task)
- **Sequential time** (sum of all times)
- **Parallel time** (critical path only)

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using story points habits (effort-based) | ACR measures review + iteration burden, not coding effort |
| Same model for everything | Match model to task: Opus for hard, Haiku for trivial |
| Summing time for parallel tasks | Use critical path, not total sum |
| Ignoring iteration cycles | I axis is often the biggest time factor |
| Overestimating scope of simple tasks | A 1-file change is E1 even if the file is complex |
| Underestimating review for security tasks | Security/auth always gets R >= 4 |
