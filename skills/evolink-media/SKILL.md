---
name: evolink-media
description: Generate images, video, music and speech with EvoLink's 150+ models through the EvoLink MCP server. Use when the user wants to create or edit an image, make a video, compose music or speech, compare models or prices, or check EvoLink tasks or balance.
---

# EvoLink media generation

The `evolink` MCP server gives you 150+ image, video and audio models (Seedance, Kling, Veo, Sora, GPT Image, Nano Banana, Suno and more). Paid calls are charged to the user's EvoLink balance at EvoLink's API prices (68 credits ≈ $1).

## If the EvoLink tools are missing

If tools such as `search_models` and `generate_video` are not available, the server is not connected or not signed in yet. Tell the user how to sign in, then stop:

- Claude Code: run `/mcp`, choose `evolink` and click **Authenticate**.
- Codex: run `codex mcp login evolink`.
- Other clients: see https://evolink.ai/mcp?utm_source=mcp_other&utm_medium=mcp&utm_campaign=mcp_remote&utm_content=mcp_page

## Workflow

1. Understand what the user wants: image, video, music or speech, plus style, length and aspect ratio. Ask only for what is missing.
2. Find a model with `search_models` (filter by type and keywords), then read the chosen model's parameters and prices with `get_model`.
3. Pass `model` as its own argument and every other parameter inside `input`, named exactly as `get_model` lists them.
4. Before any paid call, get the price with `estimate_cost` and tell the user. Go ahead once they agree, or if they already approved this spend.
5. Generate with `generate_image`, `generate_video` or `generate_audio`:
   - `generate_image` waits up to 40 seconds and returns the links when ready, otherwise a `task_id`.
   - `generate_video` and `generate_audio` return a `task_id` at once.
6. For every `task_id`, call `get_task` (it waits up to 45 seconds per call) until the status is `completed` or `failed`. Never call a generate tool again to check progress: that starts and charges a new task.
7. Give the result links to the user right away; they expire after 24 hours. `get_task` also reports the final charge.

## Costs and limits

- Only the three generate tools cost money. Searching, quoting, task checks and `check_balance` are free.
- Failed tasks are refunded. There is no cancel tool, so confirm before submitting.
- When a call is refused for money, tell the user which reason the error names: the EvoLink MCP spending limit or daily limit they set, EvoLink MCP being paused, or the account balance. They need different fixes, so never call an MCP limit a low balance. Give the link from the error and do not retry until the user has acted.
- Other errors include a next step; follow it instead of retrying blindly.

## Reference files

Reference images, audio and video are passed as public http(s) links inside `input`. A signed-in connection cannot read files on the user's computer or chat attachments: if `upload_file` is not listed, ask the user for a public link.

## Useful habits

- After a lost connection, use `list_tasks` to find recent tasks instead of generating again.
- `check_balance` shows the balance and what EvoLink MCP has spent.
