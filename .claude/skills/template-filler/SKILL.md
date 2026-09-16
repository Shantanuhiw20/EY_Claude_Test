---
name: template-filler
description: Convert checklist answers and benchmark reads into a finished AP diagnostic report. Produces the six-section shape with a tagged finding grid, maturity bands, three-point benchmarking and opportunities grouped by lever.
---

You are turning structured diagnostic notes into a finished report. Inputs: the checklist answers and the benchmark reads. Output: a report matching `sample-data/ap-reference-diagnostic-report.md`, which is the shape to follow exactly.

## The six sections, in order

1. **Executive summary** — lead with the root cause, not a list of findings
2. **As-is observations** — the tagged finding grid
3. **Maturity assessment** — five bands, current and target
4. **Benchmarking** — three-point comparison with units
5. **Opportunities by lever** — grouped, not re-listed
6. **Suggested next steps** — sequenced, with what was not established

## The finding grid

This is the core of the report. Every observation is one row, and every row is complete:

| ID | Sub-process | As-is observation | Improvement opportunity | Implementation considerations |
|---|---|---|---|---|

- **ID** — sequential, `AP01`, `AP02`, and so on. Never skip or reuse a number.
- **Sub-process** — the checklist category the finding came from.
- **As-is observation** — what is true, with the figure and its unit. No adjectives.
- **Improvement opportunity** — paired with that observation. Never an observation without one.
- **Implementation considerations** — the tags, plus one line of real-world constraint.

Every row carries four tags:

- **Impact** High / Medium / Low
- **Ease** High / Medium / Low
- **Dimension** Process / People / Technology
- **Timeframe** 0-3 / 3-6 / 6-12 / 12+ months

Tag honestly. A finding that needs procurement sponsorship is not Ease High just because the change is small. Ease is about the organisation, not the edit.

## Writing the executive summary

**Lead with the causal chain, not a summary of sections.** A diagnostic earns its fee by explaining why, not by restating what the client told you.

Most AP findings chain. Low PO coverage produces exceptions, exceptions produce cycle time, cycle time produces late payment, late payment produces vendor queries and lost discounts. Find that chain and open with it. If the answers do not support a chain, say what is actually there instead of inventing one.

Name the largest lever explicitly, and say whether it needs a system or just a decision.

## Opportunities by lever

Group by **Standardisation · Optimization · Technology · People and controls**. Reference finding IDs rather than restating the findings. If a lever has nothing under it, omit the heading.

## Rules

**Use only figures that came from the conversation or an uploaded file.** Where something was never established, say so plainly in the final section. Never estimate to fill a gap.

**Every number carries its unit.** Days, percentage, count, thousands per annum.

**Quote the client's own words in the observation** where they said something sharper than you would. A verbatim line about the month-end scramble is worth more than your paraphrase.

**Do not soften a control gap.** Segregation of duties, bank-detail changes and duplicate payment exposure are not efficiency findings. Tag them High impact and short timeframe even when the client has not raised them.

**Sequence the next steps.** Some fixes are wasted if done in the wrong order, and saying so is part of the value. Do not renegotiate payment terms on a broken approval process.

**Never name a real client, engagement or company** other than the one being interviewed.
