[← Back to Student Handbook](student-handbook.md)

---

# Session 5: The Guided Build - AP Diagnostic Chatbot

**Day 2 - the flagship build, first thing in the morning.** We do this one together, step by step, the whole room at the same pace.

By the end of this session you will have a real, working, deployed web app: a chat where an AI agent runs an Accounts Payable diagnostic interview, and a second step that turns that conversation into a proper diagnostic report, following your own report shape.

It will have a live URL. Not a local demo, not a screenshot. A URL you can send to someone.

---

## You already built every piece. Today we wire them together.

Look at what's behind you from yesterday:

- An **AP persona** file (`personas/ap-persona.md`): a rich context file that captures how an experienced AP function actually works, its pain points, its vocabulary. That becomes this agent's brain.
- A **data-checklist skill**: the logic for what questions a proper AP diagnostic needs answered, and when you've asked enough. That becomes this agent's interview plan.
- A **benchmark-interpreter skill**: the logic for turning a raw number like "18% exception rate" into a judgement like "below average." That becomes this agent's analysis.
- A **template-filler skill**: the logic for taking messy inputs and pouring them into a fixed report shape. That becomes this agent's report generator.
- A **file-reader skill**: reading an email or a CSV and pulling out the checklist answers hiding in it. That becomes this agent's upload handling.
- A **Reviewer agent** (`.claude/agents/reviewer.md`), built yesterday to close Day 1. It read a draft report or a question set the way a sceptical CFO would, and sharpened your twenty diagnostic questions into `ap-data-collection-template-v2.csv`. Today it comes back once, at the end, for an optional last look at the generated report.

Every part you built yesterday has a job in today's build. We are not starting from zero, we are assembling.

Nothing gets retyped. Your Day 1 files stay exactly where they are, and one command copies them into the app so the app can carry them to the internet later. You'll run that command in Part A and it takes a second. The reason is simple: today you deploy `ey-fin-os/` on its own, and a deployed app can only read files that travelled with it.

---

## What "done" looks like by the end of today

- A working web app: running locally on `localhost:3000` first, then pushed to your own GitHub repo, then live on a Vercel URL.
- A chat where the agent plays an EY AP diagnostic consultant, and you play the client (the AP Manager being interviewed) to try it out. It opens the conversation itself, so the client never lands on an empty box.
- You can type answers, or upload a file (an email, a CSV extract) and the agent folds that straight into what it knows, instead of asking questions the file already answered.
- The agent keeps interviewing until it has enough, then offers to generate the report.
- A finished diagnostic report in the app: Executive Summary, As-is Observations (a tagged finding grid), Maturity Assessment, Benchmarking, Opportunities by Lever, Suggested Next Steps.
- A real Vercel URL, live, that anyone in the room could open on their phone.
- **Optional, if there is time:** every interview saved to a database and resumable from its own link.

---

## How today is structured

This build is scoped to fit in one guided sitting. Pace yourselves against the checkpoints below, not against the clock.

- **Part A** - Scaffold the app, wire in the persona as the system prompt. Build and test on `localhost:3000`.
- **Part B** - Add file upload, then the report generation step. Still local.
- **Part C** - Swap the Claude Code login for an API key and re-test locally, then push to your own GitHub repo, deploy to Vercel, and test the live URL.
- **Part D (optional)** - Add a database so every interview is saved and resumable from its own link. Only start this once Part C is live and working.

The order matters: everything runs on your laptop first, `localhost:3000`, all the way through a working chat and a working report, using your Claude Code login rather than any API key. Only once that works do you touch GitHub or Vercel, in Part C. You never push to `thecrux-ey-bootcamp` itself, that stays the shared starter repo. Part C creates a brand new repo under your own GitHub account and pushes there instead.

Parts A to C are the build. Part D is a second lap over an app that already works, and it is genuinely optional. If you stop at the end of Part C you have a complete, deployed, demonstrable product.

If you're behind, say so. We wait. A working Part A beats a half-built Part B.

---

## A note on the data

Everything in `sample-data/` is sample data, in the shape a real AP diagnostic uses. Replace it with your own templates when you have them. Nothing about today's build changes when you do, you drop the new files in with the same names. See `sample-data/README.md`.

---

## Setup: your build folder

You should be in Claude Code, started from `thecrux-ey-bootcamp` the same way as yesterday. If not, open your terminal and run the same two lines as before:

```
cd thecrux-ey-bootcamp
claude
```

Today's app gets its own folder inside the project. Ask Claude to make it:

```
Create an empty folder called ey-fin-os in this project. Everything we
build this morning goes in there. For the rest of today, when I say "this
project" or "the app", I mean that folder.
```

Everything else today, installing, running the app, pushing to GitHub, deploying, is done by asking Claude Code. It runs the commands and asks you to approve them. Read what it proposes before you say yes.

---

## Before Part A: no API key today, until you deploy

You might expect to need an API key before you can build an app that calls Claude. Not today, and not for most of the day.

While you build and test on your laptop, the app calls Claude through the Claude Code CLI you already have installed and logged in. Your own Claude Code session does the work. Nothing to sign up for, nothing to paste, no spend.

An API key only becomes necessary in Part C, when the app leaves your laptop. A deployed app on Vercel has no Claude Code login to borrow, so it authenticates the way any production app does, with a key. You'll get one from the facilitator at that point, and Part C walks you through switching over and testing before you deploy.

So you build the whole thing first, prove it works, and deal with credentials once at the end. Nothing to set up before Part A.

---

# PART A - Scaffold the app, wire in the persona

**Outcome:** a Next.js app running on your laptop, with a working chat, where Claude is already answering as an AP diagnostic consultant, grounded in your Day 1 persona file.

## Step A1: Confirm your Day 1 work is where the app will read it

Your Day 1 files live at the project root: `personas/`, `.claude/skills/`, `.claude/agents/`, and `sample-data/`. `ey-fin-os/` sits alongside them. In Part A you'll copy what the app needs into `ey-fin-os/context/`, and from then on the app reads its own copies.

Two things stay at the project root no matter what, so don't move them: `.claude/skills/` and `.claude/agents/`. That is where Claude Code itself looks for them. The app gets copies, the originals stay put.

Have Claude Code confirm everything is where it should be before you build anything on top of it:

```
Confirm these files exist and show me their paths, with a one-line
description of each. Don't copy anything, just confirm and show me.

From Day 1, at the project root:
- personas/ap-persona.md
- four skills under .claude/skills/, each with a SKILL.md:
  data-checklist, benchmark-interpreter, template-filler, file-reader
- .claude/agents/reviewer.md

In sample-data/:
- ap-data-collection-template.csv
- ap-benchmark-data.csv
- ap-reference-diagnostic-report.md

Also check whether sample-data/ap-data-collection-template-v2.csv exists.
If it does, that's the file the app should read for its checklist. If it
doesn't, the app reads ap-data-collection-template.csv unchanged. Tell me
which one you found.
```

Check what it found. If it's missing something, tell it where to look, or paste the content yourself, plain English is fine: `I can't find the persona file, here's what was in it: [paste]`

The `-v2` file is the one the Reviewer agent produced yesterday in Session 4, after critiquing your twenty diagnostic questions. If it exists, those sharpened questions are what the agent will actually ask from. If it doesn't, nothing breaks and the original is used.

## Step A2: Scaffold the Next.js app

Three short prompts rather than one long one. Run them in order and check the result of each before moving on, so if something goes wrong you know exactly which step it was.

### The env file, first

```
Touch a .env.local file inside ey-fin-os/ containing exactly this one
line:

AI_PROVIDER=claude-cli
```

Doing this before the scaffolding tool touches the folder means the app can never start without it, and if something goes wrong at this step, you find out before you've scaffolded a whole app on top of it.

That is the only setting the app needs today. You'll add the API key line in Part C, just before deploying.

### The app itself

```
Scaffold a Next.js app in this project (App Router, TypeScript), inside
ey-fin-os/. Do not initialize git. If the scaffolding tool offers to,
decline. Keep the .env.local file that is already there, do not
overwrite or remove it.

Install the Vercel AI SDK (ai), its OpenAI-compatible provider package
(@ai-sdk/openai), and @ai-sdk/react.

Make sure .env.local is listed in .gitignore.

Then start the app and tell me the URL.
```

Open the URL it gives you. You should get the default Next.js starter page. That is all you need at this point.

Two things in there look fussy and are not. **No git yet:** in Part C this folder becomes its own standalone repository, so letting the scaffolding tool set git up now just means undoing it later. **Three packages, not two:** `@ai-sdk/react` is the one people miss, and without it the chat page will not build at all.

### The context folder

Today only `ey-fin-os/` gets deployed, and a deployed app can only read files that travelled with it. So rather than reaching up a directory for your persona and skills, the app carries its own copies.

```
Add a script to ey-fin-os called sync-context, runnable as
npm run sync-context. It copies four folders from the project root into
ey-fin-os/context/, replacing whatever is there: personas, .claude/skills
(as context/skills), .claude/agents (as context/agents), and sample-data.
If a source folder isn't there it says so and carries on rather than
crashing. Do not wire it into predev or prebuild, it must only ever run
on my laptop.

In next.config.ts, set outputFileTracingIncludes to cover
./context/**/* so the folder travels with the app when it deploys.

Then run it once and show me what landed in context/.
```

You should see four folders inside `ey-fin-os/context/`: your persona, your four skills, the reviewer agent, and the sample data. If something is missing it was not where the script looked, so fix that now rather than later. You will run this script again any time you change a persona or a skill, and once more before you deploy.

## Step A3: Get a chat working, then make it a consultant

Two prompts. The first gets any chat working at all. The second turns it into the diagnostic consultant. Splitting them matters: if the chat is broken you want to know that before you start judging its tone.

### First, a working chat

```
Build a chat page and a chat API route.

For now the app talks to Claude through the Claude Code CLI, so it needs
no API key. Put that behind a small provider module with one function
that takes a system prompt and the conversation and streams back text,
so a different provider can be swapped in later without touching the
chat route.

The CLI provider runs: claude -p --system-prompt-file <file>
--allowed-tools "" --output-format stream-json --verbose
--include-partial-messages, with the conversation on stdin. Read the
streamed text from the stream_event lines, taking
event.delta.text from each content_block_delta, and ignore the final
assistant event or every reply arrives twice.

Run the child process with cwd set to a temporary directory, and strip
ANTHROPIC_API_KEY, ANTHROPIC_AUTH_TOKEN and OPENROUTER_API_KEY from the
environment you pass it.

Check the installed AI SDK version in node_modules before you write any
code, and use these names: useChat is imported from @ai-sdk/react and
never from ai, the token limit option on streamText is maxOutputTokens
and not maxTokens, the API route returns
result.toUIMessageStreamResponse(), and the route converts incoming
messages with convertToModelMessages before passing them to streamText.

Give it a placeholder system prompt for now, one line, anything. Then
show me the chat answering a simple test message.
```

Send it "hello" and make sure you get a reply. A plain working chat is the checkpoint here, nothing more.

Three lines in that prompt look arbitrary and are not. Each one is a real failure someone already hit.

**Ignore the final assistant event.** The CLI streams the text token by token, then repeats the whole message at the end. Read both and every reply appears twice.

**Run it in a temporary directory.** Claude Code reads `CLAUDE.md` from wherever it starts. Run it inside this project and it quietly inherits your house rules, and the agent starts behaving like a coding assistant instead of a consultant.

**Strip the API key variables.** If `ANTHROPIC_API_KEY` is set, even to the placeholder text sitting in a `.env.local`, Claude Code prefers it over your logged-in session and the call fails with a confusing message about connectors.

That block about API names looks fussy and is not. The AI SDK changed these names in a recent version and Claude has seen a lot of older code, so left to itself it often writes the old ones. The app then fails to build, or every message errors. Telling it up front costs you nothing, and if it happens anyway the Troubleshooting table at the bottom has the exact fix.

### Then, the persona

This is the step that makes the agent a consultant instead of a generic chatbot.

```
Now replace that placeholder system prompt. Assemble it at request time
from three files inside this app's own context folder: the persona file
at context/personas/ap-persona.md (the domain expertise), the
data-checklist skill at context/skills/data-checklist/SKILL.md (what
still needs answering), and
context/sample-data/ap-data-collection-template.csv (or the -v2 version
if it exists, check live, don't assume) for the actual list of questions.

Read these from context/ only. The app must never use ".." to reach
outside its own folder.

The agent's role: an EY Finance Transformation consultant with deep
Accounts Payable domain expertise (from the persona file), running a live
diagnostic interview. The person on the other side of the chat is the
client, specifically the AP Manager being interviewed. Write this
directly into the prompt-assembly instructions: it asks like a consultant
who has done this thirty times, one focused question at a time, following
up on interesting answers before moving on. It never mentions benchmarks,
leading practice, or whether a number is good or bad during the interview,
that happens later, in the report. It never assumes an answer the client
did not give. It knows when it has enough, roughly ten to twelve solid
answers across the checklist categories, not every field, and says so
plainly when it gets there, telling the client to click "Generate
Report" at the top of the screen. That is an instruction, not a
question: it never asks "shall I generate the report?" or offers to do
it itself, since it has no way to trigger generation from inside the
chat, only that button does, and only that path makes the PDF download
available afterward.

Three standing rules it must never break. It is software, not a person:
it never presents itself as a consultant or a member of the EY team, and
if asked it says plainly that it is an AI helping the team gather
information. It never claims to have run engagements, worked with other
clients, or seen this before. And it never infers the client's industry,
sector, size or operating model from their name or anything else, which
includes not slipping "across how many sites" into a question unless the
client has already said they have several.

The opening message is not generated. Build it on the server when the
chat page loads, and show it as the agent's first message with no model
call at all. Under eighty words, in this order: one line saying it is an
AI assistant helping the EY team run this diagnostic; that it will ask
around ten to twelve short questions covering volume, process, technology
and metrics; that they can type answers or upload a file; then the first
question read straight out of the checklist CSV, quoted word for word,
with nothing added after it.

Then show me the chat with that opening message already on screen.
```

Open the chat. The greeting and the first question should be sitting there the instant the page loads.

Three of those instructions look like belt and braces and are not, because all three failures showed up in testing.

**The opening message is built, not generated.** You already know exactly what it should say, so there is no reason to spend a model call on it. Writing it on the server means the room sees the interview open instantly instead of watching a spinner, and it removes a whole class of page-load bugs before they can happen.

**Take the first question from the CSV rather than letting the model write one.** Asked to compose it, the model reliably bolts a second clause on with "and", so the client gets two questions at once and answers one. Quoting the checklist fixes it outright, and it is the checklist's job anyway.

**The no-inference rule needs saying out loud.** Given a client called "Northbridge Hospitals" the model asked how many invoices they process "across how many locations or entities". Nobody had said they have more than one. Given a manufacturing-sounding name it claimed to have run this diagnostic with many manufacturing clients before, which is simply untrue and the kind of thing a client repeats back to you.

Yesterday you tested the Reviewer by handing it a report and a question set. There was no separate interviewer to test, because there is no separate interviewer agent, the persona and the data-checklist skill carry those rules directly. Today those same rules run live, one turn at a time, with a person typing the client's answers.

Test it. Answer that first question the way an AP Manager would and see if the follow-up sounds like a consultant, not a form.

If it feels stiff or generic:

```
That felt like a form, not a consultant. Read
context/personas/ap-persona.md and
context/skills/data-checklist/SKILL.md again and make sure the agent's
questions and phrasing actually reflect that expertise and those rules.
Try again.
```

---

> **Checkpoint A - the app runs and the agent sounds like a consultant**
>
> - [ ] The app runs locally and Claude gave you an address to open
> - [ ] The chat page loads with the agent's greeting and first question already there, before you type anything
> - [ ] You can answer, and the follow-up sounds like a real consultant, not a generic assistant
> - [ ] `npm run sync-context` has been run, and `ey-fin-os/context/` now holds the persona, four skills, the reviewer agent, and the sample data
> - [ ] Nothing in `ey-fin-os/src/` uses `..` to read files outside the app folder
>
> If you're not here yet, say so. We wait for the room before Part B.

---

# PART B - Upload a file, then generate the report

**Outcome:** the client can hand the agent a file instead of typing everything out, and one click turns the whole conversation into a finished diagnostic report.

## Step B1: Polish the chat so it reads like a real interview

```
Polish the chat UI. Clean, professional, EY-appropriate: not playful,
not childish. A message list with the agent's questions and the client's
answers clearly distinguished. A text box and send button. A short header
that says "AP Diagnostic Interview" so whoever's using it knows what's
happening.

Show a subtle "thinking" indicator while the agent is composing its next
question.
```

## Step B2: Add file upload

Keep this simple. No heavy parsing, no OCR, no CSV-to-table logic. One file type family, one rule.

```
Add file upload to the chat: a button or drag-and-drop area next to the
text box, accepting text files (.txt, .csv).

When a file is uploaded, read its raw text content and add it to the
message sent to Claude, clearly labeled, for example:
"[Uploaded file: sample-invoice-aging-extract.csv]" followed by the
file's contents. Don't parse the CSV into columns or compute anything
from it yourself, let Claude read the raw text and use what's useful, the
same way a person would read a pasted spreadsheet.

After a file is uploaded, the agent should fold whatever it learns into
the interview naturally: acknowledge what it now knows, skip questions
the file already answered, and keep going with what's still missing.

Show me it working with a test upload.
```

Images are left out on purpose. Handling a screenshot means a second code path and a different way of sending content to Claude, and nothing in today's flow needs it. Once the app works, adding it is one prompt.

## Step B3: Test the upload with the sample client files

Now try it yourself. Open the chat, answer the first couple of questions, then upload one of the practice files sitting in `sample-data/sample-client-inputs/`:

- `sample-email-from-ap-manager.txt` - a fictional email with a handful of real-looking numbers.
- `sample-invoice-aging-extract.csv` - a fictional invoice-level extract.

Watch what happens. The agent should reference specifics from the file ("you mentioned 40% of invoices are non-PO, mostly from facilities and marketing") without you having to re-explain them out loud.

If it ignores the file:

```
The agent didn't use anything from the uploaded file in its next question.
Make sure the file's content is actually being included in the message
sent to Claude, and that the agent is told to read and use it before
asking its next question.
```

## Step B4: Build the report generation step

This is the benchmark-interpreter and template-filler skills from yesterday doing their real job.

The button works from the start. Answer four or five questions before you click it and you'll get something worth reading. Click it after one and you'll get a report that is mostly gaps, which is a useful thing to see once and not worth showing anyone.

```
Add a "Generate Report" button to the chat, always clickable.

Clicking it makes a second, separate call to Claude. This call's job is
different from the interview: it is not asking questions, it is writing a
finished document.

The instructions for that call are assembled from files in this app's own
context folder: context/personas/ap-persona.md (the domain expertise),
context/skills/benchmark-interpreter/SKILL.md (benchmark every metric,
never call a number good or bad without naming the band), and
context/skills/template-filler/SKILL.md (the six-section shape and the
finding grid, never invent a number, flag gaps in curly braces).

In that call, also give Claude:
- The full conversation so far. Every number in the report comes from
  here, in the client's own words.
- The raw content of any uploaded files.
- context/sample-data/ap-benchmark-data.csv, so it can compare what it
  heard against the global and India peer points for each metric.
- context/sample-data/ap-reference-diagnostic-report.md as the exact
  shape to follow: same headings, same tone, same way the benchmark table
  and the narrative sit together.

The report must have these sections, in this order: Executive Summary,
As-is Observations, Maturity Assessment, Benchmarking, Opportunities by
Lever, Suggested Next Steps.

As-is Observations is a table, and it is the core of the report. One row
per finding: ID, Sub-process, As-is observation, Improvement opportunity,
Implementation considerations. IDs run AP01, AP02 and so on in sequence.
Every row is tagged with Impact (High/Medium/Low), Ease (High/Medium/Low),
Dimension (Process/People/Technology) and Timeframe (0-3, 3-6, 6-12 or
12+ months). Never an observation without a paired opportunity.

Benchmarking gives three points per metric, global, India and client,
with the unit stated.

Use only numbers the client actually gave you in the conversation or an
uploaded file. Where a number was never established, say so plainly
rather than inventing one.

Return the report as markdown.

Set maxOutputTokens generously, at least 8192. Extended thinking runs by
default on current Claude models and shares the same output-token budget
as the report text itself, and how much of that budget thinking uses
varies from one run to the next. A tight limit means some runs finish
fine and others cut the report off mid-section for no obvious reason.

Also export maxDuration (at least 90) from this route file, so a long
generation has time to actually finish once this is deployed.
```

## Step B5: Show the report

```
When the report is generated, replace the chat with the report in the
same view and render the markdown cleanly: proper headings, a real table
for the As-is Observations grid and the Benchmarking table (not a wall of
text), readable line length. Keep it professional, this should look like
something you'd actually send to a client, or close to it.

Add a "Back to interview" link that brings the chat back exactly as it
was, so someone can keep answering questions and generate the report
again.
```

## Step B6: Test it against the reference shape

Open `sample-data/ap-reference-diagnostic-report.md` side by side with your generated report. Same six sections? Is every finding tagged, and does every observation have a paired opportunity? Does the Executive Summary explain the causal chain in the numbers, not just restate them?

If the report is thin or generic:

```
The Executive Summary just restates the numbers, it doesn't explain the
pattern behind them. Look at how
context/sample-data/ap-reference-diagnostic-report.md does this: it
traces low PO coverage through to exceptions, cycle time and late
payment as one chain, instead of listing facts. Rewrite the Executive
Summary to do the same kind of connecting, using what actually came up
in this conversation.
```

If the benchmark table doesn't read right:

```
The Benchmarking table is missing the global and India comparison points
and the read for each row. Pull those directly from
context/sample-data/ap-benchmark-data.csv, and state the unit per row.
```

Run through the whole thing once more, start to finish: fresh interview, a few answers, one file upload, generate report, check it. That's the full loop your live audience will see later.

## Step B7 (optional): The Reviewer gets the last look — silently

Yesterday the Reviewer critiqued the reference report, then the twenty diagnostic questions. Give it the same job here, on every report the app generates, before anyone sees it. But the critique itself stays internal: a skeptical CFO's notes on unsupported claims and shaky sample sizes are useful to whoever writes the report, not to whoever is reading the finished one. The Reviewer's job stays exactly what it was yesterday, critique only, it still never rewrites. A second, separate call does the fixing, using the critique as an input the client never sees.

Two prompts.

### First, the critique

```
Add a review pass to report generation. After the report is written,
make a third call to Claude using the body of context/agents/reviewer.md
as the instructions, and the generated report as the input. Hold on to
the critique, but don't show it anywhere in the UI yet.
```

### Then, a silent revision

```
Add a fourth call, right after the critique. Give it the same
instructions as report generation (persona, benchmark-interpreter skill,
template-filler skill, benchmark data, reference report), plus the draft
report and the Reviewer's critique, and ask it to produce a revised
report that fixes whatever in the critique is actually fixable from the
conversation on record. It must not invent a number to answer a gap the
critique raised. If the critique points at something genuinely never
established, that stays flagged exactly as before, just as honestly. It
must never mention the Reviewer, the critique, or the review process
anywhere in its output, the client should never know a review happened.

Show only this revised report to the user, in the same view as before.
The Reviewer's raw critique should not be visible anywhere in the app.
```

If time is short, skip this and move to deploy. It is a nice-to-have in the app. The habit behind it, a hard read over every draft before a client sees it, is the reason the Reviewer exists, but the client was only ever meant to see what that read produced, not the read itself.

## Step B8 (optional): Export the report as a branded PDF

The report on screen is markdown in a browser tab. Nobody sends a client a browser tab. `sample-data/design.md` is a design spec pulled from a real EY Finance Transformation deck — colours, the yellow banner treatment, table style, the footer — written for whoever builds the PDF, not for the model that writes the report's content. Visual layout isn't something a text-generation model produces; it gets implemented in code against that spec.

Run the sync first, since `design.md` is new in `sample-data/`:

```
Run npm run sync-context in ey-fin-os.
```

Then one prompt:

```
Add a "Download PDF" button next to "Generate Report". Clicking it turns
the current report (whichever markdown is on screen: the plain draft, or
the revised one if Step B7 is wired in) into a PDF, styled per
context/sample-data/design.md, and downloads it.

Use @react-pdf/renderer for the PDF itself, it runs in Node without a
headless browser, which matters once this deploys to Vercel. Parse the
report's markdown into a syntax tree with remark first (headings,
paragraphs, tables are all the report ever produces), then walk that
tree and map each node onto a react-pdf component styled from
design.md's tokens: the yellow banner treatment for each of the six
section headings, the yellow-header/grey-border table style for the
As-is Observations grid and the Benchmarking table, the footer with page
number and report title on every page after the cover. Build a simple
cover page too: a dark background, a plain yellow rectangle block,
client name and report title inside it.

Don't hand-write a parser for this report's exact wording. The report's
phrasing changes every time it's generated, only its markdown structure
(headings, paragraphs, tables) is stable, so render generically off that
structure rather than pattern-matching specific section names.

Show me the downloaded PDF.
```

Check the PDF against `design.md`, not against the source deck slide by slide — the deck is a presentation, this is a document, and the spec already says what carries over (the palette, the banners, the table style) and what doesn't (the org charts, the geo map, the benchmark dot-plots, none of which this report has the underlying data to draw anyway).

If time is short, skip this too. A clean markdown report on screen is already a real deliverable; this step is packaging, not substance.

---

> **Checkpoint B - the report is real**
>
> - [ ] Uploading `sample-email-from-ap-manager.txt` changes what the agent asks next
> - [ ] Uploading `sample-invoice-aging-extract.csv` works the same way
> - [ ] Clicking "Generate Report" produces a report you can read on screen
> - [ ] All six sections are present, in the right order
> - [ ] Every finding is tagged with Impact, Ease, Dimension and Timeframe, and finding IDs run in sequence
> - [ ] The Benchmarking table gives global, India and client for each metric, with units
> - [ ] The Executive Summary explains the causal chain, not just a list of facts
> - [ ] Every number in the report actually came from the conversation or an upload, nothing invented
> - [ ] "Back to interview" brings the chat back
> - [ ] (Optional) The report has been through the Reviewer's critique and a silent revision pass — you see only the improved report, never the raw critique
> - [ ] (Optional) "Download PDF" produces a branded PDF matching `design.md`: yellow section banners, the yellow-header table style, a footer on every page
>
> Stop here. Don't move to deploy until your report looks like something you'd stand behind.

---

# PART C - Deploy to Vercel

**Outcome:** a live URL. The exact same app, running on the internet, that anyone can open.

The steps below are prompts to Claude Code. It runs the git and Vercel commands for you and asks you to approve each one. Along the way it may ask you to finish a sign-in to GitHub or Vercel in your browser. Do that with your own GitHub account and your own Vercel account, then tell Claude to carry on.

## Step C1: Switch to an API key, and test before you deploy

Everything so far has run through your Claude Code login. That only works on your laptop. A deployed app has no Claude Code session to borrow, so from here it needs an API key.

Get an **Anthropic API key** now, from **console.anthropic.com** (Settings → API Keys). This is a separate credential from your Claude Code login, with its own metered billing, not covered by a Claude.ai or Claude Code subscription. Also decide which **Claude model** you're calling, e.g. `claude-sonnet-5` — use the exact current model ID, not a guess from memory, since these change.

```
Open .env.local in a text editor for me.
```

Change `AI_PROVIDER` from `claude-cli` to `anthropic`, and add the two new lines, so the file reads:

```
AI_PROVIDER=anthropic
ANTHROPIC_API_KEY=your-api-key
AI_MODEL=claude-sonnet-5
```

One value per line, no quotes, no spaces around the `=`. Save, then tell Claude: `I've switched to the API key. Restart the app.`

**Now run the whole thing again locally, on the key.** Start a fresh interview, answer three or four questions, and generate a report. This is the point of doing it here rather than after deploying: if the key or the model string is wrong, you find out on `localhost:3000` where you can read the error, not on a live URL in front of the room.

If the chat stops working:

| What you see | What it means |
| --- | --- |
| `invalid x-api-key` or `authentication failed` | The key is wrong, or has a stray space or line break. Check it is on one line. |
| `model not found` or similar | The model ID is wrong or has been retired. Check the exact current ID rather than one from memory. |
| Still behaving exactly as before | `AI_PROVIDER` is probably still `claude-cli`, so it never switched. Change it and restart. |

Once it works on the key, carry on. Everything from here uses the API.

## Step C2 (optional): Add a password gate, and test it locally

Your URL is about to go public in Step C4. Anyone with the link will be able to open it, and Vercel's own password protection is a paid feature not every plan has. Do this now, before the app ever goes live, so it's never sitting on the internet unprotected, not even briefly.

```
Add a password gate in front of the whole app: middleware.ts checking
HTTP Basic Auth against a SITE_PASSWORD environment variable. If
SITE_PASSWORD isn't set, the site stays open, this should never be able
to lock me out of my own laptop by accident. Apply it to every route,
including the API routes, not just the pages.

Add SITE_PASSWORD to .env.local, choose the password yourself, whatever
you'd like it to be. Restart the app and confirm localhost:3000 now asks
for it.
```

Whoever opens the link later gets the browser's own login prompt. It always asks for a username too, that's just how the browser renders this kind of login, not something you can turn off from the app's side. It doesn't matter what goes in that field, only the password is actually checked, so anything works there, blank included.

You'll add this same value to Vercel in Step C4, alongside the other environment variables, so the very first deployment already has it rather than a later one.

Share the password separately from the link once it's live, the same way you'd share a door code and an address separately.

## Step C3: Get your app onto GitHub

Only the app goes to GitHub. `ey-fin-os/` becomes its own standalone repo, separate from `thecrux-ey-bootcamp`. That works because in Part A you copied everything the app needs into `ey-fin-os/context/`, so the app is self-contained. Nothing it reads lives outside its own folder any more.

The course material, your session notes, and the Day 1 originals all stay on your laptop and never get pushed anywhere.

Run the sync one last time first, so the copies in `context/` match whatever you changed during the build:

```
Run npm run sync-context in ey-fin-os, then show me what changed in
ey-fin-os/context/ so I can confirm the persona and skills the app will
deploy with are the current ones.
```

Then push just the app:

```
Turn ey-fin-os/ into its own standalone git repository and push only
that folder to GitHub. Steps: add ey-fin-os/ to the .gitignore at the
project root so the outer project stops tracking it, run git init inside
ey-fin-os/, create a new PRIVATE GitHub repository under my own account
called ey-ap-diagnostic, and push ey-fin-os/ to it as the repo root, so
package.json and next.config.ts sit at the top level of the repo.

Before you push, confirm two things and show me the evidence: .env.local
is in .gitignore and is not being committed, and ey-fin-os/context/ IS
being committed, because the deployed app cannot run without it.

Show me the repo URL when done.
```

Make sure it is private, not public. Your persona file, your checklist and your benchmark numbers all travel with the app in `context/`, and on the hackathon track that content can be real client thinking. Vercel deploys from a private repo exactly the same way.

When it finishes, open the repo URL in your browser and check the file list. GitHub shows a **Private** label next to the repo name. If it says Public, tell Claude: `Make this repository private.` You should see `package.json`, `next.config.ts`, `src/`, and `context/` at the top level. `context/` is the one people forget, and without it the live app has no persona to read. You should not see `.env.local`. If you do, tell Claude: `.env.local got committed. Remove it from git tracking without deleting the file, make sure .gitignore excludes it, and push again.` Then rotate the key in console.anthropic.com, the old one counts as exposed.

## Step C4: Deploy to Vercel

The app is the repo root now, so there is no Root Directory to configure and nothing sitting outside it. This step is short.

```
Deploy this repository to Vercel as a production deployment. The Next.js
app is at the repo root, so leave the Root Directory as the default, do
not set it to a subdirectory.

Add these environment variables from .env.local on Vercel, for both
production and preview: AI_PROVIDER, ANTHROPIC_API_KEY, AI_MODEL, and
SITE_PASSWORD too if you set one up in Step C2. Give me the live URL.
```

`AI_PROVIDER` is the one people leave out, and it is the one that matters most. Without it on Vercel the deployed app never switches off the Claude Code path, and it fails there because there is no CLI on the server to call.

Claude may ask you to log in to Vercel in the browser. Sign in with your Vercel account (the one tied to your GitHub account), then tell Claude to continue. When it finishes it gives you a live URL, something like `https://ey-ap-diagnostic-yourname.vercel.app`.

## Step C5: Test the live version

Open the URL it gave you, ideally on your phone or a different browser tab so you're hitting the deployed app, not your local one. If you did Step C2, the browser should ask for the password before showing anything else, confirm that too. Then run through the whole loop once more: a short interview, one file upload, generate the report.

If something that worked locally breaks live, it's almost always one of two things: the environment variables, or `context/` not making it into the repo. See Troubleshooting below.

---

> **Checkpoint C - it's live**
>
> - [ ] You switched `.env.local` to the API key and ran a full interview locally on it before deploying
> - [ ] `ey-fin-os/` is its own **private** GitHub repo, with `package.json` and `next.config.ts` at the top level
> - [ ] `context/` is in that repo, and `.env.local` is not
> - [ ] `AI_PROVIDER`, `ANTHROPIC_API_KEY` and `AI_MODEL` are all set on Vercel
> - [ ] (Optional) If you did Step C2, `SITE_PASSWORD` is set on Vercel too, and the live URL asks for it before showing anything
> - [ ] Claude gave you a live Vercel URL
> - [ ] That URL opens in a browser you didn't build the app in
> - [ ] The chat, the file upload, and the report all work on the live URL, proving the app carried its own persona, skills, and sample data with it
>
> This is the finish line for the build. If you're here, you built and shipped a real product in one sitting.

---

# PART D (optional) - Give it a memory

**Only start this once Part C is live and working.** Everything below is a second lap over an app that already does its job. If you stop before this, you still have a complete, deployed product.

**Outcome:** every interview is saved to a real database and has its own link. The client can close the tab and reopen that link later, and the conversation carries on where it stopped.

Right now the conversation lives in the browser. Refresh the page and it is gone. That is fine for a demo you run yourself. It is not fine for a real client who gets pulled into a meeting halfway through answering.

This is also where you see how a real app stores things: a table per kind of record, a key that links them, and a server that is the only thing allowed to touch either.

## Step D1: Create a Supabase project and its tables

Do this in your browser, not in Claude Code.

**Create the project**

1. Go to **supabase.com** and sign in. Signing in with your GitHub account is the fastest route, and you already have one.
2. Click **New project**. If it asks which organisation, any one will do.
3. Give it a name, `ey-ap-diagnostic` is fine.
4. It will ask you to set a **database password**. Generate one and save it somewhere. You will not need it today, but you cannot see it again later.
5. Pick the region closest to you.
6. Click **Create new project**, then wait. Provisioning takes a minute or two. The dashboard appears before the database is ready, so if the next step errors, give it another thirty seconds and try again.

**Create the tables**

7. In the left sidebar, click **SQL Editor**, then **New query**.
8. Paste in everything below and click **Run**. You want "Success. No rows returned", which is what `create table` returns when it works.

```sql
create table if not exists diagnostic_sessions (
  id              uuid primary key default gen_random_uuid(),
  client_name     text,
  process         text        not null default 'AP',
  status          text        not null default 'in_progress',
  report_markdown text,
  created_at      timestamptz not null default now(),
  updated_at      timestamptz not null default now()
);

create table if not exists diagnostic_messages (
  id         bigserial   primary key,
  session_id uuid        not null references diagnostic_sessions(id) on delete cascade,
  role       text        not null check (role in ('user', 'assistant')),
  content    text        not null,
  created_at timestamptz not null default now()
);

create index if not exists diagnostic_messages_session_idx
  on diagnostic_messages (session_id, id);

alter table diagnostic_sessions enable row level security;
alter table diagnostic_messages enable row level security;
```

9. Click **Table Editor** in the left sidebar and confirm you can see both tables: `diagnostic_sessions` and `diagnostic_messages`. If you cannot see them, the SQL did not run, and nothing below will work.

You are pasting this rather than asking Claude to invent it for a reason. The app has to use these exact table and column names, and a schema written fresh each time drifts, so you end up debugging a mismatch between a table called `messages` and code looking for `diagnostic_messages`.

Three things in that SQL worth knowing. `diagnostic_messages` carries a `session_id` that points back at a row in `diagnostic_sessions`, which is what makes "all the messages for this one interview" a question the database can answer. `report_markdown` sits on the session itself, not as its own table, because there is only ever one current report per interview, it is not a growing list like messages. And row level security is on with no policies, which means the public key that ships to the browser can read and write nothing. All access goes through your server.

**Collect the two values the app needs**

10. In the left sidebar, click the **gear icon** (Project Settings), then under **Integrations** click **Data API**.
11. Copy the **API URL**, but only up to and including `.co`. If what you copy has anything after that, like `/rest/v1`, delete it. You want exactly `https://abcdefgh.supabase.co` and nothing more.
12. Back in **Project Settings**, click **API Keys**, then open the **Legacy anon, service_role API keys** tab. Copy the **`service_role`** key. It is hidden by default, so click to reveal it first.

Do not be put off by the word "Legacy" on that tab. Supabase has newer publishable and secret keys, but `service_role` is the one this app uses and it is the right choice here.

Step 11 catches people out. Supabase shows the URL with a path on the end, and pasting the whole thing gives you a broken `SUPABASE_URL`. Every save then fails, and the error you get back does not mention the URL at all, so it is a slow one to spot. Trim it to `.co`.

That `service_role` key is a master key for your database. It bypasses every access rule, including the row level security you just turned on. It goes in `.env.local` and later into Vercel, and nowhere else. Never paste it into the chat with Claude, never commit it, never put it in front-end code.

The free tier is plenty for today, and the project stays yours afterwards.

## Step D2: Wire the database into the app

```
Open .env.local in a text editor for me.
```

Add two lines yourself, using the values you just collected:

```
SUPABASE_URL=https://yourproject.supabase.co
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
```

One value per line, no quotes, no spaces around the `=`. Check the URL ends at `.co` with nothing after it. Save and close the editor, then tell Claude: `I've added the Supabase values.`

Do not paste the `service_role` key into the chat with Claude. It belongs in that file and nowhere else.

```
Use SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY from .env.local. The app
must talk to Supabase only from server-side API routes using the service
role key, and that key must never reach the browser.

The tables already exist in Supabase. Do not create them, do not write a
migration, and do not rename anything. Use exactly these:

- diagnostic_sessions: id (uuid), client_name, process, status,
  report_markdown, created_at, updated_at
- diagnostic_messages: id, session_id, role ('user' or 'assistant'),
  content, created_at

Restructure the app so each interview has its own URL:

- A landing page at / where someone starts a new interview. It asks for
  their organisation name and which process this diagnostic is for,
  defaulting to AP, writes both to a new diagnostic_sessions row as
  client_name and process, and sends them to that interview's page.
- The chat itself moves to /chat/[sessionId].
- The opening message is still built on the server, not generated. When
  the session is created, write it straight to diagnostic_messages as the
  first assistant row, so it is part of the transcript like any other
  message.
- Every message after that, from the client and from the agent, is saved
  to diagnostic_messages as it happens.
- Opening /chat/[sessionId] again loads that session's transcript back
  into the chat and continues it, rather than starting a new one.
- The system prompt is told when it is resuming, so the agent welcomes
  the client back instead of greeting them as a stranger.

Leave the file upload alone. For report generation, one thing has to
change now that a landing page exists: fetch that session's client_name
and process before generating, and hand them to the model explicitly,
flagged as setup information rather than something the client said
during the interview. Without this the report always shows the client
name as not established, because the model only ever sees the
conversation transcript, and the client typed their organisation name
on a page before the conversation started, not during it. Otherwise
leave report generation as it is. Just make the result persist: when
"Generate Report" produces a report, save it to that session's
report_markdown column. When /chat/[sessionId] loads a session that
already has a report saved, show that report view directly instead of
the chat, the same way "Back to interview" already reveals the chat,
just the reverse. Regenerating overwrites the saved report, it does not
version it.

Add a "Copy link to resume" button in the chat header.

Then show me it working: start an interview, confirm the opening message
is there, answer two questions, reload the page, and confirm everything
is still there.
```

Test the resume properly, because this is the part most likely to be subtly broken. Start an interview, answer two questions, copy the link, close the tab, then open the link in a fresh tab. Your conversation should still be there, and the agent should carry on rather than start again.

If it greets you from scratch:

```
On resume the agent started the interview again instead of continuing.
Make sure this session's saved messages are loaded and passed to the
model, and that the system prompt tells it to welcome the client back and
continue rather than greet them fresh.
```

Check Supabase too, not just the screen. Open the Table Editor and look at `diagnostic_sessions` and `diagnostic_messages`. You should see one row for the interview and a row per message. If the chat looks fine but those tables are empty, nothing is actually being saved. Generate a report, then look at `diagnostic_sessions` again — `report_markdown` should now hold it. Reload `/chat/[sessionId]` and you should land straight on the report, not the chat.

## Step D3: Redeploy with the database

```
Commit everything that changed in ey-fin-os and push it. Then add
SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY as environment variables on
Vercel, for both production and preview, and redeploy to production.
Give me the live URL.
```

Open the live URL, start an interview, answer a question, copy the resume link, and open it on your phone. That is the whole point of this part, and it is a good thing to be able to show.

### Troubleshooting Part D

| Problem | Fix |
| --- | --- |
| Saving fails and the error mentions a bad URL, a 404, or just "fetch failed" | `SUPABASE_URL` probably has a path on the end. Open `.env.local` and trim it to end at `.co`, with no `/rest/v1` or anything else after. Then restart the app. |
| `Supabase is not configured` when you start an interview | `SUPABASE_URL` or `SUPABASE_SERVICE_ROLE_KEY` is missing from `.env.local`, or the app was not restarted after you added them. Ask Claude to check both are set, then: `Restart the app.` |
| `relation "diagnostic_sessions" does not exist` | The tables do not exist in this Supabase project yet. Go back to Step D1 and run the SQL. |
| Reopening the link shows an empty chat | The saved messages are not being loaded. Say: `Opening /chat/[sessionId] shows an empty chat. Load that session's rows from diagnostic_messages and render them before the conversation continues.` |
| Reopening the link shows the transcript but the agent greets me as a stranger | The history is on screen but is not reaching the model. Say: `On resume, pass this session's saved messages to the model, and tell it to welcome the client back and continue from where it stopped.` |
| The landing page does not create an interview, or / is still the old chat | The restructure did not land. Say: `The / page should create a row in diagnostic_sessions and redirect to /chat/[that session id]. Show me the code that does it.` |
| It saves locally but not on the live Vercel URL | The Supabase variables are missing on Vercel. Tell Claude: `Check SUPABASE_URL and SUPABASE_SERVICE_ROLE_KEY are set on Vercel for both production and preview, then redeploy.` |
| I pasted the `service_role` key somewhere public | Treat it as compromised. In Supabase, Project Settings, API Keys, on the Legacy anon, service_role tab, roll the key, then update `.env.local` and Vercel with the new one. |
| The report always shows the client name as not established, even though I typed it on the landing page | The report call never sees that field. Say: `Fetch client_name and process from diagnostic_sessions before generating the report, and pass them to the model explicitly, flagged as setup info rather than something the client said in the conversation.` |

---

> **Checkpoint D (optional) - it remembers**
>
> - [ ] `diagnostic_sessions` and `diagnostic_messages` have real rows in them
> - [ ] You can close the tab, reopen the link, and the interview carries on where it stopped
> - [ ] Generating a report saves it to `report_markdown`, and reopening that session's link shows the report directly, not the chat
> - [ ] The same link works on the live Vercel URL, not just locally

---

## Into the hackathon

You just built one persona end to end: Accounts Payable. Same pattern, same shape, same parts you just walked through, applies directly to AR, R2R, FP&A, or a real client engagement you're already on. This afternoon, you pick one and adapt what you just built, with your own data.

---

## Troubleshooting - Session 5

Work top to bottom. Most problems are one of these. Part D has its own table above.

### API keys and auth

| Problem | Fix |
| --- | --- |
| The chat says something like "authentication failed" or "invalid API key" | Almost always `.env.local` has the wrong key, or a stray space or line break pasted in with it. Ask Claude to open `.env.local` in an editor, check the key is on one line with no quotes and no trailing space, save, then tell Claude: `Restart the app.` |
| It worked locally, breaks on the live Vercel URL, and the error is about authentication | The variables are missing in Vercel, or were only added for one environment. Tell Claude: `Check that AI_PROVIDER, ANTHROPIC_API_KEY and AI_MODEL are set on Vercel for both production and preview. Add any that are missing, then redeploy to production.` |
| It worked locally, breaks on the live Vercel URL, and the error is that it can't find the persona, a skill, or a sample-data file | `context/` did not make it into the repo, or the build isn't carrying it. Tell Claude: `Check that ey-fin-os/context/ is committed and pushed, it may have been caught by a .gitignore rule. Then check next.config.ts has outputFileTracingIncludes covering ./context/**/*. Fix whichever is wrong and redeploy.` |
| I don't have an API key yet | Get one from console.anthropic.com (Settings → API Keys) when you reach Part C. It is a separate credential from your Claude Code login, with its own billing. |
| Claude Code itself won't respond, or asks me to log in | That's your Claude Code login, unrelated to the app's API key. Close and reopen Claude Code. If it still asks you to log in, ask the facilitator. Do not sign in with a personal account. |
| `.env.local` accidentally got committed to GitHub | Stop. Tell Claude Code: `.env.local got committed, remove it from git history and tracking, keep the file locally, and confirm .gitignore excludes it.` Then rotate the key in console.anthropic.com, the old one counts as exposed. |

### The Claude Code CLI provider (Parts A and B)

While you are building locally the app shells out to `claude -p`. These are the failures that come with that.

| Problem | Fix |
| --- | --- |
| `claude CLI exited with code 1` mentioning connectors, auth, or a key taking precedence | An API key is set in your environment and Claude Code is preferring it over your login. The app is supposed to strip those before calling the CLI. Say: `Strip ANTHROPIC_API_KEY, ANTHROPIC_AUTH_TOKEN and OPENROUTER_API_KEY from the environment passed to the claude child process.` |
| `command not found: claude` or `spawn claude ENOENT` | The app cannot find the CLI. Confirm it works in your terminal with `claude -p "hello"`. If that fails, Claude Code is not installed or not on your PATH, so ask the facilitator. |
| Every reply appears twice | The code is reading both the `stream_event` deltas and the final `assistant` event. Say: `Read only the stream_event content_block_delta text and ignore the assistant event.` |
| Replies are slow, three to six seconds before text appears | Normal for the CLI path. It carries Claude Code's own startup on every call. It gets noticeably faster in Part C on the API key. |
| The agent behaves oddly, mentions files, or sounds like a coding assistant | The CLI is picking up this project's `CLAUDE.md`. Say: `Run the claude child process with cwd set to a temporary directory so it does not load the project's CLAUDE.md.` |

### The opening message

| Problem | Fix |
| --- | --- |
| The chat opens empty and waits for me to type | The opening message isn't being built. Say: `Build the opening message on the server when the chat page loads and show it as the agent's first message, with no model call.` |
| The opening message asks two things at once | It's being composed by the model instead of read from the checklist. Say: `Take the first question word for word from the checklist CSV, don't write one.` |
| The opening message takes seconds to appear | It's being generated by a model call. It shouldn't be. Say: `The opening message should be assembled on the server from the CSV, not generated by a model call.` |

### File upload not reading correctly

| Problem | Fix |
| --- | --- |
| Uploading a file does nothing, agent doesn't react | Ask Claude: `Confirm the uploaded file's content is actually being sent to Claude in the next message, log it if needed so we can check.` |
| The agent mentions the file exists but not what's in it | The file's raw text likely isn't being included, only its name. Say: `Read the actual text content of the uploaded file and include it in the message, not just the filename.` |
| The agent tries to compute totals or averages from an uploaded CSV and gets it wrong | Tell it: `Don't calculate anything yourself from the CSV, just read the numbers as they are and use them in context.` (Claude reading the file directly and reasoning over it is fine, the fix is against silently mis-adding rows, not against using the numbers.) |

### The agent asks too few or too many questions

| Problem | Fix |
| --- | --- |
| The agent says it's ready after two questions | It's not weighing the checklist properly. Say: `You said you had enough after only two questions. Check that against context/sample-data/ap-data-collection-template.csv, you're missing most of the Volume, Process, and Metric questions. Keep going.` |
| The interview drags on and on, never seems satisfied | Click "Generate Report" yourself, it works at any point. Then separately fix it: `The agent should judge 'enough' the way an experienced consultant would, roughly ten to twelve solid answers across the categories in the checklist, not every single field. Loosen the bar.` |
| It asks the same question twice | Tell it: `Track what's already been answered across the whole conversation, including anything learned from an uploaded file, and don't re-ask it.` |
| It asks several questions at once, not one at a time | Say: `Ask one focused question per turn, like a real interviewer would, not a list.` |

### Report generation

| Problem | Fix |
| --- | --- |
| The report is missing a section, or sections are in the wrong order | Point Claude back at the six required sections and the order, and at `context/sample-data/ap-reference-diagnostic-report.md` as the exact shape to match. |
| The report invents numbers that were never mentioned | Say: `Some of these numbers were never established in the conversation or an upload. Only use figures that actually came up, and say plainly where something wasn't established, don't estimate.` |
| The Benchmarking table has no comparison points | Say: `Pull the global_benchmark and india_benchmark columns from context/sample-data/ap-benchmark-data.csv for each metric, and state the unit on every row.` |
| The whole report reads generic, could be about any company | Say: `This report doesn't sound specific to this conversation, it reads like it could be about anyone. Pull in the actual details this client mentioned, the specific pain point in their own words, the specific vendor or process names if any came up.` |
| The report is mostly gaps and curly braces | You generated it too early. Go back, answer a few more questions, and generate again. Four or five solid answers is the floor for something worth reading. |
| The report cuts off partway through, missing later sections, especially on the live URL | The model hit its output-token limit before finishing. Extended thinking shares that same budget and how much it uses varies run to run, so this can pass locally and still fail live. Say: `Raise maxOutputTokens on the report generation call to at least 8192, and export maxDuration (at least 90) from that route file.` |

### GitHub push and Vercel deploy

| Problem | Fix |
| --- | --- |
| Claude says it can't push to GitHub, or can't log in to Vercel | Read what it's asking for. It usually needs you to finish a sign-in in the browser. Do that with your own GitHub or Vercel account, then tell Claude to try again. |
| Claude says the GitHub repo name is taken, or the push was rejected | Tell it: `Pick a different repo name, or fix whatever the push is complaining about, and try again.` Show it the exact message if it asks. |
| Claude tries to push the whole `thecrux-ey-bootcamp` project instead of just the app | Say: `Only ey-fin-os should be pushed, as its own standalone repo with package.json at the top level. The course material and my Day 1 originals stay on my laptop.` |
| The repo on GitHub is missing `context/` | This is the one that breaks the live app. Tell Claude: `ey-fin-os/context/ is missing from the repo. Check whether a .gitignore rule is excluding it, force-add it if needed, commit and push, then redeploy.` |
| I changed a persona or a skill and the live app still uses the old one | The copies in `context/` are stale. Say: `Run npm run sync-context, then commit the changes in context/ and push so the deployed app picks them up.` |
| The Vercel deploy fails with a build error | Tell Claude: `The Vercel deploy failed with this error: [paste it]. Fix whatever's causing the build to fail, then redeploy.` Almost always a missing dependency, a missing `context/` folder, or a leftover error from local testing. |
| The live URL loads but the chat doesn't respond | Tell Claude: `The live app isn't responding. Check AI_PROVIDER, ANTHROPIC_API_KEY and AI_MODEL are set on Vercel for production, and check the deployment logs for the error.` |
| The deploy works but shows an old version of the app | Tell Claude: `Commit and push everything that's changed, then redeploy to production.` |

### AI SDK version errors

These four are the most common failures in Part A. They all have the same
cause: Claude wrote code for an older version of the AI SDK than the one
you installed. Paste the exact error to Claude along with the fix below.

| Problem | Fix |
| --- | --- |
| `Export useChat doesn't exist in target module` on the chat page | `useChat` moved out of the `ai` package. Say: `Install @ai-sdk/react and import useChat from there instead of from ai.` |
| `response.toDataStream is not a function` in the API route | Old method name. Say: `In the chat API route, return result.toUIMessageStreamResponse() instead of toDataStream(). Don't await streamText, it isn't a promise.` |
| The chat sends a message and nothing comes back, no error in the browser | The route is probably still hand-parsing the stream, or the message shape is wrong. Say: `Use useChat from @ai-sdk/react on the page and convertToModelMessages in the route. Delete any hand-written stream parsing code.` |
| Messages render as blank bubbles | Message content is now an array of parts, not a string. Say: `Render message.parts, filtering for type "text", instead of message.content.` |

### Everything else

| Problem | Fix |
| --- | --- |
| The app won't start, or Claude shows a wall of red text | Tell Claude: `The app failed to start with this error: [paste it]. What's wrong, and fix it.` |
| Claude Code did too much at once and now things are broken | Say: `That's too much. Go back to just the working chat from before this change. Undo it, then do this one step at a time.` |
| I've lost the thread | Say so. The room waits at the next checkpoint until you're caught up. |

---

**Next:** [Hackathon →](capstone.md)
