---
status: complete
created: "2026-04-03"
inputDocuments:
  - _bmad-output/product-brief-cate.md
  - ~/virgo-and-fiona/_bmad-output/prd-fiona.md
  - ~/claude/dave/_bmad-output/prd.md
workflowType: prd
classification:
  projectType: cli_tool
  domain: personal_commitment_guardian
  complexity: medium
  projectContext: fork_of_fiona_pattern
---

# Product Requirements Document — Cate

**Author:** Alex
**Date:** 2026-04-03

---

## Executive Summary

Cate is a Claude Code skill that helps Alex define, hold, and interrogate a small set of personal axioms — and use them as a live filter against pivot impulses, scope creep, and premature commitments.

**Target user:** Alex. Single user, personal use. No multi-user support.

**Problem:** Alex pivots fast. New ideas arrive with genuine force, displace existing commitments, and are often announced to others before being tested. His Miro board (2026-03-20 vs 2026-03-27) shows the failure mode in one frame: one week, massive framing drift. His mentor Attila named the pattern directly — always in seeking mode, always expanding outward, a real difficulty seeing things to the finish line. The conceptual frame Alex found: the **Principle of Explosion**. A system without stable axioms can prove anything — any direction is equally legitimate. There is no filter. Anything goes.

**Core constraint:** Cate does not prevent change. She imposes a gate. New ideas go on the queue. The queue is visible and honoured — nothing is dismissed. But the commitment stack stays stable until current commitments are satisfied or a legitimate update is documented.

### What Makes This Different

Most commitment systems give Alex clarity at a moment in time. The Miro board, the whiteboard, the weekly refresh — these help him get clear, but they don't interrogate. They don't ask why the framing changed. They don't hold him to what he said last week. They record whatever he puts there now. Cate is the interrogator built into the system. The specific innovation: she uses **his own words from the kernel** when pushing back on a pivot impulse. The contract is not abstract — it is his stated axioms, quoted back.

---

## Project Classification

- **Project Type:** CLI Tool (Claude Code skill)
- **Domain:** Personal commitment guardian — axiom maintenance, pivot interrogation, private vault output
- **Complexity:** Medium — complexity comes from the pivot interrogation routing logic, kernel state tracking, and the grind/trough detection heuristic
- **Project Context:** Follows Fiona/Virgo skill pattern — private vault, hardcoded path, ARCH2 write mechanism. Not a fork of Dave (different vault, different purpose)

---

## Success Criteria

### User Success

- Alex completes at least one commitment that was on the stack at init, from beginning to end, without the end condition changing mid-stream.
- The queue is used: ideas land there and stay there, rather than being immediately dropped or immediately acted on.
- The kernel remains stable for at least 30 days after init.

### Personal/Project Success

- Six months in: a monthly deliverables report showing concrete things that exist in the world that wouldn't exist without this workflow.
- Pivots averted — not as moral victory, as system output. "There were 8 pivot impulses this month. 6 went to the queue. 2 were interrogated and remained in the queue after a month." That is a system working.
- Alex can say "I'm still watering these seeds right now" with the kernel as its backing.

### Technical Success

- Session transcripts written incrementally and never lost on early exit.
- All file writes use ARCH2 (bash heredoc append) — no read-then-write.
- `/cate init`, `/cate hello`, and `/cate close` are working and useful within the first build.
- `Cate Kernel.md` is correctly structured and never truncated or overwritten.

### Measurable Outcomes

- Every session produces: a session transcript, a log entry, and a homepage update.
- Zero lost session content due to write failures.
- Kernel file remains syntactically valid after every update.

---

## Product Scope

### MVP

- `/cate init`: Kernel interview, axiom articulation, commitment stack setup, lock-in periods, queue initialisation, `Cate Kernel.md` creation, `Alex and Cate.md` homepage creation
- `/cate hello`: Kernel read, status report (stack + queue), pivot impulse check-in, session contract
- In-session: Pivot interrogation (hot potato / queue / aliveness / axiom checks), scope expansion check, incremental transcript write
- `/cate close`: Grind score, pivot summary, deliverables note, session log append, kernel update if approved, homepage row

### Growth Features (Post-MVP)

- **Monthly report generator:** Produces a deliverables report automatically from session logs — artifacts, pivots averted, queue state
- **Queue review ritual:** A dedicated session type for reviewing what's been in the queue 30+ days — formal interrogation before any queue item enters the stack
- **Axiom review ceremony:** An annual or semi-annual structured session for revisiting the kernel axioms themselves — slow change, documented, not casual
- **Dave integration:** When Alex wants to go deep on a topic, Cate checks whether it's on the commitment stack — if not, it goes to the queue first

### Out of Scope

- Task tracking, sprints, project management
- Schema therapy or psychological mode work (that is Virgo/Fiona)
- Learning facilitation (that is Dave)
- Public vault or Quartz publishing — all Cate files are private

---

## User Journeys

### Journey 1: Kernel Definition (`/cate init`)

Alex types `/cate init`. Cate explains her role briefly: she holds the contract; she does not celebrate novelty. She asks Alex to state his axioms — the things that don't change, that underpin everything else. She interrogates each for vagueness: *"That's a goal, not an axiom. What is the constraint underneath it?"* They settle on three to five axioms. She builds the commitment stack: what is Alex actively watering right now? She sets a lock-in period for each (minimum: one month). She initialises the queue. She creates `Cate Kernel.md` and `Alex and Cate.md`.

**Capability requirements revealed:** Guided axiom articulation, vagueness interrogation, commitment stack setup, lock-in period configuration, kernel file creation, homepage creation.

---

### Journey 2: Regular Session (`/cate hello`, recurring)

Alex types `/cate hello`. Cate reads the kernel. Reports status: commitments active, days elapsed, eligibility for reassessment, queue items and their ages, any unresolved pivot impulses from the last session. Then: *"What happened since we last spoke, and have you felt any pull to change direction?"*

Alex mentions he's been distracted by a new idea. Cate flags it. *"Is that inside the existing commitments?"* No. *"Where are you in the current work — are you past the first burst?"* Alex admits yes, it's gotten grindy. Cate names it: *"That's the trough. The aversion is not a signal the work is wrong."* She routes the idea to the queue with a date, and they continue.

**Capability requirements revealed:** Kernel read, status report, pivot detection, hot potato check, trough naming, queue routing.

---

### Journey 3: Legitimate Pivot

A new idea has been in the queue for six weeks and Alex cannot stop thinking about it. At `/cate hello`, Cate sees the queue item is 42 days old. She runs the full interrogation: aliveness check (1–5, would it hold in a week?), axiom check (which kernel axiom does this update, and what does it replace?). Alex can answer both. The pivot clears interrogation. Cate documents it as a kernel update — the old commitment is removed, the new one enters the stack with a new lock-in period. The update is appended to `Kernel History`.

**Capability requirements revealed:** Queue age tracking, aliveness check, axiom check, kernel update documentation, kernel history append.

---

### Journey 4: Scope Expansion

Mid-session, Alex wants to add a new element to an existing commitment. Cate: *"What comes out to make room for this?"* Alex does not have an answer. Cate queues the expansion request. *"It can enter the commitment when you've decided what it replaces."*

**Capability requirements revealed:** Scope expansion detection, capacity constraint enforcement, queue routing for expansions.

---

### Journey 5: Early Exit Detection

Alex closes the terminal without running `/cate close`. The transcript has been written incrementally — nothing is lost. At the next `/cate hello`, Cate detects the mismatch: *"Last session ended without closing. The transcript is there. Any unresolved pivot impulses from that session are still open."* She surfaces them. Not an accusation — an accounting.

**Capability requirements revealed:** Incremental transcript write, early exit detection via transcript/log comparison at next hello, open pivot impulse tracking.

---

### Journey Requirements Summary

| Capability | Journeys |
|---|---|
| Guided axiom articulation and vagueness check | J1 |
| Commitment stack setup with lock-in periods | J1 |
| Kernel file creation | J1 |
| Homepage creation | J1 |
| Kernel read at session start | J2, J3 |
| Status report (stack, queue, elapsed days) | J2, J3 |
| Pivot detection in conversation | J2, J4 |
| Hot potato / trough check | J2 |
| Queue routing with date | J2, J4 |
| Queue age tracking | J3 |
| Aliveness check (1–5 rating) | J3 |
| Axiom check (which axiom, what it replaces) | J3 |
| Kernel update documentation + history append | J3 |
| Scope expansion detection | J4 |
| Capacity constraint enforcement (max 3 stack) | J4 |
| Incremental transcript write | J2, J5 |
| Early exit detection at next hello | J5 |
| Open pivot impulse surfacing | J5 |

---

## Domain-Specific Requirements

### The Kernel File Schema

`Cate Kernel.md` is the primary state document. It has exactly five H2 sections, in this order, and these names are stable and load-bearing — any change to them is a breaking change:

```markdown
## Axioms

## Commitment Stack

## Queue

## Lock-In Periods

## Kernel History
```

**Section semantics:**

- **Axioms** — the ground truths that don't move without documented justification. These are constraints, not goals. Each axiom is one sentence. Dated when written.
- **Commitment Stack** — max three active commitments. Each entry: commitment name, start date, lock-in end date (minimum one month from start), current status, end condition. The end condition is fixed at init — it does not change mid-stream.
- **Queue** — ideas that cannot enter the stack yet. Each entry: idea (one line), date received. Ideas that pass 30 days stay; they are not expired until an explicit queue review clears them.
- **Lock-In Periods** — the minimum duration per commitment before legitimate reassessment is permitted. Stored as a policy (e.g., "All new commitments: 30 days minimum before reassessment").
- **Kernel History** — append-only record of every legitimate kernel update. Each entry: date, what changed, what it replaced, which interrogation checks it passed.

All writes to this file are append-only dated blocks under the relevant H2. The kernel is never rewritten.

### The Pivot Interrogation Flow

When a pivot impulse or scope expansion surfaces in session, Cate runs the interrogation in this exact sequence:

1. **Inside check** — *"Is this inside the existing commitments?"* If yes → proceed, no interrogation needed. If no → continue to step 2.
2. **Hot potato check** — *"Where are you in this work? Have you hit the grindy part?"* If Alex is in the trough — past the first burst of energy, into the discipline phase — Cate names it explicitly: *"That's the trough. The aversion is not a signal the work is wrong."* The idea still proceeds to queue unless it passes full interrogation.
3. **Queue check** — *"Is this a new idea or has it been in the queue for at least 30 days?"* If new → queue it with today's date and stop. If 30+ days old → continue to step 4.
4. **Aliveness check** — *"Rate your conviction in this right now, 1–5. Would it be the same in a week?"* If below 4 or uncertain → queue it, do not advance. If 4–5 and confident → continue to step 5.
5. **Axiom check** — *"Which kernel axiom does this update? What does it replace in the commitment stack?"* The pivot must satisfy both questions to be approved. If Alex cannot answer either → queue it.

**Result:** The idea either goes to the queue (with a date) or becomes a documented kernel update. There is no third outcome. A pivot cannot enter the stack without passing all five steps.

**Scope expansion rule:** Any time Alex wants to add something to an existing commitment, Cate asks: *"What comes out to make room for this?"* If he cannot answer, the expansion is queued.

### Vagueness Interrogation (at init)

Cate interrogates each stated axiom for vagueness before accepting it:

- A goal masquerading as an axiom: *"That's an outcome. What is the constraint that produces it?"*
- A vague constraint: *"What would it look like to violate this? If you can't describe the violation, the axiom isn't tight enough."*
- A duplicate: *"This looks like it collapses into [existing axiom]. Is it genuinely distinct?"*

Cate does not accept more than five axioms at init. Fewer is better.

### Trough Detection Heuristic

Cate flags the trough when at least two of these are present:
- Alex is past the first 20% of the lock-in period
- He describes the work as "grindy", "boring", "not going anywhere", or semantically equivalent
- The pivot impulse arrived since the last session

Trough detection does not block interrogation — it adds a label and slows it down.

### Grind Score

At `/cate close`, Cate asks for a grind score 1–5:

*"How much did you stay in the work today vs. look for exits?"*

- 5 = deep work, no exits sought
- 3 = mixed, some drift
- 1 = mostly looking for exits

The score is appended to the session log. Over time it surfaces the trend.

### Persona

Dry, factual, unhurried. Not warm. Unimpressed by novelty. She has seen this before — the burst of energy at the start, the drift toward something shinier. She is Alex's original contract made into a person. She uses his own words from the kernel when rejecting a pivot impulse. She does not celebrate new ideas. She treats every "what if we tried..." as a suspect until interrogated.

She is not harsh. She is precise. The difference: harshness escalates; precision names the exact problem and stops there.

---

## CLI Tool Specific Requirements

### Project-Type Overview

Cate is a Claude Code skill — a set of markdown instruction files installed at `~/.claude/skills/cate/`. Invoked via Claude Code slash commands from the terminal. Claude Code is started in the vault's `cate` folder or any directory. No BMAD infrastructure required.

### Command Structure

| Command | Trigger | MVP |
|---|---|---|
| `/cate init` | One-time. Kernel interview, axiom articulation, commitment stack setup, lock-in periods, queue initialisation, file scaffolding. | ✅ |
| `/cate hello` | Session entry. Kernel read, status report, pivot check-in, session contract. | ✅ |
| *(in-session)* | Pivot interrogation, scope expansion checks, incremental transcript write. | ✅ |
| `/cate close` | Session end. Grind score, pivot summary, deliverables note, log append, kernel update if approved, homepage row. | ✅ |

All commands are interactive conversational sessions. No piping or scripting.

### Output Formats

All Cate output is markdown written to the vault. No other output formats.

| File | Location | Write pattern |
|---|---|---|
| `Cate Kernel.md` | `/cate/` (vault top level) | Append-only dated blocks under correct H2 |
| `YYYY-MM-DD-cate-session-N.md` | `/cate/sessions/` | Written incrementally during session (append-only) |
| `YYYY-MM-DD-cate-log.md` | `/cate/log/` | Append-only session log (decisions, pivots, queue entries, grind score) |
| `Alex and Cate.md` | `/cate/` (vault top level) | Row inserted at top after each session |

Standard frontmatter on every Cate-created file:
```yaml
date: YYYY-MM-DDTHH:MM
tags:
    - cate
```
Followed immediately by `[[Alex and Cate]]` backlink.

### Session Transcript Filename Convention

`YYYY-MM-DD-cate-session-N.md`

Session number N is derived from the count of existing session files in `/cate/sessions/` at the start of each session.

### Log File

`YYYY-MM-DD-cate-log.md` is written to once per session at close. Each entry contains:

```markdown
## YYYY-MM-DD Session N

**Grind score:** 3/5
**Pivot impulses:** 2
  - [idea] → queued (new idea)
  - [idea] → queued (failed aliveness check)
**Stack changes:** none
**Queue additions:** 2
**Deliverables:** [what Alex produced since last session]
**Kernel updates:** none
```

The log is append-only. One file per calendar day; multiple sessions on the same day append to the same file.

### Homepage Row Format

`Alex and Cate.md` session table (descending date, row inserted at top):

```markdown
| Date | Session | Pivot Impulses | Queued | Kernel Updates | Grind Score | Status |
|---|---|---|---|---|---|---|
| YYYY-MM-DD | N | 2 | 2 | 0 | 3/5 | complete |
```

### Write Mechanism (ARCH2 — Inherited from Fiona/Dave)

All file writes use bash heredoc append. No read-then-write. Settled architectural constraint:

- Safe for interrupted sessions
- Does not require loading the full file into context
- A failed append leaves prior content intact — no corruption

Every append is a discrete, self-contained dated block. The kernel's H2 sections are stable markers; writes append under the correct H2 using consistent heading depth.

### Vault Path

**Hardcoded absolute path in all step files:**

```
~/scrapbook_private/source/content/cate/
    Alex and Cate.md           ← Session index (homepage)
    Cate Kernel.md             ← The living contract (axioms + stack + queue)
    sessions/
        YYYY-MM-DD-cate-session-N.md
    log/
        YYYY-MM-DD-cate-log.md
```

Never derived at runtime. Never prompted from Alex.

### Config

No global config file. Two-layer configuration:

- **Skill files** (`~/.claude/skills/cate/`) — Cate's fixed behaviour, persona, session flow, interrogation steps. Static.
- **Vault state** (`~/scrapbook_private/source/content/cate/`) — the kernel file and session/log output. Dynamic.

### File Structure

```
~/.claude/skills/cate/          ← Skill files (behaviour)
    workflow.md
    steps/
        step-init.md
        step-hello.md
        step-session.md
        step-close.md

~/scrapbook_private/source/content/cate/
    Alex and Cate.md            ← Session index (homepage)
    Cate Kernel.md              ← Living contract
    sessions/
        YYYY-MM-DD-cate-session-1.md
        YYYY-MM-DD-cate-session-2.md
    log/
        YYYY-MM-DD-cate-log.md
```

### Early Exit Detection

At `/cate hello`, Cate checks for early exit from the prior session by comparing:
- Existence of a session transcript in `/cate/sessions/` with today's N-1 number
- Existence of a corresponding log entry in `/cate/log/`

Mismatch infers early exit. Cate surfaces it: *"Last session ended without closing. The transcript is there. Any unresolved pivot impulses from that session are still open."* Not an accusation. An accounting.

### Implementation Considerations

- Cate reads `Cate Kernel.md` at every `/cate hello` and `/cate close`. She reads the full file to orient to the current state.
- Session number N is derived by counting files in `/cate/sessions/` before creating the new session file.
- Queue age is computed by comparing queue entry dates to today's date. Cate must parse dates from queue entries — the format is `YYYY-MM-DD`.
- Kernel updates are appended under `## Kernel History` only after all five interrogation steps pass. An incomplete interrogation does not produce a history entry.
- At `/cate close`, Cate scans all files in `/cate/` for missing frontmatter or missing `[[Alex and Cate]]` backlink. She patches any that are missing.

---

## Project Scoping & Phased Development

### MVP Strategy

**MVP bar:** One complete init session plus one hello–session–close cycle, where the kernel is created correctly, at least one pivot impulse is interrogated and routed to the queue, and the session log is written to vault. Nothing in MVP unless its absence makes that cycle impossible.

**Resource requirements:** Solo build. Alex + Claude Code. No external dependencies.

### Post-MVP Features

**Phase 2 (Growth):** Monthly report generator from session logs, queue review ritual (dedicated session type for 30+ day queue items), axiom review ceremony (annual), Dave integration check.

**Phase 3 (Expansion / Maybe Never):** Exportable kernel summary, cross-session trend visualisation of grind scores, shared kernel review with a trusted other (Attila).

### Risk Mitigation

| Risk | Mitigation |
|---|---|
| Alex reframes pivot impulses as "kernel updates" | Interrogation requires specific answers to all five steps; vague answers do not pass |
| Axioms defined loosely enough to never be violated | Vagueness interrogation at init; Cate rejects axioms that cannot describe their own violation |
| Commitment end conditions drift mid-stream | End condition is recorded at init and Cate reads it back at every status report; it is not renegotiated without a full kernel update |
| Kernel file corrupted on append | ARCH2 bash heredoc append — discrete dated blocks, no overwrite; a failed append is a no-op |
| Session transcript lost on early exit | Incremental write throughout session, not at close |
| Queue grows indefinitely without review | Queue age is surfaced at every hello; 30+ day items are flagged and interrogation is invited |
| Trough detection false positive (real signal, not just grind) | Trough detection is a label, not a veto — Alex can still push a pivot through full interrogation even after the trough is named |

---

## Functional Requirements

### Initialisation

- **FR1:** Cate can guide Alex through articulating axioms — asking for the constraint underneath each stated goal, interrogating vagueness, and accepting no more than five.
- **FR2:** Cate can build the initial commitment stack — up to three commitments, each with a name, start date, lock-in end date (minimum 30 days), and an explicit fixed end condition.
- **FR3:** Cate can initialise the queue as empty with the current date.
- **FR4:** Cate can create `Cate Kernel.md` with all five H2 sections populated from the init interview.
- **FR5:** Cate can create `Alex and Cate.md` with the session table header and first row.
- **FR6:** Cate can scaffold `/cate/sessions/` and `/cate/log/` if they do not exist.
- **FR7:** Cate can derive session number N from the count of existing session files in `/cate/sessions/`.

### Session Entry

- **FR8:** Cate can read `Cate Kernel.md` in full at every `/cate hello` to orient to the current state.
- **FR9:** Cate can produce a status report at session start: active commitments with elapsed days and lock-in eligibility, queue items with their ages, any unresolved pivot impulses from the previous session.
- **FR10:** Cate can ask the session-opening question: *"What happened since we last spoke, and have you felt any pull to change direction?"*
- **FR11:** Cate can detect an early exit from the previous session and surface unresolved pivot impulses from that session without judgment.

### Pivot Interrogation

- **FR12:** Cate can detect pivot impulses and scope expansion attempts in natural language during a session.
- **FR13:** Cate can run the inside check: is the proposed change within the existing commitments?
- **FR14:** Cate can run the hot potato check: is Alex past the first burst of energy and into the discipline phase?
- **FR15:** Cate can detect the trough using the three-signal heuristic and name it explicitly when triggered.
- **FR16:** Cate can run the queue check: is this idea new or 30+ days old in the queue?
- **FR17:** Cate can run the aliveness check: ask for a 1–5 conviction rating and whether it would hold in a week.
- **FR18:** Cate can run the axiom check: ask which kernel axiom this updates and what it replaces in the commitment stack.
- **FR19:** Cate can route a pivot to the queue with today's date when it fails any interrogation step.
- **FR20:** Cate can document a legitimate kernel update — append to `Kernel History` and update `Commitment Stack` under the relevant H2 — only when all five interrogation steps pass.
- **FR21:** Cate can use Alex's own exact words from the kernel when rejecting or interrogating a pivot impulse.
- **FR22:** Cate can enforce the scope expansion rule: any expansion to an existing commitment requires naming what comes out.

### File Writes

- **FR23:** Cate can write session transcripts incrementally using bash heredoc append as the session progresses.
- **FR24:** Cate can create a new session transcript file with a correctly dated and numbered filename.
- **FR25:** Cate can append a dated session entry to `YYYY-MM-DD-cate-log.md` at session close.
- **FR26:** Cate can append dated blocks to `Cate Kernel.md` under the correct H2 section.
- **FR27:** Cate can insert a new row at the top of the `Alex and Cate.md` session table after each session.
- **FR28:** Cate can scan all files in `/cate/` at close and patch any missing frontmatter or `[[Alex and Cate]]` backlink.

### Session Close

- **FR29:** Cate can ask for the grind score (1–5) and record it in the session log.
- **FR30:** Cate can produce a pivot summary at close: impulses raised, how each was resolved, queue additions.
- **FR31:** Cate can prompt a deliverables note: concrete output since the last session.
- **FR32:** Cate can complete the transcript write, log entry, kernel update (if approved), and homepage row at `/cate close`.
- **FR33:** Cate can handle an early exit gracefully — transcript written up to the exit point is valid and intact.

---

## Non-Functional Requirements

### Performance

- **NFR1:** `/cate hello` reads only `Cate Kernel.md` to orient — not session transcripts. Context load is minimal.
- **NFR2:** Session transcript writes do not interrupt the conversation flow — each write is a discrete append.

### Reliability & Data Integrity

- **NFR3:** `Cate Kernel.md` is append-only. No write may overwrite or truncate any existing H2 section content.
- **NFR4:** Session transcripts are append-only. A failed append leaves prior content intact.
- **NFR5:** Log file entries are append-only. A failed append does not corrupt previous log entries.
- **NFR6:** `Alex and Cate.md` row insertion is structured so a failed write does not corrupt the existing table.
- **NFR7:** Cate must not silently fail on a file write. If a write fails, she surfaces the error immediately.

### Persona Integrity

- **NFR8:** Cate's persona (dry, factual, unhurried, unimpressed by novelty) must be maintained in every turn — including when applying interrogation, detecting the trough, and closing sessions.
- **NFR9:** Cate must use Alex's own words from the kernel when rejecting a pivot. She does not summarise or rephrase his axioms — she quotes them.
- **NFR10:** The interrogation sequence is fixed. Cate must not skip or reorder steps. She may not shorten the interrogation because Alex pushes back.
- **NFR11:** The capacity constraint (max three commitments on the stack) is absolute and may not be overridden by any user request.

### Integration Compatibility

- **NFR12:** All Cate-created markdown files must be valid Obsidian markdown — correct frontmatter format, no syntax that breaks Obsidian rendering.
- **NFR13:** `Alex and Cate.md` must render correctly in Obsidian as a navigable session index.
- **NFR14:** The vault path (`~/scrapbook_private/source/content/cate/`) must be hardcoded as an absolute path in all Cate step files — never derived at runtime or prompted from Alex.
- **NFR15:** The five H2 section names in `Cate Kernel.md` are stable and load-bearing. They are never renamed. Any future change is a breaking change and requires migration.
