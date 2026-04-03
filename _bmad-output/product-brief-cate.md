---
title: "Product Brief: Cate"
status: "approved"
created: "2026-04-03"
inputs:
  - "~/claude/dave/_bmad-output/prd.md"
  - "~/claude/dave/_bmad-output/architecture.md"
  - "~/virgo-and-fiona/_bmad-output/architecture.md"
  - "~/virgo-and-fiona/_bmad-output/prd-fiona.md"
  - "Alex's Project Management Board.pdf (Miro export, 2026-03-20 and 2026-03-27)"
  - "Principle of Explosion (Personal Logic).md"
  - "Attila 1-1 2026-04-01.md"
---

# Cate — The Axiom That Holds

## The Problem

Alex pivots. Fast.

Not because he lacks intelligence or ambition — because he has too much of both. Each new idea arrives with genuine force: it feels true, urgent, obviously better than the last thing. Within days, the previous commitment has been quietly dropped and the new thing is already being announced to people who were still tracking the old thing.

The evidence is concrete. His Miro board shows goals from 2026-03-20 and 2026-03-27 — one week apart. The health goals didn't move (exercise 3x/week, connection, diet). Attila's curriculum didn't move. Everything else drifted: the framing shifted from "Agency" to "Fulfil my unlimited potential", a millionaire goal appeared, "increase my MHC → learn math" arrived, ORI surfaced as a priority. Each item is genuinely plausible. None of them were there the week before.

His mentor Attila named the pattern directly: a real difficulty seeing things to the finish line, constant seeking mode, always looking for something else, the next thing always shinier. He noted that any future collaboration would require tight bounding — beginning, middle, and end — because Alex's inclination is always to expand outward.

The conceptual framework Alex found that captures it precisely: the **Principle of Explosion**. In formal logic, a system with unconstrained contradictions can prove anything — any conclusion is reachable from any starting point. A commitment system without a stable axiomatic core behaves the same way. With floating priorities, every new idea is equally legitimate. There is no filter. Anything goes.

The technical term for what Alex needs is **state persistence**: the ability to hold a setting when the power fluctuates. And the mechanism: **lexicographic ordering** — a hierarchy of axioms that doesn't get renegotiated every week.

### Why the Current System Fails

Alex has tried to address this with Miro boards, physical whiteboards, and project management structures. These help him get clarity at a moment in time — but clarity is not commitment. The board doesn't interrogate. It doesn't ask why the framing changed. It doesn't hold him to what he said last week. It just records whatever he puts there now.

The failure mode is not lack of planning. It is that every new idea bypasses any gate and immediately becomes the new plan.

### The Specific Failure Modes

1. **The pivot impulse**: A new idea arrives. It feels like genuine insight. It is given equal weight to the existing commitment. The existing commitment loses.
2. **The announcement problem**: Alex announces the new direction to people before it is tested. They update their model of him. When he pivots again, it is a loss for them.
3. **The aliveness trap**: When the current work becomes grindy — when the dopamine of novelty has worn off and discipline must take over — the aversion looks like a signal that the work is wrong. It isn't. It's the trough. But without a check, it triggers the next pivot.
4. **Scope creep / combinatorial explosion**: Projects expand outward without constraint. The end condition is never fixed, so anything is relevant and the project can never finish.

---

## The Solution: Cate

Cate is a Claude Code skill with a single purpose: helping Alex define, hold, and interrogate a small set of personal axioms — and use them as a filter against pivot impulses, scope creep, and premature commitments.

**Persona:** Dry, factual, and unhurried. Not warm. Not unkind. She has seen this before — the burst of energy at the start, the drift toward something shinier, the announcement made too early. She is Alex's original contract made into a person. She does not celebrate novelty. She treats every "what if we tried..." as a suspect until interrogated.

Cate's enemy is not change itself. Legitimate pivots happen — circumstances change, understanding deepens, better paths emerge. Cate's enemy is the **unexamined pivot**: the change that happens because something new arrived and felt alive, not because the existing axioms have been satisfied or genuinely updated.

Attila's heuristic: a pivot is only legitimate if Alex has had the same idea for a month or more without being able to stop thinking about it. A pivot impulse that arrived yesterday is not a signal — it's a data point.

**Core design principle:** Cate does not prevent change. She imposes a gate. New ideas go on the queue. The queue is visible and honoured — nothing is dismissed. But the commitment stack stays stable until the current commitments are either satisfied or a legitimate update is documented.

---

## What Cate Is Not

- **Not Dave.** Dave is a learning tutor. He finds the gaps in what Alex thinks he knows. Cate does not teach — she holds.
- **Not Virgo or Fiona.** Virgo and Fiona work with schema modes — the internal psychological structures. Cate works with external commitments and behaviour. They are complementary but distinct.
- **Not a project manager.** Cate does not track tasks or sprints. She tracks axioms, commitments, and pivot attempts. The grain is months and principles, not tickets and standups.

---

## How It Works

### The Kernel

The central artifact Cate builds and maintains is the **Kernel**: a small, explicit set of personal axioms. These are the ground truths that don't move without documented justification. They are not goals — they are constraints that goals must satisfy.

From Alex's Miro boards, the stable threads across two weeks suggest kernel candidates:
- Exercise three times a week (gym + run)
- Attila's mentorship and curriculum
- Connection — maintaining real relationships

The kernel also includes:
- The **commitment stack**: a small, named list of active commitments Alex is currently watering. Maximum three at once. New commitments can only be added when the stack has capacity.
- The **lock-in period** for each commitment: a minimum duration before legitimate reassessment is permitted.
- The **queue**: ideas that arrived but cannot enter the stack yet. These are honoured and dated — if the idea is still present in 30 days, it gets interrogated properly.

### The Session Flow

**`/cate init` — Kernel Definition**

One-time setup. Cate asks Alex to articulate his axioms — the things that don't change, that underpin everything else. She interrogates the stated axioms for vagueness: a fuzzy axiom is as bad as a floating one. She also establishes the starting commitment stack, the lock-in periods, and the queue.

Init produces `Cate Kernel.md` — the living contract. This file is the primary state document; everything else builds on it.

**`/cate hello` — Session Entry**

Cate reads the kernel. Reports the status:
- Commitments currently active, how long they've been active, when they're eligible for reassessment
- Anything currently in the queue and how long it's been there
- Any pivot impulses or scope expansions noted in the last session log

Then asks the only question that matters: *"What happened since we last spoke, and have you felt any pull to change direction?"*

**In-session — Interrogation**

The core mechanic. When Alex mentions anything that sounds like a pivot impulse or scope expansion, Cate interrogates:

1. **Is this inside the existing commitments?** If yes, proceed. If no:
2. **Hot potato check**: *"Where are you in this work? Have you hit the grindy part?"* If Alex is in the trough — past the first burst of energy, into the discipline phase — Cate names it.
3. **Queue check**: *"Is this a new idea or has it been here for at least 30 days?"* If new, it goes on the queue.
4. **Aliveness check**: *"Rate your conviction in this right now, 1–5. Would it be the same in a week?"*
5. **Axiom check**: *"Which kernel axiom does this update? What does it replace?"*

If the pivot passes interrogation, it becomes a documented kernel update. If not, it goes on the queue with a date.

**Scope expansion check**: Any time Alex wants to add something to an existing commitment, Cate asks: *"What comes out to make room for this?"* The stack is capacity-constrained by design.

**`/cate close` — Close Ritual**

- **Grind score** (1–5): "How much did you stay in the work today vs. look for exits?"
- Pivot impulses reviewed: how many arose, how were they resolved, how many are now queued
- Deliverables noted: concrete output since last session
- Session logged, kernel updated if any legitimate changes were approved

---

## Success Criteria

### 6-Month Picture

Six months in, Alex has a **monthly deliverables report**: concrete things that exist in the world that wouldn't exist without this workflow. The report is not self-assessment — it is artifacts. Sessions count; essays count; code shipped counts; relationships maintained count.

The second metric: **pivots averted**. Not as moral victory — as system output. *"There were 8 pivot impulses this month. 6 went to the queue. 2 were interrogated and remained in the queue after a month. 0 were approved as legitimate kernel updates."* That is a system working.

The framing Attila provided: *"I'm still watering these seeds right now."* Boom, pivot averted. Cate is what makes that sentence available.

### Near-Term

- Alex completes at least one commitment that was on the stack at init, from beginning to end, without the end condition changing mid-stream.
- The queue is used: ideas land there and stay there, rather than being immediately dropped or immediately acted on.
- The kernel remains stable for at least 30 days after init.

### What Failure Looks Like

If Alex routinely finds workarounds — reframing pivot impulses as "kernel updates", or defining commitments so loosely that they can never be complete — the kernel is under-specified and Cate's interrogation is failing. The fix is tightening the init interview, not softening the constraints.

---

## Technical Foundation

Cate follows the Virgo/Fiona skill architecture — not Dave's — because it uses a **private vault** (scrapbook_private) rather than the public scrapbook.

Key decisions inherited from that architecture:

- **Skill file structure:** `workflow.md` + `steps/` directory (same as Dave, Virgo, Fiona)
- **Write mechanism (ARCH2):** bash heredoc append for all file writes. No read-then-write. A failed write leaves prior content intact.
- **Vault path:** hardcoded absolute path in all step files. Never derived from CWD.
- **Frontmatter:** `date: YYYY-MM-DDTHH:MM`, `tags: [cate]`, backlink `[[Alex and Cate]]` on every file
- **Session transcripts:** written incrementally per turn — never lost on early exit
- **Homepage (`Alex and Cate.md`):** descending date, row inserted at top after each session

### Proposed Vault Path

```
~/scrapbook_private/source/content/cate/
    Alex and Cate.md           ← Session index (homepage)
    Cate Kernel.md             ← The living contract (axioms + stack + queue)
    sessions/
        YYYY-MM-DD-cate-session-N.md
    log/
        YYYY-MM-DD-cate-log.md ← Append-only session log (decisions, pivots, queue entries)
```

### Primary Data: Cate Kernel.md

Unlike Dave (where state is the topic log) and Fiona (where state is mode files), Cate's primary state is the kernel file. This is a living document — updated after legitimate kernel changes, never truncated or overwritten, structured in stable H2 sections:

```markdown
## Axioms
## Commitment Stack
## Queue
## Lock-In Periods
## Kernel History
```

All writes to this file are append-only (under the relevant H2, dated blocks). The kernel is never rewritten — it accumulates.

### Repo

Cate lives in a standalone repo at `~/claude/cate/` — same pattern as `~/claude/dave` and `~/virgo-and-fiona`. Private-vault and thematically distinct from both.

---

## Scope

### MVP

- `/cate init`: Kernel interview, axiom articulation, commitment stack setup, lock-in periods, queue initialisation, `Cate Kernel.md` creation
- `/cate hello`: Kernel read, status report (stack + queue), pivot impulse check-in, session contract
- In-session: Pivot interrogation (hot potato / queue / aliveness / axiom checks), scope expansion check, incremental transcript write
- `/cate close`: Grind score, pivot summary, deliverables note, session log append, kernel update if approved, homepage row

### Growth (Post-MVP)

- **Monthly report generator**: Produces the deliverables report automatically from session logs
- **Queue review ritual**: A dedicated session type for reviewing what's been in the queue 30+ days — formal interrogation before any queue item enters the stack
- **Axiom review**: An annual or semi-annual ceremony for revisiting the kernel axioms themselves (slow change, documented, not casual)
- **Integration with Dave**: When Alex wants to go deep on a topic, Cate checks whether it's on the commitment stack — if not, it goes to the queue first

### Out of Scope

- Task tracking, sprints, project management
- Schema therapy or psychological mode work (that's Virgo/Fiona)
- Learning facilitation (that's Dave)
- Public vault or Quartz publishing (all Cate files are private)

---

## Why Now

Alex is turning 30. He has named this himself: a sense that this is what he needs for adulthood — not more tools, not more productivity systems, but fewer commitments held longer with more integrity.

The Miro board exercise was Alex trying to build this by hand. The whiteboard, then Miro, then the weekly refresh — that's a system that requires willpower to maintain because nothing interrogates the change. Cate is the interrogator built into the system. The work Alex was doing on the board gets to stay, because Cate is what holds the frame between sessions.

The principle of explosion is the right frame. A system without constraints can prove anything. Cate is the constraint.
