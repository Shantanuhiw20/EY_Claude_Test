---
name: file-reader
description: Check one or more uploaded files (an email, a screenshot, a CSV or spreadsheet extract) against the 20-field checklist in sample-data/ap-data-collection-template.csv. Answers each field directly from a file when it's stated outright, calculates it with visible working when it has to be derived from raw rows, and marks it MISSING otherwise. Use whenever Ram uploads AP source material and wants to know what it actually tells us.
---

# File Reader

Reads real source files, an email, a screenshot, a CSV extract, whatever's given, and checks them against the same 20-field checklist the diagnostic conversation itself uses. The point is to pull out only what the files actually say or actually let you calculate, nothing else.

## Input

One or more files: could be an email (text), a screenshot (image), or a CSV/spreadsheet extract (tabular data). Read each one with the Read tool before doing anything else. If no file or file path is given, ask for one.

## Step 1: Load the checklist

Read `sample-data/ap-data-collection-template.csv`. It has 20 fields (ap_01 to ap_20), each with a `field_id`, `category`, `question`, `why_it_matters`, and `data_type`.

## Step 2: Read every file fully before checking anything

Read each uploaded file in full. For a CSV or spreadsheet extract, note the column headers and the row count, since both matter for the calculations in Step 4. For a screenshot, describe what's actually visible (a dashboard number, a table, an email body) before using it. For an email, read the whole thread, not just the first message.

## Step 3: Check each of the 20 fields

Go through all 20 fields in checklist order. For each one, it lands in one of three buckets:

**Directly answered.** A file states the answer outright (a number, a policy, a yes/no). Quote the exact line, and say which file it came from if more than one was given.

**Answered by calculation.** The field's answer isn't stated directly, but the raw data needed to compute it is in a CSV or spreadsheet extract (for example: PO-backed % needs a PO-number column, exception rate needs an exception or status flag, average cycle time needs receipt and posting dates, on-time % needs a due date and a payment date, distinct payment terms needs a terms column). See Step 4 for how to do this and show the work.

**MISSING.** Nothing in any of the uploaded files states or lets you calculate the answer. Say so plainly. Never guess, and never fill the gap with a number that "seems about right" from the file's general shape.

If two files disagree on the same field, for example an email says one invoice volume and a CSV extract implies another, don't silently pick one. Report both, say which file each came from, and flag the conflict instead of resolving it yourself.

## Step 4: Show the arithmetic for every calculated field

Never state a calculated number without the working behind it, in this shape:

1. The row count or record count involved (e.g. "312 rows in the extract").
2. The filter or condition applied (e.g. "rows where the PO Number column is non-blank").
3. The count that condition produced (e.g. "187 rows match").
4. The formula and result (e.g. "187 / 312 = 60%").

A consultant reading the output should be able to re-run the count themselves and get the same row numbers. If the file doesn't have enough columns to support a calculation the checklist wants (for example, no due-date column, so on-time % can't be computed even though payment dates exist), say exactly what's missing rather than approximating or using a different field as a stand-in.

## Step 5: Summarise

One line: "X of 20 fields answered (Y directly, Z by calculation), W missing."

List the MISSING fields plainly, since those are exactly what the next client conversation still needs to cover.

## Output shape

For each field, in checklist order:

- **field_id, category, question** (short form is fine, e.g. "ap_04, Invoice Processing")
- **Status:** Directly answered / Calculated / MISSING
- **Answer:** the value, with its unit
- **Source:** the quote (direct) or the full working (calculated), naming the file it came from
- For MISSING fields, a one-line reason is optional but the field must still be listed, not silently dropped

Close with the Step 5 summary line and the MISSING list.
