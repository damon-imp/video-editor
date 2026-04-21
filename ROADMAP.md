# Impruvu Roadmap for video-editor

This fork extends the original browser-use/video-use skill for Impruvu's content operations. The base skill handles generic video editing; Impruvu additions target our specific workflows.

## Status legend
- [ ] not started
- [~] in progress
- [x] done

---

## Phase 1 — Foundation (install + validate)

- [ ] Clone fork to local workstation
- [ ] Symlink into `~/.claude/skills/video-editor`
- [ ] Add `ELEVENLABS_API_KEY` to `.env`
- [ ] Install ffmpeg + yt-dlp
- [ ] Dry run on one short Fathom recording
- [ ] Dry run on one MBP session recording
- [ ] Document output quality vs original raw footage in `validation.md`

## Phase 2 — Brand presets

Each Impruvu brand has distinct visual identity. Codify subtitle + color grade presets so Claude Code picks the right style by entity name.

- [ ] `presets/impruvu.json` — neutral punch, yellow accent, 2-word uppercase caps
- [ ] `presets/mbp.json` — warm cinematic, community/education feel
- [ ] `presets/toh.json` — dark theme, health/performance aesthetic (match existing TOH site at damon-imp.github.io/toh-new-render)
- [ ] `presets/lendbox.json` — clean professional, lending/finance credibility
- [ ] `presets/opfix.json` — mechanic/diagnostic feel, technical overlay style
- [ ] Update `SKILL.md` so Claude reads brand name from prompt and applies preset

## Phase 3 — Fathom integration

Fathom already has transcripts. Skip the ElevenLabs call when the source is a Fathom recording.

- [ ] `helpers/fathom_import.py` — pull recording + transcript via Fathom MCP/API
- [ ] Convert Fathom transcript format to Scribe word-level JSON schema
- [ ] Cache in `edit/transcripts/` same format as Scribe output
- [ ] Add CLI flag `--source fathom --meeting-id <id>`

## Phase 4 — Long-form to short-form extractor

Identify the 3-5 highest-density 30-90s clips from any long-form recording (webinar, coaching call, podcast). Output each as standalone social cut.

- [ ] `helpers/extract_shorts.py` — analyze packed transcript for density signals (question + answer pairs, punchy statements, completed thoughts under 90s)
- [ ] Score candidates by: self-contained (no prior context needed), emotional/tactical payload, length fit
- [ ] Auto-generate hook-first version (strongest 2-3 seconds as opener)
- [ ] Output to `edit/shorts/<slug>.mp4`

## Phase 5 — Webinar chapter cuts

Auto-segment 60-90 min webinars into topical chapters. Useful for course delivery, replay reuse.

- [ ] Detect topic shifts via transcript embedding similarity windows
- [ ] Propose chapter markers with titles
- [ ] Output chapter index + individual chapter mp4s

## Phase 6 — Batch mode

Queue a folder of raw recordings, process overnight, output to Google Drive.

- [ ] `helpers/batch_process.py`
- [ ] Google Drive output integration (existing Impruvu workspace)
- [ ] Slack notification on batch complete (post to #content channel when exists)

## Phase 7 — Hook-first generator

For every short, auto-generate 3 hook variants and save them side-by-side for A/B selection before final render.

- [ ] Pull 3 candidate opening lines from transcript
- [ ] Render 3 preview versions with each as opener
- [ ] Present all 3 with thumbnails for selection

---

## Who works on what

- **Damon** — direction, prompts, validation, brand preset inputs
- **Greg** — Phase 3 (Fathom), Phase 6 (batch + Drive integration)
- **Everett** — Phase 2 (preset JSON wiring), Phase 4/5 (extraction helpers)
- **Lamar** — operational SOP for content VA usage once Phase 2 ships

## Non-goals

- Not rebuilding the base skill
- Not replacing Claude Code as the orchestrator
- Not writing a GUI — this stays terminal-first
- Not handling upload/publishing — output is files, downstream tools handle distribution
