# Step: `close` — Session Close

Read this file completely before acting. Execute sections in order. Do not skip.

---

## 1. Setup

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
TODAY_DATE=$(date '+%Y-%m-%d')
TODAY_FULL=$(date '+%Y-%m-%dT%H:%M')
SESSION_N=$(($(ls "$VAULT_ROOT/sessions/" 2>/dev/null | grep -c "\-cate-session") - 1))
TRANSCRIPT="$VAULT_ROOT/sessions/$(ls "$VAULT_ROOT/sessions/" 2>/dev/null | grep "\-cate-session" | sort | tail -1)"
LOG_FILE="$VAULT_ROOT/log/${TODAY_DATE}-cate-log.md"
KERNEL="$VAULT_ROOT/Cate Kernel.md"
```

Derive `SESSION_N` from session count minus 1 (the current session was counted as N+1 at hello; we're closing it as N). If `SESSION_N` is 0 or less, default to 1.

Store all variables.

---

## 2. Focus Score

Ask:

> Focus score — 1 to 5. How present and locked in were you with the work today?

Wait for Alex's response. Store as `FOCUS_SCORE`.

---

## 3. Deliverables Note

Ask:

> What did you produce since the last session? Can be nothing — just note it.

Wait. Store as `DELIVERABLES`. If Alex says nothing, store "none".

---

## 4. Pivot Summary

From the session context, derive:
- `PIVOT_COUNT` — number of pivot impulses that arose
- `QUEUED_COUNT` — number of ideas that went to queue
- `KERNEL_UPDATES` — number of approved kernel updates (stack changes)
- `PIVOT_SUMMARY` — brief list of what each impulse was and where it went

Tell Alex (brief):

> Here's what I logged:
>
> [PIVOT_SUMMARY — e.g.:]
> - "Start a newsletter" → queued (new idea, received TODAY_DATE)
> - "Drop writing, focus on product" → queued (failed aliveness check, 2/5)
> - "Replace project X with project Y" → kernel update (passed full interrogation)
>
> [Or: No pivot impulses this session.]

No preamble. State the facts.

---

## 5. Write Transcript

Write all session turns to the transcript as a single heredoc append. Include every exchange held in context, in order:

```bash
cat >> "$TRANSCRIPT" << 'RECONEOF'

**Alex:** [first message]

**Cate:** [first response]

**Alex:** [second message]

**Cate:** [second response]

[all subsequent turns in order]

RECONEOF
```

If the write fails, stop:
> Transcript write failed: [error]. Check your filesystem before continuing.

---

## 6. Kernel Updates

If any `QUEUE_ENTRY` items were held in context during the session, append them to the kernel:

```bash
cat >> "$KERNEL" << 'KERNELEOF'

---

**Kernel update — TODAY_DATE_PLACEHOLDER**

**Queue:**
- [idea name] — received TODAY_DATE_PLACEHOLDER
[additional queue entries if any]

KERNELEOF
```

Replace `TODAY_DATE_PLACEHOLDER` with actual date. Replace queue entries with actual content held in context.

If a `KERNEL_UPDATE` (stack change) was held in context during the session, append it:

```bash
cat >> "$KERNEL" << 'KERNELEOF'

---

**Kernel update — TODAY_DATE_PLACEHOLDER**

**Commitment Stack:**
- [new commitment] — added TODAY_DATE_PLACEHOLDER | lock-in until [DATE] | end condition: [exact words]
- [removed commitment] — closed TODAY_DATE_PLACEHOLDER | reason: pivot passed interrogation

**Kernel History:**
- TODAY_DATE_PLACEHOLDER: [new commitment] added — replaced [removed commitment] — passed: inside check, hot potato, queue check ([N] days), aliveness ([N]/5), axiom check ("[exact axiom text]")

KERNELEOF
```

Replace all placeholders with actual content from context. Use Alex's exact words — never paraphrase.

If the kernel write fails, surface the error immediately:
> Kernel update failed: [error]. The transcript is intact — only the kernel update is missing. Check your filesystem.

If there are no queue entries and no kernel updates, skip this section.

---

## 7. Log Append

```bash
cat >> "$LOG_FILE" << 'LOGEOF'

## TODAY_DATE_PLACEHOLDER Session SESSION_N_PLACEHOLDER

**Focus score:** FOCUS_SCORE_PLACEHOLDER/5
**Pivot impulses:** PIVOT_COUNT_PLACEHOLDER
PIVOT_SUMMARY_PLACEHOLDER
**Stack changes:** STACK_CHANGES_PLACEHOLDER
**Queue additions:** QUEUED_COUNT_PLACEHOLDER
**Deliverables:** DELIVERABLES_PLACEHOLDER
**Kernel updates:** KERNEL_UPDATES_PLACEHOLDER

LOGEOF
```

Replace all placeholders with actual values.

For `PIVOT_SUMMARY_PLACEHOLDER`, format as:
```
  - [idea] → queued (new idea)
  - [idea] → queued (failed aliveness check)
  - [idea] → kernel update (passed)
```

Or omit this line entirely if `PIVOT_COUNT` is 0.

If the log write fails, surface the error:
> Log write failed: [error]. The transcript and kernel are intact — only the log entry is missing.

---

## 8. Homepage Row

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
HOMEPAGE="$VAULT_ROOT/Alex and Cate.md"
NEW_ROW="| ${TODAY_DATE} | ${SESSION_N} | ${PIVOT_COUNT} | ${QUEUED_COUNT} | ${KERNEL_UPDATES} | ${FOCUS_SCORE}/5 | complete |"
awk -v row="$NEW_ROW" '/^\|---/{print; print row; next} {print}' "$HOMEPAGE" > /tmp/cate_home.tmp && mv /tmp/cate_home.tmp "$HOMEPAGE"
```

If the write fails:
> Homepage update failed: [error]. The session transcript and log are intact — only the homepage row is missing.

---

## 9. Frontmatter and Backlink Scan

Scan all `.md` files in `$VAULT_ROOT/sessions/` for missing frontmatter or missing `[[Alex and Cate]]` backlink. Patch any that are missing.

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
SESSIONS_DIR="$VAULT_ROOT/sessions"

for f in "$SESSIONS_DIR"/*.md; do
  [ -f "$f" ] || continue

  # Add tags: [cate] if missing
  if ! grep -q "tags:" "$f"; then
    if head -1 "$f" | grep -q "^---"; then
      awk 'NR==1{print; print "tags:"; print "    - cate"; next} {print}' "$f" > /tmp/cate_tag.tmp && mv /tmp/cate_tag.tmp "$f"
    else
      { printf -- "---\ndate: %s\ntags:\n    - cate\n---\n\n" "$(date '+%Y-%m-%dT%H:%M')"; cat "$f"; } > /tmp/cate_tag.tmp && mv /tmp/cate_tag.tmp "$f"
    fi
  fi

  # Add [[Alex and Cate]] backlink if missing
  if ! grep -q "\[\[Alex and Cate\]\]" "$f"; then
    awk 'BEGIN{count=0} /^---$/{count++; print; if(count==2){print ""; print "[[Alex and Cate]]"}; next} {print}' "$f" > /tmp/cate_link.tmp && mv /tmp/cate_link.tmp "$f"
  fi
done
```

If any individual file patch fails, note it and continue.

---

## 10. Close

Tell Alex:

> Done. Session logged.
