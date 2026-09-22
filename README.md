# skills

Agent skills I build. Each skill is one folder with a single `SKILL.md`: a
description that tells the agent when to reach for it, and a body that tells it
how to work. Plain Markdown, no runtime, no dependency on one vendor.

They run wherever an agent reads `SKILL.md`, among them Claude Code, the Claude
desktop app and Cursor.

## Install

Copy the folder into the skills directory of your agent. For Claude:

```bash
cp -r skills/meeting-to-brief ~/.claude/skills/
```

For the Claude desktop app, zip the skill folder, rename it to `.skill`, and
upload it under Customize.

## License

MIT. Take them, change them, ship them.

## meeting-to-brief

Turns a raw meeting transcript into two things. A clean record of what the
meeting decided, and a copy-paste prompt for the next agent.

A transcript is a bad input for an agent. It holds the decisions, but it also
holds the false starts, the point someone took back ten minutes later, and the
questions nobody answered. Hand that over raw and you get work back that sits
on the wrong half of the meeting. You find out when the result lands.

The chain:

- **Transcript in.** Text or file. Jamie, Otter, Fireflies, Granola, Teams
  `.vtt`, Zoom, your own notes.
- **Silent analysis.** Every gap gets one test. Does closing it change the
  result? The rest gets dropped.
- **3 to 4 questions.** Multiple choice, one option recommended.
  Contradictions get named. "Anna said end of month, Tom said the 14th. Which
  one holds?"
- **Record out.** Goal, decisions, constraints, owners, next steps, open
  points. Only the sections the meeting supports.
- **Prompt out.** Copy-paste, stands on its own. Whoever runs it never saw the
  transcript and does not need it. Open points carry the instruction to ask
  instead of guess.

Both outputs are Simple English, whatever language the meeting was in. That is
not a style preference. Simple English gives a coding agent less to interpret.
Rich English gives it more. German gives it most. Your terms, product names and
file names stay untouched, and the questions still come in your language.

Long transcripts split themselves. After each part you get a handover prompt
for a fresh chat, so the last hour does not fall off the end.
