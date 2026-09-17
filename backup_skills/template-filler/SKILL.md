---
name: template-filler
description: Turn structured AP diagnostic notes (checklist answers plus any benchmark reads) into a full diagnostic report, in the exact shape of sample-data/ap-reference-diagnostic-report.md. Use whenever Ram has gathered enough AP data (from a transcript, the data-checklist skill, or the benchmark-interpreter skill) and wants the final client-ready report written up.
---

# Template Filler

Writes the final AP diagnostic report from real notes, in the exact shape of `sample-data/ap-reference-diagnostic-report.md`. It never fills a gap with an invented number or client detail. A gap stays a gap, marked in {curly braces}, so the consultant can go back and close it.

## Input

Structured notes: answered checklist fields (from a transcript or the data-checklist skill), any benchmark reads (from the benchmark-interpreter skill), and anything else gathered in the client conversation. These can be pasted in directly or given as a file path.

If there isn't enough to write a real report (roughly, most of the Metric-tier checklist fields are still missing), say so and ask what else is available rather than writing a report that's mostly placeholders.

## Ground rule, applies to every section below

Never invent a number, a quote, or a client detail that isn't in the notes given. If the report needs something the notes don't have, write `{needs: what's missing}` in that spot instead of guessing. Read `sample-data/ap-reference-diagnostic-report.md` first, since it's the worked example of exactly this rule (it's an invented illustration and says so up front; a real report never gets that disclaimer because every number in it must come from the client).

## Header block

Before section 1, same shape as the reference file:

- Title: `# Accounts Payable Diagnostic: Current State Assessment`
- **Client:** the client name if the notes gave one, otherwise `{client name}`. Never invent one.
- **Process in scope:** Accounts Payable
- **Period reviewed:** from the notes if stated, otherwise `{period reviewed}`
- **Basis:** what the notes actually are, e.g. "Structured interview with the AP Manager" (plus anything else genuinely given, like an aging extract). Don't add sources that weren't part of the input.

## Section 1: Executive summary

Heading: `## 1. Executive summary`

Open with one bolded sentence naming the causal chain, not a list of findings. Most AP problems chain the same way: low PO coverage produces exceptions, exceptions produce cycle time, cycle time produces late payment. Look at the notes for that chain first. If the notes support all four links, open with it. If only some links are present, open with whichever chain the actual numbers support, and don't force the other links in.

Follow with 3 to 5 sentences of plain prose that:
- Walk the chain using the client's real numbers, each one with its unit.
- Name the second-order consequences the chain produces (things like per-FTE productivity, discount capture, payment terms sprawl) only if the notes contain those numbers.
- Close with one line on whether the fix is mostly a system change or mostly a policy and process change, based on what the observations in Section 2 actually show.

If a link in the chain is missing a number, write `{needs: the missing figure}` in its place rather than skipping the sentence or inventing a plausible-sounding one.

## Section 2: As-is observations

Heading: `## 2. As-is observations`

One short intro line: "Each observation carries a paired improvement opportunity. Tags: **Impact** and **Ease** are High / Medium / Low, **Dimension** is Process, People or Technology, **Timeframe** is months to realise."

Then a markdown table with exactly these columns: `ID | Sub-process | As-is observation | Improvement opportunity | Implementation considerations`

- IDs run AP01, AP02, AP03... in the order the findings are written, no gaps or reordering later.
- Every row needs both an as-is observation and its improvement opportunity. Never write one without the other. If the notes give an observation but no plausible improvement is grounded in anything the notes say, don't invent one, leave that finding out of the report entirely rather than half-fill the row.
- The Implementation considerations cell always carries all four tags in this order: `Impact **[High/Medium/Low]** · Ease **[High/Medium/Low]** · [Process/People/Technology] · **[0-3/3-6/6-12/12+ months]**.` followed by one short sentence of reasoning.
- **Ease is about the organisation, not the size of the edit.** A one-line config change owned entirely by AP can be Ease High. A policy change that needs procurement, legal, or cross-functional sign-off is Ease Medium or Low, even if the change itself is small, because getting agreement is the actual work. Judge Ease by how many people outside AP have to agree, not by how many lines of process documentation change.
- Base every observation and every tag on what the notes actually say. If Impact, Ease, Dimension or Timeframe can't be reasonably judged from the notes, write `{needs: more detail to tag this}` in that slot instead of guessing.

## Section 3: Maturity assessment

Heading: `## 3. Maturity assessment`

One line: "Five bands: **Basic · Developing · Defined · Advanced · Leading**"

Table columns: `Sub-process | Current | Target (12 months) | Basis`

One row per sub-process actually covered in the notes (Vendor Master, Invoice Processing, Payments, Reconciliation, Query & Dispute, Compliance, whichever appear). Current band comes from what the notes describe about that sub-process today. Target is a realistic 12-month reach, usually one to two bands up, never a jump that isn't earned by the size of the fix proposed in Section 2. Basis is one line citing the actual detail from the notes that justifies the current band. If a sub-process wasn't covered in the notes at all, leave it out of the table rather than guessing its maturity.

## Section 4: Benchmarking

Heading: `## 4. Benchmarking`

One line: "Client position against global and India peer points. Units stated per row."

Table columns: `Metric | Unit | Global | India | Client | Read`

One row per metric that has both a client value in the notes and a matching row in `sample-data/ap-benchmark-data.csv`. Pull Global, India and the unit straight from that file, don't retype them from memory. The Read column is one plain sentence per metric, in the same voice as the benchmark file's own read column but not copied verbatim, and grounded in the client's actual position (this is exactly what the benchmark-interpreter skill produces, reuse its output if it's in the notes). Never include a metric here that the client didn't actually give a value for.

## Section 5: Opportunities by lever

Heading: `## 5. Opportunities by lever`

Four fixed groups, in this order, using this exact spelling: **Standardisation**, **Optimization**, **Technology**, **People and controls**. These four never change or get renamed.

Under each, bullet the relevant findings by referencing their ID in parentheses, e.g. "No-PO-no-pay policy with a defined exception list (AP01)". Reference the ID, don't restate the full observation text again, that already lives in Section 2. A finding can appear under more than one lever if it genuinely spans both (e.g. a technology fix that's also a process standardisation). Not every lever needs a bullet if nothing in the findings fits it, an empty or thin section is fine, don't stretch a finding to fill a lever it doesn't belong under.

## Section 6: Suggested next steps

Heading: `## 6. Suggested next steps`

A short numbered sequence (2 to 4 phases), each phase a rough timeframe (weeks or quarters) and the finding IDs that belong in it. Sequence by dependency, not by ID order: a quick win that funds the business case or a control gap that's urgent goes early; anything that depends on an earlier fix landing goes later, the way AP05 (terms rationalisation) waits behind AP01-AP03 in the reference report.

End with one line starting **Not established during this interview:** listing, in plain language, the real gaps the notes never covered, the things a real next conversation should ask for. Don't pad this with generic finance-transformation boilerplate, only list what's genuinely missing from these specific notes.

## Tone

Plain, confident, the register a senior consultant uses writing for a CFO. Short sentences. State the finding, then the number, then what it means. No marketing language, no "unlock," "elevate," "robust," "seamless," or similar. No hedging like "it seems" or "this may suggest" when the notes are clear, state it as a fact when the data supports it, and flag it plainly with `{needs: ...}` when it doesn't.
