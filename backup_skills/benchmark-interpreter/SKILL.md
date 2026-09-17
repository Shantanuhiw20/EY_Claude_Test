---
name: benchmark-interpreter
description: Look up a single AP metric name and value against sample-data/ap-benchmark-data.csv and return the global point, the India point, the client's value, its position versus India, and a plain-English read. Use whenever Ram gives a metric and a value, like "Days Payable Outstanding = 38 days" or "exception rate is 12%", and wants to know where that number stands.
---

# Benchmark Interpreter

Turns one raw client number into a client-ready comparison, grounded only in `sample-data/ap-benchmark-data.csv`. Never invents a benchmark or a read.

## Input

The metric name and value can arrive in any everyday phrasing, e.g. "DPO is 38 days" or "Days Payable Outstanding = 38 days". If either the metric or the value is missing, ask for it before doing anything else.

## Step 1: Match the metric

Read `sample-data/ap-benchmark-data.csv`. Match the given metric name against its `metric` column, allowing for obvious synonyms and abbreviations (e.g. "DPO" = "Days Payable Outstanding", "exception rate" = "Invoice exception rate", "PO-backed" = "PO-backed invoices").

If nothing in the file matches, even loosely, say so plainly: name the metric that was given, state that it is not in the benchmark file, and stop there. Do not guess at a number or substitute a close-sounding metric without saying that's what you're doing.

## Step 2: Check if this metric needs a partner metric to mean anything

Some metrics in this file are explicitly flagged, in their own `read` column, as unsafe to interpret alone. Three pairings are known from the file and from house rules:

- **Invoices processed per AP FTE** needs the **PO-backed invoices %**. The file's own read says to compare only after normalising for non-PO mix.
- **Invoice exception rate** needs the **PO-backed invoices %**. A high exception rate on mostly non-PO spend is expected; the same rate on mostly PO-backed spend is a real problem.
- **Days Payable Outstanding** needs **Paid on time against terms %**. The file's own read says never read DPO on its own: high DPO with low paid-on-time means late payment, not working capital management.

If the requested metric is one of these three and the partner metric's value was not also given, still show the three comparison points for the requested metric, but replace the plain-English read with: "This needs [partner metric] to interpret. A number on its own here can be misleading." Name the specific partner metric needed.

If the partner value was given alongside the original request, use both to write a real read instead of the fallback line.

## Step 3: Report the three comparison points

Always state:
- Global benchmark, with its unit.
- India benchmark, with its unit.
- The client's value, with its unit (use the same unit as the file; if the client gave a number without a unit, ask for it rather than assuming one).

Never state any of these three numbers without its unit next to it.

## Step 4: Position versus India, stated as a fact

Compare the client's value to the India benchmark only (not global), as a neutral factual position, not a judgment:

- Within 10% of the India value (relative difference) → "near the India point."
- More than 10% above the India value → "ahead of the India point."
- More than 10% below the India value → "behind the India point."

This is a plain numerical position. Whether "ahead" or "behind" is good or bad depends on the metric, and that judgment belongs in Step 5, not here.

## Step 5: One line, plain English

Using the file's `read` column as background context, not copied verbatim, write one plain sentence on what this client's position actually means for their AP operation. No jargon. Assume the reader is on a client's finance team, not a data analyst, so avoid terms like "normalise," "variance," or "percentile." If Step 2 found the metric needs a partner that wasn't given, use the fallback line from Step 2 instead of inventing a read.

## Output shape

1. Global: [value] [unit]
2. India: [value] [unit]
3. Client: [value] [unit]
4. Position: [ahead of / near / behind] the India point
5. What this means: [one plain-English sentence, or the "needs X to interpret" line]
