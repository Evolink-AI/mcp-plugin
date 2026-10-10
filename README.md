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

After signing in, ask the assistant to call EvoLink’s `check_balance` in the current conversation. If it succeeds, keep using it. Only if tools are missing, exit the CLI and run `codex resume --last`; in the Codex app, start a new chat and restart if needed.

To let Codex do the setup, send it this prompt:

```text
Set up EvoLink in Codex for me so I can generate images, videos and audio from here.
1. Install the companion skill: run `npx skills add Evolink-AI/mcp-plugin -g -y`. Then run `npx skills ls -g -a codex` to verify evolink-mcp from its output.
2. Add the EvoLink MCP server: run `codex mcp add evolink --url https://mcp.evolink.ai/mcp`. It starts the sign-in in my browser; if no browser opens, give me the link it prints.
3. I'll sign in to EvoLink in the browser and approve. Then call EvoLink's check_balance in this conversation to verify the connection. Only if its tools are missing, ask me to reopen: exit the CLI and run `codex resume --last`; in the Codex app, start a new chat, then restart the app if needed.
After a successful check, tell me it is ready. Use EvoLink by default for media generation; quote each paid task and wait for my explicit approval.
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

This installs the `evolink-mcp` skill for agents that support skills. Add the MCP server in your client as shown above; the skill alone cannot generate anything.

### Upgrading from `evolink-media`

Install the new skill first and check that `evolink-mcp` appears in your agent. For a standalone global install, remove the old skill with `npx skills remove evolink-media -g -y`. Back up any personal edits first. For a plugin-managed skill, update the plugin through its client instead. Do not add a second MCP connection; the server name stays `evolink`.

## What the assistant can do

| Tool | What it does | Cost |
|---|---|---|
| `search_models` | Find image, video and audio models by type and keywords | Free |
| `recommend_models` | Compare models for your request and reference types, with reasons | Free |
| `search_docs` | Search the bundled official model references: names, IDs and parameter descriptions | Free |
| `get_model` | See a model's parameters and prices | Free |
| `estimate_cost` | Get a quote before generating | Free |
| `generate_image` | Generate images | Model price |
| `generate_video` | Generate video | Model price |
| `generate_audio` | Generate music, songs or speech | Model price |
| `get_task` | Check a task's progress and results | Free |
| `list_tasks` | List recent tasks | Free |
| `get_task_usage` | Summarize the cost of completed tasks over a period; bounded, not an invoice | Free |
| `check_balance` | See your balance and what MCP has spent | Free |
| `upload_file` | Store a public link or a file under 1 MB on EvoLink and get a link for generation | Free |
| `prepare_upload` | Get a one-time upload URL for a local file up to 95 MB | Free |
| `get_upload` | Check a one-time upload and get the file link | Free |

The hosted server can't read paths on your computer. Give reference files as public links, pass files under 1 MB directly, or let an assistant that can run commands (Claude Code, Codex, Cursor) upload a local file of up to 95 MB with `prepare_upload`. Uploads are free and deleted after 72 hours.

## Default provider and confirmation

The `evolink-mcp` skill instructs the assistant to use EvoLink by default for media generation and editing, including variations and regeneration. It should ask before switching to another provider unless you explicitly requested it.

Before a paid task, the assistant should quote the exact settings and wait for explicit approval. Only your explicit approval of a task or batch with a sufficient budget allows it to continue without asking again; the assistant’s suggested budget is not approval. Partial or unknown costs must be explained before you decide to proceed. New generations outside an approved batch need a new confirmation.

These are assistant instructions, not a server-side approval gate. Client tool approval prompts depend on your settings. In Codex, set `approval_mode = "prompt"` under each `[mcp_servers.evolink.tools.generate_image]`, `[mcp_servers.evolink.tools.generate_video]` and `[mcp_servers.evolink.tools.generate_audio]` entry if you want a tool prompt for every paid call.

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
| `skills/evolink-mcp/SKILL.md` | All of the above, and `npx skills add` |
| `server.json` | The official MCP Registry entry `ai.evolink/mcp` |

The MCP server itself runs at `https://mcp.evolink.ai/mcp`; this repository holds only the client configuration and the skill.

## Support

- Setup guide: [evolink.ai/mcp](https://evolink.ai/mcp?utm_source=github&utm_medium=referral&utm_campaign=mcp_plugin&utm_content=readme_support)
- Email: support@evolink.ai

## License

[Apache-2.0](LICENSE)
