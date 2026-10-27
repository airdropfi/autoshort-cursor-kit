---
name: plan-a-series
description: Plan a multi-episode faceless short series (a story with recurring characters) on AutoShort before any episode is made. Design the season, save the season plan, run the review, get the user's explicit confirmation, and only then quote and produce episodes. Use when the user wants a series, a season, a short drama or a recurring character across several videos.
license: MIT
compatibility: Needs the AutoShort MCP server connected and signed in, with the season-plan tools listed (autoshort_save_season_plan). Character sheets and episodes spend credits from the user's own AutoShort account.
---

# Plan a series with AutoShort: plan before you produce

The rule of this skill: a season is fully designed, reviewed and confirmed by the user before any episode is produced. The server enforces it too: an episode can only be made from a plan that is reviewed and confirmed, and the tools answer with an error and a `nextAction` when it is not. Your job is to walk the user through it and never to skip a step.

## Ground rules

- If `autoshort_save_season_plan` is not among your AutoShort tools, the season-plan feature is not available to this connection. Say so and stop. Do not fake it with `autoshort_create_series` schedules. Offer the `make-a-faceless-short` skill for single videos instead.
- Everything that costs credits is priced by a tool first and approved by the user in words. Quote prices, season totals and balances from tool output only. Never write a credit number from memory.
- Characters are original, fictional and clearly adult (or an animal, robot or mascot). No real or famous people, no lookalikes, no brands or protected characters, no sexual content. The tools refuse these with a content-policy error and so do you.
- Do not resume a series, turn on auto-post or schedule anything unless the user asks for it. Create series paused.
- Do not promise results. A series is a creative project, not a guaranteed audience.

## 1. Premise

Ask for the premise in one to three sentences (who, what they want, what goes wrong), the genre, how many episodes, and the narration language. Then call `autoshort_plan_drama_season` (free, saves nothing) to get a character bible and a draft for every episode. Show it in a readable form: cast with a one-line look each, then a table of episodes with title, one-line logline and the closing cliffhanger line. Tell the user it is a proposal. Edit it together until they like the arc. Episodes that come back with `needsEditing: true` must be fixed before they go in a plan.

For short-drama scripts to borrow a shape from, `autoshort_list_templates` with `kind: "episode"` lists them (free), when the server offers that kind.

## 2. Series and cast (the only steps before the plan that can cost credits)

1. `autoshort_list_series` to see what exists. For a new story, `autoshort_create_series` with `startPaused: true`, so nothing is generated on a schedule. Creating a series is free.
2. For each character the plan will use, the character needs a ready character sheet of this series. First call `autoshort_get_character_sheet` (free) and tell the user the exact price it returns and how many sheets the series may still make. Make a sheet with `autoshort_create_character_sheet` only after a clear yes to that price, with the character's look as `description` and an `idempotencyKey`. Show the images it returns. If the user prefers another image, `autoshort_select_character_image` picks it.
3. `autoshort_list_options` lists the narrator voices for the language, with any extra credits a premium voice costs. Pick one narrator for the whole season. Say which one and what it costs if it is not the default.

## 3. Save the season plan (free)

Build the plan from the agreed proposal and call `autoshort_save_season_plan` with `seriesId` and `plan`. Follow the tool's input schema exactly: title, logline, genre, language, narrator voice, style tokens, characters (look, sheet id, wardrobe lock), rules and every episode with its title, the character it stars, its cliffhanger line and either shots or a script. One character per episode, the same outfit in every episode, one narrator and one style for the whole season.

Saving is free. Every edit to a saved plan clears its review and its confirmation, so save the final text before you review it.

## 4. Review (free) and show it all

Call `autoshort_save_season_plan` with `review: true`. Show the user, from the answer:

- the checklist with each rule's pass or fail,
- the issues the AI review lists, if it ran (if the answer says it could not run, say so: the checklist stands on its own),
- the season price the review reports.

Fix every failed check or issue with the user by editing the plan and saving it again, then review again. Do not talk the user past a failed check.

## 5. Confirmation: the user's explicit yes

Ask: "Confirm this season plan? After this I can price and make episodes from it." Wait for a clear yes in the user's own words. Only then call `autoshort_save_season_plan` with `confirm: true`, `seriesId` and the `planHash` copied exactly from the review answer. Do not send `plan` in the same call. If the answer is `PLAN_CHANGED`, the stored plan is not the one that was reviewed: nothing was confirmed, so review the current plan, show it again and ask again.

`autoshort_get_season_plan` shows where the plan stands (none, saved, reviewed or confirmed) at any time.

## 6. Produce, one episode at a time

For each episode the user wants:

1. `autoshort_quote_character_episode` (free) with the series id and the episode exactly as planned. Show the shots, the length and the exact price it returns.
2. Ask: "Make episode N for this price?" Wait for a clear yes.
3. `autoshort_create_character_episode` with `confirm: true`, `expectedCredits` equal to the quoted price, and an `idempotencyKey`. If the price changed, the tool answers 409 and charges nothing: re-quote and ask again. Narrator voice, language, style and character come from the plan and cannot be overridden; to change an episode, edit the plan and go back to step 4.
4. Poll `autoshort_get_video` with the id it returned until COMPLETED or FAILED. An episode takes minutes, not seconds: tell the user, and poll every 30 seconds or so rather than constantly. Relay errors and refund notes as the tool states them, and the tool's per-account limits (one at a time, daily cap) as it reports them.

Never call `autoshort_create_character_episode` for an episode that has not been quoted in this conversation, and never make several episodes on one yes.

## 7. After the season

- `autoshort_get_series_performance` shows how the user's own posted YouTube Shorts are doing, per series. Report the numbers as returned; they come from daily snapshots and are not a forecast.
- To post, connect the platform first (`autoshort_get_connection_status`) and publish only on request with `autoshort_publish_video`.
- To run a series on a schedule, the user decides: `autoshort_resume_series` starts it and each video is charged when it is made. Say that before calling it, and offer `autoshort_pause_series` as the way to stop.
