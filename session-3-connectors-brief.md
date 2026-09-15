[← Back to Student Handbook](student-handbook.md)

---

# Session 3: Connectors + Daily Brief

**Day 1**

**What you'll build:** two native connectors (Gmail and Google Calendar), an Engagement Operating Manual added to your `CLAUDE.md`, and a `/brief` skill that reads your real inbox and calendar and hands you a morning briefing, triaged the way you actually want it triaged.

---

## Part A: What a connector actually is

*(Read, don't type.)*

Everything you have built so far reads files sitting on your laptop: `CLAUDE.md`, the persona, the sample data. A **connector** (the underlying standard is called MCP, Model Context Protocol) lets Claude read a live external service instead: your real inbox, your real calendar, not a file you exported.

There are two ways to connect a service:

**Native connectors.** A direct connection to one service, set up once at claude.ai → Settings → Integrations. Full control over what it can see, no third party in between. Once connected, it works in every Claude Code session on this account, not just this project.

**Gateway connectors.** One login, many services at once, through a middleman platform (Composio is the common one). Convenient for something occasional, like Slack or Notion. For the two services you use every single day, Gmail and Calendar, native is the right call: your data goes straight from Google to Claude, nothing else holds your login. If you later want a service beyond Gmail and Calendar, ask a facilitator about Composio.

Today you connect two native integrations: **Gmail** and **Google Calendar**.

---

## Part B: Connect Gmail and Calendar

### Step 1: Connect both at claude.ai

Open **claude.ai** in your browser, sign in with the same account your Claude Code CLI uses, and go to **Settings → Integrations**. Connect **Gmail** and **Google Calendar**. Each is a short Google sign-in flow.

When both show as connected, they are available in Claude Code immediately, same account, same login.

### Step 2: Authenticate Calendar inside Claude Code

Calendar needs one more step beyond the Settings connection. Back in your terminal:

```
Authenticate my Google Calendar connection so I can read events. Use the
mcp__claude_ai_Google_Calendar tools.
```

Claude runs the authentication tool and walks you through a browser approval. Come back to the terminal once that's done.

### Step 3: Confirm both are actually readable

```
Read my last 5 emails and today's calendar events, using the Gmail and
Google Calendar tools. Just list them, nothing else, so I can confirm
both connections actually work before we build anything on top of them.
```

If either comes back empty or errors, fix it now. Part C depends on both working.

---

## Part C: The Engagement Operating Manual

`/brief` needs to know what matters to **you**, not generic advice. Not "flag urgent emails", but which client, which partner, which phrase actually means urgent for your engagements. That goes into `CLAUDE.md`, so every future session reads it automatically, not just today's.

```
Add a section to my CLAUDE.md called "## Engagement Operating Manual".

Interview me to build:

TRIAGE RULES: How should you classify my emails?
- URGENT: what senders, clients, or phrases mean I need to act right now
  (a client threatening to pause an engagement, a data or confidentiality
  concern, anything from my engagement partner marked urgent)
- IMPORTANT: what belongs in my daily brief but isn't on fire (a
  deliverable due this week, a client asking a real question)
- FYI: what I should know about but don't need to act on
- IGNORE: what should never appear in my brief

KEY PEOPLE: whose messages always get priority. For each: name, role
(client sponsor, engagement partner, practice lead), what they typically
email about.

ESCALATION TRIGGERS: what should ALWAYS be flagged regardless of sender
(a deadline inside 48 hours, any mention of scope or budget overrun, a
client complaint, anything confidentiality-related).

MEETING PREP RULES: which meetings need prep before I walk in, and what
I need pulled together for them (a client call needs the latest
diagnostic status, a steering committee needs the current milestone).

Ask me 2 to 3 questions at a time. Be specific, I need real client names,
real partner names, and real phrases, not vague categories.
```

Give it real names and real triggers, not placeholders. A vague rule produces a vague brief. Spend eight to ten minutes here.

---

## Part D: Build `/brief`

Same shape as every skill you have built: a `SKILL.md` file under `.claude/skills/`.

```
Create a skill called "brief" at .claude/skills/brief/SKILL.md.

The skill runs a 3-stage morning briefing pipeline:

STAGE 1: SCAN
Read the last 20 emails via mcp__claude_ai_Gmail tools (search_threads for
recent ones, get_thread for the important ones). Read today's calendar
events via mcp__claude_ai_Google_Calendar. Capture raw data only, no
analysis yet. Save to briefing/01-scan.md.

STAGE 2: ANALYZE
Read briefing/01-scan.md and the Engagement Operating Manual in CLAUDE.md.
Classify each email URGENT / IMPORTANT / FYI / IGNORE using the triage
rules. Flag anything matching an escalation trigger. If an email sender
is also in a meeting today, note it. If a meeting needs prep, note what
recent email context applies. Save to briefing/02-analysis.md.

STAGE 3: BRIEF
Read briefing/02-analysis.md. Output:
TOP PRIORITY: what I must act on in the next 2 hours.
TODAY'S SCHEDULE: each meeting with prep notes and relevant email context.
EMAIL DIGEST: grouped by triage category, URGENT first, one line each.
CONNECTIONS: anything you connected across email and calendar.
SUGGESTED ACTIONS: specific next steps, not vague advice.
Save to briefing/03-morning-brief.md and return it as the skill's output.

Invokable as /brief with no arguments.
```

### Test it

```
/brief
```

Read the output. Did it catch the client whose email actually needed flagging? Did it connect an email to a meeting on today's calendar? If the triage feels off, that's not a bug, it means the Engagement Operating Manual needs a sharper rule. Update `CLAUDE.md`, run `/brief` again.

---

## Checkpoint

- [ ] Gmail and Google Calendar both connected, and Calendar's authenticate step completed
- [ ] Both confirmed readable with a live test read before you built anything on them
- [ ] Engagement Operating Manual added to `CLAUDE.md`, with real names and real triggers
- [ ] `/brief` skill exists at `.claude/skills/brief/SKILL.md`
- [ ] `/brief` produced a real morning brief from your actual inbox and calendar

---

## Troubleshooting - Session 3

| Issue | Fix |
|---|---|
| Gmail or Calendar shows as connected in claude.ai but Claude Code can't read it | Restart Claude Code. If still broken, confirm you're signed into the same account in the browser and the terminal. |
| Calendar returns nothing | Run the authenticate step again: `Authenticate my Google Calendar connection so I can read events. Use the mcp__claude_ai_Google_Calendar tools.` This is a separate step from connecting it in Settings. |
| `/brief` isn't found | Check `.claude/skills/brief/SKILL.md` exists and its YAML frontmatter has `name: brief`. |
| The brief's triage feels wrong | This is a rule problem, not a code problem. Add real names, real client names, and real trigger phrases to the Engagement Operating Manual in `CLAUDE.md`, then run `/brief` again. |
| One of the three briefing files is missing | The pipeline should write `briefing/01-scan.md`, `briefing/02-analysis.md`, `briefing/03-morning-brief.md`. Tell Claude to run the missing stage explicitly. |

---

**Next:** [Session 4: Agents →](session-4-research-agents.md)
