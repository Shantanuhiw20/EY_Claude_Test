---
name: data-checklist
description: Check an AP diagnostic conversation transcript against the 20-field checklist in sample-data/ap-data-collection-template.csv. Marks each field ANSWERED (with a quote) or MISSING, gives a one-line score out of 20, and drafts the next 2 to 3 questions to ask. Use whenever Ram pastes in an AP client conversation, or points at a transcript file, and wants to know what's covered and what to ask next.
---

# Data Checklist

Checks an AP client conversation against the real checklist, so the next questions asked are the ones that actually move the diagnostic forward, not random gaps.

## Input

The transcript can arrive two ways:
- Pasted directly into the conversation.
- A file path, which should be read with the Read tool.

If neither a transcript nor a file path is given, ask for one before doing anything else.

## Step 1: Load the checklist

Read `sample-data/ap-data-collection-template.csv`. It has 20 fields (ap_01 to ap_20), each with a `field_id`, `category`, `question`, `why_it_matters`, and `data_type`.

## Step 2: Classify each field for priority

Priority order for follow-up questions is Metric fields first, then Process, then everything else. Classify using the file's own `data_type` column, don't guess:

- **Metric**: `data_type` is `number`, `percentage`, or `number (days)`. These are the hard numbers the diagnostic and benchmarking depend on (ap_01, ap_03, ap_04, ap_08, ap_09, ap_10, ap_11, ap_12, ap_13, ap_14, ap_17, ap_19).
- **Process**: `data_type` is `text` or `yes/no`, and the field describes how the process runs (ap_02, ap_05, ap_06, ap_07, ap_15, ap_16, ap_18).
- **Everything else**: the open-ended pain point field (ap_20).

## Step 3: Check the transcript against each field

Go through all 20 fields in order. For each one:

- **ANSWERED**: the transcript clearly covers it. Quote the exact line (or lines) from the transcript that answers it.
- **MISSING**: the transcript does not cover it, or the coverage is too vague to count. Say briefly why (e.g. "not mentioned" or "client said 'it's fine' but gave no number").

Never mark a field ANSWERED on a guess or an inference. If it's unclear whether a line really answers the question, mark it MISSING and say why it fell short. A vague answer without a number or a specific detail does not count as ANSWERED for a Metric or Process field.

Present this as a simple table or list, field by field: field_id, category, status, and the quote or reason.

## Step 4: One-line summary

State it plainly: "X of 20 fields answered."

## Step 5: Draft the next questions

From the MISSING fields, pick the 2 to 3 highest priority ones, using the priority order from Step 2 (Metric first, then Process, then everything else). Within the same priority tier, keep the file's own field order.

Take the question text straight from the `question` column, word for word. Don't rewrite or paraphrase it.

Only ever draft 2 to 3 questions at a time, even if many fields are missing. A real client conversation can't take a wall of questions at once. If Ram wants more after these are asked, run the skill again on the updated transcript.
