[← Back to Student Handbook](student-handbook.md)

---

# Session 1: CLAUDE.md, Then Your First Persona (AP)

**Day 1**

**What you'll build:** two different files, back to back, that do two different jobs. `CLAUDE.md` is your own context: who you are, what you work on, and how you want Claude to work with you. A persona file is a finance job role, sharp enough that a chatbot built from it can run a diagnostic like an EY expert would. Today you build both, in this order: `CLAUDE.md` first (short), then your first persona, Accounts Payable (the real depth).

---

## Why this session has two parts

`CLAUDE.md` stays small on purpose. It is your own context and house rules, the same file no matter which finance process you are building a chatbot for. The actual finance expertise, the thing that makes tomorrow's chatbot sound like someone who has run this diagnostic thirty times, lives in a separate **persona file**. You build your first one today: Accounts Payable.

This is not a formality. Later today you run `/build-persona` once, for AP. In the Hackathon tomorrow afternoon, your team runs the exact same command again for a different process (AR, R2R, FP&A, or a real client engagement), and `CLAUDE.md` does not change at all. That is the whole point of building it this way: one team-context file, as many persona files as you need.

---

## What you will have built by the end of this session

1. A working **`CLAUDE.md`** at the root of your project folder, built by running `/interview-me`. Short, three sections, about you, not about AP.
2. Seen what Claude does when it runs an AP diagnostic with no domain context (the "before").
3. A working **`personas/ap-persona.md`**, built by running `/build-persona`, that captures the AP Finance Persona: how an AP expert thinks, asks, and talks to a client.
4. Seen the difference domain context makes (the "after"). Same client scenario, completely different quality of question.
5. A persona file that references your actual diagnostic data checklist, your actual benchmark data, and the shape of a finished diagnostic report, so Claude is grounded in your real materials, not general knowledge about accounts payable.
6. Understood why this persona file, not the chatbot's code, is what will make or break Day 2.

---

## Getting started

Both parts of this session are interviews: Claude asks, you answer, Claude writes the file. If either one moves too fast, type `slow down` and it goes one question at a time.

You should still be in Claude Code from Session 0. If not, open your terminal and run the same two lines as before:

```
cd thecrux-ey-bootcamp
claude
```

Confirm you can see the `sample-data/` folder before you continue:

```
List the files in sample-data/. Tell me what each one looks like it's for.
```

Among the files listed, look for `ap-data-collection-template.csv`, `ap-benchmark-data.csv`, and `ap-reference-diagnostic-report.md`. (You will also see AR, R2R, and FP&A templates for tomorrow's hackathon, and a `sample-client-inputs/` folder for Session 2. Ignore those for now.) These are sample files, in the shape a real AP diagnostic uses. Everything you build in Part B today points at these three AP files.

---

# Part A: Build CLAUDE.md (your own context)

This part is short on purpose. It is a quick, useful step, not the main event, today's real depth is Part B.

`CLAUDE.md` captures who you are and how you want Claude to work with you: your role, your practice area, what you work on, and the house rules that apply to every build. It is not about AP, AR, R2R, or any finance process. That is deliberate: this file stays the same across every persona you ever build, in the Hackathon and after.

Run:

```
/interview-me
```

Claude takes over from here. It will:

1. State what you are building (a short, personal `CLAUDE.md`, not a finance persona) and check you are ready.
2. Walk you through three sections, one at a time, confirming each before moving on: Who I Am, What I Work On, How I Want Claude To Work.
3. Show you the full file and ask before saving it as `CLAUDE.md` at your project root.

Answer plainly and move fast, keep this part brief. `back`, `skip`, `draft it`, and `slow down` all work here too, more on those shortcuts in Part B.

Once it is saved, Claude will read it back to you and tell you to move on to `/build-persona`. That is your cue for Part B.

> **Part A checkpoint**
> - [ ] `CLAUDE.md` saved at the root of your project folder
> - [ ] It has all three sections, and none of them mention AP, AR, R2R, or FP&A by name
> - [ ] You're happy with it. It will not need to change again this bootcamp.

---

# Part B: Build your first persona (AP), with /build-persona

**This is where almost all of today's real depth goes.**

When you walk into a client, you want Claude to think like a specific kind of finance expert, for that client's problem. So now you build a **persona file**: a system-prompt-style file that describes a **finance job role**, not a person. It captures how that role thinks, what it asks, what good and bad looks like in the numbers, and how it talks to a client.

We are building the **Accounts Payable (AP) expert persona** together, as a group. This is not a warm-up exercise. It is the actual first asset of the AP diagnostic chatbot you build in Day 2. Tomorrow's chatbot has no brain of its own. The file you write today, `personas/ap-persona.md`, is its brain: the system prompt it reads before every client conversation.

One more thing, because you will do this again after the bootcamp: the skill here is building a domain persona. You will later commission or oversee this exact pattern for real client engagements, R2R, AR, FP&A, treasury, whatever the mandate is. Learning to build one well, for AP, is what transfers.

---

## Part B.1: The "Before" - an AP diagnostic with no persona

Before we build anything, see what Claude does when it tries to run an AP diagnostic conversation cold, no domain context.

```
I'm a finance transformation consultant about to run an AP diagnostic
conversation with a client. Ask me the first three questions you'd
ask their AP team.
```

Read what comes back. It will likely be generic and textbook: "how many invoices do you process," "what's your DPO," maybe a stray question that does not connect to anything. It is not wrong, exactly. It is shallow. It does not know what EY's diagnostic actually checks for, what "good" looks like in the numbers, or what a finished report needs to contain. It cannot tell a healthy DPO from a real problem, because it has no benchmark.

Keep this answer in mind. You will ask something close to it again shortly.

---

## Part B.2: The interview - build the AP persona together

Now we fix it. Instead of writing the persona file by hand, run one command. Claude interviews the room, two or three questions at a time, and writes the file from the answers.

```
/build-persona
```

If it asks which process, say "AP" (or "Accounts Payable"). You can also give the argument up front: `/build-persona AP`.

Claude takes over from here. It will:

1. Confirm which process you are building for (AP), check for existing sample data (`ap-data-collection-template.csv`, `ap-benchmark-data.csv`), and tell you you are grounded in real files.
2. Tell you what you are building (an AP persona, not a personal file) and check you are ready.
3. Walk you through seven sections that define the persona, one at a time, confirming each before moving on.
4. Run a quick test near the end: it makes the persona open a real diagnostic conversation, and you score whether it sounds like an EY finance transformation consultant or a generic chatbot.
5. Save the finished file as `personas/ap-persona.md`.

### The seven sections it covers

| # | Section | What it captures |
|---|---|---|
| 1 | Role Definition | What kind of expert this persona is, what it owns, what it is not |
| 2 | The AP Process, End to End | The real steps from invoice receipt to payment, where the friction usually sits |
| 3 | Vocabulary and Volumetrics | The terms and typical ranges: DPO, PO-backed ratio, exception rate, and so on |
| 4 | The Diagnostic Data Checklist | What a good diagnostic needs to ask, grounded in `ap-data-collection-template.csv` |
| 5 | Benchmarks It Must Know | Leading practice, average, lagging practice, grounded in `ap-benchmark-data.csv` |
| 6 | How It Talks To A Client | Tone, pacing, what it never does (never invents a number, never stops probing too early) |
| 7 | What "Done" Looks Like | The shape of a finished diagnostic report, grounded in `ap-reference-diagnostic-report.md` |

Answer as a group. Whoever has run the most AP engagements should carry sections 2, 3, and 4. Whoever cares most about client experience should carry section 6.

---

### How to give good answers

**Ground every answer in the sample data, not memory.** If you are not sure whether a term belongs in Section 3, open `ap-data-collection-template.csv` or `ap-benchmark-data.csv` and check.

Weak: "AP diagnostics look at invoice volume and payment speed."

Sharp: "We look at sixteen data points across volume, process, technology, and four hard metrics: DPO, on-time payment rate, exception rate, and early discount capture. Exception rate above 15% is the single strongest predictor we've seen of hidden rework cost."

**Be precise about what the persona must never do.**

Weak: "It should sound professional."

Sharp: "It never states a DPO or exception rate as good or bad without comparing it to the benchmark file. If a client hasn't given a number yet, it asks again, it does not estimate one to keep the conversation moving."

---

### Shortcuts you can use mid-interview

| Say | What it does |
|---|---|
| `back` | Go back to the previous section and revise it. |
| `skip` | Leave a field as a TODO, come back to it later. |
| `example` | Ask for one more AP-grounded example for the current question. |
| `draft it` | Have Claude propose an answer from what has already been said, guesses marked in `{curly braces}`. |
| `slow down` | Switch to one question at a time for the rest of the interview. |
| `stop` | End the interview, save a partial file with TODOs for what's unfilled. |

The file saves once, at the very end, on a clear yes. If your laptop sleeps or the session gets interrupted before that, run `/build-persona` again: if you had already saved `personas/ap-persona.md` once, it finds that file and offers to resume and refine it; if you had not saved yet, you start the interview again, which goes quickly since you already thought it through once.

---

## Part B.3: The "After" - see the difference

Persona file saved. Type `/clear` so you start from a clean conversation, then read the persona in:

```
Read personas/ap-persona.md. I'm about to run an AP diagnostic
conversation with a client. Ask me the first three questions you'd
ask their AP team, and tell me what you'd want to hear back before
you'd feel confident in a diagnosis.
```

Compare the two answers. The second one should ask about PO-backed ratio, not just volume. It should ask about approval chain length, because that's where cycle time usually hides. It should reference what "good" looks like, not just ask a number and move on.

**That is the point of the persona file.** Claude did not get smarter about accounts payable. You gave it the shape of the expertise already in the room.

---

## A worked example - the interview in progress

This is an excerpt from a live run of `/build-persona`, partway through Section 2. Use it as a shape to aim for, not a script.

```
Claude: What does the AP process actually look like end to end, from
        when an invoice lands to when it's paid?
        e.g. invoice arrives (PO or non-PO) -> matched -> routed for
        approval -> posted -> paid on terms -> discount window
        captured or missed

You:    Invoice comes in, usually a PDF by email, sometimes scanned
        and OCR'd. If it has a PO, the system tries a three-way
        match against the PO and the goods receipt. If it doesn't,
        someone keys it in by hand and it becomes an exception by
        default. Above a threshold, usually somewhere around fifty
        to two hundred thousand depending on the client, it needs
        sign-off from both a cost centre head and a finance
        controller. That second approver is almost always the
        bottleneck, they're senior and often travelling.

Claude: Got it. Section 2 so far:
        - Invoice intake: email PDF or OCR scan
        - PO invoices: three-way match against PO and goods receipt
        - Non-PO invoices: manual entry, automatic exception
        - Approval: two-step above a client-set threshold, second
          approver (finance controller) is the typical bottleneck
        Looks right? (yes / edit / skip to next section)

You:    Yes, add that the approval bottleneck is almost always
        the same shape across clients, not a one-off.

Claude: Updated. Section 2 of 7 done. Five sections left.
```

Notice what makes this sharp: real process steps, a named bottleneck, and a pattern claim ("almost always the same shape across clients") that a generic AI would never volunteer. That last line is exactly the kind of tacit expertise this file exists to capture.

---

## A worked example - what a finished AP persona looks like

```markdown
# AP Persona - Accounts Payable Diagnostic Expert

## 1. Role Definition
- This persona is an EY finance transformation consultant's AP diagnostic
  brain: it runs the first structured conversation with a client's AP team
  and produces a benchmarked diagnostic.
- It draws on pattern recognition from dozens of client AP engagements
  across process design, not just one client's numbers.
- It is not a general accounts-payable encyclopedia. It does not answer
  "what is an invoice." It runs a diagnostic, end to end.
- It never invents a client number. If a number is missing, it asks.

## 2. The AP Process, End to End
- Invoice intake: email PDF, OCR-scanned, or EDI/system feed.
- PO-backed invoices go through a three-way match (PO, goods receipt,
  invoice). Non-PO invoices are manually keyed and become the default
  source of exceptions.
- Approval routing typically has 2 to 5 steps depending on invoice value.
  The most senior approver in the chain is almost always the bottleneck.
- Posting, then payment on agreed terms, then an early-payment discount
  window that is either captured or missed depending on how fast the
  invoice cleared approval.
- The friction almost never shows up in DPO. It shows up in exception
  rate and invoice-to-posting cycle time. DPO can look healthy while the
  underlying process is not.

## 3. Vocabulary and Volumetrics
- DPO (Days Payable Outstanding), PO-backed ratio, exception rate,
  early-payment discount capture, invoice-to-posting cycle time,
  three-way match, OCR/automated capture adoption, AP FTE per 10,000
  invoices/month.
- Typical monthly volume for a mid-size client: a few thousand to
  low tens of thousands of invoices. Team size: single digits to
  low twenties of FTEs.

## 4. The Diagnostic Data Checklist
- Full field list lives in sample-data/ap-data-collection-template.csv.
  16 fields across Volume, Process, Technology, Metric, Team, Vendor,
  Pain point, and Culture.
- Always ask the open pain-point question ("what's the single biggest
  bottleneck in your own words") before the quantitative fields. It
  surfaces what a form alone would miss.
- Never mark the checklist complete with a metric field blank. Ask again,
  do not estimate.

## 5. Benchmarks It Must Know
- Full bands live in sample-data/ap-benchmark-data.csv.
- DPO: leading 45-60 days, average 30-45, lagging under 30 or over 75.
- On-time payment rate: leading >95%, average 85-95%, lagging <85%.
- Exception rate: leading <5%, average 5-15%, lagging >15%. The single
  strongest predictor of hidden rework cost.
- Early discount capture: leading >80%, average 50-80%, lagging <50%.
- Never state a client number is good or bad without naming which band
  it falls in and why.

## 6. How It Talks To A Client
- Warm, precise, curious. Asks one clear question at a time, not a wall
  of questions.
- Keeps asking until it has enough to diagnose, does not settle for a
  vague answer on a metric field.
- Treats the open pain-point answer as data, not small talk.
- Never invents a number, a client name, or an incident. If unsure,
  it says so and asks.

## 7. What "Done" Looks Like
- Structure mirrors sample-data/ap-reference-diagnostic-report.md:
  Executive Summary, What We Heard, Volumetric Summary (table against
  benchmark), Diagnosis, Opportunities, Suggested Next Steps.
- The diagnosis section always explains the pattern behind the numbers,
  not just restates them. A high exception rate with a healthy DPO is a
  process story, not a cash story, and the report should say so plainly.
```

Notice what makes this good: it names files it must ground itself in rather than trusting its own memory, it states the persona's boundaries plainly, and Section 2 includes a pattern claim a generic AI would never know on its own. That is the bar for your version.

---

## Why this matters for tomorrow

```
Read the AP persona back to me from personas/ap-persona.md. Then
imagine you're deployed as the AP diagnostic chatbot tomorrow. Walk
me through how you'd open a conversation with a client's AP manager,
and what you'd do differently than you did in Part B.1 of this
session.
```

Tomorrow's chatbot loads this file as its system prompt. When it asks about PO-backed ratio unprompted, that is this file. When it refuses to call a DPO of 38 days a problem without checking the benchmark, that is this file. When its final report has the same six sections as the reference report, that is this file.

**The sharper this file, the sharper the chatbot you deploy tomorrow.**

---

## What makes a great AP persona

| Level | What it looks like | What Claude does with it |
|---|---|---|
| **Weak** | "AP handles vendor invoices and payments." | Generic textbook AP knowledge. Could describe any company. |
| **Better** | "AP diagnostics look at DPO, exception rate, and discount capture against a benchmark." | Grounded in the right metrics, still thin on process detail. |
| **Great** | Same, plus: the real process steps, the named bottleneck pattern, exact benchmark bands, and a clear rule that it never states a number is good or bad without the benchmark. | A diagnostic conversation that sounds like it was run by someone who has done this thirty times. |

---

## If you finish early

### Stress-test it with a real scenario

```
Using only what's in personas/ap-persona.md, run the opening of a
diagnostic conversation with a client whose AP manager just said:
"we process about 8,400 invoices a month and honestly I think we're
missing a lot of early payment discounts, but I don't have a number
for you." What do you ask next, and why?
```

If the response feels generic, the section it pulled from is too thin. Go back and sharpen it.

### Have Claude critique its own persona

```
Read personas/ap-persona.md. Tell me three things that are still too
generic, and three places where the room gave you real expertise that
will make tomorrow's chatbot sharper. Be honest.
```

### Add a second lens

```
Add one line to Section 6 (How It Talks To A Client) of
personas/ap-persona.md: how should this persona handle a client who
is defensive about a bad exception rate? Keep it in the persona's
voice, not a script.
```

---

> **Checkpoint: your AP persona is ready**
>
> - [ ] `CLAUDE.md` saved at the root of your project folder (Part A)
> - [ ] `personas/ap-persona.md` saved (Part B)
> - [ ] It has all seven sections: Role Definition, the AP process, vocabulary and volumetrics, the data checklist, the benchmarks, how it talks to a client, and what "done" looks like
> - [ ] Sections 4, 5, and 7 explicitly reference the three sample-data files, not just general knowledge
> - [ ] You ran the before/after test and saw the difference
> - [ ] The room agrees this sounds like an EY finance transformation consultant, not a generic AP explainer

---

## Troubleshooting - Session 1

| Issue | Fix |
|---|---|
| `/interview-me` not recognised | Make sure Claude Code was started from inside `thecrux-ey-bootcamp`. The command lives in `.claude/commands/`. Restart Claude Code from the correct folder. |
| `/build-persona` not recognised | Same fix as above, it lives in the same `.claude/commands/` folder. Restart Claude Code from inside `thecrux-ey-bootcamp`. |
| `CLAUDE.md` has a finance process mentioned in it | That's a Part A/Part B mix-up. `CLAUDE.md` should only have the three personal-context sections. Move anything AP-specific into `personas/ap-persona.md` instead. |
| The before/after test shows no real difference | `personas/ap-persona.md` is too thin. Add the actual benchmark bands and the named bottleneck pattern, not just "we look at metrics." |
| Claude keeps giving textbook AP answers | Your answers in Sections 2 to 4 of `/build-persona` were too general. Reopen `ap-data-collection-template.csv` and `ap-benchmark-data.csv` and pull specific numbers and fields into your answers. |
| Claude states a metric is "good" without checking the benchmark | Section 5 or Section 6 of `personas/ap-persona.md` is missing the explicit rule. Add: "never call a number good or bad without naming the benchmark band." |
| The persona invents a number that wasn't given | This is a rule violation, not a one-off bug. Fix Section 1 or Section 6 of `personas/ap-persona.md` to state plainly: "never invent a client number, ask again instead." Re-run the test. |
| Claude is asking too many questions at once | Type `slow down`. It will go one question at a time, in either `/interview-me` or `/build-persona`. |
| The session crashed or the laptop slept midway | Run the same command again (`/interview-me` for `CLAUDE.md`, `/build-persona` for the AP persona). If you'd already saved once, it finds the file and offers to resume; if not, you start that interview again. |
| Groups disagree on an answer (e.g. approval threshold) | Capture the range, not a false single number: "typically 50,000 to 200,000 depending on the client." That is more useful than a made-up average. |

---

**Next:** [Session 2: Skills - the diagnostic toolkit →](session-2-skills.md), the Skills that make this persona operational (data-checklist, benchmark-interpreter, template-filler, file-reader).
