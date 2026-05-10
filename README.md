<div align="center">

# 🐙 GitHub Meta MCP Server

**Manage GitHub repositories with natural language via the Model Context Protocol.**

An MCP server that lets Claude (or any MCP-compatible client) create and configure GitHub repositories from plain-English commands.

[![MCP](https://img.shields.io/badge/MCP-Server-d97757)](https://modelcontextprotocol.io)
[![Node](https://img.shields.io/badge/Node-18+-339933?logo=node.js)](https://nodejs.org)
[![License](https://img.shields.io/badge/License-MIT-blue)](#license)

</div>

---

## What it does

Talk to Claude. Claude talks to this server. This server talks to GitHub.

> *"Create a private repo called 'lunchbox' with topics ai, agents, mcp and the homepage thelunchbox.app"*

→ Repo created. Topics set. Homepage set. README initialized.

## Tools exposed

### `create_repo`

Create or update a GitHub repository via natural language. Handles:

- **Repo name** (auto-generated from description if not specified)
- **Visibility** (public / private)
- **Description**
- **Topics / tags** (multiple, comma-separated)
- **Homepage URL**
- **Auto-init with README**

## Install

```bash
git clone https://github.com/joewilsonai/github-meta-mcp-server
cd github-meta-mcp-server
npm install
npm run build
```

## Configure in Claude Desktop

Edit your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "github-meta": {
      "command": "node",
      "args": ["/absolute/path/to/github-meta-mcp-server/build/index.js"],
      "env": {
        "GITHUB_TOKEN": "ghp_your_personal_access_token"
      }
    }
  }
}
```

Restart Claude Desktop. The `create_repo` tool will appear.

## Required token scopes

Your GitHub personal access token needs:

- `repo` — to create and configure repos
- `delete_repo` — only if you want to support deletion in future versions

Generate one at [github.com/settings/tokens](https://github.com/settings/tokens).

## Stack

- **TypeScript** + **Node.js 18+**
- **@modelcontextprotocol/sdk**
- **@octokit/rest** for the GitHub API

## Related MCP servers

- 🧠 **[second-opinion-mcp-server](https://github.com/joewilsonai/second-opinion-mcp-server)** (⭐ 16) — Get a second opinion from Gemini / Perplexity / Stack Overflow
- 🔍 **[mcp-perplexity-server](https://github.com/joewilsonai/mcp-perplexity-server)** (⭐ 14) — Perplexity-backed code analysis

## License

MIT
