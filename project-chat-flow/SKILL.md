---
name: project-chat-flow
description: Initialize a project's project-status.md on first explicit use, then summarize progress or read it when handing work between project chats.
---

# Project Chat Flow

Use this skill only when the user explicitly invokes `$project-chat-flow`. It initializes one `project-status.md` on first use, then coordinates project summaries, next-step planning, and chat handoff through that file.

## Find the project status file

1. Resolve the project root from the current working directory: use the enclosing Git repository root when one exists; otherwise use the current working directory.
2. Read and follow applicable `AGENTS.md` instructions.
3. Search recursively inside the project root for a file named exactly `project-status.md`, without requiring a `docs/` directory. Ignore `.git` internals and do not follow links outside the project root.
4. If no matching file exists, initialize `<project-root>/project-status.md` during this explicit first invocation. Inspect applicable `AGENTS.md`, project overview files, and other relevant evidence; record only verified project goals, completed work, current state, decisions, blockers, and next steps. Mark unknowns as pending confirmation. Do not create a `docs/` directory or any other file. If the invocation also names a clear action, continue with that action after initialization; otherwise report the new file and wait for the next instruction.
5. If multiple copies exist, list their paths and ask which one is authoritative before reading or editing any copy.

If exactly one matching file already exists and the invocation does not state what to do with it, report its path and wait for the user's specific instruction. Do not read or change its contents in this idle case.

## Choose the requested action

Use the user's explicit request to select one action. If the action is unclear, ask one focused question.

### Summarize completed work or prepare a handoff

- Read the selected `project-status.md`, the current conversation, and only the relevant project files. If the project uses Git, check relevant status and diffs as evidence.
- Update the existing file to reflect verified completed work, current state, confirmed decisions, blockers, and the next concrete plan. Preserve its language and existing structure; make the smallest useful edit.
- A useful unified status can include: project goal, completed work, current state, decisions, blockers or open questions, next step with acceptance criteria, and the responsibility for the next chat when the user specified it.
- Keep a single source of truth: include the next-step plan in `project-status.md`; do not create or update `current-plan.md`.
- Never turn an assumption into a completed fact. If no material change is supported, leave the file untouched and say so.
- After a successful update, give the user a concise summary and a copyable next-chat prompt that names the file's discovered path and the next chat's responsibility.

### First use initialization

- Create the status file only when this skill is explicitly invoked and no matching `project-status.md` exists anywhere inside the project root.
- Write a concise initial snapshot based on project files and the current conversation. Include the project's goal, verified completed work, current state, confirmed decisions, blockers or open questions, and the next action if one is established.
- Preserve uncertainty explicitly; if there is not enough evidence to determine a section, mark it as pending confirmation instead of inventing details.

### Receive a handoff in a new chat

- Read the selected `project-status.md` and the project files relevant to the stated next step. Compare the recorded state with current files and Git state where available.
- Summarize what is done, what remains, important decisions, and the immediate next action. Identify stale or conflicting information instead of silently treating it as current.
- Do not edit the status file just because the user asked to read or resume it. Update it only when the user asks to summarize or record progress.
- If the next action is clear, use it when the user explicitly asks to continue. Otherwise report the next action and wait for the user's task instruction.

### Plan or revise the next step

- Use the user's stated goal, the existing status, and relevant project evidence to update the next-step plan section in `project-status.md`.
- If no concrete goal is given, ask before editing. Include goal, scope, ordered steps, and acceptance criteria; note unresolved questions without inventing answers.
- Change only `project-status.md`.

## Boundaries

- Edit only the selected `project-status.md` inside the project root. Create it only under the first-use initialization rule above. Never create, move, or delete `AGENTS.md` or `current-plan.md`; never modify those files, source code, or unrelated files.
- Do not create, switch, or configure chats or select a model or reasoning effort. The user assigns those in the app. Record only the next chat's responsibility when the user provides it, not a model name or effort setting.
- Do not implement a project plan when the user only asks for a summary, handoff, or plan update.

