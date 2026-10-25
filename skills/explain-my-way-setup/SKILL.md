---
name: explain-my-way-setup
description: Set up or update the explain-my-way output style for this user. Runs a short calibration (pick which explanation lands best, list known and new areas) and writes a personal output style file. Use when the user runs /explain-my-way-setup, or asks to set up, recalibrate, or change how Claude explains things to them.
argument-hint: "[update]"
---

# explain-my-way — setup

Goal: in about 5 minutes, learn how this person understands explanations best, and save it as their personal output style.

This skill's directory contains two files. Read both before you start:
- `calibration-pairs.md` — the tests to show the user, and how to show them.
- `style-template.md` — the output style to fill in.

## Ground rules

- Talk to the user in the language of their latest message.
- Keep your own messages short. Plain words. No praise, no filler.
- Ask one thing at a time, in plain text. Do not use pop-up question tools in this skill: they can hide the text the user needs to read.
- Never invent profile values. Every value must come from a test result or from what the user typed.
- **Files:** the only file this skill may write is the output style file in step 7 (and `settings.json` in step 8, only after a yes). Do not save tests, notes or drafts to any other file. Do not write to memory.
- **Tools:** read and write files with the normal file read and write tools. Do not use shell commands in this skill. They are slow and break on some systems, for example Windows.

## Step 0 — Check for an existing profile

Read `~/.claude/output-styles/explain-my-way.md`. If the read fails because the file does not exist, that is fine: continue with step 1.

If it exists: show the current "Reader profile" section and ask: update some settings, or start over? For "update", run only the steps for the settings the user wants to change, then steps 6–8.

## Step 1 — Say what will happen

In two or three short lines: three quick tests where they pick the explanation that lands best, one question about layout, then two questions about what they know. At the end you show the profile, and nothing is saved before they confirm.

## Step 2 — Run the tests

Run the three tests and question 4 from `calibration-pairs.md`, one per message, exactly in the format that file describes. Wait for the answer each time.

- Map each answer with the mapping in the file.
- If the user adds a comment, keep it for the "Notes" line.
- If the user says a pick was unsure, a guess, or that they could not see the texts, show that test again before you move on.

## Step 3 — Known and new areas

Ask two questions, one at a time:
1. "Which areas do you know well? Name the field and, if you like, the tools you use." (Example: "accounting — Excel" or "web development — JavaScript".)
2. "Which areas are new or hard for you right now?" (Example: "a new codebase at work" or "tax law".)

Save the answers as the user wrote them.

## Step 4 — Analogy sources

Say that you will use the "Known well" areas for analogies. Ask if they want to add anything else they know well from daily life (a sport, a hobby, a past job), or reply "skip".

## Step 5 — Optional real examples

Ask: "Do you have one explanation that did not land for you, and one that did? Paste both, or reply skip." In the same message, add: nothing confidential, please — no company code, documents or internal messages.

If they paste something:
- Compare the two. Find the one or two clearest differences (for example: the bad one started with a formula, or used three undefined terms in one sentence).
- If a difference contradicts a test result, ask the user which one is right. Do not decide alone.
- Write one short line about it for "Notes". Do not save the pasted text itself.

## Step 6 — Show the profile and confirm

Build the profile in exactly this format:

```
- Starting point: <example first | big picture first | rule first>
- Known well: <areas, as the user said them>
- New to: <areas, as the user said them>
- Analogy sources: <areas and other sources>
- Term handling: <define new terms | assume terms, define only on request>
- Depth: <short first, offer more | full by default>
- Shape: <prose | bullet steps | small diagram when it helps>
- Notes: <short notes from comments or examples, or "none">
```

Show it and ask: save as is, or change something? Apply changes and show the profile again only if something changed. If the user says a setting is unsure, offer to rerun that test.

## Step 7 — Write the output style

1. Take `style-template.md`. Replace the line `{{PROFILE}}` with the confirmed profile. Change nothing else.
2. Ask where to save it:
   - For all projects (recommended): `~/.claude/output-styles/explain-my-way.md`
   - Only this project: `.claude/output-styles/explain-my-way.md` inside the current project folder

   If the current folder is the user's home folder, both options are the same path. Do not ask; use the first one and say so in one line.
3. If the file already exists, say so and ask before you overwrite it.
4. Write the file with the file write tool. Then say where it is, as a full path.

## Step 8 — Turn it on

Tell the user, in short lines:
1. Claude Code reads output style files only when it starts, so exit and start Claude Code again.
2. Then run `/output-style explain-my-way`. This choice is saved for the current folder only. In another project folder, run it again there, or use point 3.
3. Optional: to make it the default everywhere, `"outputStyle": "explain-my-way"` can go in `~/.claude/settings.json`. Ask yes or no. On yes: read the file, add only that one key, keep everything else unchanged, and write it back with the file tools.

Then suggest one quick test after the restart: ask Claude to explain one concept from their "New to" list.

## Updating later

If the user says the style feels off ("too simple", "too long", "still too academic"), they can run `/explain-my-way-setup update`. Run only the steps for the settings they want to change, then steps 6–8.
