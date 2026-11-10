# GitHub Meta MCP Server

**Create GitHub repositories and edit their description, topics and website from a Claude conversation.**

A small [Model Context Protocol](https://modelcontextprotocol.io) server with one tool. You (or Claude) pass a plain-English command; the server picks it apart with simple pattern matching and calls the GitHub REST API with your token. There is no language model inside the server, so the wording matters.

## Status

Last updated May 2026. Checked in November 2026: `npm ci` and `npm run build` succeed, the server starts, and commands in the forms below parse and reach the GitHub API. Calls with a real token were not tested.

## Tool

### `create_repo`

One argument, `command` (string). It understands four forms:

| Command | What happens |
|---|---|
| `Create a repository for my machine learning project` | Creates a public repo under your account, initialized with a README. The words after `for`, `called` or `named`, up to the first comma or period, become the description, and a slug of them becomes the name (here `my-machine-learning-project`). |
| `Update owner/repo description to A better description` | Replaces the description. |
| `Set owner/repo topics to ai mcp tools` | Replaces all topics. Topics are split on spaces or commas and lowercased. |
| `Update owner/repo website to https://example.com` | Sets the homepage URL. |

Things to know:

- Create commands must say `repository`, not `repo`: "Create a repo called lunchbox" is rejected.
- Update commands need the repository as `owner/repo`, right after `update`, `change` or `set`.
- Any command containing `update`, `change` or `set` anywhere, even inside a word such as "dataset", is treated as an update.
- Set topics with an update command after creating the repo. The server passes topics from a create command to GitHub's create-repository API, which does not accept them.
- New repos are always public. There is no option to make them private.
- There is no delete.

## Install

Build from source with Node 18 or newer:

```bash
git clone https://github.com/joewilsonai/github-meta-mcp-server
cd github-meta-mcp-server
npm ci
npm run build
```

## Configure Claude Desktop

Add this to `claude_desktop_config.json`, with the absolute path to your clone and your own token:

```json
{
  "mcpServers": {
    "github-meta": {
      "command": "node",
      "args": ["/absolute/path/to/github-meta-mcp-server/build/index.js"],
      "env": {
        "GITHUB_TOKEN": "<your-github-token>"
      }
    }
  }
}
```

Restart Claude Desktop and the `create_repo` tool appears.

The token is read from the `GITHUB_TOKEN` environment variable only; the server will not start without it. A classic personal access token with the `repo` scope (or `public_repo`, since new repos are public) covers everything the tool does. Create one in [GitHub token settings](https://github.com/settings/tokens).

## Built with

TypeScript, `@modelcontextprotocol/sdk` 0.6.0 and Octokit.
