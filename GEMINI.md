# EvoLink media generation

Use EvoLink for image, video, music, and speech generation when the user chooses
EvoLink or has not requested a different provider. Media analysis and repository
development alone do not call for this integration.

## Connect and verify

This extension connects directly to `https://mcp.evolink.ai/mcp`. It does not
require the EvoLink CLI or an API key pasted into chat. Let Gemini CLI perform
OAuth discovery and let the user sign in and approve access in the browser.
Keep the normal tool and workspace permission controls enabled.

Use `/mcp` to inspect the server. If authentication is required, use Gemini CLI's
normal `/mcp auth evolink` flow. Listing tools is not proof of authorization:
verify a real free `check_balance` and `search_models` response. Report missing
tools, permission denials, and failed authorization truthfully.

## Prepare and approve

- Use `search_models`, `recommend_models`, `get_model`, and the available model
  references to select supported parameters. Respect the user's chosen model,
  inputs, output settings, and budget; search order does not establish quality.
- Run `estimate_cost` for the concrete request before paid generation. Show the
  model, inputs, output count or duration, settings, estimated cost, and material
  uncertainty. A starting rate is not a task total.
- Keep optional web and image search disabled unless the user requests it or
  explicitly approves its additional cost after you explain it. Do not copy a
  search-enabled example or enable search automatically. Approval of a generation
  without search does not approve adding paid search; changing the search setting
  needs a new estimate and approval.
- Search-enabled generation can incur several web or image search charges.
  Unknown search counts, token usage, or media duration mean an unknown final
  total; disclose rates and any known subtotal. Never invent usage counts or
  treat a partial subtotal as a successful budget check.
- Obtain explicit approval of each quoted paid task before using a generation
  tool. Installation, login, or approval of another task does not approve a new
  charge. Changed or expired quotes need a new estimate and approval.
- Preserve any explicit user budget. An estimate comparison is not a guaranteed
  final settlement maximum. Pause when pricing or a budget cannot be checked.

## Recover and deliver

Use the actual discovered tool schemas. Retain quote, request, and task IDs.
When a submission's outcome is unknown, inspect the existing task/history before
any retry; do not create a replacement paid task to repair a lost response,
failed query, or download. Poll existing tasks and deliver the original result
links and reported charge. Do not claim success without a verified response.

Follow the host's normal permission flow after a denied tool call. Do not switch
transports, remove a budget, or broaden permissions to bypass the denial.
Never read other applications' credentials or request secrets in chat.

Connection and discovery are free. Generation uses the user's EvoLink balance
at published API prices. Support: support@evolink.ai.
