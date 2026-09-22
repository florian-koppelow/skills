---
name: meeting-to-brief
description: |
  Turn a raw meeting transcript into a clean record of what was decided, plus a
  copy-paste prompt for another agent. Asks a short round of multiple-choice
  questions first, to close the gaps the meeting left open. Use whenever the
  user pastes or points to a meeting transcript, meeting notes, call notes, a
  voice-recording transcript, or an export from Jamie, Otter, Fireflies, Zoom,
  Teams, or Granola, and wants a briefing, a handover, a summary of decisions,
  or a prompt for another agent. Trigger on phrases like "here is the
  transcript", "make a briefing out of this meeting", "turn this into a
  prompt", "what did we decide", or "write this up for the next session" - also
  when the user never says "transcript" but pastes a long block of
  speaker-labelled talk. Do NOT use for live note taking, for transcribing
  audio, or for summarising an article, a document, or a chat log that is not a
  meeting.
---

# Meeting to Brief

A raw transcript is a bad input for an agent. It holds the decisions. It also
holds false starts, half sentences, the point that someone took back ten
minutes later, and the questions that nobody answered. An agent that reads it
raw must guess at the gaps. It can also pick the wrong side of a contradiction.
Nobody sees the error until the work comes back wrong.

This skill sits between the room and the next agent. It takes out the real
decisions. It asks the human about the few gaps that change the result. It
gives the next agent a brief that works without the transcript.

Run the five steps in order. Do not merge two steps. Do not write a summary
before Step 4. An early summary fixes the picture too soon, and it turns the
question round into a formality.

## Step 1 - Intake

Accept the transcript as pasted text or as a file (`.txt`, `.md`, `.docx`,
`.vtt`, `.srt`, `.json`, `.pdf`). If the user names a file, read it. If there is
no text and no file, ask for the transcript and stop. The skill cannot work
without the source.

Check the size before you read. If the transcript does not fit in one pass, go
to **Long transcripts** below. Do not read until you run out of room.

Find the language of the transcript. Then speak that language for the whole
session. This covers the questions and every other message that you write.

Write nothing about the content yet. No summary. No "I see you discussed X". Go
to Step 2.

## Step 2 - Silent analysis

Read the full transcript. Find these items:

- the decisions that the meeting made
- the statements that an agent can read in two ways
- the decisions that contradict each other
- the questions that the meeting raised and left open
- the constraints, the people, the deadlines and the next steps

Then rank each gap with one test: does this gap change the final prompt? If it
does not, drop it. A gap that only makes the record tidier is not worth a
question.

This step has no output. The user sees nothing until Step 3.

## Long transcripts

A long transcript can fill your context before you reach Step 3. An analysis
that stops in the middle is worse than no analysis. It drops the last part of
the meeting, and that part usually holds the decisions.

Use this test. If you read the full transcript, do you keep enough room to run
Steps 3 to 5? If you do not, the transcript is too long. Decide this before you
read, not after.

**1. Tell the user first.** Give the length of the source, the number of parts,
and what happens at each handover. A user who learns this in the middle of the
work loses trust.

**2. Cut at natural seams.** Cut at a topic change, an agenda item, a break, or
a gap in the timestamps. Never cut inside a sentence. Never cut inside an
exchange. If a decision falls across two parts, you will get that decision
wrong. Make each part small enough to analyse with room to spare.

**3. Analyse one part at a time.** Step 2 stays silent for a transcript that
fits. Across parts it must leave a record, because the next context starts
blind. After each part, write a short part note:

```
## Part N of M - <topic or time range>
Decisions:
Vague statements:
Contradictions with earlier parts:
Open questions:
Constraints, people, dates:
Carry forward:            <- open items that a later part can settle
```

Keep the note short. The note replaces the transcript for every later step, so
it must hold the facts. Quote a line only when the exact words matter, for
example one side of a contradiction.

If file tools are available, write the notes to a working file. A file survives
a context reset, and the next chat reads the file instead of the transcript.

**4. Hand over between parts.** At the end of each part, print a handover
prompt. The user can paste it into a new chat:

~~~~
```
Task: Continue a meeting analysis that runs across several chats.
Use the meeting-to-brief skill.

Source: <file path, or where the transcript is>
Progress: parts 1 to N of M are analysed.
Notes so far: <file path, or the part notes inline>

Next: analyse part N+1 (<topic or time range>) and write its part note.
Ask the user nothing yet. The question round runs once, after every part is
analysed.
```
~~~~

**5. Merge, then continue.** After the last part, read all part notes together
and reconcile them before Step 3. Contradictions across parts appear here. A
person can settle a point in part 1 and reverse it in part 4. Rank the gaps
across the full meeting. Then run Step 3 once, on the merged picture. Never run
one question round per part. The user must then answer the same question
several times, and a later part can make an early answer wrong.

Steps 4 and 5 then run as normal, from the merged notes.

## Step 3 - Refinement (interactive)

Ask the user about the gaps that survive the rank test.

**Ask in rounds.** Each round holds 3 to 4 questions. Never more in one round.
Use 3 rounds at most. Stop earlier as soon as more questions cannot change the
result. A user who just left a meeting has little patience. This step must feel
like a quick check, not a second meeting.

**Make each question a real choice.** Give 2 to 4 concrete options. Mark one
option as recommended and give a short reason. Always accept a free-text
answer. If an interactive question tool with tappable options is available, use
it, because it is faster for the user. If no such tool is available, number the
questions so the user can answer by number.

**Each question must change the brief.** Before you ask, know what you write
differently for each answer. If every answer leads to the same prompt, cut the
question.

**Never ask what the transcript answers.** This shows the user that you did not
read it, and it wastes a question.

**Name a conflict openly.** If two statements clash, quote both in short form
and ask which one holds. Do not take the later one and hope.

Two habits from hard interviewing apply here, but keep them light. Go at the
soft spots, not at the safe ground. Kill a vague word with a concrete choice.
"Soon" becomes a date. "We should look into it" becomes in scope or out of
scope.

A partial answer is an answer. Apply what the user gave and move on. Do not ask
the same question again in new words.

**Example round:**

> The transcript leaves three things open. Quick check:
>
> 1. Deadline. Anna said "end of the month". Tom said "before the fair on the
>    14th". Which one holds?
>    a) 14th - it is the hard external date **(recommended: a fair date cannot move)**
>    b) End of month
>    c) Other
>
> 2. The export feature was "nice to have" and later "we need it for the demo".
>    In scope or out?
>    a) Out of scope for now **(recommended: the demo comes up only once)**
>    b) In scope
>
> 3. Who owns the API part? Nobody said a name.
>    a) Tom  b) Anna  c) Nobody yet - leave it open

## Step 4 - Synthesis

Merge the transcript and the answers of the user into one record.

Write the record in Simple English. This holds for every transcript language.
Use short sentences, common words, active voice, and one idea per sentence.
Keep project terms, product names, file names and quoted phrases exactly as the
meeting said them. Your own chat around the record stays in the language of the
user.

Use only the sections that the material supports. An empty section is worse
than a missing one, because it tells the reader that something exists when it
does not.

- **Goal** - what this is for, in one or two sentences
- **Decisions** - what the meeting settled, one per line
- **Constraints** - budget, tech, dates, rules, dependencies
- **Out of scope / rejected options** - only if the meeting rejected something.
  Not a list of things that nobody mentioned.
- **People and owners** - who does what
- **Next steps and deadlines** - concrete actions, with dates where known
- **Open questions** - what is still open

Invent no fact. Each line comes from the transcript or from an answer of the
user. If a line is your assumption, say so in plain words.

## Step 5 - Follow-up prompt

Build the copy-paste block.

Give the prompt the shape of the task that the meeting decided to build: a
skill, a deck, code, a concept, a document, a research task. The shape follows
the task. There is no fixed template. A prompt for a slide deck speaks about
slides and audience. A prompt for code speaks about interfaces and tests.

Every prompt must do this:

- Stand on its own. The reader never saw the transcript and cannot ask the
  room. The block holds everything that the reader needs.
- Use Simple English and speak to the agent directly ("Build...", "Write...",
  "Use...").
- State the task first, then the context, then the decisions to honour.
- List the constraints and the out-of-scope items.
- End with an **Open questions** block. It holds every item that the user could
  not answer. Tell the agent to ask about these items and not to guess.
- Name the output format.

## Output

Print the record in chat, so the user can read it. Then print the follow-up
prompt below it, in a fenced code block, ready to copy. Put nothing between
them but one short line of your own.

**Shape of the final message:**

~~~~
[record, as headed sections]

---

Here is the prompt for the next agent:

```
Task: ...

Context: ...

Decisions to honour:
- ...

Constraints:
- ...

Out of scope:
- ...

Open questions - ask me about these, do not guess:
- ...

Output: ...
```
~~~~

## General rules

**Stand alone.** Do not call another skill and do not depend on one. This file
holds everything that the skill needs.

**If the user is not there, continue.** Skip the question round only on a clear
signal: the user says "just do it" or "no questions", or the run has no human
in it, for example a batch or a subagent. Then make the reasonable choice for
each gap. Label the choice as an assumption in the record. Carry the same
assumption into the prompt. A labelled guess costs the user ten seconds to
correct. A blocked task costs more.

**Few sharp questions beat many complete ones.** Two questions that change the
brief are worth more than six that make it tidier. If you are unsure whether to
ask, do not ask.

**A messy transcript is normal.** Expect broken sentences, wrong speaker
labels, overlapping talk and filler. Read through it. Do not ask the user to
clean it up. Do not quote the mess back at the user.
