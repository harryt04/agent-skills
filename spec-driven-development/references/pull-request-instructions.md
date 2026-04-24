# Pull Request Description Instructions

These instructions are self-contained. An agent following them should be able to produce a complete PR description from a branch diff without additional context from the dispatching session.

---

## Session Folder

Before writing the PR description, resolve the selected session folder:

- Use `.agents/specs/<work-item-id>/` when the developer has provided or already implied a work item context.
- Otherwise use `.agents/spec/`.

Write new PR description artifacts to `pr-description.md` in that selected folder.

## Goal

Compare the current branch against master/main and write a concise, well-structured pull request description.

---

## Step 1: Analyze the Branch Diff

Use sub-agents if the diff is large. Produce a summary that captures:

- What changed and why (new features, bug fixes, refactors, config changes)
- Files added, modified, and deleted
- Any breaking changes or migration steps required
- Dependencies added or removed

---

## Step 2: Write the PR Description

Save the description to: **`<session-folder>/pr-description.md`**

### Length constraints

- **Hard limit:** 4,000 characters. Exceeding this will cause the description to be truncated or rejected by most platforms.
- **Target:** Under 1,000 characters. Reviewers read many PRs — brevity is a feature.

### Formatting rules

Write in clean markdown that renders well in browser-based PR text boxes. These guidelines apply broadly across platforms (GitHub, Azure DevOps, Bitbucket, GitLab):

- **Numbered lists over bullet lists.** Numbered lists create a natural reference system ("see item 3") and impose structure that helps reviewers scan quickly.
- **Tables for structured data.** When comparing before/after states, listing files with their changes, or showing config values, a table is easier to scan than prose.
- **Prefer structure over prose.** A PR description is a reference document, not an essay. If something can be a list or a table, make it one.
- **Code blocks for file paths, commands, and snippets.** Use backtick-fenced blocks with a language hint when applicable.

If the target platform is Azure DevOps, consult `references/ado-wiki-markdown-reference.md` (relative to this skill's directory) for ADO-specific markdown quirks — notably that Mermaid uses `graph` not `flowchart`, `[[_TOC_]]` is case-sensitive, and some advanced features aren't supported.

### Content structure

There is no rigid template — adapt to the size and nature of the changes. For small PRs, a single paragraph with a short list may be enough. For larger PRs, a structure like this works well:

1. **Summary** — one or two sentences on what this PR does and why
2. **Changes** — numbered list or table of what was changed
3. **Testing** — how the changes were tested or how a reviewer can verify them
4. **Notes** (optional) — breaking changes, migration steps, follow-up work, or anything the reviewer should know

---

## Step 3: Present to the Developer

Show the description and ask if they want any changes.

Remind the developer: when pasting the PR description into a platform's web UI, use **Paste and Match Style** (Cmd+Shift+V on Mac, Ctrl+Shift+V on Windows/Linux) to avoid rich-text formatting artifacts that clipboard copy can introduce.
