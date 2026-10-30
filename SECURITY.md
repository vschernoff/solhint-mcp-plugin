# Security policy

## Reporting a vulnerability

Report privately through GitHub:
**[open a security advisory](https://github.com/vschernoff/solhint-mcp-plugin/security/advisories/new)**.

Please do not open a public issue for a vulnerability.

Expect an acknowledgement within three working days and an assessment within ten.

## What this repository is, and where to look

This repository is the Claude Code plugin: a manifest, a skill, an MCP server declaration
and a lockfile. **It contains no server code.**

The server is [solhint-mcp](https://github.com/vschernoff/solhint-mcp), a separate
package that this plugin pins and installs. A vulnerability in the linting itself — in
how configs, plugins or Solidity source are handled — belongs there, and that repository
has [its own security policy](https://github.com/vschernoff/solhint-mcp/blob/main/SECURITY.md)
describing the surface in detail.

Report here if the problem is in what this repository ships:

- the pinned version or the dependency tree in `package-lock.json`
- the MCP server declaration in `.mcp.json`
- the skill in `skills/solidity-linting/SKILL.md`
- the manifest

If you are unsure, report it here and it will be routed.

## What installing this plugin runs

Claude Code installs the dependencies pinned in `package-lock.json` into the plugin's own
directory, then starts the server from there with `node`. Nothing is fetched at launch.

The dependency install is constrained by Claude Code rather than by this plugin: registry
packages only, exact versions from the lockfile, `--ignore-scripts`, and no access to the
plugin's own npm configuration. That means a dependency cannot run install hooks on your
machine as part of setting this plugin up.

## The exposure worth knowing about

**Solhint executes a project's own plugins and shareable configs.** It resolves them
relative to the working directory and loads them with `require`, so pointing an agent at
an untrusted repository runs that repository's JavaScript — the same exposure as running
`npx solhint` there yourself, but reached automatically rather than deliberately.

This is inherent to running Solhint at all. It is not introduced by this plugin and no
version of it removes the exposure. Treat the directory your agent works in as trusted
input: a repository you would be willing to run `npm install` in.

## What this tool does not do

Solhint is a linter, not a security analyser. A clean result is not evidence that a
contract is safe, and neither this plugin nor the skill it ships changes that. Do not
treat a passing lint as an audit.
