# Cate — Commitment Guardian

A Claude Code skill that acts as an axiom anchor and pivot interrogator. Cate holds a kernel of your values and commitments, gates new impulses through a structured queue, and runs a fixed interrogation before any pivot. It never prevents you from changing direction — it makes sure you're doing it for the right reasons.

Forked from [Dave](https://github.com/alex-is-learning/dave) / [Virgo and Fiona](https://github.com/alex-is-learning/virgo-and-fiona).

---

## Philosophy

**The problem isn't that you pivot. It's that you pivot before you've diagnosed why.**

New ideas arrive with the feeling of aliveness. But that feeling looks identical whether the idea is genuinely better or whether you've hit the trough — the grindy phase past the first 20% of a commitment where novelty has worn off and the work gets hard. Cate distinguishes the two.

The **kernel** is the stable core: your axioms (beliefs about what matters), your commitment stack (max 3 active), your queue (where new impulses wait), and lock-in periods (minimum durations before reassessment). Cate never changes the kernel mid-session — it only appends at close.

The **five-step interrogation** runs every time something new is proposed:

1. **Inside check** — is this within an existing commitment?
2. **Hot potato / trough check** — past 20% of lock-in, grindy language, new impulse arriving?
3. **Queue check** — new impulse or 30+ days old?
4. **Aliveness check** — conviction rated 1–5
5. **Axiom check** — which axiom supports this? What leaves the stack to make room?

```mermaid
flowchart LR
    A[New impulse arrives] --> B{Inside existing\ncommitment?}
    B -- Yes --> C[Continue]
    B -- No --> D{Trough signal?\n>20% lock-in\n+ grindy language}
    D -- Yes --> E[Name the trough.\nRate aliveness.\nQueue it.]
    D -- No --> F{Queue age\n30+ days?}
    F -- New --> G[Add to queue]
    F -- 30+ days --> H[Aliveness 1–5]
    H --> I{Which axiom?\nWhat leaves\nthe stack?}
    I --> J[Stack update\nat close]
```

---

## Commands

| Command | Purpose |
|---|---|
| `/cate init` | One-time setup: kernel interview, axiom elicitation, commitment stack, queue, lock-in periods, vault scaffold |
| `/cate hello` | Session entry: context load, early exit check, status report of stack / queue / lock-in dates |
| `/cate close` | Session close: grind score, pivot summary, transcript write, kernel update, log append, homepage update |
| *(in-session)* | Pivot interrogation, trough detection, queue routing, aliveness check, axiom anchoring |

---

## How It Works

### Session flow

```mermaid
flowchart TD
    Start([First time]) --> Init["/cate init"]
    Init --> I1[Kernel interview:\naxioms + vagueness check]
    I1 --> I2[Commitment stack\nmax 3 + lock-in periods]
    I2 --> I3[Queue initialised\nempty]
    I3 --> I4[Kernel file +\nhomepage created]
    I4 --> Hello

    Hello["/cate hello"] --> H1[Read full kernel file]
    H1 --> H2{Unclosed\nsession?}
    H2 -- Yes --> H3[Log as incomplete]
    H3 --> H4
    H2 -- No --> H4[Status report:\nstack · queue · lock-in dates]
    H4 --> H5["What are you\nworking on today?"]
    H5 --> Session

    Session[Session] --> S1{"/cate close?"}
    S1 -- Bail --> S2["Detected next\n/cate hello"]
    S2 --> Hello

    S1 -- Yes --> Close["/cate close"]
    Close --> C1[Grind score 1–5]
    C1 --> C2[Pivot summary]
    C2 --> C3[Transcript write\n& kernel update]
    C3 --> C4[Log append\n& homepage update]
    C4 --> C5{Another\nsession?}
    C5 -- Yes --> Hello
    C5 -- No --> Done([Done])
```

### In-session interrogation

Cate runs the same five-step sequence for every proposed pivot or new impulse. The sequence is fixed — it cannot be reordered or shortcut:

```mermaid
flowchart TD
    Open([Session begins]) --> Turn[Alex sends a message]

    Turn --> Cate{Cate reads\nthe message}
    Cate -- Business as usual --> Continue[Acknowledge and\ncontinue]
    Cate -- New impulse or pivot --> Step1

    Step1["① Inside check:\nwithin existing commitment?"]
    Step1 -- Yes --> Continue
    Step1 -- No --> Step2

    Step2["② Hot potato check:\n>20% lock-in + grindy language\n+ new impulse arriving?"]
    Step2 -- Yes --> Trough["Name the trough.\nAsk: still there in 48h?"]
    Step2 -- No --> Step3

    Step3["③ Queue check:\nnew or 30+ days old?"]
    Step3 -- New --> Queue[Route to queue]
    Step3 -- 30+ days --> Step4

    Step4["④ Aliveness check:\nconviction 1–5"]
    Step4 -- Low --> Queue
    Step4 -- High --> Step5

    Step5["⑤ Axiom check:\nwhich axiom supports this?\nwhat leaves the stack?"]
    Step5 --> Stack[Stack update flagged\nfor close]

    Continue --> Again
    Trough --> Again
    Queue --> Again
    Stack --> Again

    Again{"/cate close?"} -- No --> Turn
    Again -- Yes --> Close([→ /cate close])
```

### The kernel

`Cate Kernel.md` is the persistent state file. It has five fixed H2 sections:

- **Axioms** — stable beliefs about what matters; elicited at init with vagueness checks
- **Commitment Stack** — max 3 active commitments, each with a lock-in period and end condition
- **Queue** — new impulses waiting; items 30+ days old surface for review
- **Lock-In Periods** — minimum durations per commitment before reassessment
- **Kernel History** — append-only dated blocks recording all changes

Kernel updates are never made mid-session. Every change is written as a dated append block at close.

---

## Vault Structure

```
~/scrapbook_private/source/content/Cate/
├── Alex and Cate.md          ← Homepage (session index)
├── Cate Kernel.md            ← Axioms, stack, queue, lock-in periods, history
├── sessions/
│   └── YYYY-MM-DD-cate-session-N.md
└── log/
    └── YYYY-MM-DD-cate-log.md
```

`scrapbook_private` — never published.

---

## Skill Files

```
~/.claude/skills/cate/  →  ~/claude/cate/skills/cate/
```

```
skills/cate/
├── SKILL.md
├── workflow.md          # Persona, hard constraints, kernel schema, dispatch
└── steps/
    ├── step-init.md     # /cate init
    ├── step-hello.md    # /cate hello
    ├── step-session.md  # In-session interrogation
    └── step-close.md    # /cate close
```

---

## Technical Design

- **Incremental append writes** — every turn appended via bash heredoc immediately. Crash-safe. No read-then-write.
- **Transcript held at close** — session transcript is held in context during the session and written as a single heredoc at `/cate close`. Mid-session, nothing is written to the transcript file.
- **awk exception for homepage** — the session index table requires row insertion rather than append; uses awk + temp file swap.
- **Hardcoded vault path** — `~/scrapbook_private/source/content/Cate/` defined once in `workflow.md`. Never derived at runtime.
- **Fixed interrogation sequence** — the five steps are enforced at the skill level, not subject to conversational negotiation.
- **Max 3 commitments** — hard constraint. Stack update requires naming what leaves before anything enters.
- **Early exit detection** — if a session ends without `/cate close`, the next `/cate hello` detects it, logs the incomplete session, and surfaces a continuity note.
- **Frontmatter + backlink scan at close** — patches any session file missing `date`/`tags` frontmatter or `[[Alex and Cate]]` backlink.

---

## Project Artefacts

Design and planning documents in `_bmad-output/`:

| File | Contents |
|---|---|
| `prd-cate.md` | Full PRD — 33 functional requirements, 15 non-functional requirements |
| `architecture-cate.md` | Architecture decisions — write mechanisms, kernel strategy, data flow |
| `product-brief-cate.md` | Original product brief |

---

## Status

Skill files complete and installed via symlink. Vault directories created. Next: `/cate init` first real session.
