# EY Finance Transformation × theCRUX.ai - Claude Code Bootcamp

Welcome. Over two days you go from AI user to AI builder.

You will not become a developer. You will build a real diagnostic tool: a chat-based Accounts Payable persona that interviews a client, reads what they upload, and produces a benchmarked diagnostic report, deployed live on the internet. Then you adapt that same pattern to your own practice area.

This handbook is your map. Each session has its own file. Open one session at a time. Every prompt you need is inside the session files.

---

## Before Day 1

Three accounts to have ready before you arrive. All free and quick to set up.

**A GitHub account.** Go to github.com and sign up with a personal email address. This is where the code you build on Day 2 will live. The free plan is all you need.

**A Vercel account.** Go to vercel.com, click Sign Up, and choose "Continue with GitHub". Sign in with the GitHub account you just created. That is the whole step. Vercel is where your app will run live on Day 2.

**A Supabase account.** Go to supabase.com and sign up, again choosing "Continue with GitHub". Signing up is all you need to do now. You will create the actual project and its tables during Session 4, following the steps there. Supabase is the database that saves each client interview so it can be picked up again later.

You do **not** need an API key of your own. While you build on Day 2 the app runs through your Claude Code login. A key is only needed right at the end, to deploy, and the facilitator gives you one then.

---

## What You Will Walk Away With

| What | Where it ends up |
| --- | --- |
| `CLAUDE.md`, your own context and house rules, the same file no matter how many personas you build | Project root |
| An AP persona file, describing the AP finance role you are building for | `personas/ap-persona.md` |
| Four small skills: data-checklist, benchmark-interpreter, template-filler, file-reader | `.claude/skills/` |
| The Reviewer agent, wrapping a job and rules it never breaks around critiquing a report or a question set, plus a sharpened set of diagnostic questions from the Reviewer | `.claude/agents/` |
| A deployed AP diagnostic chatbot: chat intake, file upload, every interview saved and resumable from its own link, benchmarked report, code on your own GitHub repo, live on Vercel | `ey-fin-os/` |
| A second persona (AR, R2R, FP&A, or your own real client use case), built with `/build-persona` without touching `CLAUDE.md`, also on GitHub and deployed | `personas/` (new persona file) + your hackathon folder |

---

## Day 1 - Foundations

1. [Session 0: Intro + Hello Claude](session-0-hello-claude.md)
2. [Session 1: CLAUDE.md, Then Your AP Persona](session-1-claude-md.md)
3. [Session 2: Skills - the diagnostic toolkit](session-2-skills.md)
4. [Session 3: Agents](session-3-research-agents.md)

## Day 2 - Build + Hackathon

1. [Session 4: The Guided Build - AP Diagnostic Chatbot](session-4-builds.md)
2. [Hackathon: Your Own Persona](capstone.md)
3. [Feedback, testimonials + Close](closing.md)

---

## Sessions At A Glance

| # | Session | What you leave with |
| - | --- | --- |
| 0 | [Hello Claude](session-0-hello-claude.md) | The project folder on your laptop, Claude Code running |
| 1 | [CLAUDE.md, Then Your AP Persona](session-1-claude-md.md) | A short `CLAUDE.md` (your own context), then `personas/ap-persona.md`, a file describing the AP expert your chatbot will become |
| 2 | [Skills](session-2-skills.md) | Four small skills live: data-checklist, benchmark-interpreter, template-filler, file-reader |
| 3 | [Agents](session-3-research-agents.md) | The Reviewer agent in `.claude/agents/`, plus a sharpened set of diagnostic questions, Day 1 closes here |
| 4 | [The Guided Build](session-4-builds.md) | A deployed AP diagnostic chatbot: code on your own GitHub repo, live on Vercel |
| - | [Hackathon](capstone.md) | Your own persona (AR, R2R, FP&A, or a real client), built with `/build-persona` without touching `CLAUDE.md`, also on GitHub and deployed |
| - | [Closing](closing.md) | Your plan for after the bootcamp |

**Stuck?** Open the [Appendix](appendix.md). Each session also has its own troubleshooting at the bottom.

---

## Your Project Folder

Open your terminal and run these three lines. Session 0 walks you through it.

```
git clone https://github.com/thecrux-ai/thecrux-ey-bootcamp.git
cd thecrux-ey-bootcamp
claude
```

On Day 2 you push the code you build to your own GitHub repo and deploy it on Vercel. Claude Code does that for you when you ask.

---

## A note on the sample data

The AP data collection template, benchmark numbers, and reference report in `sample-data/` are sample data, in the shape a real AP diagnostic uses. Replace them with your own templates when you have them. No session changes when you do. See `sample-data/README.md`.
