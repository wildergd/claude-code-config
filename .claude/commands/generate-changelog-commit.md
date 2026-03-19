---
name: generate-changelog-commit
description: Generate a changelog entry and create a git commit based on the code changes.
---

You are helping commit changes and update the changelog documentation.

## Task Overview

1. Review the staged git changes (use `git diff --staged` or `git status`)
2. If there is no change to commit, check latest commit made and ask if user wants to
   update changelog for that commit. If user says no, exit.
3. Create a git commit with a technical, structured commit message
4. Update the **single unified changelog** at `CHANGELOG.md` with detailed, technical descriptions of what changed

## Step 1: Create Git Commit

Write a conventional commit message with this structure:

```
<type>: <short description>
```

Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`, `perf`, `style`, `build`, `ci`

## Step 2: Update Unified Changelog

Update `CHANGELOG.md` - the single source of truth for all significant code changes across the entire platform.

### Changelog Format

```markdown
# Changelog

## 2025-01-22

- **[Feature]** Short title of the change
  ### Changes
  - Specific component, hook, or module added/changed and what it does
  - Another concrete change — name the thing, describe the behavior
  - Architectural decision or approach taken and why
  - New interfaces, config options, API parameters, or data structures introduced
  - Breaking changes or migration steps if applicable
  ### Known Issues
  - Any known limitation, edge case, or caveat worth flagging
  - If none, omit this section entirely

- **[Fix]** Short title of the fix
  ### Changes
  - Root cause of the bug and what triggered it
  - What specifically was changed to fix it
  - Any related defensive changes or cleanups made alongside the fix
  ### Known Issues
  - Remaining edge cases or limitations if any
```

**Entry Type Tags**:

- `**[Feature]**` - New functionality, capabilities, or significant additions
- `**[Fix]**` - Bug fixes, crash fixes, incorrect behavior corrections
- `**[Refactor]**` - Structural or architectural changes with no behavior change
- `**[Docs]**` - Documentation additions or improvements
- `**[Perf]**` - Performance improvements
- `**[Chore]**` - Dependency updates, tooling, config changes

### Date Organization Rules

- Organize entries by date: `## YYYY-MM-DD` (ISO format)
- Newest dates appear first (reverse chronological order)
- Each date can have multiple entries with different type tags
- Use today's date when adding new entries

### Changelog Philosophy

This is a **technical developer changelog** used to track what changed in the codebase and why. It exists so that developers — including your future self — can understand the history of significant changes without having to dig through git blame or source code.

### Changelog Guidelines

- **Read the actual diff** before writing — do not generate entries from file names or commit messages alone
- Name the specific components, hooks, modules, services, and systems involved
- Describe what each added/changed thing does, not just that it was added
- Explain the root cause for fixes, not just the symptom
- Describe architectural decisions and trade-offs where relevant
- Include new interfaces, config keys, API shapes, or data structures introduced
- Call out breaking changes explicitly
- Write **4–8 items** under `### Changes` — do not collapse everything into one or two vague lines
- Only include `### Known Issues` if there are real limitations or caveats; otherwise omit it
- Skip trivial changes: minor typo fixes, formatting-only changes, inconsequential renames

### What to Include

✅ **Include**:

- New features, nodes, endpoints, tools, or system capabilities
- Bug fixes — especially the root cause and what was changed
- Refactors that meaningfully change how a system works internally
- New configuration options, environment variables, or API parameters
- Data model or schema changes
- Breaking changes and migration steps
- Performance improvements and what caused them
- Dependency upgrades that affect behavior
- Security fixes

❌ **Skip**:

- Minor code style or formatting changes
- Trivial variable or file renames with no behavioral impact
- Test-only changes (unless they reveal something important about the system)
- Documentation typo fixes

### Examples

❌ Bad (too vague — does not tell a developer what actually changed):

```markdown
## 2025-01-22

- **[Feature]** System lock during backend initialization
  ### Changes
  - Added system lock feature
  - Improved error handling
  - Updated routing
```

✅ Good (technical, specific, names real components and behaviors):

```markdown
## 2025-01-22

- **[Feature]** System lock screen during backend initialization
  ### Changes
  - Added `SystemLockedAlert` component to handle system lock states.
  - Introduced `SystemNotReadyPage` for user feedback during backend initialization.
  - Implemented `useSystemHealth` and `useSystemLockStatus` hooks and helpers for system health checks and lock status.
  - Refactored and renamed existing hooks for consistency with new naming conventions.
  - Updated routing and component exports to include new system lock features.
  - Enhanced error handling to support system initialization and prevent dispatching certain errors during the boot sequence.
  - Added a `LockableQueryClient` to pause all React Query fetches and manage query state during system initialization.
  ### Known Issues
  - The lock mechanism depends on backend error codes; if these change, frontend logic may need updates.
  - Overlay mode for system lock is available but not enabled by default.

- **[Fix]** Intermittent auth failures on concurrent API requests
  ### Changes
  - Root cause: token validation middleware was executing out of order during simultaneous requests, causing a race condition that rejected valid sessions.
  - Serialized token validation through a request-scoped lock in `authMiddleware.ts`.
  - Removed redundant defensive null checks in downstream endpoint handlers that had been masking the underlying issue.
  - Added integration test covering concurrent auth scenarios to prevent regression.

- **[Feature]** Parent ID support for container resolution
  ### Changes
  - Extended the `ResolveContainerBy` union type to include a `parentId` case alongside existing `name`, `externalId`, and `id` options.
  - Added `resolveByParentId()` handler in `resolveContainer.implementation.ts` querying containers by `parent_id` foreign key.
  - The Resolve Container node UI now exposes a "Parent ID" input field wired to the new resolution case.
  - When multiple children match the parent ID, returns the first result ordered by creation date ascending.
  ### Known Issues
  - Resolving by parent ID returns only one container even if multiple children exist; multi-match support is not yet implemented.
```

## Process for Adding New Entries

1. Run `git diff --staged` or `git diff HEAD~1` to review the actual diffs — read the code, not just file names
2. Identify what components, hooks, modules, and systems were touched and what they do
3. Draft the full changelog entry and propose it to the user before writing:
   - Show the proposed commit message
   - Show the full proposed changelog entry including all `### Changes` bullets and `### Known Issues` if relevant
   - Wait for confirmation or edits
4. Create the commit with the technical commit message
5. Write the entry to `/CHANGELOG.md` under today's date, creating the date section if needed
6. Stage the changelog (`git add CHANGELOG.md`) and ask if the user wants to amend it into the latest commit

For **internal-only changes** (pure refactors, tooling, tests):
- Still consider a `[Refactor]` or `[Chore]` entry if significant enough to matter to a future developer
- Ask the user if they want to document it or skip

## Changelog Location

**Single unified changelog**: `/CHANGELOG.md`

This file tracks all significant changes across:

- Backend Services (node definitions, core functionality)
- API (endpoints, authentication, middleware)
- Integrations (plugins and connectors)
- Frontend / Portal (components, hooks, routing)

If `/CHANGELOG.md` doesn't exist, create it with:

```markdown
# Changelog

Entries are organized by date, newest first.

---

```

## Important Notes

- Always stage changelog updates (`git add CHANGELOG.md`)
- **Always use today's date**: `## YYYY-MM-DD`
- Create the date section if it doesn't exist
- Tag every entry: `**[Feature]**`, `**[Fix]**`, `**[Refactor]**`, `**[Docs]**`, `**[Perf]**`, `**[Chore]**`
- Maintain reverse chronological order (newest at top)
- **Read the actual diff** before writing — entries must reflect real code changes, not assumptions
- Write 4–8 items under `### Changes` — a one or two line entry is almost always incomplete
- Only add `### Known Issues` when there are real caveats worth flagging; do not force it