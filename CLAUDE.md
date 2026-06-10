# ECHO — project context

Single self-contained `index.html` — mobile-first showcase app (camera → ~100k GPGPU particles with optical-flow wind). Part of the single-prompt AI showcase series (siblings: `~/projects/flux`, `~/projects/portal`).

## Invariants

- ONE file, zero dependencies, no build step, zero network requests (the privacy promise in the README and the in-app gate copy depends on this — never add analytics, fonts, CDNs).
- Public repo `trusch/echo`, deployed via GitHub Pages from `main` root. Pushing to `main` is allowed for this repo.
- Requires WebGL2 + EXT_color_buffer_float/half_float (state textures are RGBA16F render targets).
- Audio changes must keep the unlock dance AND the activity-gating of the pad (ambient beds must fade out when idle — user-mandated across the series).
- Camera fallback (procedural ghost) must keep working — test the denied-permission path.

## Verification

- Syntax: `awk '/<script>/{f=1;next}/<\/script>/{f=0}f' index.html > /tmp/e.js && node --check /tmp/e.js`
- Runtime: headless chromium with `--use-fake-ui-for-media-stream --use-fake-device-for-media-stream --autoplay-policy=no-user-gesture-required` and swiftshader flags. IMPORTANT: `--virtual-time-budget` stalls getUserMedia (media is real-time) — drive screenshots via CDP instead (see /tmp/cdp_shot.py pattern: remote-debugging-port + Page.captureScreenshot after a real-time wait).
- The fake camera shows a bright moving test pattern — additive blowout in that shot is expected and not representative of a real feed.
