# retro

End-of-session self-review for Claude Code. Scores your interaction patterns and produces actionable takeaways so you get better at prompting over time.

## Setup

1. Copy `SKILL.md` into your Claude Code skills directory (e.g. `~/.claude/skills/retro/SKILL.md`)
2. Run `/retro` at the end of any session

## Usage

Type `/retro` before ending a session. Claude reviews the full transcript, scores your patterns, and gives you concrete things to do differently next time.

## Workflow

1. **Session ends** (you `/clear`, close terminal, etc.) → `SessionEnd` hook writes a flag to `~/.claude/pending-feedback/last-session`
2. **Next session** → when you run `/checkin` or `/startwork`, they check for that flag and ask:
   > You didn't run /retro last session. Want a quick retro? (yes / skip)
3. **If you run `/retro` before ending** → the skill deletes the flag, so the next session won't prompt

## Sample Output

```
## Session Scores

| Dimension              | Score | Trend                                      |
|------------------------|-------|--------------------------------------------|
| Specificity            | 3/5   | Good on file paths, vague on expected output |
| Request Confidence     | 4/5   | Clear intent, one justified pivot           |
| Guidance Effectiveness | 2/5   | Let two wrong approaches run too long       |
| Tool & Skill Awareness | 3/5   | Used sub-agents once, missed two opportunities |

### Evidence

**Specificity**
- **What happened:** "Can you fix the auth flow?" — no file, no error, no expected behavior.
- **What it cost:** Two clarification rounds before work started.
- **Better approach:** "The login redirect in `src/auth/callback.ts:42` returns 401 instead of redirecting to `/dashboard`. Fix the redirect target."

**Request Confidence**
- **What happened:** Started with "build a CLI tool", pivoted to "actually make it a library" after seeing the API shape.
- **What it cost:** Nothing — the pivot was informed and early.
- **Better approach:** N/A — this was a good pivot.

**Guidance Effectiveness**
- **What happened:** Claude generated a 200-line component with inline styles. User said "that's not quite right" but didn't specify what was wrong.
- **What it cost:** Claude guessed wrong on the next iteration, burning another round.
- **Better approach:** "The structure is fine but extract the styles to a CSS module and split the form into a separate component."

**Tool & Skill Awareness**
- **What happened:** Asked Claude to update three independent config files one at a time.
- **What it cost:** ~2 minutes of sequential waiting.
- **Better approach:** "Update these three files in parallel using sub-agents" or let Claude batch independent edits.

### Missed Opportunities

- **Moment:** Debugging a failing test for 4 rounds of guess-and-check
- **Tool/Skill:** `/debug` skill — would have forced hypothesis-first debugging
- **Benefit:** Likely resolved in 1-2 rounds instead of 4

- **Moment:** Building a new API endpoint from scratch
- **Tool/Skill:** `/brainstorm` skill before implementation
- **Benefit:** Would have surfaced the auth requirement before coding started

### Actionable Takeaways

- **Do this:** When reporting a bug, include the file path, line number, actual behavior, and expected behavior.
- **Because:** The vague "fix the auth flow" request cost two clarification rounds.
- **Could become:** CLAUDE.md rule — "Bug reports must include: file, line, actual, expected."

- **Do this:** When Claude's output is wrong, name the specific part that's wrong, not just "that's not right."
- **Because:** Vague corrections led to a wasted iteration on the component refactor.
- **Could become:** Habit — always point to the specific element before asking for changes.

- **Do this:** When you have 3+ independent file changes, tell Claude to parallelize them.
- **Because:** Sequential config updates took 3x longer than necessary.
- **Could become:** Habit — batch independent work by default.
```
