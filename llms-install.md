# EvoLink remote MCP setup for Cline and Careti

Native remote setup guide. Verify browser authorization and an actual free tool call in your own client before generating media. This guide does not itself certify marketplace approval or compatibility with every client release.

EvoLink hosts a remote MCP server at `https://mcp.evolink.ai/mcp`. This repository contains public client configuration and a companion skill, rather than the hosted server implementation. No server build, local EvoLink package, API key, or shared credential is needed for this remote configuration.

## Add the remote server

1. Open your client's MCP Servers interface. In Remote Servers, use the name `evolink`, the URL above, and **Streamable HTTP** transport.
2. Alternatively, open the client's MCP configuration editor and merge the following entry into its existing `mcpServers` object. Preserve other servers and settings. If `evolink` already exists, inspect and update that entry rather than adding another connection.

```json
{
  "mcpServers": {
    "evolink": {
      "type": "streamableHttp",
      "url": "https://mcp.evolink.ai/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

Specify `type` explicitly: the current Careti parser defaults a URL-only entry to SSE. Keep automatic approval disabled while reviewing the available tools.

### Cline CLI

If Cline CLI is already installed, add this same remote connection with:

```sh
cline mcp add evolink https://mcp.evolink.ai/mcp --transport http --yes
```

This command saves the connection; it does not complete browser authorization. Run `cline mcp`, choose **Authorize OAuth**, and select `evolink` to complete browser sign-in. Keep automatic tool approval disabled. Use an isolated Cline configuration when testing, and preserve other server entries in an existing configuration.

The CLI MCP settings file is normally `~/.cline/data/settings/cline_mcp_settings.json`. If `CLINE_MCP_SETTINGS_PATH` is set, use that exact file instead. Read the existing file, preserve other entries, and ensure `mcpServers.evolink.autoApprove` is `[]` in this actual MCP settings file. Do not create `~/.cline/config.json` for MCP settings.

To resolve the actual path without dumping environment variables, run this shell command, then read the exact absolute path it prints. A file-reading tool may not expand `~` or environment variables itself.

```sh
printf '%s\n' "${CLINE_MCP_SETTINGS_PATH:-$HOME/.cline/data/settings/cline_mcp_settings.json}"
```

Recent CLI versions write connection fields inside a `transport` object. Preserve that generated object when updating the entry. The graphical-client JSON example above is not a reason to discard existing CLI fields.

Cline CLI also has a separate session approval setting. Start a normal session with `cline --auto-approve false` to retain tool approval prompts; an empty per-server `autoApprove` entry alone does not enforce CLI session approvals.

`cline mcp` opens an interactive menu and requires a TTY. In a non-interactive setup check, use `cline mcp add ... --yes`, verify the actual settings file above, and stop for the user to complete OAuth in an interactive session. Do not treat a failed menu launch as a successful authorization check.

## Sign in and verify

When the client requests authentication, choose **Authenticate** for EvoLink and complete browser sign-in with the intended EvoLink account. Review the permissions before granting access. Keep passwords, authorization codes, tokens, and account balances out of screenshots or public reports.

After returning to the client, confirm the connection status and inspect the available tools. Then request actual free `check_balance` and `search_models` calls, followed by a suitable `get_model` and `estimate_cost` request. Tool discovery alone is not proof of authenticated access. Do not invoke `generate_image`, `generate_video`, or `generate_audio` during this installation check.

Generation uses the user's EvoLink balance. Review an estimate and obtain explicit approval before each paid request. Keep optional search disabled until its additional costs are reviewed and approved. These instructions do not replace client permissions or enforce a final billing cap.

If authentication fails, record the client version, transport, and redacted error. Do not replace OAuth with a shared API key, disable certificate checks, or enable blanket automatic approval to bypass the failure.

Product: https://evolink.ai/mcp

Documentation: https://evolink.ai/docs/en/mcp/overview

Privacy: https://evolink.ai/privacy

Terms: https://evolink.ai/terms

Support: support@evolink.ai
