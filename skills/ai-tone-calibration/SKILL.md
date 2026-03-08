---
name: ai-tone-calibration
description: "Use when designing or adjusting AI assistant personality, chatbot tone of voice, or LLM system prompt persona. Triggers: tone too formal, too casual, robotic responses, forced enthusiasm, emoji overuse, personality mismatch, user complaints about AI voice"
---

# AI Tone Calibration

## Overview

Calibrate AI assistant tone using a 3-axis scoring system instead of binary "formal vs casual" thinking. Maps personality to measurable dimensions, then translates scores into concrete system prompt instructions.

**Core principle:** Tone is not a single slider. It's three independent axes that combine to create a distinct voice.

## When to Use

- Designing a new AI assistant persona
- Users complain tone is "too robotic" or "too bubbly"
- AI responses feel inconsistent or "off-brand"
- Switching LLM models and tone shifts unexpectedly
- Building RAG/chat systems that need domain-appropriate voice

**Not for:** Marketing copy tone (use `copywriting`), general prompt engineering (use `prompt-engineer`)

## The 3-Axis Framework

Score each axis 1-5 independently:

| Axis | 1 | 3 | 5 |
|------|---|---|---|
| **Warmth** | Clinical, detached | Approachable, clear | Nurturing, effusive |
| **Formality** | Slang, fragments | Contractions, direct | Ceremonious, elaborate |
| **Humor** | Deadpan serious | Dry wit, confident | Playful, jokes, emojis |

### Common Profiles

| Profile | W | F | H | Best for |
|---------|---|---|---|----------|
| **Knowledgeable Friend** | 3.5 | 2 | 2 | Utility apps, knowledge bases, RAG assistants |
| **Professional Advisor** | 2 | 4 | 1 | Finance, legal, healthcare |
| **Enthusiastic Helper** | 5 | 2 | 4 | Onboarding, tutorials, kids' apps |
| **Neutral Expert** | 2 | 3 | 1 | Documentation, technical support |
| **Warm Professional** | 4 | 3 | 2 | Customer support, SaaS products |

## System Prompt Pattern

Translate axis scores into a `<tone>` block with concrete behavioral instructions:

```
You are {name}, a {role} for "{context}".

<tone>
- {warmth instruction}
- {formality instruction}
- {humor instruction}
- {explicit prohibitions based on low-scoring axes}
</tone>

<rules>
- {domain-specific behavior rules}
</rules>
```

### Translating Scores to Instructions

**Warmth 1-2:**
```
- Be direct and factual. Skip pleasantries.
```

**Warmth 3-4:**
```
- Sound like a knowledgeable friend. Be helpful without being effusive.
- Use casual, clear language. Contractions are fine.
```

**Warmth 5:**
```
- Be warm and encouraging. Celebrate user progress.
```

**Formality 1-2:**
```
- Use casual, direct language. Contractions, short sentences.
- Do NOT use formal phrases like "I'd be happy to assist."
```

**Formality 3-4:**
```
- Use clear, professional language. Contractions acceptable.
```

**Formality 5:**
```
- Use formal, precise language. No contractions or colloquialisms.
```

**Humor 1:**
```
- Do NOT use emojis, exclamation marks, or playful language.
- Personality comes from clarity and confidence, not from being bubbly.
```

**Humor 2-3:**
```
- Light personality through confidence, not through jokes or emojis.
- Do NOT use emojis or cheerful sign-offs.
```

**Humor 4-5:**
```
- Be playful where natural. Light humor is welcome.
- Use emojis sparingly to add warmth.
```

## Explicit Prohibitions (Critical)

Low-scoring axes MUST have explicit "Do NOT" instructions. LLMs default to helpful-enthusiastic without guardrails.

```
# Humor = 1-2: MUST include
- Do NOT use emojis, exclamation marks at end of answers,
  or cheerful sign-offs.

# Warmth = 1-2: MUST include
- Do NOT add filler phrases ("Great question!", "Happy to help!").

# Formality = 1-2: MUST include
- Do NOT use formal phrases ("I'd be delighted to assist",
  "Please don't hesitate").
```

**Why prohibitions matter:** Without explicit "Do NOT" rules, LLMs revert to default RLHF training (overly helpful, emojis, enthusiasm). Positive instructions alone ("be direct") are weaker than paired positive + negative ("be direct" + "do NOT add filler").

## Quick Reference: Implementation Checklist

1. **Define audience** -- Who uses this? What's their context?
2. **Score 3 axes** -- Rate Warmth, Formality, Humor (1-5 each)
3. **Pick closest profile** -- Use table above as starting point
4. **Write `<tone>` block** -- Translate scores using instruction patterns
5. **Add prohibitions** -- Explicit "Do NOT" for every axis scoring 1-2
6. **Test with 5 queries** -- Normal question, edge case, off-topic, same-language, different-language
7. **Adjust axes** -- If too much/little, shift by 0.5 and re-test

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Binary thinking ("friendly OR professional") | Use 3 axes independently |
| Positive-only instructions ("be warm") | Pair with prohibitions ("do NOT use emojis") |
| Describing personality instead of behavior | "Use contractions" not "be casual" |
| Ignoring LLM model tendencies | Chinese models may leak language; test multilingual |
| Same tone for all contexts | Calibrate per product area (onboarding vs. error vs. core) |
| Skipping the prohibitions | LLMs default to enthusiastic without explicit "do NOT" |

## Multilingual Considerations

When the AI must respond in multiple languages:

- Always include: "Respond in the user's language (detect from message)"
- Test with non-English queries specifically
- Some models (DeepSeek) leak their training language -- test before deploying
- Tone perception varies by culture -- "casual" in English may feel rude in Japanese
- Translate game/domain terminology naturally, don't leave in English

## Sources

- [Chatbot Personality Playbook: Tone Mapping](https://www.kommunicate.io/blog/chatbot-personality-playbook/) -- 3-axis scoring system
- [LLM Personas: System Prompts & Tone](https://brimlabs.ai/blog/llm-personas-how-system-prompts-influence-style-tone-and-intent/)
- [How to Build a Chatbot Persona](https://www.chatbot.com/blog/personality/) -- 10-component framework
- [Conversational AI Design](https://botpress.com/blog/conversation-design) -- UX best practices
- [14 Best Practices for Chatbot Design](https://www.uxness.in/2024/07/ai-chatbot-design-best-practices.html)
