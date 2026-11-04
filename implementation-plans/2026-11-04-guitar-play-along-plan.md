# "Come As You Are" Guitar Play-Along Implementation Plan

## Overview
A phone-friendly web page for practicing Nirvana's "Come As You Are". It shows the tab with a moving playhead and plays a synthesized guitar part. The tempo can be changed from slow to full speed without changing the rhythm. It ships as a single self-contained HTML file, published as a **private claude.ai Artifact** that you open in your phone's browser.

**Decisions so far (from you):**
- Tab source: the dc-guitar.com page. You're allowing the domain in the environment's network settings.
- Scope: the main riff plus the full song, with looping of any section.
- Tuning: down a half step (E♭ A♭ D♭ G♭ B♭ E♭), as the linked page says. This gets checked against the page in Phase 0.
- Hosting: a private claude.ai link.

**My estimates (not set by you; change them if they're wrong):** Medium priority · Medium complexity · about 3–4 hours of build time.

## Current State Analysis
- **Problem**: You need a way to play along with the song at an adjustable speed, with correct rhythm, on a phone.
- **Current Implementation**: Nothing exists yet. The repo (`wdcoff/claudeskills`) only has `.claude/` config, so there's nothing to integrate with and nothing to conflict with.
- **Gap Analysis**: Everything is new. The only external dependency is the tab content from dc-guitar.com, which this session **cannot reach yet**: the proxy returns 403 because the host isn't in the allowed domains.

## Strategy: Clean build, single self-contained HTML file

Why I'm building it this way:
- **Synthesized audio, not the recording.** I can't use the original recording (it isn't available here, and it's copyrighted), so the guitar part is synthesized from the tab. That's what makes slowing down clean. Every note is stored as a beat position and a duration in beats. Playback converts beats to seconds with `seconds = beats × 60 / (BPM × speed)`. All timings scale by the same factor, so the rhythm stays exact at any speed. No time-stretching is involved, so there are none of the "underwater" artifacts that slowed-down recordings have.
- **No build step and no libraries.** The page uses plain HTML, CSS and JS with the Web Audio API. It loads instantly on a phone and nothing can break from a dependency.
- **No lyrics.** The page shows section names only (Intro, Verse, Chorus, …), with no song lyrics.

### Phase 0: Get the source content (blocked on you) (~15 min)
- [ ] You add `dc-guitar.com` under environment settings → Network access → Allowed domains, then tell me.
- [ ] I fetch the page and pull out the tuning, BPM, time signature, section order with repeat counts, the tab for each section, and any rhythm or technique markings (palm mutes, slides, let-ring).
- [ ] I check the tuning against the page (half step down, as you reported).
- [ ] If the page gives no BPM or no note durations, **I ask you** rather than guessing.
**Files**: none (data gathering)

### Phase 1: Song data + timing engine (~60 min)
- [ ] Write the song as data: `sections[] → { name, timeSig, repeat, notes[]: { beat, durBeats, string, fret, tech } }`.
- [ ] Build a lookahead scheduler: a 25 ms timer schedules notes about 120 ms ahead on `AudioContext.currentTime`. This is the standard drift-free Web Audio approach; plain `setTimeout` timing would drift.
- [ ] Speed range: 25%–125% of the original BPM. A speed change takes effect at the next beat, by re-anchoring the clock, so playback never jumps or stutters.
- [ ] Metronome click (accented downbeat) and an optional 1-bar count-in.
- [ ] Section looping: loop one section or a range, starting from any section.
- [ ] Pitch: standard string MIDI notes `[40,45,50,55,59,64]` plus a tuning offset (−1 for half step down). This also drives a "tuning reference" button that plays each open string.
**Files**: `guitar-play-along/index.html` (one file, script inline)

### Phase 2: Guitar tone (the nice-to-have) (~45 min)
- [ ] Plucked-string synthesis using Karplus-Strong: a short noise burst through a filtered delay line, which sounds like a real plucked string. Notes are rendered once per pitch and cached, so playback stays light on a phone.
- [ ] Effects chain: light overdrive (WaveShaper) → **chorus** (modulated delay, slow LFO; the shimmering chorus is the song's signature sound) → speaker-cabinet low-pass filter → output.
- [ ] Palm-mute notes get a shorter decay and darker filter; let-ring notes sustain.
- [ ] Tone switch: Clean / Chorus (default) / Off (metronome only, so you play the part alone).
- Honest expectation: it will sound like a convincing chorused electric guitar, not like the record.
**Files**: `guitar-play-along/index.html`

### Phase 3: Phone UI (~45 min)
- [ ] The tab scrolls horizontally past a fixed playhead. The current note is highlighted, and the section name and bar count are shown.
- [ ] Large touch controls: Play/Pause, speed slider with −5% / +5% buttons, live BPM readout, section picker, loop toggle, metronome toggle, tone switch.
- [ ] **Speed trainer** (optional mode): starts at X%, adds 5% every N loops, and stops at the target speed.
- [ ] Screen Wake Lock so the phone doesn't sleep mid-song, wrapped in a check in case the browser doesn't support it.
- [ ] Works in portrait and landscape, 16 px gutters, no horizontal page scroll (only the tab strip scrolls), light and dark themes.
- [ ] Remembers your last speed and section in `localStorage`, wrapped in try/catch so the page still works when storage is blocked.
**Files**: `guitar-play-along/index.html`

### Phase 4: Testing, publish (~30 min)
- [ ] Headless Chromium (Playwright): no console errors; the page renders at iPhone and Android viewport sizes; screenshots for me to review.
- [ ] Timing test through a test hook: the note schedule at 50% speed is exactly 2× the 100% schedule (to 1 ms), and note order and relative spacing are identical. This is the "rhythm preserved" check.
- [ ] Publish as a private Artifact and send you the link.
- [ ] **What I can't verify:** I can't hear the audio from here. You check the tone and the tab accuracy by ear on your phone, and I adjust from your feedback.
**Files**: `guitar-play-along/index.html`, test script in the scratchpad (not committed)

## Implementation Timeline
### Hour 1: Content + engine
- [ ] Phase 0 extraction (once the domain is allowed)
- [ ] Song data and scheduler, verified with the timing test
### Hour 2: Tone
- [ ] Karplus-Strong voice and effects chain, with tone switch
### Hour 3: UI
- [ ] Tab strip with playhead, controls, looping, speed trainer, wake lock
### Hour 4: Test + publish (includes buffer)
- [ ] Playwright checks, mobile screenshots, publish, send you the link

## Success Criteria
1. ✅ It opens from a private claude.ai link in your phone's browser with no install.
2. ✅ The tab and section order match the dc-guitar.com page.
3. ✅ Speed is adjustable from 25% to 125%, and the timing test confirms the rhythm scales exactly at every speed.
4. ✅ Any section can be looped, and the metronome and count-in work.
5. ✅ The playhead and highlighted note stay in sync with the audio.
6. ✅ The synthesized chorused guitar tone plays at the right pitch for half-step-down tuning (nice-to-have).
7. ✅ No console errors, and the layout is usable at phone width.

## Risk Mitigation
- **Source access risk**: The page is currently blocked. Nothing gets built from guessed tab data. If the page can't be reached, the fallback is that you paste the tab text into chat.
- **Missing rhythm info**: Many text tabs show which frets to play but not how long each note lasts. If that's the case here, I'll tell you and ask how you want to handle it before inventing durations.
- **iPhone silent switch**: On iOS, Web Audio is muted when the ringer switch is on silent. Mitigation: play a silent HTML `<audio>` element on the first tap, which moves the page into the media audio channel. The page will also show a "flip off silent mode" hint if needed.
- **iOS audio unlock**: Audio can only start after a tap, so the Play button creates or resumes the AudioContext.
- **Tone quality risk**: Synthesis won't match the record. The tone switch and metronome-only mode keep the app useful either way.
- **Timer throttling**: If the screen locks, browsers throttle timers. Wake Lock prevents this, and playback pauses cleanly if the page is hidden.

## Rollback Plan
1. Everything is one new file in a new folder. To roll back, delete `guitar-play-along/`, or delete the Artifact.
2. Minimal version: the main riff only, with the speed slider, metronome and basic plucked tone.
3. If time runs short: ship the engine, tab and UI with the plain Karplus-Strong tone, and add the chorus and overdrive chain in a follow-up publish to the same link.

## Files to Modify/Create
### New Files
- `guitar-play-along/index.html` - The whole app: song data, scheduler, synth, UI.
- `implementation-plans/2026-11-04-guitar-play-along-plan.md` - This plan.
### Modified Files
- None
### Removed/Deprecated
- None

## Implementation Checklist (in order)
1. ⛔ **Blocked:** you allow `dc-guitar.com`, or paste the tab.
2. Extract the tab; confirm the tuning, BPM and durations (ask if any are missing).
3. Song data → scheduler → timing test passes. *Checkpoint: rhythm scaling verified.*
4. Synth voice → effects chain. *Checkpoint: no console errors, no audio-graph errors.*
5. UI → mobile screenshots. *Checkpoint: usable at 375 px width.*
6. Publish the private Artifact → you test by ear → I adjust.

## Time & Resource Planning
- **Total**: about 3–4 hours (Phase 0: 0.25 h, engine: 1 h, tone: 0.75 h, UI: 0.75 h, test and publish: 0.5 h, plus buffer).
- **What could make it take longer**: the tab lacking rhythm notation; tuning the synth tone after your listening feedback.
- **External dependencies**: network access to dc-guitar.com; a phone browser (Safari or Chrome).
- **Buffer**: about 30–45 min, included in the 4-hour upper bound.
