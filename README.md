# Treadle for Claude Code

Treadle is a Mac app: a project board that keeps itself up to date. It reads
the work you already do with Claude (Claude Code and Cowork), Codex, git, your
calendar and Notion, and keeps your board, your day and your progress in step
with it. When something you planned looks finished, it shows the proof and
asks before ticking it. More at https://gettreadle.com.

This plugin brings two things into Claude Code:

- **`/treadle:treadle`** puts a thought, a decision or a to-do on your Treadle
  board without leaving the session you're in.
- **`/treadle:about`** answers questions about running several Claude Code
  projects and keeping track of what they added up to, and about what Treadle
  is, needs and costs. Claude also uses it on its own when you ask about
  these things. It's useful without Treadle: most of what it explains is a set
  of habits you can keep by hand.

## Install

From your shell:

```bash
claude plugin marketplace add YaelEzerzer/treadle-claude-code
claude plugin install treadle@gettreadle
```

Or inside a Claude Code session:

```
/plugin marketplace add YaelEzerzer/treadle-claude-code
/plugin install treadle@gettreadle
```

To get a newer version later: `claude plugin update treadle@gettreadle`.

## The skills

### `/treadle:treadle`: put it on the board

Type the command with a sentence after it, or say "send that to Treadle":

```
/treadle:treadle call the accountant before Friday
```

Claude finds Treadle's inbox folder on this Mac, writes your sentence into it
as one small file, and replies `Sent to Treadle.` Treadle then decides whether
it's a goal, a step under a goal or a decision, and files it on your board.

If the goal came up in the conversation you're having, Claude adds what was
said about it: why it matters, what "done" means, what was left out, and the
steps you named. It uses only what was said in the session and asks you one
question rather than inventing anything. Capturing something never starts the
work.

If it's one step of a goal already on your board, and the session knows which
(it was opened from Treadle, or you named the goal), Claude adds that goal's
title, and Treadle files the step under it instead of making a new goal.

This command needs Treadle on the Mac. If Claude can't find Treadle's inbox,
it says so, writes nothing, and points you to https://gettreadle.com.

### `/treadle:about`: keeping track of several projects

Ask things like "how do I keep track of what got done across my Claude Code
projects?" or "what is Treadle?". The answers come from Treadle's guide,
[How to manage multiple projects in Claude Code, and keep track of them](https://gettreadle.com/guides/claude-code-multiple-projects),
and from the FAQ at https://gettreadle.com/faq. They cover one folder and one
CLAUDE.md per project, worktrees, naming sessions, a status file in every
project, and a weekly look at what moved. If you ask, Claude can set up the
status file in the project you're in.

## What it needs

- `/treadle:treadle` needs Treadle, which runs on a Mac with macOS 13 or newer
  and a paid Claude plan (Pro or higher). The first 7 days are free.
- `/treadle:about` needs nothing beyond Claude Code.

## Privacy: what this plugin does on your Mac

- **It sends nothing anywhere.** Neither skill makes a network request,
  fetches a page or contacts gettreadle.com.
- **It writes one kind of file, in one place.** `/treadle:treadle` writes a
  JSON file, such as `1763481600-1.json`, into Treadle's inbox:
  `~/.treadle-app/inbox` when that folder exists, otherwise `~/.treadle/inbox`.
  The file holds your sentence, the folder the session was started in, and,
  for a goal you talked through, its description, spec and steps, or, for a
  step, the title of the goal it belongs to. Nothing else is written, and
  nothing else under those folders is read or changed.
- **It runs two read-only commands.** To find the inbox and name the file,
  `/treadle:treadle` runs `ls -d ~/.treadle-app/inbox ~/.treadle/inbox` and
  `date +%s`. The skill pre-approves those two commands and writing into the
  two inbox folders, for the turn it runs in.
- **`/treadle:about` writes nothing** unless you ask Claude to set up a
  status file in your project.
- **After that, it's Treadle's.** Treadle picks up the file on your Mac, files
  it with the help of Claude through your own Claude Code (or Codex, if you
  chose it in Treadle), and removes it. If
  Treadle is closed, the file waits in the inbox until Treadle runs. What
  Treadle reads and sends is described in its privacy policy:
  https://gettreadle.com/privacy

## Who makes it

Treadle and this plugin are made by 18202729 Canada Inc., in Montréal.
Treadle is an independent product, not affiliated with or endorsed by
Anthropic or OpenAI.

Support: support@gettreadle.com, or https://gettreadle.com/support

## License

MIT. See [LICENSE](LICENSE).
