[← Back to Student Handbook](student-handbook.md)

---

# Session 2: Skills - The Diagnostic Toolkit

**Day 1**

**What you'll build:** four skills. A skill is a reusable `/slash-command` that holds one piece of diagnostic logic, so Claude does the same job the same way every time you call it.

---

## The one thing to understand before you start

Tomorrow, every team builds one thing: a live AP diagnostic chatbot. It talks to an "AP client," asks the right questions, reads uploaded files, compares the numbers to benchmarks, and writes a diagnostic report.

That chatbot is built from four small parts. You build all four this afternoon, as the exact pieces tomorrow's build assembles. When you build the chatbot tomorrow, you will recognise each one.

---

## What a skill is (just read)

### The picture

A skill is a checklist you hand Claude. You write it once, in plain English. From then on you run it by name, with a short command like `/data-checklist`, and Claude does that job exactly the way the checklist says. Every time. No re-explaining, no drift between Monday's version and Friday's.

Think of the procedure notes a good senior would leave for a new joiner: what to check, in what order, what never to do. That, as a file Claude reads.

### The shape of a skill file

The files live in a folder called `.claude/skills/`. Each skill gets its own folder, and inside it a file called `SKILL.md`. So a skill called `data-checklist` lives at:

```
.claude/skills/data-checklist/SKILL.md
```

A `SKILL.md` file has two lines up top, between the `---` markers, and the instructions underneath:

```yaml
---
name: data-checklist
description: Checks a conversation against the AP data collection checklist and drafts the next question to ask. Use whenever I have a partial client conversation and need to know what is missing.
---

Your instructions here.
```

- The **name** is the command. `name: data-checklist` means you call it with `/data-checklist`. It must match the folder name.
- The **description** is how Claude decides to pick the skill up on its own. Ask "what am I still missing from this client conversation?" and, if the description is clear, Claude runs `/data-checklist` without being told. A vague description means the skill only ever runs when you type the command.
- The **body**, everything under the second `---`, is the checklist itself: the steps, the rules, the output shape.

### Where a skill lives decides where it works

A skill in this project's `.claude/skills/` folder works in this project only, which is right for the four you build today: they are AP diagnostic logic, and they belong with the AP build. A skill in `~/.claude/skills/` (the `.claude` folder in your home directory, not the project) follows you into every project you ever open.

### A skill is not CLAUDE.md

`CLAUDE.md` is standing context. It loads in every session, on its own, and it holds what is always true: who you are, and the house rules for how you want Claude to work with you. A skill is a recipe for one task. It loads when you call it, or when its description fits, does the job, and gets out of the way. Put a rule in `CLAUDE.md` when it should shape everything. Put it in a skill when it belongs to one job. Session 4 adds a third thing, an agent: a skill given a job title, a point of view, and its own separate conversation to work in.

### Skills can be found, not only written

You will write four skills today. You do not have to write every skill you ever use. Claude Code ships with built-in skills, and there is an official plugin marketplace, opened with `/plugin`, that includes document skills for Excel, PowerPoint, Word and PDF, the file types most finance work lives in. Installing from the marketplace is not part of today's timing. What is worth knowing now is that the option exists, and how to see what you already have:

```
What skills do I have available right now, built in and in this
project? List them with one line each on what they do.
```

Optional, if you have a spare minute later: type `/plugin` on its own and look at what the marketplace offers. Nothing installs until you say so.

### How you build one

You will not write these files by hand. You describe what you want, Claude writes the file, you test it, and if it is off, you tell Claude what to fix. Describe, test, correct. That is the whole loop, four times over, this session.

---

## Skill 1: `/data-checklist` - keep asking until we have enough

**Outcome:** given a conversation with an AP client (even a partial one), the skill tells you which required fields are answered, which are missing, and drafts the next question to ask. This is the exact logic that will sit behind tomorrow's chatbot and stop it from writing a report on half the facts.

The checklist it works from is at `sample-data/ap-data-collection-template.csv`. Open it and look at the columns: `field_id`, `category`, `question`, `why_it_matters`, `data_type`. Twenty fields across eight categories: vendor master, invoice processing, payments, reconciliation, query and dispute, compliance, team, and an open pain-point question.

```
Build a skill called "data-checklist" at .claude/skills/data-checklist/SKILL.md

It takes a conversation transcript (pasted in, or a file path) and checks it
against the checklist at sample-data/ap-data-collection-template.csv.

For each of the 20 fields in that file, it should:
- Mark it ANSWERED if the transcript covers it, and quote the line that
  answers it.
- Mark it MISSING if the transcript does not cover it.

Then it should:
- Give a one-line summary: how many of 16 are answered.
- Draft the next 2 to 3 questions to ask, in plain language, taken straight
  from the "question" column for the highest-priority MISSING fields.
  Priority order: Metric fields first, then Process, then everything else.

Rules the skill must enforce:
- Never mark something ANSWERED on a guess. If it is unclear, mark it
  MISSING and say why.
- Only ask 2 to 3 questions at a time, never all of them at once. A real
  client conversation cannot handle a wall of questions.

Use proper YAML frontmatter (name, description) and a markdown body.
```

Test it with a short, partial conversation:

```
/data-checklist on this conversation:

AP Manager: We process around 8,400 invoices a month, and roughly 40%
of them come in without a PO attached.
Diagnostic bot: Got it. What system do you use for invoice processing?
AP Manager: SAP, mostly. We have OCR turned on for scanned invoices but
a lot still come in as email PDFs someone has to key in by hand.
```

Check the output. Does it correctly mark volume, PO-backed ratio, technology, and OCR adoption as ANSWERED, and everything else (DPO, on-time rate, exception rate, discount capture, FTEs, cycle time, vendor concentration, approval steps, pain point) as MISSING? Are the next questions it drafts pulled straight from the checklist, not invented? If not, tell Claude what is off and have it fix `SKILL.md`.

**Why this matters for tomorrow:** tomorrow's chatbot cannot ask all 20 questions in one breath, and it cannot write a report with half the fields blank. This skill is that exact discipline, built and tested today.

---

## Skill 2: `/benchmark-interpreter` - turn a number into a read

**Outcome:** given a metric name and a value, the skill places it against the global and India peer points, states the unit, and says in plain English what that position actually means. This becomes the benchmark-comparison logic inside tomorrow's report.

The benchmark table is at `sample-data/ap-benchmark-data.csv`. Columns: `field_id`, `metric`, `unit`, `global_benchmark`, `india_benchmark`, `commonly_observed`, `read`.

```
Build a skill called "benchmark-interpreter" at
.claude/skills/benchmark-interpreter/SKILL.md

It takes a metric name and a value (for example: "Days Payable Outstanding
= 38 days") and looks it up in sample-data/ap-benchmark-data.csv.

It should return:
1. The three comparison points with the unit: global, India, and the
   client's value.
2. Position, stated as a fact: ahead of, near, or behind the India point.
3. One line, plain English, on what that position actually means. Use the
   "read" column from the benchmark file as context, not verbatim.

Rules the skill must enforce:
- If the metric name does not match anything in the benchmark file
  (even close variants), say so plainly instead of guessing.
- Never report a number without its unit.
- Some metrics are meaningless alone. DPO read without paid-on-time,
  or exception rate read without PO coverage, tells you nothing. Say
  "this needs X to interpret" rather than giving a false read.
- No jargon. This gets read by a client's finance team, not a data analyst.

Use proper YAML frontmatter (name, description) and a markdown body.
```

Test it with three numbers from a real-shaped conversation:

```
/benchmark-interpreter on these:

1. Days Payable Outstanding = 58 days
2. Invoice exception rate = 18%
3. PO-backed invoices = 62%
```

Check the output. Does DPO come back with all three points and the unit, and does it flag that it cannot be read without paid-on-time? Does the exception rate come back as well behind the India point of 8%? Does PO coverage at 62% get called out as the likely root cause of that exception rate? If the reads or the reasoning feel off, correct the skill once.

**Why this matters for tomorrow:** every number in tomorrow's report needs this same read next to it. Building it as a skill today means the build session tomorrow calls it, instead of explaining benchmark logic from scratch under time pressure.

---

## Skill 3: `/template-filler` - pour answers into the fixed report shape

**Outcome:** given a set of structured notes or answers, the skill produces a full diagnostic write-up in one fixed shape, every time. This becomes tomorrow's report generator.

The target shape is at `sample-data/ap-reference-diagnostic-report.md`. Open it now. Notice the headings: Executive Summary, As-is Observations (a table), Maturity Assessment, Benchmarking, Opportunities by Lever, Suggested Next Steps. That shape does not change. Only the content inside it does.

Look closely at the As-is Observations grid. Every row pairs an observation with an opportunity, and carries four tags: Impact, Ease, Dimension and Timeframe. That pairing is the whole discipline. A finding with no recommendation is just a complaint.

```
Build a skill called "template-filler" at .claude/skills/template-filler/SKILL.md

It takes structured notes (answers to the AP data checklist, plus any
benchmark reads) and produces a diagnostic report in exactly this shape,
matching sample-data/ap-reference-diagnostic-report.md:

1. EXECUTIVE SUMMARY. 3 to 5 lines, leading with the causal chain rather
   than a list of findings. Most AP problems chain: low PO coverage
   produces exceptions, exceptions produce cycle time, cycle time
   produces late payment. Find that chain and open with it.
2. AS-IS OBSERVATIONS. A markdown table, one row per finding: ID,
   sub-process, as-is observation, improvement opportunity, and
   implementation considerations. IDs run AP01, AP02 in sequence. Every
   row carries four tags in the considerations cell: Impact
   (High/Medium/Low), Ease (High/Medium/Low), Dimension
   (Process/People/Technology) and Timeframe (0-3, 3-6, 6-12, 12+ months).
3. MATURITY ASSESSMENT. A table, one row per sub-process, with current
   and 12-month target on five bands: Basic, Developing, Defined,
   Advanced, Leading, plus the basis for the band in one line.
4. BENCHMARKING. A markdown table: metric, unit, global, India, client,
   and a one-line read.
5. OPPORTUNITIES BY LEVER. Grouped under Standardisation, Optimization,
   Technology, and People and controls. Reference the finding IDs rather
   than restating them.
6. SUGGESTED NEXT STEPS. Sequenced, and ending with what was not
   established during the interview.

Rules the skill must enforce:
- Never invent a number, a quote, or a client detail that was not in the
  notes it was given. If something needed for the report is missing,
  flag it in {curly braces} instead of making it up.
- Never an observation without a paired improvement opportunity.
- Ease is about the organisation, not the size of the edit. A policy
  change needing procurement sign-off is not Ease High.
- Keep the headings and their order exactly as above. Do not add or
  rename sections.
- Tone: plain, confident, the way a senior consultant writes for a CFO,
  not a marketing tone.

Use proper YAML frontmatter (name, description) and a markdown body.
```

Test it with a small, messy set of notes:

```
/template-filler on these notes:

Client: Northwind Industrial Supplies. 8,200 invoices/month, 9 FTEs.
Non-PO invoices are 40% of volume and the main source of exceptions.
Approval chain has 5 steps for anything over Rs 50,000, two approvers
are frequently unavailable. Team knows discounts are being missed but
has no visibility into vendor terms.

Metrics: PO-backed invoices 60% (global 95, India 85).
Exception rate 18% (global 2, India 8).
Paid on time 41% (global 95, India 70).
DPO 58 days (global 45, India 55).
Early discount capture 32% (global 85, India 45).
```

Compare the output against `sample-data/ap-reference-diagnostic-report.md`. Same six sections, same order? Is every finding tagged and paired with an opportunity? Nothing invented that was not in the notes? If a section is missing or a number looks made up, push back once and have Claude tighten the skill.

**Why this matters for tomorrow:** this is, almost word for word, the report generator tomorrow's build wires up at the end of the chatbot. You are not practising for that step. You are building it.

---

## Skill 4: `/file-reader` - turn an uploaded file into checklist answers

**Outcome:** given a file a client would actually hand you (an email, a spreadsheet extract), the skill pulls out the data points that answer your checklist, doing any needed calculation and showing its working. This is the exact muscle tomorrow's chatbot needs for its file-upload feature.

Two files to test on, both in `sample-data/sample-client-inputs/`:

- `sample-email-from-ap-manager.txt` - a fictional AP manager replying to a few questions by email.
- `sample-invoice-aging-extract.csv` - a fictional export of individual invoices, with columns like `vendor`, `po_backed`, `days_to_approval`, `exception_flag`, `paid_on_time`.

```
Build a skill called "file-reader" at .claude/skills/file-reader/SKILL.md

It takes one or more uploaded files (an email, a screenshot, a CSV or
spreadsheet extract) and checks them against the checklist at
sample-data/ap-data-collection-template.csv.

For each field in that file, it should:
- Say whether any of the uploaded files answers it, directly or
  indirectly.
- Give the answer, quoting or calculating from the source file.
- If a field needs a calculation (for example, PO-backed ratio or
  exception rate from a CSV extract), do the calculation and show the
  working, not just the final number.
- If nothing answers a field, mark it MISSING, not a guess.

Rules the skill must enforce:
- Never invent a number that is not derivable from the uploaded files.
- Show its arithmetic for any calculated field, so a consultant can
  check the row count and the working.

Use proper YAML frontmatter (name, description) and a markdown body.
```

Test it on the two practice files:

```
/file-reader on sample-data/sample-client-inputs/sample-email-from-ap-manager.txt
and sample-data/sample-client-inputs/sample-invoice-aging-extract.csv
```

Read the output. The email should answer volume, PO-backed share, the SAP/OCR setup, and the approval bottleneck. The CSV should let Claude calculate a PO-backed ratio and an exception rate directly from the ten rows, which the email does not give you as clean numbers. Metrics like DPO, on-time payment rate at the aggregate level, FTE count, and vendor concentration should come back MISSING from both files, because they genuinely are not in there.

Once that works, chain it straight into Skill 1:

```
Take the checklist answers /file-reader just pulled from those two files
and run /data-checklist on them, the same way you would on a live
conversation.
```

**Why this matters for tomorrow:** tomorrow's chatbot has a file-upload feature. A client will drop in an email, a screenshot, or an extract mid-conversation, and the bot has to read it and update the checklist without being told exactly what is in it. This skill is that exact logic, built and tested today.

---

> **Checkpoint: four skills, one toolkit**
>
> - [ ] `/data-checklist` built at `.claude/skills/data-checklist/SKILL.md`, tested on a partial conversation, correctly separates ANSWERED from MISSING
> - [ ] `/benchmark-interpreter` built at `.claude/skills/benchmark-interpreter/SKILL.md`, tested on three real metrics, gives a plain-English read with no invented ranges
> - [ ] `/template-filler` built at `.claude/skills/template-filler/SKILL.md`, tested on messy notes, output matches the six-section shape in `ap-reference-diagnostic-report.md`
> - [ ] `/file-reader` built at `.claude/skills/file-reader/SKILL.md`, tested on the sample email and the sample CSV extract, checklist answers pulled out and calculated, chained into `/data-checklist`
> - [ ] You can say, in one sentence, which part of tomorrow's chatbot each of these four skills becomes

---

## Troubleshooting - Session 2

| Issue | Fix |
| --- | --- |
| Claude does not seem to pick up the skill on its own | Open `.claude/skills/[name]/SKILL.md` and check it has a clear `description:` line. That is how Claude knows when to use the skill. |
| `/skill-name` does not load the skill | Check the YAML frontmatter at the top of the file. The `name` field must exactly match the folder name. Then run `/reload-plugins`, or close and reopen Claude Code. |
| I cannot see the `.claude` folder on Windows | It is a hidden folder. Ask Claude: `List the files in my .claude/skills folder so I can see what is there.` You do not need to find it in File Explorer. |
| `/data-checklist` marks something ANSWERED that was only implied | Tell Claude: `You marked [field] as answered but the transcript does not actually say that. Only mark ANSWERED when it is explicit. Fix this in SKILL.md.` |
| `/benchmark-interpreter` gives a verdict for a metric that is not in the benchmark file | Tell Claude: `That metric is not in ap-benchmark-data.csv. The skill should say so, not guess a verdict. Fix SKILL.md so it checks the file first.` |
| `/template-filler` invents a number or a client detail | Tell Claude: `You added something that was not in my notes. The skill says flag missing items in curly braces. Fix the output, and strengthen that rule in SKILL.md.` |
| `/template-filler` output does not match the six headings in the reference report | Point Claude back at the file: `Compare your output section by section against sample-data/ap-reference-diagnostic-report.md and fix any heading that does not match.` |
| The CSV extract calculation looks wrong | Ask Claude to show its working: `Show me exactly which rows you counted and how you got that percentage.` Check the row count against the file. |
| The group is at different stages | Get the current skill working for everyone before moving to the next one. Four working skills matter more than finishing early. |

---

**Next:** [Session 3: Connectors + Daily Brief →](session-3-connectors-brief.md)
