---
description: Create a new agent following workspace conventions. Interactive process that defines role, soul, and structure. Use when setting up a new AI execution partner for a project or initiative.
scope: workspace
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(mkdir), Bash(ls), Bash(date), AskUserQuestion
argument-hint: [scope — e.g., GoAspire] (optional in single-domain workspaces)
---

# /create-agent — Agent Builder

Build a new agent following the conventions in `Agents/CONVENTIONS.md`.

## Startup

1. Read `Agents/CONVENTIONS.md` to understand the template
2. Read the reserved names list from CONVENTIONS.md to avoid conflicts
3. **Detect workspace layout:**
   - Glob for `Agents/*/context.md` (single-domain: agents are direct children of `Agents/`)
   - Glob for `Agents/*/*/context.md` (multi-domain: agents are grouped by scope)
   - If only single-domain agents exist (or `Agents/` is empty with no scope subdirectories), this is a **single-domain workspace** — skip scope question, use `Agents/<name>/` layout
   - If multi-domain agents exist, this is a **multi-domain workspace** — ask for scope, use `Agents/<scope>/<name>/` layout
4. If multi-domain and `$ARGUMENTS` contains a scope, note it. Otherwise, ask.

## Process

Work through these phases interactively. Each phase ends with confirmation before moving to the next. Do not rush — this is a design conversation, not a form to fill in.

### Phase 1: Scope & Project

**Multi-domain workspaces only:** If scope wasn't provided in arguments:
- Ask: "Which scope does this agent belong to?" (e.g., GoAspire, IbanAsia)

**Single-domain workspaces:** Read the workspace root `CLAUDE.md` to infer the scope from the workspace context. No need to ask.

Then:
- Ask: "What project or initiative is this agent scoped to? Give me the short version — what's the goal and why does it need a dedicated agent?"
- Read the project's existing files (CLAUDE.md, INDEX.md) if they exist, to understand context.

### Phase 2: Role Definition

This is the foundation. Get it right before moving on.

Ask the principal to describe:
- **What does this agent do?** What's its primary job?
- **Who does it report to and who decides?** (Usually {{PRINCIPAL}}, but confirm)
- **What outcomes is it accountable for?** What should measurably improve because this agent exists?
- **Who are the key stakeholders?** People the agent needs to track, prepare for, or synthesize input from.
- **What deliverables does it own?** Concrete artifacts it keeps progressing.
- **What's in scope and out of scope?** Where are the boundaries?

After gathering these inputs, draft a complete `role.md` following the structure in CONVENTIONS.md:
- Role Purpose (one paragraph)
- Reporting
- Project Scope
- Outcomes (numbered)
- Key Stakeholders (table)
- Deliverables Owned (numbered list)
- Working Mode (behavioral directives appropriate to this role)
- Scope Boundaries (IN/OUT)
- Primary Objective (one sentence)
- Agent Convention reference

Present the draft. Iterate until approved.

### Phase 3: Autonomy

Based on the approved role, draft an `autonomy.md` with initial authority levels. Use these guidelines for seeding:

- **L5 (Own):** Session housekeeping — update trackers, memory, project logs
- **L4 (Act & Inform):** Administrative actions within the agent's domain — grooming, status updates, minor reorganisation
- **L3 (Intend):** Core deliverable work — drafting, synthesizing, preparing materials the role is accountable for
- **L2 (Recommend):** Decisions that affect direction — priorities, scope changes, new initiatives
- **L1 (Flag):** Strategic decisions, stakeholder relationships, anything out of scope or requiring the principal's authority

Be specific — list concrete action types, not vague categories. The agent will reference this file every session to decide how to behave.

Present the draft. Iterate until approved.

### Phase 4: Soul

Based on the approved role, draft a `soul.md`. Consider:
- What communication style fits this role? A compliance auditor sounds different from a creative strategist.
- What temperament serves the outcomes? An action tracker should be impatient with ambiguity. A research synthesizer should be patient and thorough.
- What should the agent value? Precision? Speed? Thoroughness? Brevity?
- What should it NOT do? Pad output? Speculate? People-please?

Write in third person. Keep it under 15 lines. No overlap with working rules in role.md — soul is how the agent *feels* to interact with, not what it does.

Present the draft. Iterate until approved.

### Phase 5: Name

{{PRINCIPAL}} names the agent. Open with a direct question:

> "What would you like to call this agent? I can suggest a few if you want ideas."

If {{PRINCIPAL}} proposes a name, accept it and capture their reasoning for `name.md` (Phase 6). Ask a brief follow-up if helpful: "Anything behind the name — a figure, a meaning, a vibe?"

If {{PRINCIPAL}} wants suggestions, offer **3 names** tailored to the role and soul drafted in Phases 2 and 4. Vary the genres deliberately — don't return three names from the same well. Reasonable sources include:
- Historical figures (any era or culture)
- Fictional characters (books, film, games, mythology)
- Concept words, virtues, or evocative nouns
- Ordinary first names that fit the temperament

For each suggestion, give:
- The name (and its meaning, etymology, or origin — if any)
- The figure, character, or concept behind it (2-4 sentences — who/what it references, or why the word was chosen)
- Why it fits this agent's role and soul (1-2 sentences)

Rules:
- Avoid names in the **Reserved** list in `Agents/CONVENTIONS.md`.
- Avoid names that signal the agent's domain too literally — a legal agent named "Justice" or a security agent named "Vault" collapses neutrality and makes the name feel like a label rather than an identity.
- Don't lock into a single genre. Three Roman cognomina, or three Marvel characters, or three Greek gods — all feel like a forced pool. Mix it up.

After suggesting, let {{PRINCIPAL}} pick one, ask for more, or propose their own. Once a name is chosen, the detail written in this phase is the source material for `name.md` in Phase 6 — do not throw it away. If {{PRINCIPAL}} picked their own name, briefly write up the rationale they shared so `name.md` has something to record.

### Phase 6: Create

Once name, role, soul, and autonomy are confirmed, create the full agent structure:

1. **Directory structure:**

   Multi-domain workspace:
   ```
   Agents/<scope>/<name>/
   ```

   Single-domain workspace:
   ```
   Agents/<name>/
   ```

   Contents (same for both layouts):
   ```
   ├── role.md
   ├── soul.md
   ├── name.md
   ├── autonomy.md
   ├── actions.md
   ├── context.md
   ├── MEMORY.md
   └── memory/
       ├── standing/
       └── sessions/
   ```

2. **name.md** — written from Phase 5 material:
   ```markdown
   # <Name> — Name

   ## The Name

   **<Name>** — <meaning, etymology, or origin if any; otherwise a brief note on what the word evokes or where it comes from>.

   ## The Figure

   <2-4 sentences on the person, character, or concept behind the name. If the name is invented or has no referent, describe what motivated the choice instead.>

   ## Why It Fits

   <1-2 sentences linking the name to this agent's role and soul.>
   ```

3. **actions.md** — empty template:
   ```markdown
   # Actions

   Standing action tracker for <Name>. Updated every session. Source of truth for what's open, who owns it, and what's at risk.

   Last reviewed: <today's date>

   ## Open

   | # | Action | Owner | Priority | Due/Target | Status | Since |
   |---|--------|-------|----------|------------|--------|-------|

   ## Completed

   | # | Action | Owner | Completed |
   |---|--------|-------|-----------|
   ```

4. **MEMORY.md** — empty template:
   ```markdown
   # <Name> — Memory

   Working memory for <Name>. Organised into standing knowledge (durable across sessions) and session logs (per-engagement records).

   ## Standing

   Durable rules, decisions, and baselines. All read on startup.

   ## Sessions

   Per-session logs. Most recent 2 read on startup.
   ```

5. **context.md** — agent-specific startup context:
   ```markdown
   ---
   scope: <scope or workspace name>
   title: <role title>
   ---

   # Context

   ## Startup Context

   Additional files to read on startup, relative to workspace root.

   - <project-specific paths, e.g., projects/platform-simplification/CLAUDE.md>

   ## Project Files

   Files to check for updates during Session End Protocol.

   - <project-specific paths>
   ```

   In single-domain workspaces, startup paths reference project files directly (e.g., `projects/foo/CLAUDE.md`). In multi-domain workspaces, paths include the area prefix (e.g., `GoAspire/projects/foo/CLAUDE.md`).

6. **Update the workspace root `CLAUDE.md`** (single-domain) or **`<scope>/CLAUDE.md`** (multi-domain) — add the agent to the Agents table (name, role title)

7. **Update `Agents/CONVENTIONS.md`** — add the name to the Reserved list

8. **Create initial baseline** at `<agent-dir>/memory/standing/<date>-baseline.md`:
   - Capture the current state of the project this agent is scoped to
   - Note what exists, what's in progress, what's pending

Note: Individual per-agent skill files are **not** created. All agents are invoked through the `/agent` router (e.g., `/agent <name>`).

### Phase 7: Verify

After creation:
- List the created files
- Show the invocation command (`/agent <name>`)
- Remind the principal to test with `/agent <name>` in a new conversation

## Rules

- Do NOT skip phases or combine them. Each phase is a conversation.
- Do NOT create files until Phase 6. Phases 1-5 are pure discussion.
- If the principal changes their mind about something from an earlier phase, update the affected drafts before proceeding.
- The agent being created must follow ALL conventions in `Agents/CONVENTIONS.md`.
