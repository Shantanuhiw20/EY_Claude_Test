# Appendix - Troubleshooting

The common problems and the fix for each. Each session file also has its own troubleshooting at the bottom. The pattern that solves most of them: tell Claude what happened, paste the exact message, and let it fix it.

## Claude Code won't start

**`claude` is not recognized**
- Close the terminal panel in VS Code, open a fresh one (**Terminal -> New Terminal**), run `claude` again.
- Still failing? Ask the facilitator.

**Nothing happens after `claude`**
- Wait a few seconds on first run. It is loading.
- If it asks you to log in, stop. This laptop uses the workshop login already set up on it. Do not sign in with a personal account. Ask the facilitator.

## During a session

**`/interview-me` or `/build-persona` not recognised**
- Claude Code needs to be started from inside `thecrux-ey-bootcamp`. Both commands live in `.claude/commands/`.
- Exit Claude Code, check VS Code has the `thecrux-ey-bootcamp` folder open, run `claude` again, and retry.

**Claude keeps asking permission for everything**
- That is normal and safe. Stay in the default mode. If the same safe action keeps coming up (say, reading files), use `/permissions` to allow just that action. Avoid accept-edits mode while you are still learning the tool.

**Claude edited the wrong file**
- Tell it plainly: `Undo that last change.` It will.
- For bigger tasks, switch to plan mode first (`Shift + Tab`) so you see the plan before anything changes.

**I lost my work / the conversation got messy**
- Your files are saved on disk, not in the chat. They are still there.
- Type `/clear` to start a clean conversation. Your `CLAUDE.md`, your AP persona file (`personas/ap-persona.md`), and your skills stay.

## When stuck

1. Paste the error to Claude. Ask it to fix it.
2. If unsure what Claude is about to do, switch to plan mode (`Shift + Tab`) and ask it to explain its plan first.
3. Scope down. A smaller version that works beats a big one that does not. This is why Day 2 builds one persona properly instead of four half-working ones.
4. Ask the facilitator.
