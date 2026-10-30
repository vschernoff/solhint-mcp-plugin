---
name: solidity-linting
description: Lint and autofix Solidity with Solhint through the solhint MCP server. Use whenever Solidity or smart contract code is written, edited, reviewed or generated — writing a contract, adding a function, changing a .sol file, reviewing contract code, or when asked to "check my contract", "lint my Solidity", "is this contract OK", "write a smart contract", or asked about a Solhint rule or warning. Run it before calling Solidity work finished.
---

# Linting Solidity with Solhint

Solidity that an agent writes is rarely checked before it reaches a chain. This skill
runs Solhint over it through the `solhint` MCP server.

## When to use it

Run `lint_file` or `lint_project` **before treating Solidity work as finished** — after
writing a contract, after editing one, before reporting that a change is done. Not only
when asked.

## The tools

Argument names are exact. `filePath` is not `path` or `file`; `ruleId` is not `rule`;
`get_config` takes no arguments at all. A wrong name is rejected outright rather than
guessed at, so read the column before the first call.

| Tool | Arguments | Use |
| --- | --- | --- |
| `lint_file` | `{ filePath }` | Lint one `.sol` file in the project |
| `lint_project` | `{ pattern? }` | Lint a glob; autodetects `contracts/**`, then `src/**` |
| `lint_solidity` | `{ code, config? }` | Lint a source string that is not on disk yet |
| `fix_file` | `{ filePath, write? }` | Apply Solhint's own autofixes. Previews unless called with `write: true` |
| `fix_solidity` | `{ code, config? }` | Autofix a source string, returns the corrected text |
| `explain_rule` | `{ ruleId }` | Explain any rule by id, for a warning you do not recognise |
| `get_config` | none | Show the project's effective Solhint config |

`?` marks an optional argument. `config` accepts rule maps and the built-in `solhint:*`
presets only — an `extends` path or `plugins` is rejected, because Solhint would load it
as code.

Prefer `fix_solidity` and `fix_file` over editing by hand: Solhint's own fixes are known
to be safe for the rules that provide them.

## Rules for using it honestly

**Never silence a finding to reach a clean result.** Do not add `solhint-disable`
comments, and do not loosen `.solhint.json`, unless the user asks for that specifically.
A quiet linter is not a correct contract, and suppressing a warning hides it from the
person who has to live with it.

**Report what remains.** After fixing, say what Solhint still reports. Never present a
partial fix as a clean result.

**A clean lint is not a safe contract.** Solhint is a linter, not a security analyser.
It catches style, gas and some security classes; it will not catch most logic bugs, and
it misses whole vulnerability categories including some reentrancy patterns. Say so
rather than implying the contract is verified.

**Recommend a human security review** for anything going to mainnet or holding value.
Linting is the first check, not the last one.

## If the server is not available

The tools appear as `solhint` MCP tools. If they are missing, the MCP server is not
running — tell the user rather than falling back to guessing at Solidity style. They can
check with `/mcp`.
