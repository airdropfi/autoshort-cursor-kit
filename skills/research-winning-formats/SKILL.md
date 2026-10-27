---
name: research-winning-formats
description: Find short-video formats worth testing before making a video. Read the user's own AutoShort performance data, browse the AutoShort template gallery, study reference Shorts the user names, and turn the findings into format cards and a small test plan. Use when the user asks what to make next, what format or hook style to try, or how to learn from videos that did well.
license: MIT
compatibility: Works with the AutoShort MCP server connected. The performance step needs a YouTube channel with posted Shorts. No AutoShort format-presets endpoint exists yet, so the reference-video step is manual and uses text the user or a web fetch provides.
---

# Research formats before you make the video

A format is a repeatable shape of a short: how the hook works, how many beats, how long, what the viewer sees. A format that did well for someone else is a hypothesis for the user, not a guarantee. Your output is a short list of formats to test and a way to measure them, not a promise.

## Ground rules

- Everything is free: this skill only reads. It never renders, charges or posts. When the user wants to make something from a format, hand over to `make-a-faceless-short` (one video) or `plan-a-series` (a story).
- Report numbers exactly as the tools return them. Do not round them up, extrapolate, or predict views.
- Do not copy another creator's script, voice, footage or channel concept. Learn the structure (hook type, beats, pacing) and write something original.
- Be clear about what you can and cannot see: you can read titles, descriptions, transcripts and comments that the user pastes or that a web fetch returns. You cannot watch a video, and AutoShort does not give you other creators' view counts.
- Do not make claims about earnings, monetisation or going viral.

## Step 1: What already worked for this user (own data)

1. `autoshort_list_series` lists their series. Ask which one to look at, or look at all.
2. `autoshort_get_series_performance` (optionally with `seriesId` and `days`) returns, per series, how many videos were posted and measured, total and median views, the top three and bottom three videos with their `outlierScore` (views divided by the series median), and views gained in the last 7 days versus the 7 days before. It covers only the user's own posted YouTube Shorts, comes from daily snapshots, and has no retention data.
3. For each top and bottom video, `autoshort_get_video` or `autoshort_list_videos` gives the title and details. Describe what the top videos have in common (hook type, topic, length, visual style) and what the bottom ones share. Mark each observation as a pattern in a small sample, and say how many videos it rests on. If fewer than a handful were measured, say it is too early to conclude anything.

If the tool returns no posted videos, skip to Step 2 and say why.

## Step 2: What is available inside AutoShort

- `autoshort_list_templates` is the template gallery: video templates, and when enabled motion and episode templates. Filter by `niche`, or by `kind` and `genre` where the tool allows it. Show the user a few that match what they care about, with each template's name and what it is for. Prices come from the tool: show them only as the tool returns them.
- `autoshort_list_niches` lists the niches, art styles, qualities and durations the generation tools accept.
- `autoshort_list_options` lists voices, languages and the other options with their prices.

## Step 3: Reference Shorts (manual)

There is no format-presets endpoint on AutoShort to call. Do this by hand with the user:

1. Ask for 3 to 8 public Shorts they admire in their niche: links, or pasted titles, descriptions and transcripts. If the user has a web search or fetch tool available in this session, you can use it to read the public page text; otherwise ask them to paste.
2. For each one, write down only structure: the hook (first sentence: question, claim, contrast, "POV"), the number of beats, the turn, the payoff or cliffhanger, the approximate length, and what the viewer sees (talking head, stock-like b-roll, character, text on screen).
3. Group the references into 2 to 4 formats. Name each format in plain words.

## Step 4: Format cards

For each format worth testing, write one card:

- **Name** and one-line description.
- **Hook pattern**: the shape of the first sentence, with one original example for the user's topic.
- **Beats**: 3 to 6 beats in order.
- **Length and pacing**.
- **Visual style**: what each scene shows, and which AutoShort art style or template is the closest match.
- **Why it might work** (one honest sentence, flagged as a hypothesis) and **what could go wrong**.
- **What to vary** so that ten videos in this format do not feel like one video ten times.

## Step 5: A small test plan

Propose 3 videos in 2 formats, one variable changed at a time (for example the hook, with the same topic and length). Tell the user when to measure: after the videos have been posted for a while, run `autoshort_get_series_performance` again and compare against the median that was there before. State up front that a handful of videos is a weak signal and that results vary.

Then ask which card to turn into a draft and hand over to `make-a-faceless-short` or `plan-a-series`.

## If a presets endpoint appears later

If a formats or presets tool shows up among your AutoShort tools, prefer it for Step 2, and keep Step 3 for formats that are not in it.
