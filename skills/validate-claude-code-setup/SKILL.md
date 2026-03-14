---
name: validate-claude-code-setup
description: Use when auditing or re-validating Claude Code configuration after time has passed, when suspecting outdated settings, when compaction is happening too often or too rarely, or when model/effort settings may no longer reflect current best practices
---

# Validate Claude Code Setup

## Overview

Claude Code settings degrade over time as models evolve, context windows change, and community best practices shift. This skill provides a structured audit and research process to keep `~/.claude/settings.json` optimized.

**Core principle:** Validate every setting against current docs + community consensus before changing. Don't guess — research first, then apply.

---

## Step 1 — Read Current State

```bash
cat ~/.claude/settings.json
```

Extract the values for these four critical settings:

| Setting | Key | Location |
|---|---|---|
| Model | `ANTHROPIC_MODEL` | `env` block |
| Autocompact threshold | `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` | `env` block |
| Context window | Model ID suffix (e.g. `sonnet[1m]`) | `env.ANTHROPIC_MODEL` |
| Effort level | `effortLevel` | top-level |

---

## Step 2 — Research Latest Recommendations

Use WebSearch to validate each setting. Always search for current year.

### 2a. Model context window

Search: `"claude sonnet [current model version] context window size 2026"` and `"claude opus [version] context window"`

**Decision matrix (as of early 2026):**

| Model | Context Window | 1M suffix needed? |
|---|---|---|
| `sonnet` (no suffix) | 200K | No |
| `sonnet[1m]` | 1M | Yes (degrades at >400K) |
| `opus` (no suffix) | 200K | No |
| `opus[1m]` | 1M | Yes (good up to ~800K) |

**Verdict for daily coding:**
- Sonnet → **stay at 200K** (retrieval collapses at 1M)
- Opus → **1M viable** only for massive codebase sessions (76% retrieval at 1M vs 93% at 256K)

### 2b. Autocompact threshold

Search: `"CLAUDE_AUTOCOMPACT_PCT_OVERRIDE best value 2026 context rot"`

**What the variable does:**
- LOWERS the autocompact trigger below the system default (~83-90%)
- Values above ~83 are silently clamped (`Math.min` bug — search GitHub issues to verify if fixed)
- Setting too low (e.g. 60) → compacts too often, loses useful context
- Setting too high → context rot (model degrades from overloaded window)

**Sweet spot as of early 2026: 75**

| Value | Effect |
|---|---|
| 60 | Too early — excessive compaction |
| **75** | **Sweet spot — avoids rot without over-compacting** |
| 80 | Acceptable, slightly more context rot risk |
| 83+ | At/above clamp limit — effectively same as default |

### 2c. Effort level

Search: `"Claude Code effortLevel best default 2026"`

**Available values:** `low`, `medium`, `high` (persisted); `max` (Opus only, session-only)

| Value | Thinking | Best for |
|---|---|---|
| `low` | Minimal | Batch, boilerplate, renaming |
| **`medium`** | **Selective** | **Daily dev — official Anthropic default** |
| `high` | Almost always | Complex arch, hard debugging |
| `max` | Always deepest | Persistent bugs, critical decisions |

**Default recommendation: `"medium"`** — Anthropic changed Opus default from `high` to `medium` in 2026; Sonnet docs explicitly recommend medium.

---

## Step 3 — Apply Validated Settings

After research, update only settings that are out of sync with current best practices.

**Optimal baseline config (validate against research before applying):**

```json
{
  "env": {
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "75",
    "ANTHROPIC_MODEL": "sonnet"
  },
  "effortLevel": "medium"
}
```

**To temporarily override per-session:**
- Context: `/model opus` or `ANTHROPIC_MODEL=opus[1m]` before starting
- Effort: type `ultrathink` in prompt or `/effort high` in session

---

## Step 4 — Verify After Applying

After editing `settings.json`, restart Claude Code and confirm:

1. Status line shows correct model
2. Compaction doesn't trigger prematurely in a normal session
3. Response quality feels appropriate for effort level

---

## What to Re-Research When Things Feel Off

| Symptom | What to search |
|---|---|
| Compacting every 10 turns | `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE clamp bug fix` + current CC version |
| Slow responses / high latency | `Claude Code 1M context latency [year]` |
| Model ignoring middle of files | `Claude lost in the middle [model version] [year]` |
| Shallow reasoning on complex tasks | `Claude Code effortLevel high vs max [year]` |
| Cost exploding | `Claude Code token usage by effortLevel [year]` |

---

## Common Mistakes

| Mistake | Fix |
|---|---|
| Setting autocompact to 60 to "prevent context loss" | That makes it compact MORE. Raise to 75. |
| Adding `[1m]` suffix to Sonnet | Retrieval collapses at 400K. Remove suffix. |
| Setting `effortLevel: "max"` as default | max is Opus-only and session-only. Silently ignored on Sonnet. |
| Not researching before changing | Best practices shift every 1-3 months as models update. Always search first. |
