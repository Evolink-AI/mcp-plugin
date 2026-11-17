---
name: evolink-ad-rebuild
description: Rebuild a reference ad with the user's own product and creator through EvoLink - analyze the reference, build keyframes, generate one complete video with sound, and deliver it with a check of every shot. Use for competitor-ad rebuilds, viral-ad remakes, UGC product ads, try-ons and product demos. Needs a signed-in EvoLink connection. Does not copy the reference's people, footage or audio.
---

# EvoLink Ad Rebuild

Rebuild a reference ad as a new ad with the user's product and creator. The structure, hook and rhythm stay; the person, product, room, words and music are new. The result is one finished vertical video with sound, generated in a single video task and delivered with a keyframe contact sheet, a verdict for every shot and the total cost.

This skill is only the entry point. EvoLink sends the working rules for each step after sign-in. Fetch them with `get_media_workflow` and follow them; do not improvise the steps from this page.

## What you need from the user

- The reference ad: a video of 30 seconds or less. If it is longer, ask for a segment of 30 seconds or less.
- Product images, one line on what the product does, and which claims are verified.
- A creator photo if a specific person should appear. Without one, a fictional adult creator is prepared when the ad needs a person.
- Choices that cannot be inferred: language, on-camera speech or voiceover, music, aspect ratio, length (up to 30 seconds) and resolution.

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

Call `get_media_workflow` with `workflow` set to `ad-rebuild` to start. It returns the overview, a `run_id` and the list of sections. Pass that `run_id` on every later call. Fetch the section of each step as you reach it (`section`), and ask for the estimated total with `estimate` once the output length, the resolution and the number of shots are known.

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
- Present one estimated total and what it covers, and ask once. Charges are then made step by step as the work runs.
- After confirmation, run through to delivery. Ask again only when the total would be exceeded, when a decision is the user's, or when the finished ad needs a new attempt.
- If the user asks what the total includes, say the total and what it covers. Every call is listed in the dashboard history: https://evolink.ai/dashboard

## If something stops the run

- **Balance too low.** Nothing is charged for the refused step. Give the top-up link from the error. After the user tops up, continue from the same step; finished steps are reused.
- **A generation fails.** It is not charged. Retry at most twice for a temporary failure, then stop and explain.
- **The connection drops or the conversation restarts.** Fetch the rules again with the run ID, and look for submitted tasks with `list_tasks` before submitting anything again. Result links stay downloadable for 24 hours and uploads for 72 hours.

## Rights and limits

- The reference guides structure only. The new ad must not show the reference's people, faces, clothing, room, brand or audio.
- Do not invent logos, prices, statistics or product claims. Use only claims the user confirmed.
- Some image and video models refuse inputs that look like real people. Report a refusal; never reword a prompt to get past review.
- One creator and one product per run unless the user asks otherwise.
