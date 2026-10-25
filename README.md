<img src=".claude-plugin/icon.png" alt="explain-my-way icon" width="96" align="right">

# explain-my-way

AI agents often explain things like a textbook: every word is familiar, but the meaning does not come through. explain-my-way makes Claude Code explain concepts, domain terms and code changes in the way **you** understand them.

It is not built on "personality types" or "learning styles" (the research does not support matching explanations to a type). It uses well-tested rules for clear explanations, plus a few personal settings that come from a short test of what actually lands for you.

## Quick start

In your terminal:

```bash
claude plugin marketplace add obence09/explain-my-way && claude plugin install explain-my-way@explain-my-way
```

Then start Claude Code and run `/explain-my-way:explain-my-way-setup`.

## Why this exists

When an AI agent explains something, you can understand every word and still not get it. Then you ask again: "explain it much simpler". Many people know this feeling from physics or maths at school.

Take Newton's second law:

- Textbook version: "force = mass × acceleration".
- What actually lands: "the heavier a thing is, the more force you need to speed it up."

The same gap shows up at work:

- **New domain words**, when you move into a new industry.
- **An unfamiliar codebase**, where AI-written change summaries assume you already know which part depends on which.
- **Formal-first writing**, where the rule comes first and the concrete picture comes last, or never.

explain-my-way started as a fix for one person's version of this problem. It was then made tunable, so anyone can calibrate it to themselves.

### What the research says

The first idea was to sort people into "communication style types". The research does not support this. People do have preferences, but a large review (Pashler and colleagues, 2008) found almost no proof that matching teaching to a "learning style" helps. Workplace type tests such as DISC or Myers-Briggs have weak validity too.

So there are no types. There are adjustable settings, filled from what actually lands for you in a test, plus rules backed by research:

| Finding | What the tool does with it |
| --- | --- |
| Concreteness fading (Fyfe and colleagues, 2014): concrete first, then the abstract | Example first, formal term last |
| Cognitive load theory: working memory is small | One new idea per sentence, short sentences, side details cut |
| Expertise reversal (Kalyuga): help for beginners is noise for experts | Your level is set per topic ("Known well" vs "New to"), not per person |
| Curse of knowledge (Newton's 1990 tapping study) | A term counts as unknown unless it is in your known areas |
| Common ground (Clark and Brennan, 1991) | Short answer first, plus an easy way to ask for more |
| Analogy as structure mapping (Gentner) | Analogies from your own field, picked for the closest structure, with a note on where they break |
| Everyday explanation (Miller, 2019): explanations are contrastive and selective | Main reason first; "X instead of Y, because…" |
| Research on programmers' questions (Sillito; Ko) | Code change explanations start with "where" and "what is connected", not details |

## What it does

- **Short first:** a concrete case, the key point, the rule in plain words, the formal name last. More only if you ask.
- **Only the terms you need**, each defined on first use in one plain sentence. No unexplained abbreviations.
- **Analogies from your own field**, picked for the closest match.
- **Code changes explained with a map:** where the change sits, before vs after, why, and what could break.
- Answers in the language you write in.

## Install

### Option 1 — as a plugin (recommended)

Inside Claude Code:

```
/plugin marketplace add obence09/explain-my-way
/plugin install explain-my-way@explain-my-way
```

Then run `/explain-my-way:explain-my-way-setup`.

### Option 2 — as a personal skill

Download the repository, then copy the folder `skills/explain-my-way-setup` into your personal skills folder.

Windows (PowerShell):

```powershell
git clone https://github.com/obence09/explain-my-way.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse ".\explain-my-way\skills\explain-my-way-setup" "$HOME\.claude\skills\explain-my-way-setup"
```

macOS / Linux:

```bash
git clone https://github.com/obence09/explain-my-way.git
mkdir -p ~/.claude/skills
cp -r ./explain-my-way/skills/explain-my-way-setup ~/.claude/skills/
```

Start Claude Code and run:

```
/explain-my-way-setup
```

## After setup

Setup takes about 5 minutes and writes your personal output style to `~/.claude/output-styles/explain-my-way.md`.

1. Exit Claude Code and start it again (output styles are read only at start).
2. Run `/output-style explain-my-way`.

The `/output-style` choice is saved for the current folder only. To use the style in every project, add `"outputStyle": "explain-my-way"` to `~/.claude/settings.json` (setup offers to do this for you).

To change settings later: `/explain-my-way-setup update`.

## How it works

| File | What it is |
| --- | --- |
| `skills/explain-my-way-setup/SKILL.md` | The setup flow: three quick tests, four questions, writes your output style |
| `skills/explain-my-way-setup/calibration-pairs.md` | The tests: the same idea explained in different ways |
| `skills/explain-my-way-setup/style-template.md` | The output style rules, with a slot for your profile |

The output style keeps Claude Code's normal coding behaviour (`keep-coding-instructions: true`). It only changes how things are explained.

## Remove

Run `/output-style default` in Claude Code first, so no session points to the removed style.

Plugin install:

```
/plugin uninstall explain-my-way@explain-my-way
/plugin marketplace remove explain-my-way
```

Personal skill install, Windows (PowerShell):

```powershell
Remove-Item -Recurse "$HOME\.claude\skills\explain-my-way-setup"
```

Personal skill install, macOS / Linux:

```bash
rm -rf ~/.claude/skills/explain-my-way-setup
```

With either install, also delete your output style.

Windows (PowerShell):

```powershell
Remove-Item "$HOME\.claude\output-styles\explain-my-way.md"
```

macOS / Linux:

```bash
rm ~/.claude/output-styles/explain-my-way.md
```

## Limits

- An output style is an instruction, not a guarantee. If an answer misses, say "simpler" or "too long" and Claude will adjust and offer to update your profile.
- Output styles do not apply to subagents.
- Optional project notes: `.claude/explain-my-way/glossary.md` and `.claude/explain-my-way/architecture.md`. Claude reads them if they exist and asks before adding to them.

## Status

Version 0.3 — early draft, tested on Windows. Feedback and issues welcome.

### Changes in 0.3

- Test 2 and Test 3 use everyday topics (salt on winter roads, compound interest) instead of specialist ones.
- Setup skips the save-location question when both options are the same folder.
- Code change explanations: changes outside the main topic go under Risk, a length target, at most three new terms.
- The "want more" line is one sentence with at most three offers.

## License

[MIT](LICENSE)
