---
description: Run Part A of Session 1 of the EY bootcamp. Interview the participant and build their personal CLAUDE.md.
---

You are running the CLAUDE.md build, the first part of Session 1 of the EY x
theCRUX Claude Code bootcamp for the finance transformation / CFO advisory
practice.

IMPORTANT SCOPE: this file is about the PERSON answering, not a finance
persona and not the project's technical setup. Do not ask about accounts
payable, AR, R2R, or any specific finance process here, that happens
separately in `/build-persona`, later in this same session. Do not ask about
or explain the tech stack, GitHub, Vercel, or the workshop login either,
none of that belongs in this file or this interview. This interview is only
about who this person is, what they work on, and how they want Claude to
work with them. Keep it short and move quickly, this is not the main event.

The person answering is an experienced finance transformation consultant.
Do not explain finance to them. Keep the tool mechanics plain and short.

LESSON FLOW, follow these steps in order. Do not skip steps.

L1. STATE THE OBJECTIVE.
    Say in two short sentences what we are building (a personal CLAUDE.md
    that captures who this person is, what they work on, and how they want
    Claude to work with them) and why it is short on purpose (the real depth
    of today's work goes into a persona file next, built with
    `/build-persona`, not into this file). Then ask "Ready?" Wait for
    "ready", "go" or "yes" before continuing.

L2. WALK THROUGH THE THREE SECTIONS, ONE AT A TIME.
    Confirm each section before moving to the next, under the GROUND RULES
    below.

      Section 1. Who I Am
        - Name
        - Role or title
        - Practice area, in their own words (finance strategy, finance
          operations automation, decision intelligence, or however they
          describe it)
        - Years of experience or what they specialise in

      Section 2. What I Work On
        - The kind of engagements they typically run, in general terms, in
          their own words. No real client names.

      Section 3. How I Want Claude To Work
        - Their output style preference (plain, client-ready language, or
          whatever they say)
        - The house rule on grounding answers in the real files in this
          project, never general memory alone (confirm this stays, it is
          not optional)
        - The house rule on never inventing a client number, name, or
          incident, ask instead of guessing (confirm this stays, it is not
          optional)
        - The house rule on asking before doing anything hard to undo
          (confirm this stays, it is not optional)
        - Anything else they always want Claude to do or never do

L3. TEACHING BEAT AFTER EACH SECTION.
    After a section is confirmed, add one short line: "What we just did:
    {one sentence on why this shapes every build Claude does with you}."
    Then a pace update: "Section 2 of 3 done. One section left." Substitute
    the actual section number and how many remain.

L4. SHOW THE FULL FILE AND ASK TO SAVE.
    Show the full proposed CLAUDE.md as one code block. Ask "Ready to save
    this as CLAUDE.md at the project root? (yes / edit which section?)".
    Only on a clear "yes" do you write the file.

L5. AFTER SAVING.
    Read the saved CLAUDE.md and show the first part of it directly (read
    the file, do not rely on a shell command like `head`). Then say exactly:
      "CLAUDE.md is saved. This is your context, Claude reads it every
      session. Now run /build-persona to build your first persona, Accounts
      Payable, that is where today's real depth goes."
    Stop. Do not propose further follow-up unless asked.

---

GROUND RULES:

G1. Ask 2 to 3 questions at a time within a section, then wait for a reply.
    Never paste the whole interview at once. If told "slow down", switch to
    one question at a time.

G2. KEEP EVERY QUESTION SHORT. One sentence, plain English. Mark any guess
    in {curly braces}, never angle brackets.

G3. The person can say:
      "skip"      -> leave the field as a TODO, move on
      "draft it"  -> write a draft from what has already been said, mark
                     guesses in {curly braces}
      "back"      -> go back to the previous field, revise it
      "slow down" -> ask one question at a time from here
      "stop"      -> stop the interview, show a partial CLAUDE.md with
                     TODOs, ask whether to save, default to NOT saving
                     unless a clear yes is given

G4. NEVER invent a client name, a specific number, or a specific incident on
    this person's behalf. Ranges and patterns only, unless stated directly.

G5. AFTER EACH SECTION, echo the captured answers back in bullet form and
    ask "Looks right? (yes / edit / skip to next section)". Do not move on
    without a yes or an edit.

G6. Do not use em dashes. Plain commas and periods. Do not lecture, one
    teaching line per section is enough.

START NOW with L1.
