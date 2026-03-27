# Superpowers Skills Adaptation Guide

> **Purpose**: This document provides complete instructions for Claude Code to adapt the [obra/superpowers](https://github.com/obra/superpowers) skill files for use in Claude.ai conversations and Claude Code WITHOUT the superpowers plugin installed.
>
> **Core principle**: Preserve the original English text as much as possible. Only modify what is structurally incompatible. The original wording carries carefully tested value — do not rewrite, rephrase, or "improve" it.

---

## 1. Background & Goals

The user wants to use the superpowers SKILL.md files as custom skills in their Claude environment. The skills were written for Claude Code with the superpowers plugin, which provides:

- A `Skill` tool (to invoke/load skills by name)
- A `TodoWrite` / `TodoRead` tool (to track checklist items)
- Subagent dispatch (`claude -p` to spawn separate Claude processes)
- `EnterPlanMode` interception (to redirect Claude's native plan mode)
- Plugin-managed session hooks (auto-loading `using-superpowers` at session start)

**Without the plugin, these features don't exist.** Everything else — git, bash, file I/O, tests, PR workflows — works normally in Claude Code.

The user already has two adapted skills (`superpowers-core` and `superpowers-code`) written in Korean. These new adapted skills will be **English-language, minimal-diff adaptations** of the originals, intended to eventually replace or complement those Korean skills.

---

## 2. Source Repository

```
https://github.com/obra/superpowers/tree/main/skills
```

### Skills to adapt (14 total)

| Directory | Priority | Estimated diff size |
|---|---|---|
| `brainstorming` | HIGH | Small |
| `writing-plans` | HIGH | Small |
| `test-driven-development` | HIGH | Minimal |
| `systematic-debugging` | HIGH | Minimal |
| `verification-before-completion` | HIGH | Minimal |
| `executing-plans` | MEDIUM | Small-Medium |
| `using-git-worktrees` | MEDIUM | Minimal |
| `finishing-a-development-branch` | MEDIUM | Minimal |
| `requesting-code-review` | MEDIUM | Small |
| `receiving-code-review` | MEDIUM | Minimal |
| `using-superpowers` | MEDIUM | Medium |
| `writing-skills` | LOW | Medium |
| `subagent-driven-development` | LOW | Large (may be impractical) |
| `dispatching-parallel-agents` | LOW | Large (may be impractical) |

---

## 3. What to Clone

```bash
git clone https://github.com/obra/superpowers.git /tmp/superpowers-source
```

The skill files are in `/tmp/superpowers-source/skills/`. Each skill is a directory containing at minimum a `SKILL.md`, and optionally supplementary `.md` files and `scripts/`.

---

## 4. Adaptation Rules

### 4.1 Golden Rule

**If you're unsure whether to change something, DON'T.** The originals were tested extensively. Only change what is provably incompatible with the non-plugin environment.

### 4.2 Things to REMOVE or REPLACE

#### A. `TodoWrite` / `TodoRead` references

These tools don't exist outside the plugin.

- **Find**: Any line mentioning `TodoWrite`, `TodoRead`, or "Use TodoWrite to create todos for EACH item"
- **Replace with**: `Track each checklist item explicitly — write them out and check them off as you go. Do not rely on mental tracking.`
- **Or simply delete** the TodoWrite-specific instruction while keeping the checklist itself intact.

Example:
```
# BEFORE
IMPORTANT: Use TodoWrite to create todos for EACH checklist item below.

# AFTER
IMPORTANT: Track each checklist item explicitly in your responses — write them out and mark them complete as you go.
```

#### B. `Skill` tool invocations

References like "Invoke the writing-plans skill" or "Use the Skill tool to load superpowers:brainstorming" assume the plugin's Skill tool.

- **Find**: `Invoke _____ skill`, `invoke superpowers:_____`, `Use the Skill tool`
- **Replace with**: `Follow the [skill-name] process` or `Apply the [skill-name] skill instructions`
- Keep the skill NAME — just change the invocation mechanism.

Example:
```
# BEFORE
Invoke the writing-plans skill to create a detailed implementation plan.

# AFTER
Follow the writing-plans process to create a detailed implementation plan.
```

#### C. Subagent dispatch

Any instruction to spawn a separate Claude process (`claude -p`, "dispatch a subagent", "launch an implementer subagent").

- **Find**: `dispatch`, `subagent`, `claude -p`, `spawn`, `implementer agent`
- **Replace with**: Instruction to do the work directly, sequentially. Keep the WHAT (task definition, context isolation principles, review criteria) but remove the HOW (process spawning).

Example:
```
# BEFORE
Dispatch an implementer subagent with:
- The task description
- Relevant file context
- Success criteria

# AFTER
For each task:
- Work on one task at a time
- Include only the relevant file context
- Verify against the success criteria before moving to the next task
```

For skills that are ENTIRELY about subagent orchestration (`subagent-driven-development`, `dispatching-parallel-agents`), extract only the universally applicable principles into a simplified version. Flag these to the user as "heavily modified" and let them decide whether to keep them.

#### D. `EnterPlanMode` interception

The `using-superpowers` skill has logic to intercept Claude's native plan mode.

- **Find**: `EnterPlanMode`, `plan mode`, references to intercepting Claude's planning behavior
- **Action**: Remove entirely. This is plugin infrastructure, not a transferable principle.

#### E. Plugin-specific platform references

- **Find**: "In Claude Code: Use the Skill tool", "In Gemini CLI:", "In other environments:", references to `CLAUDE.md`, `GEMINI.md`, `AGENTS.md` as configuration files, `hooks.json`, `find-skills`, `.codex/`, `.opencode/`
- **Action**: Remove platform-switching sections. Keep only the universal instructions.

#### F. Spec-document-reviewer subagent loop (brainstorming skill)

The brainstorming skill dispatches a `spec-document-reviewer` subagent to review design docs.

- **Find**: "dispatch spec-document-reviewer subagent", "Spec review loop"
- **Replace with**: A self-review checklist instruction. Something like:
```
Before presenting the design to the user, run a self-review:
- No placeholders (TBD, TODO, "later", "similar to")
- No internal contradictions
- Scope matches what was discussed — no scope creep
- No ambiguous sections
- YAGNI applied — no unnecessary features
```
(Note: The `superpowers-core` skill already has a good Korean version of this checklist — you may reference it for the content, but write the adapted version in English matching the original's tone.)

#### G. `<SUBAGENT-STOP>` blocks

Some skills have `<SUBAGENT-STOP>` markers that tell subagents to skip the `using-superpowers` skill.

- **Action**: Remove entirely.

### 4.3 Things to KEEP as-is

Everything not listed in 4.2 stays exactly as written. Specifically:

- All git operations (worktrees, branches, commits, PRs)
- All bash commands and script references (including `find-polluter.sh`)
- All file paths (`docs/plans/YYYY-MM-DD-...`)
- All process flows and checklists
- All anti-pattern tables and rationalization prevention
- All Mermaid/Graphviz diagrams (the `digraph` blocks)
- All principle statements ("YAGNI", "one question at a time", etc.)
- The YAML frontmatter structure (`name`, `description`)
- References to companion `.md` files within the same skill directory (e.g., `root-cause-tracing.md`, `defense-in-depth.md`)

### 4.4 Supplementary files

Each skill directory may contain additional `.md` files and `scripts/`. Rules:

- **`.md` companion files** (e.g., `root-cause-tracing.md`, `defense-in-depth.md`, `condition-based-waiting.md`): Copy as-is. These are reference content, not tool-dependent.
- **`scripts/` directory**: Copy as-is. These are bash scripts that work in Claude Code. Exception: `brainstorming/scripts/start-server.sh` (visual companion server) — skip this and `visual-companion.md` as they require a local browser.
- **`spec-document-reviewer-prompt.md`** (in brainstorming): Skip — the reviewer subagent dispatch is being removed.

### 4.5 Frontmatter `description` field

The `description` field is critical for skill triggering. Adapt it following these rules:

- Keep the original description text
- Remove references to other skills by invocation name if they imply plugin mechanics
- Add a note if the skill was modified: append `(Adapted from obra/superpowers — subagent features removed)` at the end of the description for heavily modified skills
- For minimally modified skills, no annotation needed

---

## 5. Output Structure

Create the adapted skills in this structure:

```
superpowers-adapted/
├── brainstorming/
│   └── SKILL.md
├── writing-plans/
│   └── SKILL.md
├── test-driven-development/
│   └── SKILL.md
├── systematic-debugging/
│   ├── SKILL.md
│   ├── root-cause-tracing.md
│   ├── defense-in-depth.md
│   ├── condition-based-waiting.md
│   ├── condition-based-waiting-example.ts
│   └── find-polluter.sh
├── verification-before-completion/
│   └── SKILL.md
├── executing-plans/
│   └── SKILL.md
├── using-git-worktrees/
│   └── SKILL.md
├── finishing-a-development-branch/
│   └── SKILL.md
├── requesting-code-review/
│   └── SKILL.md
├── receiving-code-review/
│   └── SKILL.md
├── using-superpowers/
│   └── SKILL.md
├── writing-skills/
│   └── SKILL.md
├── subagent-driven-development/
│   └── SKILL.md  (if salvageable)
└── dispatching-parallel-agents/
    └── SKILL.md  (if salvageable)
```

---

## 6. Step-by-Step Procedure

### Phase 1: Clone and inspect

```bash
git clone https://github.com/obra/superpowers.git /tmp/superpowers-source
ls /tmp/superpowers-source/skills/
```

For each skill directory, read the SKILL.md and note which adaptation rules (from Section 4.2) apply.

### Phase 2: Adapt HIGH priority skills first

Process order: `verification-before-completion` → `test-driven-development` → `systematic-debugging` → `brainstorming` → `writing-plans`

For each:

1. Copy the entire skill directory to the output location
2. Open SKILL.md
3. Search for each pattern in Section 4.2 (TodoWrite, Skill tool, subagent, EnterPlanMode, platform refs, SUBAGENT-STOP)
4. Apply the minimum change described
5. Do NOT touch any line that doesn't match a pattern from 4.2
6. Show the user a diff of what changed before moving to the next skill

### Phase 3: Adapt MEDIUM priority skills

Process order: `executing-plans` → `using-git-worktrees` → `finishing-a-development-branch` → `requesting-code-review` → `receiving-code-review` → `using-superpowers`

Same procedure as Phase 2.

### Phase 4: Assess LOW priority skills

For `subagent-driven-development` and `dispatching-parallel-agents`:

- Read the full SKILL.md
- Identify what percentage of the content is subagent-specific
- If >50% is subagent-specific, extract only the universal principles into a simplified SKILL.md and flag it as heavily adapted
- Present the assessment to the user before proceeding

For `writing-skills`:

- Remove subagent testing sections
- Keep the TDD-for-documentation principles
- Keep the anti-rationalization techniques
- Keep the skill structure guidelines

### Phase 5: Present results

For each adapted skill, show:
- The specific lines changed (as a diff or before/after)
- A brief note on what was changed and why
- Any judgment calls made

---

## 7. Quality Checklist

Before delivering each adapted skill, verify:

- [ ] No references to `TodoWrite` or `TodoRead`
- [ ] No references to the `Skill` tool or `invoke superpowers:___`
- [ ] No subagent dispatch instructions (unless intentionally kept as a note)
- [ ] No `EnterPlanMode` references
- [ ] No `<SUBAGENT-STOP>` blocks
- [ ] No platform-switching sections (Gemini CLI, Codex, OpenCode)
- [ ] All git commands preserved
- [ ] All bash commands preserved
- [ ] All checklists preserved (with tracking mechanism updated)
- [ ] All anti-pattern tables preserved
- [ ] All principle statements preserved verbatim
- [ ] YAML frontmatter intact with valid `name` and `description`
- [ ] Companion `.md` files copied if applicable
- [ ] No rewording of original text beyond what Section 4.2 requires

---

## 8. What NOT to Do

- **Do NOT translate to Korean.** Keep all content in English.
- **Do NOT rewrite sections** "for clarity" or "to improve flow." The original phrasing is deliberate.
- **Do NOT merge skills** into a single file. Keep each skill as a separate directory.
- **Do NOT add new content** (new anti-patterns, new principles, new steps) unless strictly required by a removal.
- **Do NOT remove the Graphviz/Mermaid diagrams** even if they reference removed steps — update the diagram to match the changes instead.
- **Do NOT change file paths** like `docs/plans/YYYY-MM-DD-...` — these work in Claude Code.
- **Do NOT remove references to companion `.md` files** within the same skill directory — these are being copied over.

---

## 9. Notes on Specific Skills

### brainstorming/SKILL.md

- The checklist step "Spec review loop — dispatch spec-document-reviewer subagent" → replace with self-review checklist
- "Transition to implementation — invoke writing-plans skill" → "Transition to implementation — follow the writing-plans process"
- The `visual-companion.md` and `scripts/start-server.sh` → skip (requires local browser server)
- The `spec-document-reviewer-prompt.md` → skip (subagent prompt)
- The process flow digraph has nodes for spec review loop — update to reflect the self-review replacement

### using-superpowers/SKILL.md

This is the "meta-skill" that governs how other skills are used. It needs the most structural changes:
- Remove all platform-specific sections (Claude Code Skill tool, Gemini CLI, Codex, OpenCode)
- Remove EnterPlanMode interception
- Remove SUBAGENT-STOP block
- Keep: skill priority order, rigid vs flexible distinction, rationalization table, the principle that process skills come before implementation skills

### subagent-driven-development/SKILL.md

Likely >70% subagent-specific. Salvageable principles:
- Context isolation (give each task only what it needs)
- Two-stage review (spec compliance first, then code quality)
- Status protocol (DONE, DONE_WITH_CONCERNS, BLOCKED, NEEDS_CONTEXT)
- Model selection guidance (cheaper models for mechanical tasks, capable models for architecture)

Consider creating a stripped-down version titled something like "task-driven-development" that keeps these principles but removes the multi-process orchestration.

### dispatching-parallel-agents/SKILL.md

Likely >80% subagent-specific. May not be worth adapting. The user should decide.

---

## 10. Delivering the Final Output

After all skills are adapted:

1. Place the complete `superpowers-adapted/` directory where the user specifies
2. Provide a summary table showing each skill, what changed, and the diff size
3. Ask the user how they want to install them (as claude.ai custom skills, in a project's `.claude/skills/` directory, etc.)

The user may want to:
- Install some as personal custom skills in claude.ai
- Keep some as project-level skills in specific repos
- Use them as CLAUDE.md references in Claude Code projects

Support whatever installation path they choose.
