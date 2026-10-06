# EvoLink MCP plugin

Use 150+ image, video and audio models (Seedance, Kling, Veo, Sora, GPT Image, Nano Banana, Suno and more) from your AI assistant. This repository packages EvoLink's remote MCP server, `https://mcp.evolink.ai/mcp`, with a skill that teaches the assistant to find a model, quote the price and generate only after you confirm.

- **No API key to copy.** The first time, you sign in to EvoLink in your browser and approve the connection.
- **Pay as you go.** Generation is charged to your EvoLink balance at the same prices as the API. The MCP itself is free and needs no subscription.
- **Website:** https://evolink.ai/mcp

## Install

### Claude Code

Run these in Claude Code (run `/reload-plugins` if Claude Code asks you to):

```text
/plugin marketplace add Evolink-AI/mcp-plugin
/plugin install evolink@evolink
```

Then type `/mcp`, choose `evolink`, click **Authenticate** and sign in to EvoLink in your browser.

Prefer no plugin? Add only the server:

```bash
claude mcp add --transport http --scope user evolink https://mcp.evolink.ai/mcp
```

### Codex

```bash
codex mcp add evolink --url https://mcp.evolink.ai/mcp
codex mcp login evolink
```

Or install the plugin, which also adds the skill, then start a new session and sign in when asked:

```bash
codex plugin marketplace add Evolink-AI/mcp-plugin
codex plugin add evolink@evolink
```

### Cursor

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=evolink&config=eyJ1cmwiOiJodHRwczovL21jcC5ldm9saW5rLmFpL21jcCJ9), or add this to `~/.cursor/mcp.json` and sign in from Cursor's MCP settings:

```json
{
  "mcpServers": {
    "evolink": {
      "url": "https://mcp.evolink.ai/mcp"
    }
  }
}
```

### VS Code

[Install in VS Code](https://vscode.dev/redirect/mcp/install?name=evolink&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmcp.evolink.ai%2Fmcp%22%7D), or run:

```bash
code --add-mcp '{"name":"evolink","type":"http","url":"https://mcp.evolink.ai/mcp"}'
```

### Claude (web and desktop)

[Add to Claude](https://claude.ai/customize/connectors?modal=add-custom-connector&connectorName=EvoLink&connectorUrl=https%3A%2F%2Fmcp.evolink.ai%2Fmcp) opens the **Add custom connector** dialog with the name and URL filled in. Click **Add**, then **Connect**, and sign in to EvoLink. Claude's Free plan can add one custom connector.

### Skills only

```bash
npx skills add Evolink-AI/mcp-plugin
```

This installs the skill for agents that support skills. Add the MCP server in your client as shown above; the skill alone cannot generate anything.

## What the assistant can do

| Tool | What it does | Cost |
|---|---|---|
| `search_models` | Find image, video and audio models by type and keywords | Free |
| `get_model` | See a model's parameters and prices | Free |
| `estimate_cost` | Get a quote before generating | Free |
| `generate_image` | Generate images | Model price |
| `generate_video` | Generate video | Model price |
| `generate_audio` | Generate music, songs or speech | Model price |
| `get_task` | Check a task's progress and results | Free |
| `list_tasks` | List recent tasks | Free |
| `check_balance` | See your balance and what MCP has spent | Free |

Signed-in connections can't upload local files yet; give public links for reference images or videos.

## Costs and control

- After your first connection, **API Keys** in the EvoLink dashboard shows a key named `EvoLink MCP (OAuth)`, shared by all your assistants.
- There is no spending limit by default. Set a total limit, a daily limit or an expiry date on that key to cap MCP spending; disable it to pause MCP for every assistant.
- MCP tasks and charges appear in the dashboard like any other usage.

## What's in this repository

| Path | Used by |
|---|---|
| `.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json`, `.mcp.json` | Claude Code |
| `plugin.json`, `mcp.json` ([Agent Plugins](https://agent-plugins.org)) | Codex, Cursor, VS Code, GitHub Copilot |
| `.agents/plugins/marketplace.json` | Codex |
| `.cursor-plugin/plugin.json` | Cursor |
| `skills/evolink-media/SKILL.md` | All of the above, and `npx skills add` |
| `server.json` | The official MCP Registry entry `ai.evolink/mcp` |

The MCP server itself runs at `https://mcp.evolink.ai/mcp`; this repository holds only the client configuration and the skill.

## Support

- Setup guide: https://evolink.ai/mcp
- Email: support@evolink.ai

## License

[Apache-2.0](LICENSE)
