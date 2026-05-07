# Example: livraria-comunitaria (FastAPI)

> **This is a fictitious didactic project. Do not copy as a scaffold.**
> **Este é um projeto fictício para fins didáticos. Não copie como scaffold.**

This directory shows what an Innova Cortex installation looks like end-to-end on a real (small) project. Use it as a **conference reference** — to check whether your own install ended up structurally similar — not as a starting point.

To start your own project, follow the **Quickstart** in the main repository `README.md`. Run the Init Project workflow and fill the docs based on *your* project, not this one.

---

## What this example contains

```text
projeto-fastapi-ficticio/
├── README.md                 ← this file
├── .cortex/                  ← (omitted in draft — in a real install, this is a full copy of Innova Cortex governance)
├── CLAUDE.md                 ← local connector with placeholders filled
├── docs/
│   ├── as-is/
│   │   └── system-overview.md
│   ├── business/
│   │   └── domain-overview.md
│   ├── analysis/
│   │   ├── current-state-assessment.md
│   │   └── gaps-and-unknowns.md
│   └── context/
│       ├── current-status.md
│       └── handoff.md
└── memory/
    └── project-context.md
```

> **Note about `.cortex/` in this draft:** to avoid duplicating the entire governance (~30+ files) inside the example, the `.cortex/` folder is omitted from this draft. In a real installation, you would copy the whole Innova Cortex governance into `.cortex/` as described in the main `README.md` Quickstart.

---

## The fictitious project

**Domain:** a small neighborhood library that lends books between residents.
**Goal of the API:** register books, record loans, mark returns, list open loans.
**Actors:** the volunteer librarian; cataloged neighbors who borrow.
**Stack chosen by the (fictitious) author:** FastAPI + SQLAlchemy + SQLite.

The technical stack is the **author's choice**, not an Innova Cortex prescription. The harness does not pick technologies for you.

---

## How to read this example

1. Start with `CLAUDE.md` — see how the local file connects the project to `.cortex/`.
2. Read `docs/as-is/system-overview.md` — the broad initial picture.
3. Read `docs/business/domain-overview.md` — the domain side.
4. Skim `docs/analysis/*` — the critical assessment and the open gaps.
5. Read `docs/context/current-status.md` — the live state.
6. Read `docs/context/handoff.md` — what a session pickup would look like.
7. Read `memory/project-context.md` — the persistent project memory.

The whole point of this example is to show what these files **should look like after** a real conversation with Claude using Innova Cortex — not to show what they should literally contain. Yours will look different.

---

## Language

Internal docs are in Brazilian Portuguese (the language of the original Innova Cortex governance). When you install Innova Cortex on your own project, you may write your docs in any language — Innova Cortex does not enforce a specific one.
