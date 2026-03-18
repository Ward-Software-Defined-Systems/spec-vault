---
description: List and fetch spec documents from the spec vault
---

Manage spec documents from the spec vault repository. This command supports
three usage patterns depending on the arguments provided.

## Usage Patterns

### No arguments → List available specs
List all spec files in the ACTIVE directory of the spec vault repo, organized
by project subdirectory. Display each file with its relative path so the user
can reference it in subsequent commands. This is the user's control layer —
they review what's available before deciding what to load.

### One or more filenames → Fetch specific specs
`$ARGUMENTS` contains one or more filenames or paths relative to ACTIVE/.

Fetch and display the contents of each named spec file. Examples:
- `OPENSEARCH-INTEGRATION.md` → fetch ACTIVE/**/OPENSEARCH-INTEGRATION.md
- `CEF-PARSER-SPEC.md ENRICHMENT-STRATEGY.md` → fetch both files
- `wardsondb/BITMAP-INDEX-SPEC.md` → fetch with explicit project path

When multiple files are requested, fetch and present each one with a clear
separator so the user can review them in context and discuss before proceeding.

### "completed" → List archived specs
List files under COMPLETED/ for historical reference. Does not fetch content
by default — just lists what has been archived.

## Behavior

**FIRST: Read the environment variables before making any MCP calls.**
Run a bash command to read the required configuration:

```bash
echo "OWNER_REPO:$SPEC_VAULT_REPO"
echo "BRANCH:${SPEC_VAULT_BRANCH:-main}"
echo "ACTIVE:${SPEC_VAULT_ACTIVE_DIR:-ACTIVE}"
echo "COMPLETED:${SPEC_VAULT_COMPLETED_DIR:-COMPLETED}"
```

Parse the SPEC_VAULT_REPO value to extract the `owner` and `repo` parameters
(format is `owner/repo`, e.g., `Ward-Software-Defined-Systems/DevOps-ClaudeCode-Specs`).
Split on `/` — the part before the slash is `owner`, the part after is `repo`.

Do NOT guess the owner or repo name from the plugin name or any other source.
The env vars are the only source of truth.

**THEN** use the spec-vault-github MCP server (`get_file_contents` tool) with
the resolved `owner`, `repo`, `path`, and `branch` parameters.

### Disambiguation

If a filename is given without a project subdirectory, search all project
directories under ACTIVE/ for a match. If multiple matches exist for an
ambiguous filename (e.g., two projects both have `API-SCHEMA.md`), list the
matches with their full paths and ask the user to clarify which one they want.

### After Fetching

Summarize what was loaded (file names, approximate size, key topics) and
**wait for the user's direction**. Do NOT begin implementation unprompted.
The user may want to:
- Fetch additional specs
- Discuss the contents
- Scope the implementation work
- Issue the command again for different files
