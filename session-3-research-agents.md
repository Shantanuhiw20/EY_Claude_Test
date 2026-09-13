[← Back to Student Handbook](student-handbook.md)

---

# Session 3: Agents

**Day 1, last session of the day**

**What you'll build:** one agent, the Reviewer, at `.claude/agents/reviewer.md`. It gets its first real job today: sharpening the sixteen diagnostic questions before anyone builds them into a live tool. Tomorrow's app is built directly from the persona and the four Session 2 skills, no extra agent layer needed for that. The Reviewer comes back once, at the end of tomorrow's build, to give the generated report a last look.

---

## What an agent is (just read)

### One Claude, until now

Everything you have done today happens in one conversation. `CLAUDE.md` gave Claude your context. The persona gave it AP expertise. The four skills gave it four jobs it can do on command. It is still one Claude, in one conversation, with one working memory.

An **agent** is a separate Claude that the Claude in your chat can hand work to. It has its own name, its own instructions, and its own working memory. You give it a job, it does the work on its own, and it comes back with the result.

### The picture to hold

The Claude in your chat window is the **chief of staff**. Agents are the **specialists** it calls on: an interviewer, an analyst, a reviewer. Each works at its own desk, with its own notes. What lands back with the chief of staff is the finished piece of work, not the pile of reading it took to get there.

### Why this matters

Three reasons. Each one shows up in tomorrow's build.

**1. A specialist with strict rules beats a generalist.** An agent with one narrow job and rules it never breaks gives sharper output than all-purpose Claude. "Read this report the way a CFO would, never invent a number, end by naming the single biggest gap" produces something a polite general assistant never will. The rules are what make it a specialist.

**2. Several can run at once.** Hand four agents four jobs and all four work at the same time. You keep talking to the chief of staff while they do.

**3. The heavy reading happens somewhere else.** Every conversation has a working memory, the context window. It is big, not infinite, and a conversation stuffed with raw files gets slow and forgetful (that is what `/context` and `/compact` from Session 0 are for). An agent reads the ten-row CSV or the long reference report inside its own context window. Only the conclusion comes back to yours. Your main conversation stays clean.

### CLAUDE.md, skill, agent

| | CLAUDE.md | Skill | Agent |
| --- | --- | --- | --- |
| What it is | Standing context: who you are and your house rules | A recipe for one task | A specialist with its own identity and rules |
| When it is active | Every session, on its own | When you call it, or when its description fits the task | When the chief of staff hands it work |
| Where it works | Inside your conversation | Inside your conversation | In its own separate conversation |
| Best for | Facts, conventions, house rules | Repeatable procedures | A fixed point of view, heavy reading, several jobs at once |

The one-line version: **CLAUDE.md is what Claude always knows about you. A skill is a checklist you hand Claude. An agent is a colleague you hire.**

The persona file is a fourth thing: the domain knowledge. Tomorrow the app's system prompt reads the persona the way a new joiner reads the practice's method before a first client call.

### What an agent looks like on disk

An agent is a small text file at `.claude/agents/[name].md`. Put it at `~/.claude/agents/[name].md` instead and it follows you into every project.

```
---
name: reviewer
description: Use when a draft diagnostic report needs a hard read before it goes to a client. Finds weak claims, numbers the evidence does not support, and what a CFO would push back on.
---

You are the Reviewer. [Who it is, the rules it never breaks, how every response ends.]
```

Three parts:

- **name**: the handle. How you and Claude refer to it.
- **description**: the job posting. Claude reads this line to decide when to hand this agent work, even when you did not ask. A vague description means it never gets picked.
- **body**: everything after the second `---`. The identity: the job, the point of view, the never-rules, the fixed ending.

You will not write this file by hand. You describe the specialist in plain English and Claude writes the file. That is the rest of this session.

### Three ways to put an agent to work

1. **Plain English:** `Have my reviewer agent read sample-data/ap-reference-diagnostic-report.md.`
2. **@-mention:** type `@`, start typing the agent's name, and pick it from the list that appears. Agents show there as `reviewer (agent)`.
3. **Automatic:** if the description is clear, Claude hands the agent work on its own when a task fits. You ask for a hard read of a report, and the Reviewer turns up uninvited.

### Feel it now (type this)

Agents do not even need files. Describe roles inside one prompt and Claude spins up throwaway agents for that one task. Paste this:

```
Act as three independent agents and run them in parallel. Each one
reacts to question ap_09 in sample-data/ap-data-collection-template.csv:
"What percentage of invoices have an exception (mismatch, missing PO,
duplicate) needing manual resolution?"

- The AP Manager: answers the way a real AP manager would on a first
  call. Vague on the number, certain about the causes.
- The Sceptical Controller: has sat through diagnostic interviews
  before. Says what is wrong with how the question is worded and what
  they would need clarified before giving a real number.
- The Operator: ignores the wording. Says where this number actually
  lives in a client's systems and how long it takes to pull.

Each agent writes its own take in its own voice, five lines at most.
Then tell me in two lines where they disagree.

Run all three on the Haiku model.
```

That last line is deliberate. Three agents at once use more of your budget than one conversation does, and this is a taste, not real work. Haiku is the fast, cheap model, plenty for this. Drop the line when the answer has to be good.

Three distinct voices from one message. When the task ends, these agents are gone. Nothing was saved. Files are what make an agent permanent: a specialist you keep, edit, and reuse. Build one now.

---

## Build the first one together: the Reviewer

Everyone builds the same agent first, because building it teaches the moves. It is also the agent you will use most after this bootcamp: one that reads a draft diagnostic and tells you, before a client does, what is weak.

### Step 1: Describe it, Claude builds it

Paste this as written:

```
Create a reviewer agent in this project, at .claude/agents/reviewer.md.

Its job: read a draft AP diagnostic report and argue what is weak, what
the numbers do not actually support, and what a real CFO would push
back on in the first five minutes.

It reads like a sceptical CFO, not a colleague trying to be helpful.

It never invents a number. It never softens a finding to be polite. It
never rewrites the report, it critiques it.

Every response ends with this exact line:
"The single biggest gap: [one sentence]"
```

Those lines are four answers, typed out.

| The question every agent answers | The Reviewer's answer |
| --- | --- |
| **The job.** What one thing does it do? | Reads a draft report and argues what is weak |
| **The lens.** What point of view does it always bring? | A sceptical CFO in the first five minutes |
| **The never-rules.** What does it never do? | Invent a number, soften a finding, rewrite the report |
| **The ending.** How does every response close? | "The single biggest gap: ..." |

Every agent you build today answers those same four questions. Being good at writing prompts has nothing to do with it. Being precise about the rules does. Ask for "a reviewer agent" and nothing more, and Claude hands back something balanced and reasonable that will tell you the report is fine. Useful. Not a Reviewer. **The rules are the agent.**

> **Give it a few seconds, then check the `@` list.** Claude Code watches `.claude/agents/` and picks up new files on its own. One exception: if that folder did not exist when you started this session, which is true the first time you create an agent in a project, Claude Code will not see it until you restart. If `reviewer` is not in the `@` list, type `/exit`, then start Claude Code again with `claude`. Nothing is lost. `CLAUDE.md`, the persona, and the skills all reload on their own.

### Step 2: Open the file and read it

```
Show me .claude/agents/reviewer.md
```

Yours will be longer than the version below and worded differently. Claude usually adds rules of its own. What matters is that the same bones are there:

```
---
name: reviewer
description: Use when a draft diagnostic report needs a hard read before it goes to a client. Finds weak claims, numbers the evidence does not support, and what a CFO would push back on.
---

You are the Reviewer. You read draft AP diagnostic reports the way a
sceptical CFO reads them in the first five minutes of a meeting.

Rules you never break:
- Never invent a number. Quote the report's own figures back at it.
- Never soften a finding. "This is a good start" is banned.
- Never rewrite the report. Name what is wrong and why, then stop.
- For every claim you challenge, quote the exact line you are challenging.
- End every response with: "The single biggest gap: [one sentence]"
```

Match it to the three parts. The **name** is the handle. The **description** is why Claude will send it work on its own. The **body** is the character, and what makes it work is the rules it never breaks and the fixed closing line.

Claude may add a `tools:` or `model:` line under the description. Those control what the agent may touch and which model runs it. Leave them for now.

### Step 3: Test it

Use the reference report as the draft:

```
Have my reviewer agent read sample-data/ap-reference-diagnostic-report.md
as if it were a draft about to go to the client's CFO.
```

Watch what happens. The chief of staff hands the file to the Reviewer, the Reviewer reads it in its own context, and the critique comes back. Read it against the report. A good Reviewer notices things like these: the Executive Summary says "meaningful hidden cost" and never puts a rupee figure on it; the report says 68% of discounts are missed and never says what that is worth; the Diagnosis blames non-PO invoices for the exception rate without a number that links the two. If your Reviewer said the report was solid with a few minor points, it is too polite. Step 4 fixes that.

Then the second way in: type `@`, start typing `rev`, pick `reviewer (agent)` from the list, and give it the same file. That list is also your proof the agent is installed.

### Step 4: Sharpen it once

Agents are files. You improve them the same way you improved the persona and the skills: say what to change.

```
Edit my reviewer agent. Add two rules: every challenge must quote the
exact line from the report it is challenging, and every response must
name at least one number the report needed and did not have.
```

Run the test again. The critique gets more specific and harder to wave away. **Describe, test, sharpen. That loop is how every agent gets good.**

---

## The Reviewer's first real job: the sixteen questions

The sixteen questions in `sample-data/ap-data-collection-template.csv` shipped with the repo as sample data. In Session 2 you built `/data-checklist` on them without ever challenging them. They read fine on a screen. That is the problem. A question can look reasonable and still be leading, or two questions in one, or something no AP manager could answer with a real number. Tomorrow morning the app starts asking them of a live "client". This is the last checkpoint before that.

### Step 1: The critique

```
Have my reviewer agent read sample-data/ap-data-collection-template.csv,
the sixteen questions (ap_01 to ap_16) the app will ask a client
tomorrow. This time the draft is a question set, not a report.
Same lens: a sceptical CFO hearing these asked of their AP manager.

For each question it challenges, quote the exact question text and say
which of these it is:
- LEADING: the wording assumes an answer or fishes for one.
- COMPOUND: two questions wearing one field_id.
- UNANSWERABLE AS WORDED: a real AP manager would guess a number rather
  than know one, or the data_type does not fit the question.
Questions that are fine, it leaves alone.

Then name any category (Volume, Process, Technology, Metric, Team,
Vendor, Pain point, Culture) that is missing a question a real AP
diagnostic would need, and say what that question would be.

End the usual way: the single biggest gap, this time in the question set.
```

Read it with your own client conversations in mind. You know what a real AP manager can answer on a first call better than the Reviewer does. Where it is wrong, say so.

### Step 2: The fix list

```
Turn that critique into one prioritised fix list and save it to
dry-run/question-fixes.md. Three levels:

MUST FIX: blocks tomorrow's build. A question that is leading,
  unanswerable as worded, or a gap that would make the diagnostic
  misleading even with honest answers.
SHOULD FIX: worth doing before this pattern is reused for AR, R2R, or
  FP&A tomorrow afternoon. Does not block the AP build.
POLISH: the question set works without it.

Each item names the field_id and the exact fix: the new question text,
a split into two field_ids, or a data_type change. Not just the
problem. Keep MUST FIX short. If everything is must-fix, nothing is.
```

### Step 3: Save the sharpened version

Do not edit the original. Other sessions read its exact column headers. Save the sharpened set alongside it.

```
Read dry-run/question-fixes.md. Create
sample-data/ap-data-collection-template-v2.csv with the same five
columns as the original and only the MUST FIX changes applied. For
every field_id you change, show me the before and after here before
you save. Leave sample-data/ap-data-collection-template.csv untouched.

Wrap every field containing a comma in double quotes. Question text and
why_it_matters text very often contain one, and an unquoted comma shifts
every later column in that row. Before you save, check each row has the
same number of columns as the header.
```

Look at each before and after. Keep, reject, or adjust. This is your call, not the Reviewer's. Tomorrow's build looks for `ap-data-collection-template-v2.csv` and uses it if it exists, and falls back to the original if it does not.

---

## Bridge to Day 2

You have one file in `.claude/agents/`, `reviewer.md`, and a sharpened question set. Tomorrow the app is built directly from the persona and the four Session 2 skills, no extra agent layer needed for that. The Reviewer comes back once, at the end, to give the generated report a last look before it's shown. Same project folder, same `CLAUDE.md`. Nothing you built today gets rebuilt tomorrow, it gets wired in.

---

> **Checkpoint: Session 3 complete**
>
> - [ ] You can say in one sentence each what `CLAUDE.md`, a skill, and an agent are for
> - [ ] Ran the throwaway panel and saw three distinct voices from one prompt
> - [ ] `reviewer` built, tested on the reference report, sharpened once
> - [ ] `reviewer` appears when you type `@`
> - [ ] `sample-data/ap-data-collection-template-v2.csv` exists, and you personally reviewed every changed question

---

## Troubleshooting - Session 3

| Issue | Fix |
| --- | --- |
| The `reviewer` agent you just created is not in the `@` list | Wait a few seconds and type `@` again, new files normally load on their own. Still missing? The `.claude/agents/` folder probably did not exist when the session started. Type `/exit`, then `claude`. |
| Not sure the agent exists | Ask Claude: `List my agents and what each one is for.` |
| The Reviewer sounds like a generic assistant, not a specialist | Its rules are too generic. Open the file and add two or three rules that make it distinctive. "A reviewer" is weak. "A reviewer who quotes the exact line it is challenging and never uses the word 'consider'" is a specialist. |
| The Reviewer invents a number | That is a rule violation, not a one-off. Tell Claude: `The reviewer agent produced a number that was not in its input. Strengthen the never-invent rule in its file and re-run.` Check its `description` too: a vague description gets a vague agent. |
| The Reviewer is too polite | Tell Claude: `Edit my reviewer agent. Ban 'good start', 'consider', 'might want to', and any compliment. Every point must quote the line it is challenging.` |
| Claude never hands the Reviewer work on its own | The `description` line is too vague. Rewrite it to say exactly when the agent should be used. |
| Everything in the fix list is MUST FIX | Tell Claude: `MUST FIX means it blocks tomorrow's build. Downgrade anything important but not blocking to SHOULD FIX.` |
| The room disagrees with a proposed question fix | This is a judgement call for the people in the room. Pick the wording closest to a real client conversation and move on. You can revisit in the hackathon. |
| `dry-run/` already has files from an earlier run | Fine to overwrite. Tell Claude: `Overwrite the existing files in dry-run/ with a fresh run.` |

---

**Next:** [Session 4: The Guided Build →](session-4-builds.md)
