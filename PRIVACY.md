# Privacy

Short version: this plugin stores nothing and sends nothing anywhere of its own accord.
What your agent does with the results is a separate matter, and it is covered below
because it is the part people usually mean.

## What the plugin reads

When you ask your agent to lint, the server reads, from the project directory you are
working in:

- the `.sol` files you point it at
- `.solhint.json` or whichever Solhint config the project uses
- `.solhintignore`, if present

It reads nothing outside the project directory. Paths and glob patterns that resolve
outside the project root are rejected.

## What it stores

Nothing. There is no cache, no log file, no database, no telemetry. The server holds
your source in memory for as long as it takes Solhint to parse it, and then the process
ends.

## What it sends

Nothing of its own. The server makes no network requests. It runs Solhint through its
JavaScript API, in the same process, on your machine.

The one piece of network activity around this plugin happens before it runs: when you
install the plugin, Claude Code downloads the packages pinned in `package-lock.json`
from the npm registry. That is an ordinary package install, it happens once, and after
it the plugin is self-contained.

## What your agent does with the results

This is the part worth being clear about, because it is where your code actually travels.

The server returns lint findings to the agent that called it — rule names, line numbers,
messages, and for the `fix_*` tools the corrected source. Those results become part of
your conversation with that agent, and go wherever that conversation goes. If you are
using Claude, they reach Anthropic exactly as anything else you paste into a conversation
does.

That is a property of using an AI coding agent on your code at all, not something this
plugin introduces — your agent has already read the files to work on them. But it is the
honest answer to "where does my contract go", and it is not "nowhere".

## Personal data

The plugin is not designed for personal data and does not look for any. It reads Solidity
source. If your source contains personal data, the plugin treats it as it treats any other
text, and the section above applies.

## Children

This plugin is a developer tool. It is not directed at anyone under 18.

## Questions

Open an issue: https://github.com/vschernoff/solhint-mcp-plugin/issues
