# bstack marketplace

A one-plugin marketplace holding [bstack](./bstack), rigorous agent workflows that run on any agent harness.

## Install

**Codex, Cursor, GitHub Copilot, VS Code, Kiro, ChatGPT** read the [Agent Plugins](https://agent-plugins.org/) standard. Point your client at this repo and install the plugin from the `bstack/` directory.

**Claude Code:**

```bash
/plugin marketplace add binarystride/bstack
```

```bash
/plugin install bstack@bstack
```

**Cursor** reads either manifest. Add this repo as a marketplace and install `bstack`.

## Layout

```
.agents/plugins/marketplace.json  Agent Plugins marketplace (Codex et al.)
.claude-plugin/marketplace.json   Claude Code marketplace
.cursor-plugin/marketplace.json   Cursor marketplace
bstack/
  plugin.json                     Agent Plugins v1 manifest
  .claude-plugin/plugin.json      Claude Code manifest
  .cursor-plugin/plugin.json      Cursor manifest
  skills/                         13 skills (one holds the 19 principles)
  agents/                         3 review subagents (Claude Code only)
  docs/guide/                     the walkthrough
```

One `skills/` tree, three manifests. The Agent Plugins spec covers skills and MCP servers only, so the subagents load only where the harness reads an `agents/` directory. Nothing breaks without them; see the plugin [readme](./bstack/README.md#subagents).

## License

Private. All rights reserved.
