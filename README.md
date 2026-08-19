# pstack marketplace

A one-plugin marketplace holding [pstack](./pstack), a portable build of [poteto's pstack](https://github.com/cursor/plugins/tree/main/pstack) that runs on any agent harness.

## Install

**Codex, Cursor, GitHub Copilot, VS Code, Kiro, ChatGPT** read the [Agent Plugins](https://agent-plugins.org/) standard. Point your client at this repo and install the plugin from the `pstack/` directory.

**Claude Code:**

```bash
/plugin marketplace add <your-org>/<this-repo>
```

```bash
/plugin install pstack@pstack
```

**Cursor** reads either manifest. Add this repo as a marketplace and install `pstack`.

## Layout

```
.claude-plugin/marketplace.json   Claude Code marketplace
.cursor-plugin/marketplace.json   Cursor marketplace
pstack/
  plugin.json                     Agent Plugins v1 manifest
  .claude-plugin/plugin.json      Claude Code manifest
  .cursor-plugin/plugin.json      Cursor manifest
  skills/                         43 skills, shared by all three
  agents/                         2 subagents (Claude Code and Cursor only)
  docs/guide/                     the walkthrough
```

One `skills/` tree, three manifests. The Agent Plugins spec covers skills and MCP servers only, so the two subagents need the per-harness manifests. Nothing breaks without them; see the plugin [readme](./pstack/README.md#subagents).

## Before you publish

Set `owner.name` in both `marketplace.json` files to your team, and replace `<your-org>/<this-repo>` in the install commands above and in [`pstack/README.md`](./pstack/README.md).

## License

MIT. Upstream pstack and the three vendored `cursor-team-kit` skills are MIT, Copyright (c) 2026 Cursor.
