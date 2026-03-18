# Privacy Policy

**spec-vault** — Claude Code Plugin
Ward Software Defined Systems LLC
Effective Date: March 18, 2026

## Overview

spec-vault is an open-source Claude Code plugin that reads files from a user-configured private GitHub repository. It does not collect, store, transmit, or process any personal data.

## What This Plugin Does

spec-vault connects Claude Code to a GitHub repository via the official GitHub MCP Server (maintained by GitHub, not by this plugin). It performs read-only operations to list and fetch Markdown spec files from a repository the user owns or has access to.

## Data Handling

**No data collection.** This plugin does not collect, log, store, or transmit any information about its users.

**No analytics or telemetry.** This plugin contains no tracking, analytics, or telemetry of any kind.

**No external services.** This plugin does not communicate with any servers operated by Ward Software Defined Systems LLC. All network communication occurs between the user's local machine and the GitHub API, mediated by the official GitHub MCP Server.

**No credentials stored.** The plugin requires a GitHub Personal Access Token (PAT) to authenticate with the GitHub API. This token is configured by the user as a local environment variable (`SPEC_VAULT_GITHUB_TOKEN`) and is never read, logged, or transmitted by the plugin itself. The token is passed directly to the GitHub MCP Server process running on the user's machine.

## Third-Party Services

This plugin configures the [official GitHub MCP Server](https://github.com/github/github-mcp-server), which communicates with the GitHub API on the user's behalf. GitHub's handling of API requests is governed by [GitHub's Privacy Statement](https://docs.github.com/en/site-policy/privacy-policies/github-general-privacy-statement). Ward Software Defined Systems LLC has no access to or visibility into these API interactions.

## Open Source

This plugin is open source under the MIT License. The complete source code is available at [https://github.com/ward-software-defined-systems/spec-vault](https://github.com/ward-software-defined-systems/spec-vault) and can be audited by anyone.

## Changes to This Policy

If this policy changes, the updated version will be posted to the plugin's GitHub repository with a new effective date. Since the plugin collects no data, material changes are unlikely.

## Contact

Ward Software Defined Systems LLC
GitHub: [https://github.com/ward-software-defined-systems](https://github.com/ward-software-defined-systems)
