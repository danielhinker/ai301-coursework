# Voice guide: how I talk upstream


## Who I am in threads

I'm a working full-stack developer (Python and TypeScript) taking CodePath AI301, and
this is one of my first open-source contributions. On an issue I say what I checked,
what I saw, and what I'll do next. Readers can expect short comments with the output
pasted, not opinions.

## Rules I write by

### Rule: name the issue, not the vibe

Every claim names this issue's function, trigger, or symptom, so it couldn't be pasted
onto another issue.

- Wrong: "Hi, I'd like to work on this one if that's ok!"
- Right: "I'm going to reproduce the empty-sections result from `_detect_sections()` when the header lines are indented, and post what I find."

### Rule: promise the next step, not the fix

I promise the investigation and a report. I never promise a fix, a PR, or a date
before I've reproduced it.

- Wrong: "I'll have a fix up by Friday."
- Right: "I'll post a reproduction report here once I've run it."

### Rule: show it, then say it

A sentence that says something happened sits next to the output that shows it. If I
didn't see it, I call it a guess.

- Wrong: "Confirmed, the regex is broken."
- Right: "With indented headers the snippet detects no sections (output below). My guess is the line-start anchor, but I haven't tested that yet."

### Rule: say when AI helped

When an AI tool helped me write code or a comment, I say so in one line, even when
the repo doesn't ask.

- Wrong: (no mention, after Claude Code drafted the report)
- Right: "I used Claude Code (AI) to set up the environment, run these commands, and draft this report; I reviewed the output before posting."

## Things I never post

- A fix, PR, or date promise before I've reproduced the bug.
- "+1", "same here", or "can confirm" with nothing of my own attached.
- Output I didn't run myself, or output edited to look cleaner.
- A root cause stated as fact when it's a guess.
- Anything that talks down to another contributor's claim or repro.
