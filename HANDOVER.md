# Cate — Handover Document

**Date:** 2026-04-03
**Status:** Architecture complete. Skill files not yet started.
**Repo:** https://github.com/alex-is-learning/cate.git
**Brief:** `_bmad-output/product-brief-cate.md`
**PRD:** `_bmad-output/prd-cate.md`
**Architecture:** `_bmad-output/architecture-cate.md`

---

## What Cate Is

A Claude Code skill — a personal axiom anchor and commitment guardian. Cate holds a small set of locked personal axioms and a stable commitment stack, and interrogates every pivot impulse before it becomes a real pivot.

---

## Architectural Decisions Made This Session

### 1. Vault path (corrected)
```
~/scrapbook_private/source/content/Cate/
```
Capital C. Hardcoded in all step files as `VAULT_ROOT=~/scrapbook_private/source/content/Cate`. Never derived at runtime.

### 2. Skill file structure
```
~/claude/cate/skills/cate/
    workflow.md
    steps/
        step-init.md
        step-hello.md
        step-session.md
        step-close.md
```
Same as Fiona/Virgo/Dave.

### 3. Write mechanism: ARCH2 (bash heredoc append)
All content writes use bash heredoc append. One accepted exception: homepage row insertion uses awk + temp-file-move (same as Fiona's actual step-close.md implementation).

### 4. Session state model: vault file presence
- Session counter: `ls "$VAULT_ROOT/sessions/" | grep -c "\-cate-session"` + 1
- Early exit detection: at `/cate hello`, check if last session transcript exists but has no corresponding log entry for that session in `/cate/log/YYYY-MM-DD-cate-log.md`

### 5. Kernel append strategy: end-of-file dated blocks
Post-init kernel writes append dated blocks at the end of the file with bold section labels (e.g. `**Queue:**`, `**Commitment Stack:**`). No H2 targeting. Claude reads the full kernel at each hello/close and synthesises current state from all blocks in order.

### 6. Transcript strategy: hold in context, write at close
Turns are held in context during the session. The full transcript is written as a single heredoc append at `/cate close`. If Alex exits early, the transcript header exists but turns are lost — surfaced at next hello. This matches Fiona's actual implementation (not "incremental" as the PRD implies).

### 7. Log file: one per calendar day, append-only
`YYYY-MM-DD-cate-log.md` in `$VAULT_ROOT/log/`. Each session appends a dated session block at close. Multiple sessions on the same day append to the same file.

### 8. Homepage row: awk insert at top
```bash
awk -v row="$NEW_ROW" '/^\|---/{print; print row; next} {print}' "$HOMEPAGE" > /tmp/cate_home.tmp && mv /tmp/cate_home.tmp "$HOMEPAGE"
```
Descending date order (newest at top). Accepted ARCH2 exception — applies only to the homepage.

### 9. Step dispatch in workflow.md
workflow.md contains a table mapping commands to step files. step-hello.md loads step-session.md at its end.

### 10. Five kernel H2 sections (frozen, load-bearing)
```
## Axioms
## Commitment Stack
## Queue
## Lock-In Periods
## Kernel History
```
Never renamed. Any change is a breaking change.

---

## Blockers

None. Architecture is solid and complete.

---

## Exact Next Steps

1. **Build `workflow.md`** in `~/claude/cate/skills/cate/`:
   - Persona: dry, factual, unhurried, unimpressed by novelty, uses Alex's own words when rejecting a pivot
   - Hard constraints: max 3 commitments (absolute), interrogation sequence fixed (no skipping), end conditions not renegotiated, stack max not overridable
   - Vault path: `~/scrapbook_private/source/content/Cate/`
   - Shared conventions: frontmatter format, backlink `[[Alex and Cate]]`, ARCH2, filename conventions
   - Context loading rules: kernel-only at hello, nothing extra in-session
   - Step dispatch table
   - Reference: `~/virgo-and-fiona/skills/fiona/workflow.md` as the nearest template

2. **Test bash heredoc append to vault path before building any step files:**
   ```bash
   cat >> ~/scrapbook_private/source/content/Cate/test.md << 'EOF'
   test
   EOF
   ```
   Verify the path resolves correctly (capital C, path exists). Delete test file after confirming.

3. **Build step files in order:**
   - `step-init.md` — vault scaffolding (`mkdir -p sessions log`), kernel interview, axiom vagueness check, commitment stack setup, lock-in periods, queue init, kernel file creation (full H2 scaffold), homepage creation
   - `step-hello.md` — derive session state, early exit detection (transcript vs log), kernel read, status report (stack + queue ages), opening question, transcript file creation (header only), load step-session.md
   - `step-session.md` — pivot detection, 5-step interrogation (fixed sequence), trough detection heuristic, queue routing, kernel update on pass, scope expansion rule
   - `step-close.md` — grind score, pivot summary, deliverables note, transcript write (single heredoc), log append, kernel update (if approved), homepage row (awk), frontmatter/backlink scan of sessions/

4. **Install and run `/cate init`** from the vault directory to smoke-test:
   - Check `Cate Kernel.md` created with correct H2 scaffold
   - Check `Alex and Cate.md` created with table header
   - Check `sessions/` and `log/` directories exist

5. **Run a full hello → session → close cycle** to verify:
   - Session transcript created at hello (header only)
   - Turns written at close
   - Log entry appended
   - Homepage row inserted at top
   - Early exit detection works on next hello if close is skipped

---

## Reference Files

| File | Purpose |
|---|---|
| `~/virgo-and-fiona/skills/fiona/workflow.md` | Nearest workflow.md template |
| `~/virgo-and-fiona/skills/fiona/steps/step-init.md` | Nearest init template |
| `~/virgo-and-fiona/skills/fiona/steps/step-hello.md` | Nearest hello template (session counter, early exit, transcript creation) |
| `~/virgo-and-fiona/skills/fiona/steps/step-session.md` | Nearest session template (context tracking, no file writes mid-session) |
| `~/virgo-and-fiona/skills/fiona/steps/step-close.md` | Nearest close template (transcript write, awk homepage, frontmatter scan) |
| `_bmad-output/architecture-cate.md` | All architectural decisions, bash patterns, data flow |
