---
name: spec-loader
description: |
  Automatically activates when Claude Code is working on implementation tasks.
  Fetches relevant spec documents from the spec vault (private GitHub repo)
  to provide Phase 1 research context during Phase 2 implementation.
  Triggers on: implementation work, architecture questions, "check the spec",
  references to design documents, CLAUDE.md mentions of spec files.
---

# Spec Vault Loader

You have access to the spec-vault-github MCP server which contains research
and design documents in a private GitHub repository (the "spec vault").

## Repo Structure

- `ACTIVE/` — Current specs available for implementation. Organized by project subdirectory.
- `COMPLETED/` — Archived specs from finished work. Do NOT read from this
  directory unless explicitly asked to reference historical decisions.

## How to Use

1. **List first, fetch second.** The user controls what gets loaded.
   - Suggest using `/spec-vault:specs` to list available specs before fetching
   - Do NOT proactively fetch specs without the user's direction
   - The user may want to discuss what's available before loading context

2. When the user requests spec(s), fetch them using the `get_file_contents`
   tool from the spec-vault-github MCP server.

   **Before your first MCP call, resolve the repo coordinates via bash:**
   ```bash
   echo "OWNER_REPO:$SPEC_VAULT_REPO"
   echo "BRANCH:${SPEC_VAULT_BRANCH:-main}"
   echo "ACTIVE:${SPEC_VAULT_ACTIVE_DIR:-ACTIVE}"
   ```
   Split SPEC_VAULT_REPO on `/` to get the `owner` and `repo` parameters.
   Do NOT guess these values from the plugin name or any other source.

   - Path: `{ACTIVE_DIR}/{project}/{filename}`
   - Spec files contain endpoint URLs, auth patterns, data schemas, and
     implementation guidance designed for you to act on autonomously

3. After fetching, summarize what was loaded and **wait for direction**.
   Do NOT begin implementation until the user says to proceed. The user may
   want to fetch additional specs, discuss the approach, or scope the work.

4. After implementation is complete and CLAUDE.md + architecture docs are
   updated, inform the user that the spec can be moved to COMPLETED/
   (this is a manual step — the user handles it via git or GitHub UI).

## Important

- Specs are READ-ONLY via MCP. You cannot move or modify files in the vault.
- Do not fetch specs unless they are relevant to the current task.
- Each spec fetch costs API calls and context tokens — be targeted.
- If the user references a spec by name, fetch that specific file rather
  than listing the whole directory first.

## Troubleshooting

If you see an error like:
```
Permission to use mcp__spec-vault-github__get_file_contents
has been auto-denied in dontAsk mode.
```

This means Plan Mode is blocking MCP tools. The user needs to add spec-vault
tools to their permissions allow list in `.claude/settings.json`:

```json
{
  "permissions": {
    "allow": [
      "mcp__spec-vault-github__get_file_contents",
      "mcp__spec-vault-github__list_directory"
    ]
  }
}
```

This is safe — these tools only read from the GitHub API.
