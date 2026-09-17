[← Back to Student Handbook](student-handbook.md)

---

# Session 0: Hello Claude

**Day 1**

**What you'll do:** Get the bootcamp project folder onto your laptop, start Claude Code, confirm it is running with the workshop login, and run your first prompt. Then a short look at where the two days go.

---

## What Claude Code is

Claude Code is Anthropic's tool for working with Claude directly on your own computer, instead of in a browser chat window. It can read your files, write files, run commands, and hold a conversation with you about your actual work.

You will type one word today to start it: `claude`. After that, you talk to Claude in plain English and it handles the rest.

You can copy the commands and paste them, but read what you're pasting. That's where the learning is.

---

## The two tools you'll use

- **Visual Studio Code** (we'll call it **VS Code**): a free app where you'll do all your work for two days. Think of it as the room you work in.
- **Claude Code**: the AI. You start it by typing `claude` once inside VS Code, then you talk to it in plain English.

That's the whole setup. One room, one assistant.

---

## Step 1: Clone the bootcamp materials in VS Code

Instead of making a folder and downloading files by hand, you'll let VS Code fetch everything in one go. It creates the folder, pulls in all the files, and opens it for you. This is the same on Windows and Mac.

1. Open **Visual Studio Code**. It opens on the **Welcome** tab. (If you don't see it, open **Help -> Welcome**.)
2. On the Welcome screen, under **Start**, click **Clone Git Repository...**
3. A box drops down from the top of the window. Paste this in and press **Enter**:

```
https://github.com/thecrux-ai/thecrux-ey-bootcamp
```

4. A dialog opens asking where to put it. It's already pointing at your **home folder**, that's fine, don't change it. Click **Select as Repository Destination** to accept.
5. Look at the **bottom-right corner** of VS Code. You'll see a small **cloning** progress message. It takes a few seconds.
6. When it finishes, a message pops up: *"Would you like to open the cloned repository?"* Click **Open**. This opens it right here in the same window.
7. If VS Code asks *"Do you trust the authors of the files in this folder?"*, click **Yes, I trust the authors**. These are your own course files.

> **Don't see "Clone Git Repository" on the Welcome screen?** Open the Command Palette (**View -> Command Palette**, or `Cmd/Ctrl + Shift + P`), type `Git: Clone`, press **Enter**, and carry on from step 3. It's the same thing.

---

## Step 2: Confirm the files are there

Look at the left panel in VS Code (the **Explorer**). The folder `THECRUX-EY-BOOTCAMP` is now open, with all the session files inside:

```
THECRUX-EY-BOOTCAMP
├── session-0-hello-claude.md   <- this file
├── session-1-claude-md.md
├── session-2-skills.md
├── session-3-connectors-brief.md
├── session-4-research-agents.md
├── session-5-builds.md
└── ...
```

That's everything for the two days, sitting on your machine, and VS Code is already pointed at the right folder.

> If the files didn't appear, or the clone reported an error, ask the facilitator. It's usually sorted in under a minute.

---

## Step 3: Open a terminal and start Claude

A **terminal** is a small panel at the bottom of VS Code. You'll only type one word into it to get going.

1. From the top menu, choose **Terminal -> New Terminal**. A panel opens along the bottom of VS Code.
2. Because VS Code already has the bootcamp folder open, this terminal starts **inside** it, exactly where you want to be.
3. Click inside the panel, type the word below, and press **Enter**:

```
claude
```

Claude Code starts up. You'll see something like a `>` prompt on a new line. That's Claude waiting for your message.

> **This is the only thing you ever type into the terminal to start Claude: `claude`.** Everything from here on is plain English typed at the `>` prompt, or menu clicks in VS Code.

> **How to tell it's in the right place:** ask Claude `What folder are you in right now?` It should name the `thecrux-ey-bootcamp` folder. That means it can see all the session files.

> **Mac/Windows note:** before you type `claude`, the terminal prompt might end in a `%` or `$`, or start with `PS`. That's all normal. Just type `claude` after it.

---

## Step 4: Confirm the workshop login

You're signed in with the workshop login already set up on this laptop. Stay on that login, not a personal account.

When Claude Code opens, it should go straight to a prompt waiting for your text, with no sign-in screen. The facilitator will show you on screen what the correct account banner looks like.

If you instead see a screen asking for an email or password, stop and ask the facilitator. Do not sign in with a personal account to get past it.

---

## Step 5: Your first prompt

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
| The clone fails, or asks for a username and password | The repo address is wrong, or the repo needs a sign-in | Ask the facilitator for the correct URL or access. |
| "Clone Git Repository..." isn't on the Welcome screen | The Welcome tab may be closed | Open the Command Palette (`Cmd/Ctrl + Shift + P`), type `Git: Clone`, press Enter, and carry on from Step 1. |
| VS Code doesn't offer to open the cloned repository | Sometimes the pop-up is missed | Open it yourself: **File -> Open Folder...**, pick `thecrux-ey-bootcamp`. |
| `claude` says "command not found" | The terminal opened before setup finished | Close the terminal panel, open a fresh one (**Terminal -> New Terminal**), run `claude` again. If it still fails, ask the facilitator. |
| Claude opens and asks me to log in | The workshop login on this laptop wasn't applied | Do not sign in with a personal email. Ask the facilitator. |
| I'm not sure if this is the workshop account or a personal one | Hard to tell from the banner alone | Check with the facilitator before doing anything else. |
| Claude seems stuck or confused | It happens | Press `Escape` to stop it. If it's still odd, `/clear` for a fresh conversation. |
| The chat feels slow after a long task | The conversation has grown large | Run `/context` to check, then `/compact` to summarise and keep going, or `/clear` to start fresh. |

---

**Next:** [Session 1: CLAUDE.md, Then Your AP Persona →](session-1-claude-md.md)
