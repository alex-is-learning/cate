# Cate — Handover Document

**Date:** 2026-04-03  
**Status:** Product brief complete. PRD not yet started.  
**Repo:** https://github.com/alex-is-learning/cate.git  
**Brief:** `_bmad-output/product-brief-cate.md`

---

## What Cate Is

A Claude Code skill — a personal axiom anchor and commitment guardian. Cate helps Alex maintain a small set of locked personal axioms and a stable commitment stack, and interrogates every pivot impulse before it becomes a real pivot.

**The problem she solves:** Alex pivots fast — within days, not months. New ideas arrive with energy, displace existing commitments, get announced prematurely, and are often dropped before producing anything. His Miro board (2026-03-20 vs 2026-03-27) shows the failure mode in one frame: one week, massive framing drift. The stable threads across both weeks (exercise, Attila mentorship, connection) are the kernel candidates.

**Conceptual framework:** The Principle of Explosion — a system without stable axioms can justify any direction. Cate is the constraint that prevents explosion.

---

## Architectural Decisions Made

### 1. Skill pattern: Fiona/Virgo, not Dave
Cate uses a **hardcoded absolute vault path** (private vault), not Dave's CWD-derived topic slug. Reason: Cate files are private (scrapbook_private), not public.

### 2. Vault path (proposed)
```
~/scrapbook_private/source/content/cate/
    Alex and Cate.md           ← session index / homepage
    Cate Kernel.md             ← primary state file (axioms + stack + queue)
    sessions/
        YYYY-MM-DD-cate-session-N.md
    log/
        YYYY-MM-DD-cate-log.md
```

### 3. Write mechanism: ARCH2 (bash heredoc append)
All file writes are append-only bash heredoc. No read-then-write. Inherited from Dave/Virgo/Fiona.

### 4. Skill file structure
```
~/claude/cate/skills/cate/
    workflow.md
    steps/
        step-init.md
        step-hello.md
        step-session.md
        step-close.md
```
Same pattern as Dave, Virgo, Fiona.

### 5. Primary state artifact: Cate Kernel.md
Unlike Dave (topic log) or Virgo (mode files), Cate's primary state is a single **Kernel file** with stable H2 sections:
- `## Axioms` — the ground truths that don't move
- `## Commitment Stack` — max 3 active commitments
- `## Queue` — dated ideas that cannot enter the stack yet
- `## Lock-In Periods` — minimum duration per commitment before reassessment
- `## Kernel History` — append-only record of legitimate kernel updates

All writes to this file are append-only (dated blocks under the correct H2).

### 6. Repo: standalone
`~/claude/cate/` — its own repo, same pattern as `~/claude/dave` and `~/virgo-and-fiona`. Not merged into either existing repo.

### 7. Commands (MVP)
| Command | Purpose |
|---|---|
| `/cate init` | Kernel interview, axiom articulation, stack/queue setup, file scaffolding |
| `/cate hello` | Kernel read, status report (stack + queue), pivot check-in, session open |
| *(in-session)* | Pivot interrogation, scope checks, incremental transcript write |
| `/cate close` | Grind score, pivot summary, deliverables note, log append, homepage update |

### 8. Core in-session mechanic: Pivot Interrogation
When a pivot impulse or scope expansion surfaces:
1. Is it inside existing commitments? If yes → proceed. If no:
2. Hot potato check — is Alex in the trough of the current work?
3. Queue check — has this idea been present for 30+ days?
4. Aliveness check — conviction 1–5; would it hold in a week?
5. Axiom check — which kernel axiom does this update, and what does it replace?

Result: either goes to the queue (with a date), or becomes a documented kernel update.

### 9. Persona
Dry, factual, unhurried. Not warm. Unimpressed by novelty. She has seen this before. She is Alex's original contract made into a person. She uses his own words from the kernel when rejecting a pivot impulse.

---

## Source Context (Read in This Session)

- `/Users/alexanderlarge/claude/dave/skills/dave/` — Dave skill files (installed pattern)
- `/Users/alexanderlarge/virgo-and-fiona/skills/fiona/` — Fiona skill files (closer template for Cate)
- `/Users/alexanderlarge/virgo-and-fiona/_bmad-output/architecture.md` — Fiona/Virgo architecture doc
- `/Users/alexanderlarge/claude/dave/_bmad-output/prd.md` — Dave PRD
- `/Users/alexanderlarge/claude/dave/_bmad-output/architecture.md` — Dave architecture doc
- `Alex's Project Management Board.pdf` — Miro board export showing goal drift week-to-week
- `The Principle of Explosion (Personal Logic).md` — conceptual framework
- `Attila 1-1 2026-04-01.md` — mentor session notes (source of the failure mode diagnosis)

---

## Blockers

None. Brief is solid. One open question the PRD will settle: exact H2 structure of `Cate Kernel.md` and whether the queue and log should be separate files or sections of the kernel.

---

## Exact Next Steps

1. **Review the product brief** (`_bmad-output/product-brief-cate.md`) — correct anything wrong or missing before the PRD is written against it.

2. **Run the BMAD PRD flow** from within `~/claude/cate/`:
   - Use `/bmad-agent-pm` or `/bmad-create-prd`
   - Input: `_bmad-output/product-brief-cate.md`
   - Reference: `~/virgo-and-fiona/_bmad-output/prd-fiona.md` as the nearest PRD template
   - Output: `_bmad-output/prd-cate.md`
   - Key things the PRD must specify: full FR list, kernel file schema, pivot interrogation flow as FRs, session close mechanics, early exit detection, frontmatter/backlink conventions

3. **Run the BMAD architecture flow**:
   - Input: `_bmad-output/prd-cate.md`
   - Reference: `~/virgo-and-fiona/_bmad-output/architecture.md`
   - Output: `_bmad-output/architecture-cate.md`
   - Key decisions to settle: exact kernel H2 structure (frozen, load-bearing), log vs kernel for pivot history, session state model (vault file presence like Fiona, not CWD like Dave)

4. **Build the skill files** in `~/claude/cate/skills/cate/`:
   - Start with `workflow.md` (persona, hard constraints, vault path, step dispatch)
   - Test ARCH2 bash heredoc append to the vault path before building step files
   - Build step files in order: `step-init.md` → `step-hello.md` → `step-session.md` → `step-close.md`

5. **Install and run `/cate init`** from the vault directory to smoke-test the full flow.
