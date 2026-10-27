---
name: explain-my-way-cursor-setup
description: Set up or update the explain-my-way rule in Cursor. Runs a short calibration (pick which explanation lands best, list known and new areas) and creates a personal rule that makes the agent explain things the way this user understands them. Use when the user runs /explain-my-way-cursor-setup, or asks to set up, recalibrate, or change how the agent explains things to them.
---

# explain-my-way — setup for Cursor

Goal: in about 5 minutes, learn how this person understands explanations best, and save it as their personal Cursor rule.

This skill's directory contains two files. Read both before you start:
- `calibration-pairs.md` — the tests to show the user, and how to show them.
- `style-template.md` — the rule to fill in.

## Ground rules

- Talk to the user in the language of their latest message.
- Keep your own messages short. Plain words. No praise, no filler.
- Ask one thing at a time, in plain text. Do not use pop-up question tools in this skill: they can hide the text the user needs to read.
- Never invent profile values. Every value must come from a test result or from what the user typed.
- **Files:** the only file this skill may write is the project rule in step 7, and only if the user picks that option. Do not save tests, notes or drafts to any other file. Do not save anything to memories.
- **Tools:** read and write files with the normal file read and write tools. Do not use terminal commands in this skill. They are slow and break on some systems, for example Windows.

## Step 0 — Check for an existing profile

Read `.cursor/rules/explain-my-way.mdc` in the current project. If the read fails because the file does not exist, that is fine.

- If the file exists: show its "Reader profile" section and ask: update some settings, or start over?
- If it does not exist and the user asked to update: they may have a User Rule, which you cannot read. Ask them to paste the "Reader profile" part of it, or reply "start over".

For "update", run only the steps for the settings the user wants to change, then steps 6–8.

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

## Step 7 — Create the rule

1. Take `style-template.md`. Replace the line `{{PROFILE}}` with the confirmed profile. Change nothing else.
2. Ask where it should apply:
   - **All projects (recommended):** a User Rule. Cursor keeps User Rules in its settings, not in a file, so the user pastes it in once.
   - **Only this project:** a project rule file, `.cursor/rules/explain-my-way.mdc` inside the current project folder.

   If an update started from an existing project rule, save back to that file and do not ask.

### All projects

Show the filled-in text in one code block, so the user can copy it in one go. Leave out the header at the top (the lines from the first `---` to the second `---`): User Rules are plain text. Do not write any file.

### Only this project

If the file already exists, say so and ask before you overwrite it. Write the filled-in text, header included, with the file write tool. Then say where it is, as a full path.

## Step 8 — Turn it on

Tell the user, in short lines, the steps for the option they picked.

All projects:
1. Copy the text from the code block above.
2. In Cursor, open **Customize → Rules**, add a **User Rule**, and paste the text.
3. Start a new chat. User Rules apply in every project.

Only this project:
1. The rule is on already: it is set to always apply. Start a new chat to use it.
2. The file holds this user's personal profile. In a shared or company repository, keep it out of version control: add `.cursor/rules/explain-my-way.mdc` to `.git/info/exclude` (a local ignore list that is never shared), unless the user wants to share it.

Then suggest one quick test: in a new chat, ask for an explanation of one concept from their "New to" list.

## Updating later

If the user says the style feels off ("too simple", "too long", "still too academic"), they can run `/explain-my-way-cursor-setup update`. Run only the steps for the settings they want to change, then steps 6–8.
