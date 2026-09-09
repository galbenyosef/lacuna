# Phase III correction — one-button voice

September 8, 2026. Christopher rejected the first Phase III headset workflow: laser-pointer typing and voice draft review were unacceptable. The corrected build removes all conversational text-entry fields, the virtual keyboard, Send controls and the credential modal. One high-contrast shoulder button is visible immediately. It moves through ready, microphone opening, listening and thinking states, automatically sending finished speech and returning to ready after a reply or error.

## Connection verified with the actual credential

The explicitly authorized `/home/gagekappes/.config/codex-home/gemini-api-key` is loaded only by `voice-server.mjs`. It is never bundled or stored in browser localStorage. Existing browser-stored provider keys from the prior build are removed. The server exposes only bounded conversation and transcription endpoints, allows the Lacuna browser origins, caps concurrent/rate usage, and has no filesystem or agent-tool endpoints.

A real Gemini 2.5 Flash request returned 404: the model is unavailable for new users on this account, with Google's response directing migration to Gemini 3.6 Flash. A subsequent real 3.6 Flash request succeeded. The service uses that model with minimal thinking, a 150-token response limit, recent history and actual simulator telemetry. It aggregates the answer server-side because the current Cloudflare development tunnel does not support SSE. The complete reply appears overhead automatically.

Native SpeechRecognition is single-shot, final-results-only and English. A transcript is submitted at most once; onend cannot reset an in-flight reply. If native recognition is absent, the browser captures mono PCM at 16 kHz, stops after detected speech and 1.2 seconds of silence, then uses Gemini to transcribe WAV audio. Eight seconds without speech times out; recordings cap at 25 seconds. Microphone tracks and audio nodes are released on completion, errors, reset and page exit. Native service errors select microphone transcription on the next attempt. No recording is persisted, no voice is played back and no paid fallback model is selected automatically.

The Cloudflare quick tunnel currently keeps the public Quest page connected without key entry. It requires the Chromebook/Linux host awake and online; its URL can change on restart and has no production uptime guarantee. A stable domain/cloud deployment would be a separate hosting step. Supabase was not required, and its project was not modified.

## Whole-body appearance

The imported rig remains. All body panels now use a consistent matte graphite carbon shader, with subdued woven variation, sea-glass telemetry seams and narrow copper cuff details. The helmet's antenna, side housing and cylinder ear were removed. The visor and orbital insignia remain. This replaces the previous mismatched body finish and bolted-on instrument silhouette. Natural eye-height calibration and animated foot grounding remain.

## Evidence

- `voice-live-results.json`: a spoken audio fixture enters actual Chromium microphone capture, silence stops capture automatically, real Gemini transcribes and replies through the public HTTPS bridge, the bubble displays the answer, and the validated navigation action begins a walk. Microphone tracks end. The first successful full run took about 12 seconds including the spoken utterance, silence detection and both provider requests; no subsecond promise is made.
- `voice-browser-results.json`: mocked native-event edge cases, duplicate-result suppression, permission-denial recovery, reset and mobile layout.
- `xr-results.json`: emulated Quest controller activates the voice button; a recognized utterance reaches real Gemini. Existing teleportation, collision, snap-turn and boundary behavior are exercised.
- Unit checks include WAV encoding, recognition lifecycle, motor validation, route clearance and physics. The Astra server regression checks cover the Lacuna-only connection policy.
- A physical Quest microphone and sustained headset frame rate cannot be measured from this Linux test environment. Christopher's headset acceptance remains distinct from these browser and provider checks.

Current setup and maintenance instructions are in README.md. Older Phase II material is historical.
