---
name: retro
description: End-of-session self-review of the user's interaction patterns. Scores specificity, request confidence, guidance effectiveness, and missed tool/skill opportunities. Produces actionable takeaways for building better skills and CLAUDE.md instructions. Use when the user says /retro or asks for session feedback.
---

# Retro

Review the full session transcript. Score the user's interaction patterns. Produce actionable takeaways.

## The Rule

**Be honest.** Inflated feedback wastes the user's time. The goal is calibration — helping the user see where their prompting, specificity, and tool awareness cost them time or quality.

## Scoring Dimensions

Score each dimension 1-5. Provide 1-2 specific examples from the session as evidence for each score.

### 1. Specificity (1-5)

How precisely did the user describe what they wanted?

| Score | Meaning |
|-------|---------|
| 1 | Vague requests requiring multiple clarification rounds |
| 2 | General direction but missing key details (which files, what format, what scope) |
| 3 | Adequate — clear enough to act on, but could be tighter |
| 4 | Precise — included context, constraints, and expected output |
| 5 | Surgical — every request was unambiguous and actionable on first read |

**Look for:** Did the user specify file paths? Did they say what format they wanted output in? Did they give enough context that I could act without asking follow-ups? Did vague requests cause wasted rounds?

### 2. Request Confidence (1-5)

Did the user know what they wanted, or were they exploring?

| Score | Meaning |
|-------|---------|
| 1 | Uncertain about goals — frequent direction changes, contradictory requests |
| 2 | Exploring but without structure — jumping between topics without resolution |
| 3 | Mixed — some clear asks, some open-ended exploration (this is fine for learning sessions) |
| 4 | Clear intent with occasional pivots that made sense |
| 5 | Decisive throughout — knew what they wanted, asked for it, moved on |

**Note:** Exploration and learning sessions naturally score 2-3 here. That's not bad — flag it but don't penalize genuine learning. The issue is when lack of confidence causes thrashing (asking for X, undoing X, asking for Y, undoing Y).

### 3. Guidance Effectiveness (1-5)

When the user needed to steer me, how well did they do it?

| Score | Meaning |
|-------|---------|
| 1 | Accepted wrong answers or let me go off track without correction |
| 2 | Noticed problems but couldn't articulate what was wrong |
| 3 | Corrected me but took multiple attempts to communicate the issue |
| 4 | Clear corrections that got us back on track quickly |
| 5 | Proactively steered before I went wrong — anticipated issues |

**Look for:** Did the user catch my mistakes? Did they redirect efficiently? Did they ask the right follow-up questions? Did they challenge answers they didn't understand (good) or accept everything uncritically (bad)?

### 4. Tool & Skill Awareness (1-5)

Did the user leverage available tools, skills, and capabilities effectively?

| Score | Meaning |
|-------|---------|
| 1 | Unaware of available tools — did everything manually or asked me to do things tools could handle |
| 2 | Used basic tools but missed opportunities for sub-agents, skills, or parallel execution |
| 3 | Used some tools/skills but left efficiency on the table |
| 4 | Good tool usage with minor missed opportunities |
| 5 | Expert — used skills, sub-agents, and parallel execution to maximize throughput |

**Look for:** Could sub-agents have parallelized work? Were there skills that applied but weren't invoked? Did the user ask me to do something sequentially that could have been parallel? Did they know about relevant slash commands?

## Output Format

Present the scores, then the takeaways.

```
## Session Scores

| Dimension | Score | Trend |
|-----------|-------|-------|
| Specificity | X/5 | [brief note] |
| Request Confidence | X/5 | [brief note] |
| Guidance Effectiveness | X/5 | [brief note] |
| Tool & Skill Awareness | X/5 | [brief note] |
```

### Evidence

For each dimension, cite 1-2 specific moments from the session. Use the pattern:
- **What happened:** [quote or describe the interaction]
- **What it cost:** [time, quality, or clarity lost]
- **Better approach:** [what the user could have done instead]

### Missed Opportunities

List specific moments where skills, sub-agents, or tools could have been used but weren't. For each:
- **Moment:** [what was happening]
- **Tool/Skill:** [what could have been used]
- **Benefit:** [what it would have gained — speed, quality, structure]

### Actionable Takeaways

3-5 concrete things the user can do differently next session. Each should be:
- **Specific** — not "be more specific" but "when asking about code, include the file path and line number"
- **Actionable** — something they can do immediately
- **Buildable** — could become a skill, a CLAUDE.md instruction, or a habit

Format each as:
- **Do this:** [the action]
- **Because:** [why it matters, citing session evidence]
- **Could become:** [skill / CLAUDE.md rule / habit]

## Flag Cleanup

After running, delete the pending-feedback flag so the next session
doesn't re-prompt:

```bash
rm -f ~/.claude/pending-feedback/last-session
```

## Anti-Patterns

- Don't be sycophantic. "Great session!" with all 5s is useless.
- Don't penalize exploration. Learning sessions look different from execution sessions — adjust expectations.
- Don't invent missed opportunities. Only cite tools/skills that actually exist and would have genuinely helped.
- Don't compare to other users. Score against the user's own potential and the session's goals.
- Don't lecture. Scores + evidence + takeaways. Done.
