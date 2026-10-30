# solhint-mcp — Claude Code plugin

Lint and autofix Solidity with [Solhint](https://github.com/protofire/solhint) from
inside your coding agent, and have the agent reach for it on its own.

Installing this plugin does two things: it configures the
[solhint-mcp](https://www.npmjs.com/package/solhint-mcp) server, and it adds a skill that
tells the agent to lint Solidity before calling the work finished — so you do not have to
remember to ask.

## Install

At the prompt of a terminal session:

```
/plugin install solhint --marketplace vschernoff/solhint-mcp-plugin
```

Press `y` to add the marketplace, then choose a scope. Requires **Node.js 20 or newer**.
Claude Code installs the server's pinned dependencies when the plugin installs, and runs
it from there — nothing is fetched at launch.

## Try it

Three prompts, each run against this plugin before being written here.

**Ask for a contract and have it checked.**

```
Write a simple escrow contract in src/Escrow.sol and make sure it is clean before you finish.
```

The agent writes the contract, lints it, fixes what Solhint can fix, and reports what is
left. A phrase like *"before you finish"* matters: it is the cue that the work is not done
until it is checked.

**Ask whether existing code is sound.**

```
Is contracts/Vault.sol OK?
```

It lints the file and reports the findings — and separates them from anything the linter
cannot see. On a contract with a reentrancy, a real run answered with the lint warnings
and then, under its own heading, *"Security (not caught by linter)"*.

**Ask what a warning means.**

```
Solhint says 'avoid-low-level-calls' on my contract. What does that mean and should I care?
```

It explains the rule from Solhint's own metadata, for the exact Solhint version your
project runs, and says when the warning is worth acting on and when it is not.

## What the agent gets

Seven tools: `lint_file`, `lint_project`, `lint_solidity`, `fix_file`, `fix_solidity`,
`explain_rule`, `get_config`.

It lints the project you are working in, honours that project's `.solhint.json` and
`.solhintignore`, and applies Solhint's own autofixes rather than rewriting your code by
hand. `fix_file` previews by default and only writes when asked to.

## What it is not

Solhint is a **linter, not a security analyser**. It catches style, gas and some security
classes. It will not catch most logic bugs and it misses whole vulnerability categories,
including some reentrancy patterns. A clean lint is the first check, not proof that a
contract is safe.

The bundled skill says the same thing to the agent, so it does not report a clean lint as
a verified contract.

**Shipping something that holds value?** A linter is not a substitute for a human review.
[Book a call](https://calendly.com/vitaliy-chernov/30min) if you want a smart contract
audit.

## Reporting a problem

Bugs and suggestions: [open an issue](https://github.com/vschernoff/solhint-mcp-plugin/issues).

Security vulnerabilities: see [SECURITY.md](SECURITY.md) — report them privately through
a GitHub security advisory, not a public issue. If the problem is in the linting itself
rather than in what this plugin ships, it belongs in
[solhint-mcp](https://github.com/vschernoff/solhint-mcp); report it either place and it
will be routed.

## Related

- [solhint-mcp](https://github.com/vschernoff/solhint-mcp) — the MCP server this installs,
  usable from any MCP client (Codex, Cursor, Windsurf, Claude Desktop)
- [Solhint](https://github.com/protofire/solhint) — the linter itself, maintained by
  Protofire. This plugin depends on it; it is not part of it.

## Licence

MIT. See [solhint-mcp](https://github.com/vschernoff/solhint-mcp) for the server's own
licence and notices.
