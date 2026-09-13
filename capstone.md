[← Back to Student Handbook](student-handbook.md)

---

# Day 2 Hackathon: Make It Yours

**After lunch, right after the guided build.**

**What you'll build:** the same pattern as this morning, adapted to your own process. Same chat interface, same file upload, same answer tracking, same report generator. Different persona, different data.

---

## What this session is

This morning you built one thing end to end: an Accounts Payable diagnostic chatbot. It takes a chat conversation, an uploaded file, a checklist of what data is still missing, and a benchmarked report at the end. You deployed it live to Vercel.

That AP build was never the point. It was the demonstration. The real point is the pattern underneath it: a chat interface that asks structured questions, a tool that records each answer so you always know what is still missing, a benchmark table to compare against, and a report generator that writes it all up in a consistent format.

That pattern works for any finance process, not just AP. This session, you point it at your own.

You built the pattern. Now make it yours.

---

## What you're carrying forward

Do not rebuild any of this. Reuse it.

- The chat interface you deployed this morning.
- The file upload flow.
- The checklist logic itself: what has been answered, what is still missing, and what to ask next.
- The answer extraction step and the progress bar. These read the field ids straight out of whichever checklist CSV is loaded, so they work for your new process without any changes. The landing page already asks which process an interview is for, so pick your track there when you start one and the rows get labelled correctly.
- Your Supabase project, the three tables, and the resumable session links. Same database, same schema. A new process does not need a new project.
- The report generator and its output format.
- The four skills and the Reviewer agent from Day 1. Their rules are about how to run a diagnostic, not about AP. They carry over as they are.
- Your GitHub push flow and your Vercel deployment setup.
- `CLAUDE.md`, your context and house rules. It does not change for this session, it was already built in Session 1.

What changes is the persona this app serves, the questions it asks, the benchmark it compares against, and the data you feed it. Everything else is scaffolding you already have.

---

## First step: build your new persona with /build-persona

Whichever track you pick below, the first thing you do is run `/build-persona` to create a new persona file: `personas/ar-persona.md`, `personas/r2r-persona.md`, `personas/fpa-persona.md`, or a custom slug if you're on Track 4. It asks which process you're building for if you don't say up front (or give it as an argument: `/build-persona AR`, `/build-persona R2R`, `/build-persona FPA`).

For AR, R2R, and FP&A, a data collection template already exists (`sample-data/ar-data-collection-template.csv`, `sample-data/r2r-data-collection-template.csv`, `sample-data/fpa-data-collection-template.csv`), and `/build-persona` grounds the checklist section in it automatically. There is **no** benchmark file and **no** reference report for those three tracks, only AP has them. So `/build-persona` sketches a leading / average / lagging benchmark table live with you and saves it to `sample-data/` as `[slug]-benchmark-data.csv`, and reuses the AP report's six-section shape for the report. Where nothing exists at all (a real client use case, Track 4), it says so plainly and builds both the checklist and the benchmark table fresh with you, then saves them to `sample-data/` so the persona has something real to point at.

**This does not touch `CLAUDE.md`.** That file is done, it stayed the same through this morning's AP build and it stays the same now. Only a new file under `personas/` gets created.

**It does not need any new agent work either.** `/build-persona` covers the persona. The new persona's build reuses the four skills directly, the same way the AP build did: point the system prompt and the report call at the new persona, checklist, and benchmark files instead of the AP ones. The Reviewer agent can be reused as-is too, no edits needed, its job is to critique a report or a question set, and that job doesn't change per persona.

`/build-persona` writes your new persona to `personas/` at the project root, the same place your AP persona lives. Your app does not read from there. It reads from `ey-fin-os/context/`, which is why it can be deployed on its own.

So there is one extra step this afternoon, and it takes a second: run `npm run sync-context` to copy your new files into the app. Do it right after `/build-persona` finishes, and again any time you change a persona, a skill, or a sample-data file. Every wiring prompt below starts with it.

After that, the rest of the build (chat app, file upload, checklist tracking, report generation, GitHub push, Vercel deploy) works exactly as it did this morning.

One thing worth knowing: your new checklist CSV has its own field ids, `ar_01`, `r2r_01`, and so on, not `ap_01`. The extraction step reads the ids from whichever CSV is loaded, so it picks these up on its own. You do not need to change it, the database, or the progress bar. If it starts recording `ap_01` for an AR interview, it is reading the wrong CSV, and that is a sync problem.

---

## The track menu

Four tracks. Pick one. Tracks 1 to 3 start from a data collection template already sitting in your repo, in `sample-data/`. If you're mid-engagement with a real client, Track 4 is for you and it uses your own (anonymised) data instead.

---

### Track 1: Accounts Receivable (AR)

**What changes:**
- The persona becomes an AR / Collections lead, not an AP manager, built fresh in its own file, `personas/ar-persona.md`.
- The questions come from `sample-data/ar-data-collection-template.csv`: monthly invoice volume, Days Sales Outstanding (DSO), percentage over 90 days past due, collection escalation steps, dispute resolution rate.
- There is no ready-made AR benchmark table or reference report in the repo. That's deliberate. `/build-persona` builds one with you as part of the interview. It's a fast, valid exercise, not a shortcut you're skipping.

**What stays the same:**
- The chat flow, the upload step, the checklist logic, the report generator, the GitHub push, and the deployment. `CLAUDE.md` too, untouched.

**Starter prompts:**

```
/build-persona AR
```

Walk through the interview it runs. It will find `sample-data/ar-data-collection-template.csv` already there and ground the checklist in it, then build an AR benchmark table and a reference report shape with you since neither exists yet, and save everything to `personas/ar-persona.md` (plus the new sample-data files if you build them).

Once it's saved:

```
First run npm run sync-context so my new AR files are copied into
ey-fin-os/context/. Then wire context/personas/ar-persona.md into this
app the same way context/personas/ap-persona.md was wired this morning:
assemble the system prompt from it, update the chat questions to match
its checklist, and update the report generator to use its benchmark
table and report shape. Test it end to end: upload a sample AR extract
in chat, answer a few checklist questions, and generate the report.
Show me the output.
```

---

### Track 2: Record to Report (R2R)

**What changes:**
- The persona becomes a Controller or R2R lead running month-end close, not an AP manager, built fresh in its own file, `personas/r2r-persona.md`.
- The questions come from `sample-data/r2r-data-collection-template.csv`: close cycle time in business days, manual journal entries per close, reconciliation automation, percentage of balance sheet accounts reconciled within 5 days, audit adjustments raised.
- No ready-made R2R benchmark or report template exists yet. Same exercise as AR: `/build-persona` sketches one with you.

**What stays the same:**
- Chat flow, upload, answer tracking, report generator, GitHub push, deployment. `CLAUDE.md` too, untouched.

**Starter prompts:**

```
/build-persona R2R
```

Walk through the interview. It finds `sample-data/r2r-data-collection-template.csv`, builds an R2R benchmark table and reference report shape with you since neither exists yet, and saves everything to `personas/r2r-persona.md`.

Once it's saved:

```
First run npm run sync-context so my new R2R files are copied into
ey-fin-os/context/. Then wire context/personas/r2r-persona.md into this
app the same way context/personas/ap-persona.md was wired this morning:
system prompt, chat questions, and report generator all read from this
file instead. Run it end to end with a made-up close-cycle conversation
and show me the finished report.
```

---

### Track 3: FP&A

**What changes:**
- The persona becomes an FP&A lead running budgeting and forecasting, not AP, built fresh in its own file, `personas/fpa-persona.md`.
- The questions come from `sample-data/fpa-data-collection-template.csv`: forecast refresh cadence, forecast accuracy over the last 4 quarters, budget cycle length in business days, planning tool in use, percentage of time spent gathering data versus analysing it.
- No ready-made FP&A benchmark or report template yet. `/build-persona` sketches one with you, same as the other two tracks.

**What stays the same:**
- Chat flow, upload, answer tracking, report generator, GitHub push, deployment. `CLAUDE.md` too, untouched.

**Starter prompts:**

```
/build-persona FPA
```

Walk through the interview. It finds `sample-data/fpa-data-collection-template.csv`, builds an FP&A benchmark table and reference report shape with you since neither exists yet, and saves everything to `personas/fpa-persona.md`.

Once it's saved:

```
First run npm run sync-context so my new FP&A files are copied into
ey-fin-os/context/. Then wire context/personas/fpa-persona.md into this
app the same way context/personas/ap-persona.md was wired this morning.
Test the full flow: upload, chat, checklist, report. Show me the output
and point out anything that still reads like an AP report by mistake.
```

---

### Track 4: Your own real client engagement

**What changes:**
- Nothing invented. If you're mid-engagement with a real client, point the exact same pattern at that real use case instead of a generic persona, built fresh in its own file, `personas/[your-client-slug]-persona.md`. Use the client's real questions, real benchmark thinking, real report structure, real data.
- Everything about the pattern is identical to what you built this morning. Only the content is real.

**What stays the same:**
- Chat flow, upload, answer tracking, report generator, GitHub push, deployment. All of it, unchanged. `CLAUDE.md` too, untouched.

**Data safety, read this before you touch real client data:**

This is a shared laptop environment. Be careful with real client PII on a laptop other people may use, don't paste anything under NDA into a tool you haven't checked, and if in doubt, use anonymised or redacted figures for the live demo even if your working file has the real numbers. If you're not sure your client data is safe to use here, use one of the other three tracks instead and apply what you build to the real engagement later, on your own machine.

**Starter prompts:**

```
/build-persona
```

When it asks which process, describe your client's real engagement instead of picking AP/AR/R2R/FP&A: "[client name / anonymised name], here's what the diagnostic should cover: [describe the process, the questions, and what a finished diagnostic report needs to say for this client]." There is no sample data for this one, so it will build the checklist and benchmark table fresh with you and save them, and the persona, under a slug you choose.

```
Here's a real data file for this client: [point to file]. Read it and
tell me what's in it before we do anything else, just so I know you
understood it correctly.
```

Once the persona file is saved:

```
First run npm run sync-context so my new client files are copied into
ey-fin-os/context/. Then wire
context/personas/[your-client-slug]-persona.md into this app the same
way context/personas/ap-persona.md was wired this morning. Keep the chat
flow and checklist logic as they are, just point them at this new
persona and its benchmark and report shape. Test it end to end and show
me the output before we touch anything else.
```

---

## Push and deploy, same as this morning

Your app reads everything from its own `context/` folder, and `ey-fin-os/` is its own standalone GitHub repo. That hasn't changed. What has changed is that your new persona and any new sample-data files were written to the **project root**, so they are not in `context/` yet. The app cannot see them until you sync.

This is the one step people forget in the hackathon. Do it first:

```
Run npm run sync-context in ey-fin-os so my new persona and any new
sample-data files get copied into ey-fin-os/context/. Then show me
what's now in context/personas/ and context/sample-data/ so I can
confirm the new files are there.
```

Then ship to the same repo you deployed this morning:

```
Commit everything that changed in ey-fin-os, including context/, and
push it to the same GitHub repo we deployed this morning. Then redeploy
to Vercel as a production deployment, and confirm OPENROUTER_API_KEY,
AI_MODEL, SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY are all still set
there for both production and preview. Give me the live URL.
```

If you'd rather ship the hackathon build as a separate, standalone repo:

```
Create a new PRIVATE GitHub repository under my account called
ey-ar-diagnostic and push ey-fin-os to it as the repo root, so
package.json sits at the top level. Before you push, make sure .env.local is in .gitignore and is
not being committed, and that context/ IS being committed. Show me the
repo URL when done. Then deploy it to Vercel as a production deployment,
leaving Root Directory as the default, and add OPENROUTER_API_KEY,
AI_MODEL, SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY from .env.local as
environment variables for both production and preview. Give me the live
URL.
```

If Claude asks you to finish a sign-in to GitHub or Vercel in the browser, do that with your own accounts and tell it to carry on. Deploy early, then redeploy as you improve things. Don't leave the first deploy until the end.

---

## What "done" looks like by presentation time

Check every line before you present.

> - [ ] You picked one track: AR, R2R, FP&A, or your real client engagement.
> - [ ] You ran `/build-persona` and saved a new persona file under `personas/` (`ar-persona.md`, `r2r-persona.md`, `fpa-persona.md`, or your client's slug). `CLAUDE.md` is untouched.
> - [ ] The chat asks the right questions for your track, not the AP ones.
> - [ ] You have a benchmark table for your track (built by `/build-persona` with you, brought from a real client, or reused AP's if your track is AP-adjacent).
> - [ ] You have a report template shape for your track.
> - [ ] The progress bar moves as you answer, using your new field ids, not the AP ones.
> - [ ] You closed the tab, reopened the session link, and the interview carried on where it stopped.
> - [ ] You ran the whole flow at least once, start to finish: upload, chat, checklist, generated report.
> - [ ] Your updated code is pushed, either to this morning's repo or to a fresh one with the remote re-pointed the same way, not the original starter repo.
> - [ ] You ran `npm run sync-context` after `/build-persona`, so your new persona and any new sample-data files are inside `ey-fin-os/context/` and committed.
> - [ ] The app is deployed and live on your Vercel URL, not just running on your laptop.
> - [ ] You can open that URL and show it to someone else in the room right now, and you have your GitHub repo link ready too.

If any box is unchecked as time runs low, stop adding features and fix that box first. A working app that only does the basics beats a broken app that was going to do more.

---

## What not to build (yet)

Good ideas to leave for later. If you finish everything above with time to spare, reach for one, but do not plan around it.

- **Multiple personas' agents talking to each other and combining into one report.** A second build on top of a first build that needs to work standalone first.
- **A live status dashboard with an automated daily email summary.** Real automation work that needs its own scoping.
- **Sentiment or cultural analysis on chat transcripts.** A different kind of analysis, not an extension of the diagnostic pattern.
- **Pulling in public internet data automatically.** A whole new integration and a new set of things that can break, for a 2-hour build.
- **Integration with your real design-template library.** Branding polish is fine; wiring into an internal template system is an IT conversation.

Note them for later and get back to the thing in front of you.

---

## How you'll present

The real deliverable is that everyone has their own live Vercel URL ready by the end of this session. With this many people, not everyone presenting live in turn makes sense, so a handful of volunteers walk the room through theirs in four beats:

1. **The persona.** "This is for [an AR collections lead / an R2R controller / an FP&A lead / our client, X]."
2. **What changed from the AP build.** One line: the questions, the benchmark, the report shape.
3. **Show it live.** Open the actual URL (and have your GitHub repo link ready). Upload the sample file. Run a bit of the chat. Show the generated report. Don't describe it, show it.
4. **One thing you'd do next** if you had another hour.

Everyone else keeps their app open on their own screen, so during the close people can walk around and open each other's URLs directly. No slide deck, ever. The deployed app is the deck.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| Chat still asks AP questions after your new persona file is saved | Two possible causes, check in this order. First, you may not have synced: run `npm run sync-context` and look for your new persona in `ey-fin-os/context/personas/`. If it's there, the app is still wired to the AP file, so tell Claude: `The chat is still asking AP questions. Wire the system prompt and chat questions to read my new persona from context/personas/ instead, and confirm it's actually being read.` |
| The progress bar says a field is missing when you already answered it | The extraction step returned an id that isn't in the loaded CSV, so it was thrown away. Check `npm run sync-context` has run and that the app is reading your track's CSV, not the AP one. |
| Report comes out with AP framing or AP metric names baked in | The report generator template still has the old persona's wording hardcoded. Point Claude at your new persona file's reference report section and ask it to rebuild the generator from that shape, not patch the old one. |
| No benchmark file exists for your track and you're not sure what "leading practice" numbers to use | That's fine, this is expected for AR, R2R and FP&A before `/build-persona` runs. Say so out loud in the demo: "this benchmark table is our own quick estimate, not an EY-validated one." Directionally right is enough for today. |
| `/build-persona` says a persona file already exists | That's fine if you're resuming. Say `start fresh` if you want to rebuild it from scratch instead, and confirm the overwrite. |
| App works locally but the live Vercel version can't find your new persona or its benchmark/reference files | Your new files are at the project root but were never copied into the app. Tell Claude: `Run npm run sync-context, confirm the new persona and sample-data files are now in ey-fin-os/context/, then commit context/ and redeploy.` This is the most common hackathon failure, check it first. |
| The chat still asks the old questions on the live URL but the right ones locally | Same cause. `context/` on your laptop is fresh, but the committed copy is stale. Sync, commit, redeploy. |
| Vercel deployment breaks after you change the code | Deploy early and often, don't wait until the end to deploy for the first time. If it breaks, ask Claude to read the deployment error message and fix that specific error, don't start guessing. |
| Running out of time with half the checklist done | Cut scope, not quality. Ship a working app with 5 checklist questions instead of a broken app that was trying to handle 15. |
| Real client data feels risky to demo live | Use anonymised figures for the live demo, or fall back to one of the sample tracks and note that the real version exists on your own machine. |
| Can't decide which track to pick | Pick whichever process you'd actually use after today, that's the one worth finishing. |

For anything not on this list, ask the facilitator.

---

**Next:** [Closing: Feedback, Testimonials + What's Next →](closing.md)
