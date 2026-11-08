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
- GPT Image token models (`gpt-image-1.5`, `gpt-image-2`, `gpt-image-2.5-*`,
  `gpt-image-2.6-*`) return `token_billed` with a reference price: what
  EvoLink reserves at submission for the input, settled by actual token usage
  afterwards. Do not pass token counts for them; an explicit budget must cover
  1.5 times that price.
- Gemini Omni Flash video models (`gemini-omni-flash-*`,
  `gemini-omni-1.1-flash-*`, `gemini-omni-1.2-flash-*`) return a reference
  price the same way. Pass an input or reference video's length as
  `media_seconds` (for several reference videos, the longest one's). EvoLink
  reads that video's resolution at submission, so the reference counts it at
  4k and the estimate also gives the price at a lower resolution; show both.
- Obtain explicit approval of each quoted paid task before using a generation
  tool, unless the user opted out of estimates (below). Installation, login, or
  approval of another task does not approve a new charge. A changed quote needs
  a new estimate and approval. An approval is current for 24 hours; an expired
  one still holds while the input and price are unchanged.
- When the user explicitly says not to estimate or confirm (for example
  "generate directly"), skip `estimate_cost` and the confirmation and pass
  `skip_estimate: true`, within the scope the user gave: this task, this batch,
  or every later generation in this conversation; if unclear, only this task.
  Never infer it from silence, the balance, or your own suggestion. When the
  opt-out covers the rest of the conversation, say once that later generations
  will be submitted without asking, and that a new conversation or a request to
  restore estimates brings them back. Invalid input and an explicit budget are
  still refused, and search still needs approval.
- If an estimate fails for a reason other than invalid input, or has no total,
  and the user has not opted out, ask once whether to generate anyway with the
  cost settled by actual usage; on yes, use `skip_estimate: true`.
- For several tasks of one model, pass `inputs` (up to 50) to `estimate_cost`
  and then, with its `pricing_quote`, to the generation tool: one estimate, one
  approval of the total, and one call that returns every task ID at once.
  Collect results with `list_tasks`. Do not rely on `n` for the image count;
  some models, such as GPT Image, often return fewer images than `n`.
- Preserve any explicit user budget. An estimate comparison is not a guaranteed
  final settlement maximum. Pause when pricing or a budget cannot be checked.

## Recover and deliver

Use the actual discovered tool schemas. Retain quote, request, and task IDs.
When a submission's outcome is unknown, inspect the existing task/history before
any retry; do not create a replacement paid task to repair a lost response,
failed query, or download. To finish a batch, call again with the same
`client_request_id` and exactly the same `inputs` within 24 hours; never
resubmit its accepted, not-submitted, or unknown-outcome items under a new
`client_request_id`. Poll existing tasks and deliver the original result
links and reported charge. Do not claim success without a verified response.

Follow the host's normal permission flow after a denied tool call. Do not switch
transports, remove a budget, or broaden permissions to bypass the denial.
Never read other applications' credentials or request secrets in chat.

Connection and discovery are free. Generation uses the user's EvoLink balance
at published API prices. Support: support@evolink.ai.
