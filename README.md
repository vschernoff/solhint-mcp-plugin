# solhint — Claude Code plugin

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

Press `y` to add the marketplace, then choose a scope. Requires **Node.js 20 or newer**;
the server is fetched with `npx` on first use.

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

## Related

- [solhint-mcp](https://github.com/vschernoff/solhint-mcp) — the MCP server this installs,
  usable from any MCP client (Codex, Cursor, Windsurf, Claude Desktop)
- [Solhint](https://github.com/protofire/solhint) — the linter itself, maintained by
  Protofire. This plugin depends on it; it is not part of it.

## Licence

MIT. See [solhint-mcp](https://github.com/vschernoff/solhint-mcp) for the server's own
licence and notices.
