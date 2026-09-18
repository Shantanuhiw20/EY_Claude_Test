# AP Data Collection Checklist — Prioritised Fix List

Source of the critique: Reviewer read `sample-data/ap-data-collection-template.csv` as a sceptical CFO hearing these questions asked of their AP manager.

---

## MUST FIX

Blocks tomorrow's build: leading, unanswerable as worded, or a gap that would make the diagnostic misleading even with honest answers.

**ap_02** — *"Who can create or amend a vendor master record, and what check sits in front of that?"*
Compound, and the compound half is the one that matters. This is the single fraud-control question in the entire set, and a merged field lets "who can create it" get answered while "is there a check at all" quietly goes unasked. Split it.
- **Fix:** Reword `ap_02` to the access half only: *"Who can create or amend a vendor master record today?"* Add a new field `ap_21` for the control half: *"What check or approval sits in front of a vendor master creation or amendment, if any?"*

**ap_08** — *"What percentage of invoices hit an exception and need manual resolution?"*
Unanswerable as worded. Most AP teams do not track exception rate as a named, reported KPI, and this is the field the entire diagnostic's readiness gate depends on. Asked this way, a manager either guesses a precise-sounding number or stalls.
- **Fix:** Reword to *"Roughly what percentage of invoices hit an exception and need manual resolution? A best estimate is fine if this isn't formally tracked."*

**ap_09** — *"What is the average time in days from invoice receipt to posting?"*
Unanswerable as worded. "Receipt to posting" as a single measured span rarely exists in the system; most clients can only give approval time, which is a component, not the whole cycle.
- **Fix:** Reword to *"Roughly how many days does it typically take from when an invoice is received to when it's fully posted and ready for payment? An estimate, or the approval-time figure if that's what's tracked, is fine."*

**ap_14** — *"What percentage of eligible early-payment discounts do you actually capture?"*
Unanswerable as worded, and not hypothetically: this project's own sample client email has the AP manager saying outright that they don't have this number. Asking for a precise percentage where the honest answer is "I don't know" produces either a guess or a stall.
- **Fix:** Reword to *"Roughly what percentage of eligible early-payment discounts do you actually capture? A rough estimate is fine if this isn't tracked as a formal metric."*

**Missing: Vendor Master — bank-detail change control.**
The checklist covers who can create or amend a vendor record in general, but never asks the one control question that actually protects the client's money: what happens when a vendor's bank account changes. A checklist that is silent here can produce a diagnostic that looks complete and misses the highest-consequence gap in the whole function.
- **Fix:** Add `ap_22`, category Vendor Master: *"When a vendor requests a change to their bank details, what verification happens before the record is updated (for example, a callback to a number already on file)?"*

---

## SHOULD FIX

Worth doing before this pattern is reused for AR, R2R or FP&A tomorrow afternoon. Does not block the AP build.

**ap_07** — *"Do you run a three-way match, and what tolerance is set before an invoice needs manual review?"*
Compound. If the answer to the first half is no, the tolerance half is moot but the field still expects a figure.
- **Fix:** Split into `ap_07` *"Do you run a three-way match on PO-backed invoices?"* (yes/no) and a new field *"If yes, what tolerance is set before an invoice needs manual review?"* (text).

**ap_15** — *"How many approval steps does a payment go through, and do thresholds change who approves?"*
Compound. Step count and threshold logic are two different facts.
- **Fix:** Split into *"How many approval steps does a payment typically go through?"* (number) and *"Do approval thresholds change who signs off, and at what amounts?"* (text).

**ap_16** — *"How large is your GR-IR clearance backlog, and how often is it reviewed?"*
Compound. Size and review cadence are answered with different confidence levels.
- **Fix:** Split into *"How large is your GR-IR clearance backlog today?"* (text/number) and *"How often is the GR-IR balance reviewed?"* (text).

**ap_18** — *"How do you track MSME vendors and the interest exposure on late payment to them?"*
Compound, and the second half is a number most clients have never calculated, which makes it easy to answer only the tracking-method half.
- **Fix:** Split into *"How do you identify and track MSME vendors in your vendor master?"* (text) and *"Has the interest exposure on late payment to MSME vendors ever been calculated? If so, roughly what is it?"* (text).

**Missing: Invoice Processing — exception reason codes.**
`ap_08` produces a rate with no diagnosis attached. Without a reason-code breakdown, every recommendation built on the exception rate is a guess dressed as a finding.
- **Fix:** Add a new field, category Invoice Processing: *"When invoices hit an exception, what are the two or three most common reasons (for example: price variance, quantity variance, missing PO, missing goods receipt)?"* (text)

**Missing: Query & Dispute — what the queries are about.**
The checklist counts vendor queries but never asks what they're about, and query volume is usually a symptom of late payment rather than a cause in its own right.
- **Fix:** Add a new field, category Query & Dispute: *"What are vendor queries typically about (for example: payment status, short payment, missing remittance)?"* (text)

---

## POLISH

The question set works without these.

- Standardise the "roughly / a rough estimate is fine" framing across every field where a client is unlikely to have a precise system-reported number (`ap_01` vendor count, `ap_17` query volume), so the estimate-friendly wording isn't only on the four fields fixed above.
- `ap_05`'s channel-mix question lists options inline ("post, email, EDI, a vendor portal, or a mix"). Fine as worded, but consider moving the option list into `why_it_matters` so the question itself reads cleaner.
- No field anywhere asks for a rupee value on any of the quantifiable costs (discount leakage, exception rework, MSME interest exposure). Even with every other fix applied, the report this checklist feeds will have no cost-of-inaction number for a CFO conversation. Worth a follow-up field once the SHOULD FIX items land, not before.
