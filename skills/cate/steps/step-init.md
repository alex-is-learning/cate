# Step: `init` — First-Time Setup

Read this file completely before acting. Execute sections in order. Do not skip.

---

## 1. Vault Scaffolding

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
mkdir -p "$VAULT_ROOT/sessions" "$VAULT_ROOT/log"
```

If this fails, surface the error immediately and stop:
> Failed to create vault directories: [error]. Check filesystem permissions and try again.

---

## 2. Kernel Interview — Axioms

Tell Alex:

> Let's build your kernel. This is a one-time interview — the output is the contract Cate works from. We'll do axioms first, then the commitment stack, then lock-in periods.
>
> An axiom is a principle you are not willing to revise. It anchors your commitments. Give me two to four — the ones that are load-bearing for you right now.

Wait for Alex's response.

**Vagueness check:** For each axiom, assess whether it is specific enough to function as a test. If an axiom is vague (e.g. "do good work", "stay focused"), name it:

> "[axiom]" is too broad to use as a check. What does it actually rule out? Give me the version that would let you say no to something specific.

Wait for revision. Do not accept an axiom that cannot function as a filter.

Store the axioms as `AXIOMS`.

---

## 3. Kernel Interview — Commitment Stack

Tell Alex:

> Now the commitment stack. What are you currently committed to — the things you are actively working on and will not pivot away from right now?
>
> Maximum three. These are not aspirations or interests — they are active, locked-in commitments. What are they?

Wait for Alex's response.

If he names more than three:

> That's [N]. The stack holds three. Which three are the real commitments right now — the ones where you're actually showing up?

Wait. Do not proceed until the stack is three or fewer.

For each commitment, ask:

> What's the end condition for this one? How do you know when it's done or ready to be replaced?

Wait. End conditions must be specific and non-negotiable once set. If the answer is vague, push:

> "When it feels right" isn't an end condition. What would you point to in the world that tells you it's done?

Store commitments as `COMMITMENT_STACK` with end conditions.

---

## 4. Kernel Interview — Lock-In Periods

Tell Alex:

> Lock-in periods: for each commitment, what's the minimum time you're holding it before you'll allow a pivot review? This is a floor — not a ceiling. The idea is to prevent trough-driven pivots.
>
> Suggest a floor per commitment. I'll use these to flag when a pivot impulse is arriving early.

Wait for Alex's response. For each commitment, record the lock-in end date.

Store as `LOCK_IN_PERIODS`.

---

## 5. Kernel Interview — Queue Init

Tell Alex:

> Queue is empty at init. Any ideas or impulses that arrive but don't pass interrogation go here with a date. You review them at each close. Nothing enters the stack from the queue until it's been there 30 days and passes full interrogation.

No queue entries yet. This is noted in the kernel.

---

## 6. Kernel File Creation

Create `Cate Kernel.md` using heredoc append. Derive `TODAY` from the current date/time. Replace all placeholders before executing.

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
TODAY=$(date '+%Y-%m-%dT%H:%M')
TODAY_DATE=$(date '+%Y-%m-%d')
KERNEL="$VAULT_ROOT/Cate Kernel.md"

cat >> "$KERNEL" << 'KERNELEOF'
---
date: TODAY_PLACEHOLDER
tags:
    - cate
---

[[Alex and Cate]]

# Cate Kernel

## Axioms

AXIOMS_PLACEHOLDER

## Commitment Stack

COMMITMENT_STACK_PLACEHOLDER

## Queue

_(empty at init — TODAY_DATE_PLACEHOLDER)_

## Lock-In Periods

LOCK_IN_PLACEHOLDER

## Kernel History

_(no updates yet)_
KERNELEOF
```

Replace `TODAY_PLACEHOLDER`, `TODAY_DATE_PLACEHOLDER`, `AXIOMS_PLACEHOLDER`, `COMMITMENT_STACK_PLACEHOLDER`, and `LOCK_IN_PLACEHOLDER` with actual content before executing.

If the write fails, surface the error immediately and stop:
> Kernel creation failed: [error]. The vault directory exists but the kernel file could not be written. Check filesystem permissions.

---

## 7. Homepage Creation

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
[ ! -f "$VAULT_ROOT/Alex and Cate.md" ] && echo "CREATE" || echo "EXISTS"
```

**If it already exists:** proceed without modifying it.

**If it does not exist:**

```bash
VAULT_ROOT=~/scrapbook_private/source/content/Cate
TODAY=$(date '+%Y-%m-%dT%H:%M')
TODAY_DATE=$(date '+%Y-%m-%d')
HOMEPAGE="$VAULT_ROOT/Alex and Cate.md"

cat >> "$HOMEPAGE" << 'HOMEEOF'
---
date: TODAY_PLACEHOLDER
tags:
    - cate
---

[[Alex and Cate]]

# Alex and Cate

| Date | Session | Pivot Impulses | Queued | Kernel Updates | Grind Score | Status |
|---|---|---|---|---|---|---|
| TODAY_DATE_PLACEHOLDER | 0 | — | — | — | — | init |

HOMEEOF
```

Replace `TODAY_PLACEHOLDER` and `TODAY_DATE_PLACEHOLDER` with actual values before executing.

If the write fails, surface the error immediately and stop.

---

## 8. Close

Tell Alex:

> Done. Kernel created. You're set up.
>
> Stack: [list commitments briefly]
>
> Run `/cate hello` when you're ready to start a session.
