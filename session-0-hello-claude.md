[← Back to Student Handbook](student-handbook.md)

---

# Session 0: Hello Claude

**Day 1**

**What you'll do:** Get the bootcamp project folder onto your laptop, start Claude Code, confirm it is running with the workshop login, and run your first prompt. Then a short look at where the two days go.

---

## What Claude Code is

Claude Code is Anthropic's tool for working with Claude directly on your own computer, instead of in a browser chat window. It can read your files, write files, run commands, and hold a conversation with you about your actual work.

You start it from the terminal, the window where you type instructions to your computer as text. You will type three lines there today. After that, you talk to Claude in plain English and it handles the rest.

You can copy the commands and paste them, but read what you're pasting. That's where the learning is.

---

## Step 1: Open your terminal

- **Windows:** click Start, type `powershell`, press Enter.
- **Mac:** press Command + Space, type `terminal`, press Enter.

A window opens with some text in it. Leave it open.

---

## Step 2: Get the project folder and start Claude

Everything you build over the two days lives in a project folder called `thecrux-ey-bootcamp`. You get it the way any real project starts: by cloning it from GitHub.

**Windows PowerShell or Mac Terminal, same commands:**

```
git clone https://github.com/thecrux-ai/thecrux-ey-bootcamp.git
cd thecrux-ey-bootcamp
claude
```

Line one copies the project folder from GitHub onto your laptop. Line two moves you into it. Line three starts Claude Code.

---

## Step 3: Confirm the workshop login

You're signed in with the workshop login already set up on this laptop. Stay on that login, not a personal account.

When Claude Code opens, it should go straight to a prompt waiting for your text, with no sign-in screen. The facilitator will show you on screen what the correct account banner looks like.

If you instead see a screen asking for an email or password, stop and ask the facilitator. Do not sign in with a personal account to get past it.

---

## Step 4: Your first prompt

Type this into Claude:

```
What folder am I in right now, and what's inside it? Explain it in plain English.
```

Claude works out the right command, runs it, reads the result, and tells you where you are and what's there. You asked in plain English and it did the work. Everything over the two days follows that pattern.

One thing to know from the start: before Claude creates, changes, or deletes a file, it asks you first. You will see a permission prompt with options like **Yes**, **Yes, and don't ask again this session**, and **No**. Read what it is proposing, then answer.

---

## Commands inside Claude

Anything you type that starts with `/` is a command to Claude Code itself, not a message to the AI. Type `/` on its own and the full list pops up. The ones you will use:

| Command | What it does |
| --- | --- |
| `/help` | List all commands |
| `/permissions` | Control what Claude can do without asking |
| `/model` | See or switch the Claude model |
| `/context` | See how full the chat's memory is |
| `/compact` | Summarise the chat so far to free up space, and keep going |
| `/clear` | Start a fresh conversation (your files stay as they are) |
| `/exit` | Leave Claude Code |

`/clear` and `/compact` only affect the conversation, never your files. Clearing the chat is like starting a new email thread, not deleting your documents.

Two keys worth knowing: `Escape` stops Claude mid-response. `Ctrl + C` twice exits.

---

## Where the two days go

| Session | What you'll build |
| --- | --- |
| **CLAUDE.md, then your AP Persona** (next) | A short `CLAUDE.md` for your own context and house rules, then `personas/ap-persona.md`, a persona file that captures how an AP expert thinks and talks to a client |
| **Skills** (after lunch) | Four small skills, data-checklist, benchmark-interpreter, template-filler, file-reader, the exact parts tomorrow's build assembles |
| **Agents** (later today) | What an agent is, then one agent built: the Reviewer, a skill given a job and rules it never breaks. Tomorrow's app is built directly from the persona and the four skills, no extra agent layer needed. The Reviewer sharpens your diagnostic questions today, then comes back once tomorrow for a last look at the generated report |
| **Guided Build** (Day 2) | One persona, Accounts Payable, built end to end, pushed to your own GitHub repo, and deployed live to Vercel |
| **Hackathon** (Day 2) | You build AR, R2R, FP&A, or your own client use case |

By the end of Day 2 you will have a working Claude Code build you deployed yourself, and a way of thinking about AI you can take straight back to a client engagement.

---

## Checkpoint: Session 0 complete

> - [ ] `thecrux-ey-bootcamp` cloned onto your laptop
> - [ ] Claude Code running, workshop login confirmed
> - [ ] First prompt run
> - [ ] You know what `/clear` and `/compact` do

---

## Troubleshooting - Session 0

| Issue | What's happening | Fix |
| --- | --- | --- |
| `git clone` fails with "repository not found" or asks for a username and password | The repo address is wrong, or the repo needs a sign-in | Ask the facilitator for the correct URL or access. |
| `git` says "command not found" | Git isn't set up on this laptop | Ask the facilitator. Don't install it yourself on this machine. |
| `claude` says "command not found" | The terminal opened before setup finished | Close the terminal, open it fresh, run `claude` again. If it still fails, ask the facilitator. |
| Claude opens and asks me to log in | The workshop login on this laptop wasn't applied | Do not sign in with a personal email. Ask the facilitator. |
| I'm not sure if this is the workshop account or a personal one | Hard to tell from the banner alone | Check with the facilitator before doing anything else. |
| Claude seems stuck or confused | It happens | Press `Escape` to stop it. If it's still odd, `/clear` for a fresh conversation. |
| The chat feels slow after a long task | The conversation has grown large | Run `/context` to check, then `/compact` to summarise and keep going, or `/clear` to start fresh. |

---

**Next:** [Session 1: CLAUDE.md, Then Your AP Persona →](session-1-claude-md.md)
