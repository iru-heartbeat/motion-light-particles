# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project state

This directory currently contains only `要件定義書.md` (requirements definition document). No implementation exists yet — do not assume any source files, build tooling, or tests are present until they are actually created. There is no package.json, no build system, and no test runner in this repo.

## What this project is

Per `要件定義書.md`, this is a browser-only, single-HTML-file interactive art piece: a webcam motion-reactive light-particle effect, extending an earlier project called "マウスに引き寄せられる光の粒子アート" (`light_particles.html`, a mouse-follow particle art piece referenced as a design basis but not present in this directory).

Read `要件定義書.md` in full before implementing — it is the authoritative spec. Key constraints from it:

- **Single file, vanilla JS**: HTML + Canvas API + `getUserMedia`, no frameworks, no external/paid libraries, no server component.
- **Privacy**: camera video must never be displayed on screen or sent/stored externally — it is read only internally for motion-diff computation.
- **Fallback required**: if the user denies camera permission (or it's unavailable), show a message and fall back to the original mouse-driven particle interaction instead of failing.
- **Performance**: target ~30fps; motion-diff detection should be throttled (e.g. computed every 3rd frame, not every frame) rather than run on every animation frame.
- **Scope boundaries**: no ML-based tracking (face/pose estimation), no recording/saving/uploading of video, no mobile-specific optimization work.

## Architecture (intended, per spec)

Since this is meant to ship as one HTML file, expect the implementation to be organized as sections within a single document rather than multiple modules:

1. **Permission/bootstrap flow**: request camera via `getUserMedia` on load → on grant, proceed to camera-driven mode; on denial/error, switch to mouse-fallback mode. This branch is the core control flow of the app (see the screen-transition diagram in the requirements doc, section 6).
2. **Motion detection**: sample webcam frames to an offscreen/hidden canvas, diff against the previous frame in block/grid units, and derive coordinates where motion occurred. The diff threshold should be a variable (not hardcoded inline) so it can be tuned for lighting conditions (F-06, sensitivity adjustment).
3. **Particle system**: spawn particles at detected motion coordinates (or mouse coordinates in fallback mode), render them on the visible canvas, and fade/remove them over time. This reuses/extends the visual language of the earlier `light_particles.html` mouse-follow art piece.

When implementing, keep the motion-detection input source decoupled from the particle system (i.e., both camera-derived coordinates and mouse coordinates should feed the same particle-spawning function), since the fallback mode requirement depends on swapping the input source without duplicating the rendering logic.

## Working rules

- **Do not test in Chrome.** Never launch or drive a browser (including Claude in Chrome / browser automation tools) to test this app. The user tests it themselves. Implement and hand off for the user to verify.
- **Implement one feature at a time.** Do not implement all functional requirements (F-01, F-02, ...) in one pass. Complete and hand off one feature before starting the next, rather than writing the whole thing at once.
