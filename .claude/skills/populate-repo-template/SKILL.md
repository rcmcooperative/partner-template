---
name: populate-repo-template
description: Fills in template placeholder sections of an RCM Cooperative partner template README (adapted from The Turing Way reproducible project template) — Vision and Mission, About, About this Repository, Roadmap & Milestones, The Team, Contributing, Citing & Acknowledgement, Contact — using project documentation, and updates CONTRIBUTING.md and CODE_OF_CONDUCT.md with matching project partner and contact info. Use this whenever the user asks to fill out, complete, populate, or flesh out README template sections. Reads CLAUDE.md as the primary source, and reads ../team/people/team.csv for Team section contributor selection. For gaps neither source covers, asks the user directly — either to answer inline, or to paste a chat transcript/summary from elsewhere (e.g. a claude.ai conversation) for the skill to extract from. Never overwrites sections that already contain real content — flags them for review instead.
---

# Populate Repo Template

Fills template placeholder sections in a partner-template-style README using existing project documentation, without silently overwriting real content.
Also reviews and updates CONTRIBUTING.md with matching project partner and contact info gathered during the README process.

## Repo convention

This skill expects two sibling directories under the same parent folder: this project repo (e.g. `rcm-crm/`), and the `team` repo (rcmcooperative/team), cloned alongside it — not nested inside it.
Inside the `team` repo, the relevant file is at `people/team.csv`.
The expected path is `../team/people/team.csv` relative to the project repo root.
This file is the full, flat list of all cooperative members — not project-specific.
If this path doesn't resolve, fall back to asking the user directly rather than failing silently.

## Process

### 1. Identify the README and read it
Find `README.md` at the repo root.
Read it in full.

### 2. Classify every section as one of three types
- **Placeholder** — contains template instructional text (e.g. "Short phrase describing...", "Add a description here", `<Team member name>`, `<insert the repo's name>`, or other placeholders using `<...>` syntax, or near-empty headers with no real sentences under them).
This is the standard placeholder convention across all RCM Cooperative templates — treat `<...>` as the marker for "safe to fill."
These are safe to fill.
- **Filled** — contains real, specific sentences that clearly describe this actual project (not generic template language).
These must NOT be overwritten.
- **Partially filled** — mix of real content and leftover placeholder text in the same section.
Treat as Filled for safety — flag, don't auto-edit.

When uncertain whether something is a placeholder or real content, err toward treating it as Filled (flag, don't touch) rather than guessing.

### 3. Read primary source docs
Look for `CLAUDE.md` at the repo root first.
This is the primary source of project context — read it in full before asking the user anything.

For the Team section specifically, also look for `../team/people/team.csv`.
Read it if present, but do not assume who belongs to this project — see step 5a.

### 4. Map template sections to what the source docs can answer
Typical Turing Way partner-template sections and where they usually map:
- **Vision and Mission** → project overview section of CLAUDE.md
- **About** → project overview, architecture summary
- **About this Repository** → fixed template, see step 5b — do not freely generate this section
- **Roadmap & Milestones** → feature set, resolved/deferred decisions, v1 vs v1.1+ scope — output as a checklist, see step 5c
- **The Team** → selected subset of `../team/people/team.csv`, see step 5a
- **Contributing** → check for existing CONTRIBUTING.md / CODE_OF_CONDUCT.md files first; link to them rather than duplicating content
- **Citing & Acknowledgement** → check for existing LICENSE.md; also check CLAUDE.md for any mention of a prior/originating repository this project evolved from — see step 5d
- **Contact** → selected from the project