# Counter fix: Chromium evidence

Issue: https://github.com/AgentCityBuilders/prooflane-fork-benchmark-cline/issues/1

- Base: `838b1a555e59091607ce412b737a19058386d0cd`
- Candidate: `11c1f64920a67da8d7828b82ebdbae41ae6acf33`
- Browser: Chromium 159.0.8154.6, headless, 1280×720.
- Before: [video](before.mp4), [original WebM](before.webm), [result screenshot](before.png). Initial count 0; one click on **Add one** visibly yields **2**.
- After: [video](after.mp4), [original WebM](after.webm), [result screenshot](after.png). Same action visibly yields **1**.

Both recordings are actual Playwright Chromium recordings, without overlays or fabricated UI. MP4 files are H.264 transcodes of original WebM files for convenient playback. The candidate journey separately verifies initial 0, first click 1, second click 2 and reload 0. Syntax checks and both behavior assertions passed; the baseline original journey failed as expected. See `test.log` and `manifest.json` (including SHA-256 hashes).

## Manual reproduction

At each identified commit, run `node --check server.mjs` then `PORT=3000 node server.mjs`. Open http://127.0.0.1:3000 in Chromium, verify initial Count 0 and click Add one exactly once. Base gives 2; candidate gives 1. Stop the server before switching commits. At the candidate a second click gives 2, and reloading resets to 0.

## Limitations

Headless Chromium on macOS only; no other browser or mobile coverage. These viewport videos do not include OS/browser chrome or a separate cursor overlay. The fixture has no package manifest or standalone journey runner: `journey.mjs` is an exported journey with injected page/baseURL/expect, run here with existing local Playwright. The local verification harness is not part of the implementation branch.

The installed GitHub CLI has no supported native inline attachment option. Evidence is therefore on this separate public fork-only branch. Use commit-pinned links from the PR for durable access; no unofficial upload endpoints were used.
