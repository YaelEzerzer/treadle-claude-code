---
name: treadle
description: Send one sentence to Treadle, the user's project board on this Mac, to be filed as a goal, a step under a goal, or a decision. Use when the user runs this command with a thought, decision or to-do after it, or says "send that to Treadle" or "put that on the board".
argument-hint: "[what to put on the board]"
allowed-tools:
  - Bash(ls -d *)
  - Bash(date *)
  - Edit(~/.treadle-app/inbox/**)
  - Edit(~/.treadle/inbox/**)
---

# Put a sentence on the Treadle board

The user wants a thought captured on their Treadle board without leaving this
session. Treadle is a Mac app that watches an inbox folder on this Mac and
decides for itself whether a sentence is a goal, a step or a decision. All
this command does is write one small file into that folder.

What the user gave with the command: $ARGUMENTS

## 1. Find Treadle's inbox

Run this one command with the Bash tool, exactly as written:

`ls -d ~/.treadle-app/inbox ~/.treadle/inbox; date +%s`

- If `~/.treadle-app/inbox` is listed, use it. It is the inbox of the Treadle
  app from gettreadle.com.
- Otherwise, if `~/.treadle/inbox` is listed, use that one.
- If neither is listed, write nothing and reply with this, then stop:
  "I can't find Treadle on this Mac. If it's installed, open it once (it makes
  its inbox when it starts) and try again. If it isn't, Treadle is at
  https://gettreadle.com."

The last line of the output is the current time in seconds. Use it in the file
name below.

## 2. Decide what goes on the board

1. Take the text the user gave with the command. If they gave none, use the
   single thing they most recently said they want to remember, decided, or
   need to do in this conversation. If that is unclear, ask one short
   question: what should go on the board?
2. **A goal you talked through together goes on with its context.** If it is
   a goal (an outcome that takes several steps, not a single to-do or a
   decision) and this conversation is where it came up, or you are adding it
   yourself, write these beside the sentence:
   - `description`: why it matters and what led to it, from this
     conversation, in two to five plain sentences.
   - `spec`: one opening line saying what the goal is, then `Done when:` with
     a bullet for each thing that will be true when it is finished, then
     `Not now:` with bullets for what was deliberately left out (or
     `Not now: nothing named yet`). Twelve lines at most.
   - `steps`: the steps you named together, in order, each a short action.
     Leave it out if none were named.
   Use only what was said or decided here; never invent. If you cannot write
   the description and spec from this conversation, ask the user one short
   question before filing, rather than filing the sentence bare. A quick
   thought the user hands you with nothing discussed ("call the dentist") is
   filed as the sentence alone.

## 3. Write the file

Write exactly one file per item with the **Write tool** (not a shell command,
so no quoting can change the words):

- Path: `<inbox folder>/<seconds>-1.json`, with the home directory written out
  in full, for example `/Users/sam/.treadle-app/inbox/1763481600-1.json`. For
  a second item in the same go, end the name in `-2`, and so on.
- Contents, valid JSON with proper escaping:
  `{"text": "<the sentence, in the user's words>", "cwd": "<this session's working directory>"}`
- For a goal from step 2, add the other three:
  `{"text": "…", "cwd": "…", "description": "…", "spec": "…", "steps": ["…", "…"]}`
  (`\n` between the spec's lines).

## 4. Reply

Reply with one line, `Sent to Treadle.`, and nothing else.

## Rules

- File it; do not do it. Capturing a task is not permission to start on it.
- Keep the user's words. Fix only obvious dictation slips.
- One item per file. If they gave several distinct items, write one file each.
- Write only new files in the inbox folder. Never read, change or delete
  anything else under `~/.treadle-app` or `~/.treadle`.
