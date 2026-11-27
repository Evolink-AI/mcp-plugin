---
name: evolink-ad-rebuild
description: Rebuild a reference ad with the user's own product and creator through EvoLink - analyze the reference, build keyframes, generate one complete video with sound, and deliver it with a keyframe contact sheet, a brief check summary, any important open issues and the cost. Use for competitor-ad rebuilds, viral-ad remakes, UGC product ads, try-ons and product demos. Needs a signed-in EvoLink connection. Does not copy the reference's people, footage or audio.
---

# EvoLink Ad Rebuild

Rebuild a reference ad as a new ad with the user's product and creator. The structure, hook and rhythm stay; the person, product, room, words and music are new. The result is one finished vertical video with sound, generated in a single video task and delivered with a keyframe contact sheet, a brief summary of the checks, any important open issues, and the cost: the estimate and the actual charge. Give shot-by-shot verdicts only when the user asks for them.

Name it Ad Rebuild when you speak English with the user and 广告复刻 when you speak Chinese, and keep one name for the whole conversation. The user's source video is the reference ad; it is not what this skill is called.

This skill is only the entry point. EvoLink sends the working rules for each step after sign-in. Fetch them with `get_media_workflow` and follow them; do not improvise the steps from this page.

## What you need from the user

- The reference ad: a video of 30 seconds or less. If it is longer, ask for a segment of 30 seconds or less.
- Product images, one line on what the product does, and which claims are verified.
- A creator photo if a specific person should appear. Without one, a fictional adult creator is prepared when the ad needs a person.
- Choices that cannot be inferred: language, on-camera speech or voiceover, music, aspect ratio and length (up to 30 seconds). Do not ask for the resolution up front: without one, the estimate is made for 720p, and 480p, 1080p, 4K or a 480p draft first are offered next to it.

The user must own the product and creator material or have the rights to use it.

## Before you start

- The EvoLink connection must be signed in. If `get_media_workflow` is not available, the EvoLink MCP server is not connected or not signed in. Tell the user how to sign in, then stop:
  - Claude Code: run `/mcp`, choose `evolink` and click **Authenticate**.
  - Codex: run `codex mcp login evolink`.
  - EvoLink CLI: run `evolink auth login`.
  - Other clients: see https://evolink.ai/mcp?utm_source=mcp_other&utm_medium=mcp&utm_campaign=mcp_remote&utm_content=mcp_page
- Never ask for an API key. Charges go to the signed-in account.
- Frames: with a shell, use local ffmpeg and ffprobe; if they are missing, ask before installing them, or use `make_frames` when it is listed. Without a shell, use `make_frames`. When there is no shell and `make_frames` is not among the tools, this rebuild cannot run here: say that it needs an assistant able to run commands, and go no further.

## Steps

Call `get_media_workflow` with `workflow` set to `ad-rebuild` to start. It returns the overview, a `run_id` and the list of sections. Pass that `run_id` on every later workflow call. When the service lists `generate_workflow_media`, use it for every paid generation in this workflow, with the issued run ID and the generation step; when it does not, generate as usual. Fetch the section of each step as you reach it (`section`), and ask for the estimated total with `estimate` once the output length, the resolution (720p unless the user named another) and the number of shots are known.

0. Check inputs and confirm the cost
1. Read the reference
2. Write the target
3. Plan the whole ad
4. Build and check all keyframes
5. Build the complete video input
6. Generate and check
7. Deliver

## Cost: one estimate, one confirmation

- Before the user confirms, do only free work: sign-in and balance, the reference's duration and aspect ratio, frames and shot count, rules and prices.
- Present one estimated total and what it covers, and ask once. If the user named no resolution, that total is for 720p and the other choices follow it; when the user picks one of them, estimate it (free) and present that single total instead. Charges are then made step by step as the work runs: steps finished before a stop or a failure stay charged.
- If the client can show a question with answers to pick, ask through it: the suggested answer first, and room for the user to type a different one. Otherwise ask in one short message.
- After confirmation, run through to delivery. Ask again only when the total would be exceeded, when a decision is the user's, when the finished ad needs a new attempt, or, on the draft route, once the draft is delivered: then ask whether to convert it to 1080p or 4K. Quote and confirm each conversion on its own; it is not part of the total.
- At delivery, show the actual charge next to the estimated total. If the user wants detail, break the charge down by task; they can also list their tasks, or sign in to the dashboard with the same EvoLink account, where every call is listed: https://evolink.ai/dashboard

## If something stops the run

- **Balance too low.** Nothing is charged for the refused step. Give the top-up link from the error. After the user tops up, continue from the same step; finished steps are reused.
- **A generation fails.** That generation is not charged; the steps finished before it stay charged. Retry at most twice for a temporary failure. Then stop and tell the user that the ad is not finished, what has been charged so far, and that they can continue from the failed step later without paying again for the finished steps.
- **The connection drops or the conversation restarts.** Fetch the rules again with the run ID, and look for submitted tasks with `list_tasks` before submitting anything again. Result links stay available for 7 days; uploaded files are valid for 72 hours.

## Rights and limits

- The reference guides structure only. The new ad must not show the reference's people, faces, clothing, room, brand or audio.
- Do not invent logos, prices, statistics or product claims. Use only claims the user confirmed.
- Some image and video models refuse inputs that look like real people. Report a refusal; never reword a prompt to get past review.
- One creator and one product per run unless the user asks otherwise.
