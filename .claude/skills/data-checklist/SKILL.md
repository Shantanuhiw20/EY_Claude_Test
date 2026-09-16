---
name: data-checklist
description: Check an AP diagnostic conversation against the 20-field data collection checklist. Marks each field answered or missing, quotes the answer, tracks open follow-ups, and drafts the next priority questions.
---

You are checking a diagnostic conversation against the AP data collection checklist. Your job is to say what has been answered, what is missing, what is still open, and what to ask next.

## Setup

Read the checklist from `sample-data/ap-data-collection-template.csv`. Twenty fields, `ap_01` to `ap_20`, across eight categories:

**Vendor Master · Invoice Processing · Payments · Reconciliation · Query & Dispute · Compliance · Team · Pain point**

Never hardcode the fields. Read them from the CSV, so this works unchanged when the checklist changes.

## Status for each field

Mark one of three, not two:

- **Answered** — you have a usable value, quoted in the client's words
- **Partial** — they addressed it but the answer is not usable yet ("a few days", "most of them")
- **Missing** — not covered

**Partial is the status that earns its keep.** "Roughly 40%" is answered. "Quite a lot are non-PO" is partial, and needs a follow-up rather than a fresh question.

## Output

```
**{field_id}** ({category}) — {ANSWERED / PARTIAL / MISSING}
  Answer: {quoted, in their words}
  Numeric: {the plain number where the field is a number or percentage}
  Follow-up: {what is still open on this field, if anything}
```

Then:

**Coverage:** how many of 20 answered, and across how many of the eight categories.

**Open follow-ups:** every partial answer, and anything the client promised to send.

**Ask next:** the two or three questions that matter most now, with why.

## Choosing what to ask next

Not all gaps are equal. Prioritise on three things:

1. **Spread beats depth.** Six categories covered thinly beats two covered completely. A report needs breadth to say anything about the function.
2. **Some answers unlock others.** Invoice volume and headcount together give invoices per FTE. PO coverage explains the exception rate. Ask the unlocking question first.
3. **A control gap outranks an efficiency number.** If vendor master segregation of duties (`ap_02`) is unanswered, ask it before another metric.

## When there is enough

Roughly ten to twelve solid answers spread across at least six categories. Not all twenty, and never every field for its own sake.

Say so plainly when you get there, and say what is still missing so the gap is a known one rather than a silent one.

## Rules

**Quote, do not paraphrase.** The client's own words carry information your summary loses.

**Capture the number and the words.** "About 5,000 a month, though it spikes at quarter end" gives you 5000 and a finding.

**Never mark a field answered on an assumption.** If they said the ERP is SAP, that does not tell you whether OCR is in use.

**Track what was promised.** "I'll send the aging report" is an open item, and it belongs in the follow-up list until it arrives.

**Do not judge the numbers here.** This skill establishes what is known. Interpretation is `benchmark-interpreter`'s job.
