# bstack marketplace

A one-plugin marketplace for [bstack](./bstack): rigorous agent workflows for coding agents. Fewer lines of code, at higher quality, with the evidence attached. It runs on Claude Code, Codex, Cursor, GitHub Copilot and other harnesses.

## Install

**Claude Code:**

```bash
/plugin marketplace add binarystride/bstack
```

```bash
/plugin install bstack@bstack
```

**Codex:**

```bash
codex plugin marketplace add binarystride/bstack
```

```bash
codex plugin add bstack@bstack
```

**Cursor** reads either manifest. Add this repo as a marketplace and install `bstack`.

**GitHub Copilot, VS Code, Kiro, ChatGPT** read the [Agent Plugins](https://agent-plugins.org/) standard. Point your client at this repo and install the plugin from the `bstack/` directory.

## Team install

Commit this to your repository's `.claude/settings.json`. Claude Code and VS Code read it.

```json
{
  "extraKnownMarketplaces": {
    "bstack": {
      "source": { "source": "github", "repo": "binarystride/bstack" },
      "autoUpdate": true
    }
  },
  "enabledPlugins": { "bstack@bstack": true }
}
```

After a teammate trusts the folder, Claude Code adds the marketplace and loads bstack without an install step. VS Code offers the plugin on the first chat message. Codex and Cursor users install it once with the commands above.

Keep your team's own rules in the repository: `AGENTS.md` for rules that always apply, and repository skills for procedures. bstack reads them. For example, the conventions pass in `/review` checks the change against your agent instruction files.

## Layout

```text
.claude-plugin/marketplace.json   Claude Code marketplace
.cursor-plugin/marketplace.json   Cursor marketplace
.agents/plugins/marketplace.json  Codex marketplace
bstack/
  plugin.json                     Agent Plugins manifest
  .claude-plugin/plugin.json      Claude Code manifest
  .cursor-plugin/plugin.json      Cursor manifest
  skills/                         the skills, shared by all three
  agents/                         review subagents (Claude Code only)
  docs/guide/                     the walkthrough
```

## License

MIT. bstack started as a portable build of [poteto's pstack](https://github.com/cursor/plugins/tree/main/pstack), which is MIT licensed, and much of its text still comes from pstack. Both copyright notices are in [`LICENSE`](./LICENSE).
