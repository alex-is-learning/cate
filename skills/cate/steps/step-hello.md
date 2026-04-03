# Step: `hello` — Session Entry

Read this file completely before acting. Execute sections in order. Do not skip.

---

## 1. Derive Session State

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
TODAY=$(date '+%Y-%m-%dT%H:%M')
TODAY_DATE=$(date '+%Y-%m-%d')
KERNEL="$VAULT_ROOT/Cate Kernel.md"
```

Check kernel exists:

```bash
[ -f "$KERNEL" ] && echo "EXISTS" || echo "MISSING"
```

**If missing:**
> Cate Kernel.md isn't there. Run `/cate init` first.

Stop. Do not proceed.

Store `TODAY`, `TODAY_DATE`, `VAULT_ROOT`, `KERNEL`.

---

## 2. Early Exit Detection

Derive session count and last session file:

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
SESSION_N=$(( $(ls "$VAULT_ROOT/sessions/" 2>/dev/null | grep -c "\-cate-session" || echo 0) + 1 ))
LAST_FILE=$(ls "$VAULT_ROOT/sessions/" 2>/dev/null | grep "\-cate-session" | sort | tail -1)
```

If `LAST_FILE` is empty: no previous sessions — skip to section 3.

If `LAST_FILE` is non-empty, check whether it has a corresponding log entry:

```bash
LAST_DATE=$(echo "$LAST_FILE" | cut -c1-10)
LAST_N=$(echo "$LAST_FILE" | grep -o 'session-[0-9]*' | grep -o '[0-9]*')
LOG_FILE="$VAULT_ROOT/log/${LAST_DATE}-cate-log.md"
grep -q "Session ${LAST_N}" "$LOG_FILE" 2>/dev/null && echo "LOGGED" || echo "MISSING"
```

**If `LOGGED`:** previous session closed properly. Continue to section 3 without comment.

**If `MISSING`:** an early exit occurred. Tell Alex (no judgment):

> Last session — [LAST_DATE], Session [LAST_N] — ended without closing. The transcript is there. Any unresolved pivot impulses from that session are still open. Do you want to surface them now, or start fresh?

Insert an incomplete row in the homepage:

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
HOMEPAGE="$VAULT_ROOT/Alex and Cate.md"
NEW_ROW="| ${LAST_DATE} | ${LAST_N} | — | — | — | — | incomplete |"
awk -v row="$NEW_ROW" '/^\|---/{print; print row; next} {print}' "$HOMEPAGE" > /tmp/cate_home.tmp && mv /tmp/cate_home.tmp "$HOMEPAGE"
```

If this write fails, note the error briefly and continue — do not stop the session.

Store `SESSION_N`.

---

## 3. Read Kernel

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
cat "$VAULT_ROOT/Cate Kernel.md"
```

Read the full kernel file. Synthesise current state by reading all content in order:
1. The init scaffold gives baseline axioms, stack, queue, lock-in periods.
2. Each subsequent dated block updates the state — later blocks override earlier blocks for the same field.

Derive current state for:
- `CURRENT_AXIOMS` — the axioms as last stated
- `CURRENT_STACK` — commitments currently on the stack (max 3), with end conditions
- `CURRENT_QUEUE` — items in queue with dates added
- `CURRENT_LOCK_IN` — lock-in end dates per commitment

---

## 4. Status Report

Tell Alex (brief and factual):

> Stack:
> - [Commitment 1] — end condition: [condition] | lock-in until: [date] | [X] days in
> - [Commitment 2] — end condition: [condition] | lock-in until: [date] | [X] days in
> - [Commitment 3 if present]
>
> Queue: [count] item(s) — [oldest item name] has been queued [N] days / (empty)
>
> [If any item has been queued 30+ days:] [Item name] has been in queue [N] days — eligible for review.

No preamble. No framing. Just the facts from the kernel.

---

## 5. Opening Question

Ask:

> What are you working on today?

Wait for Alex's response. This is not a pivot interrogation trigger — it is an open entry point. Receive what he offers. If the response describes work within existing commitments, reflect that briefly and continue to step 6. If it sounds like a new direction or scope expansion, flag it:

> That sounds like it might be outside [commitment name]. We'll run it through when it comes up properly.

Do not run interrogation at hello — that is step-session territory.

---

## 6. Session Transcript Creation

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
TODAY=$(date '+%Y-%m-%dT%H:%M')
TODAY_DATE=$(date '+%Y-%m-%d')
SESSION_N=$(( $(ls "$VAULT_ROOT/sessions/" 2>/dev/null | grep -c "\-cate-session" || echo 0) + 1 ))
TRANSCRIPT="$VAULT_ROOT/sessions/${TODAY_DATE}-cate-session-${SESSION_N}.md"

cat >> "$TRANSCRIPT" << 'SESSEOF'
---
date: TODAY_PLACEHOLDER
tags:
    - cate
---

[[Alex and Cate]]

# TODAY_PLACEHOLDER — Cate Session SESSION_N_PLACEHOLDER

---

SESSEOF
```

Replace `TODAY_PLACEHOLDER` and `SESSION_N_PLACEHOLDER` with actual values before executing.

If the write fails, surface the error immediately and stop:
> Transcript creation failed: [error]. The session cannot start without a transcript file.

Store `TRANSCRIPT` and `SESSION_N` — every in-session reference targets this file.

---

## 7. Begin Session

Load `./steps/step-session.md`. All in-session behaviour follows from there.
