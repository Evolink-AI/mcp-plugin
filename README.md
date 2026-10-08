# EvoLink MCP plugin

Use 150+ image, video and audio models (Seedance, Kling, Veo, Sora, GPT Image, Nano Banana, Suno and more) from your AI assistant. This repository packages EvoLink's remote MCP server, `https://mcp.evolink.ai/mcp`, with a skill that teaches the assistant to find a model, quote the price and generate only after you confirm.

- **No API key to copy.** The first time, you sign in to EvoLink in your browser and approve the connection.
- **Pay as you go.** Generation is charged to your EvoLink balance at the same prices as the API. The MCP itself is free and needs no subscription.
- **Website:** [evolink.ai/mcp](https://evolink.ai/mcp?utm_source=github&utm_medium=referral&utm_campaign=mcp_plugin&utm_content=readme_intro)

## Install

### Claude Code

One command adds EvoLink and signs you in (run it in your terminal, or type `!` in Claude Code and paste it):

```bash
claude mcp add --transport http --scope user evolink https://mcp.evolink.ai/mcp && claude mcp login evolink
```

Your browser opens the EvoLink sign-in page; approve, then reopen Claude Code. On Windows PowerShell 5, replace `&&` with `;`.

To get the skill as well, install the plugin instead (run `/reload-plugins` if Claude Code asks you to), then type `/mcp`, choose `evolink` and select **Authenticate**:

```text
/plugin marketplace add Evolink-AI/mcp-plugin
/plugin install evolink@evolink
```

### Codex

One command; Codex 0.77 or later opens the browser sign-in by itself:

```bash
codex mcp add evolink --url https://mcp.evolink.ai/mcp
```

Run `codex mcp login evolink` only if the output says the server "may or may not require login" or no browser opens.

Or install the plugin, which also adds the skill, then sign in with `codex mcp login evolink`:

```bash
codex plugin marketplace add Evolink-AI/mcp-plugin
codex plugin add evolink@evolink
```

### Cursor

[Add to Cursor](https://cursor.com/install-mcp?name=evolink&config=eyJ1cmwiOiJodHRwczovL21jcC5ldm9saW5rLmFpL21jcCJ9), then find `evolink` in Cursor's MCP settings and click **Connect** to sign in. Or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "evolink": {
      "url": "https://mcp.evolink.ai/mcp"
    }
  }
}
```

With the Cursor CLI, sign in with `agent mcp login evolink` after adding the config.

### Hermes Agent

```bash
hermes mcp add evolink --url https://mcp.evolink.ai/mcp --auth oauth --connect-timeout 300
```

The browser sign-in opens by itself; press Enter when asked to enable the tools, then `/reload-mcp` in an open session. (Based on Hermes' documentation; not tested by us yet.)

### OpenClaw

Run on the machine that hosts the OpenClaw Gateway, then open the printed link to sign in:

```bash
openclaw mcp add evolink --url https://mcp.evolink.ai/mcp --transport streamable-http --auth oauth && openclaw mcp login evolink
```

(Based on OpenClaw's documentation; not tested by us yet.)

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
| `upload_file` | Store a public link or a file under 1 MB on EvoLink and get a link for generation | Free |
| `prepare_upload` | Get a one-time upload URL for a local file up to 95 MB | Free |
| `get_upload` | Check a one-time upload and get the file link | Free |

The hosted server can't read paths on your computer. Give reference files as public links, pass files under 1 MB directly, or let an assistant that can run commands (Claude Code, Codex, Cursor) upload a local file of up to 95 MB with `prepare_upload`. Uploads are free and deleted after 72 hours.

## Costs and control

- After your first connection, **API Keys** in the EvoLink dashboard shows a key named `EvoLink MCP (OAuth)`, shared by all your assistants.
- There is no spending limit by default. Set a total limit, a daily limit or an expiry date on that key to cap MCP spending; disable it to pause MCP for every assistant.
- MCP tasks and charges appear in the dashboard like any other usage.

## What's in this repository

| Path | Used by |
|---|---|
| `.claude-plugin/marketplace.json`, `.claude-plugin/plugin.json`, `.mcp.json` | Claude Code |
| `plugin.json`, `mcp.json` ([Agent Plugins](https://agent-plugins.org)) | Codex, Cursor |
| `.agents/plugins/marketplace.json` | Codex |
| `.cursor-plugin/plugin.json` | Cursor |
| `skills/evolink-media/SKILL.md` | All of the above, and `npx skills add` |
| `server.json` | The official MCP Registry entry `ai.evolink/mcp` |

The MCP server itself runs at `https://mcp.evolink.ai/mcp`; this repository holds only the client configuration and the skill.

## Support

- Setup guide: [evolink.ai/mcp](https://evolink.ai/mcp?utm_source=github&utm_medium=referral&utm_campaign=mcp_plugin&utm_content=readme_support)
- Email: support@evolink.ai

## License

[Apache-2.0](LICENSE)
