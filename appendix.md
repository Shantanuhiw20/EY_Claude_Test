# Appendix - Troubleshooting

The common problems and the fix for each. Each session file also has its own troubleshooting at the bottom. The pattern that solves most of them: tell Claude what happened, paste the exact message, and let it fix it.

## Claude Code won't start

**`claude` is not recognized**
- Close the terminal. Open it again. Run the three lines from Session 0 once more.
- Still failing? Ask the facilitator.

**Nothing happens after `claude`**
- Wait a few seconds on first run. It is loading.
- If it asks you to log in, stop. This laptop uses the workshop login already set up on it. Do not sign in with a personal account. Ask the facilitator.

## During a session

**`/interview-me` or `/build-persona` not recognised**
- Claude Code needs to be started from inside `thecrux-ey-bootcamp`. Both commands live in `.claude/commands/`.
- Exit Claude Code, run the lines from Session 0 again, and retry.

**Claude keeps asking permission for everything**
- That is normal and safe. Stay in the default mode. If the same safe action keeps coming up (say, reading files), use `/permissions` to allow just that action. Avoid accept-edits mode while you are still learning the tool.

**Claude edited the wrong file**
- Tell it plainly: `Undo that last change.` It will.
- For bigger tasks, switch to plan mode first (`Shift + Tab`) so you see the plan before anything changes.

**I lost my work / the conversation got messy**
- Your files are saved on disk, not in the chat. They are still there.
- Type `/clear` to start a clean conversation. Your `CLAUDE.md`, your AP persona file (`personas/ap-persona.md`), and your skills stay.

**A build won't run (Day 2)**
- Paste the error to Claude. Say: `Fix this.` It usually can.
- If something is missing, Claude will install it when you ask.

**The Vercel deploy fails**
- Paste the exact error to Claude and ask it to fix it. Most deploy failures are one missing setting or environment variable.
- If Claude says it cannot log in to Vercel, it needs you to finish a sign-in in the browser. Use your Vercel account (the one tied to your GitHub account), then tell Claude to try again.
- If the chat fails on your laptop before you have added any API key, that is the Claude Code CLI path. Check `claude -p "hello"` works in your terminal first. See Session 4's Troubleshooting for the rest.
- If the live app loads but does not respond, tell Claude: `Check OPENROUTER_API_KEY, AI_MODEL, SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY are all set on Vercel for both production and preview, add any that are missing, and redeploy.`
- If the app works on your laptop but the live version can't find the persona, a skill, or a sample-data file, `context/` did not make it into the repo. Tell Claude: `Check that context/ is committed and pushed, and that next.config.ts has outputFileTracingIncludes covering ./context/**/*. Fix whichever is wrong and redeploy.`
- If the live app loads but saving fails, or you see `relation "diagnostic_sessions" does not exist`, the Supabase tables were never created in that project. Go back to the SQL in Session 4's setup section and run it.

**Claude can't push to GitHub**
- Read what it is asking for. It usually needs you to finish a sign-in in the browser with your GitHub account. Do that, then tell Claude to try again.

**The chatbot asks too few or too many questions**
- Too few: tell Claude to check its progress against `sample-data/ap-data-collection-template.csv` and keep going until the checklist is reasonably complete.
- Too many, or repeating itself: tell it which fields you already answered and ask it to move on.

**A file upload did not get read correctly**
- Screenshots and images should go straight to Claude as images.
- Plain text, CSV, or email-style files should be read as text. If it is not picking up the content, tell Claude: `Read the actual text of the uploaded file and use it, not just the filename.`

## When stuck

1. Paste the error to Claude. Ask it to fix it.
2. If unsure what Claude is about to do, switch to plan mode (`Shift + Tab`) and ask it to explain its plan first.
3. Scope down. A smaller version that works beats a big one that does not. This is why Day 2 builds one persona properly instead of four half-working ones.
4. Ask the facilitator.
