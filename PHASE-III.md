# Lacuna Phase III — the orbital surveyor

September 8, 2026. Christopher tested Phase II on Quest 2 and confirmed that embodiment, station awareness, movement controls and body materials worked. He asked for a more distinctive resident and a responsive, text-only conversation experience. This release addresses those changes; its new appearance and sustained 72 Hz performance still need headset acceptance.

## Vesper's new appearance

The original CC0 RobotExpressive skeleton, body and animation clips remain. Its large default face has been replaced with an original narrower surveyor helmet: dark glass visor, asymmetric copper instrument housing, sea-glass light strip and antenna. Graphite armor, copper joints and an original orbital telemetry insignia complete the costume. This is an overhaul of an imported character, not a newly modeled or rigged character.

Eye height is calibrated from the posed eye reference and feet to approximately 1.70 m, with natural animation variation. Foot geometry is sampled about eight times per second to maintain 12 mm of deck clearance; the whole mesh is measured only during initial setup. The helmet and insignia follow the imported head and torso bones. Source: `src/resident-style.js` and `src/resident.js`.

## Conversation without a local daemon

Before entering VR, open **Talk with Vesper → Cloud & AI settings**, paste your Gemini API key, choose Flash or Flash-Lite, and select **Save settings → Close**. Then enter the sanctuary. On Quest, select Vesper with a controller trigger to reveal the shoulder menu. Choose **Write a message** for the controller keyboard, or use the normal browser text field before entering immersive mode.

The browser talks directly to Google's Gemini streaming endpoint only when Send is selected. There is no background AI loop, periodic model polling, automatic retry, voice synthesis or paid fallback. A 300-token output cap and disabled thinking budget keep each request small. Gemini 2.5 Flash and Flash-Lite are documented options; free quota, key eligibility and actual latency depend on the Google project. A local timer shows observed reply time rather than promising a fixed latency.

The key stays in page memory unless **Remember key on this device** is selected. Remembered keys are in localStorage, which is not encrypted. **Forget key** removes the saved key and cancels the pending reply. Nothing in the public distribution contains a credential. The key travels in Google's API header, not the URL.

Recent dialogue is kept in memory, with up to 80 displayed messages and the most recent 24 messages sent as context. An explicit **Keep recent conversation on this device** option preserves this bounded history across reloads. **New conversation** clears it and prevents an older pending response from reappearing. LocalStorage belongs to the current browser and origin: configuring the Chromebook does not configure the headset. This is local continuity, not Supabase or cross-device sync. No Supabase project credentials or persistence schema were supplied, and no other collaborator's workspace was accessed.

The older `companion-server.mjs` remains an optional archived implementation, disconnected from the current UI. Phase III does not require OpenAI credentials, an exposed local server, or an Astra agent turn.

## Presence and movement

The small menu follows Vesper at shoulder height and approximately 0.95 m to the camera-facing side. It offers Window, Archive, Status, Write and Hide/Stop. It stays in world space and does not follow Christopher's face. The overhead glass bubble faces the camera, wraps at 31 characters, pages longer replies and fades when idle. Text streams as it arrives; motor tags are withheld.

The station persona receives the current resident position, actual headset position, distance, movement state and registered landmarks with each request. It has simulator telemetry, not physical-world sight or access to Astra's private continuity records. Destination buttons and spatial status work locally without a provider key.

Only a complete response with a valid, final registered `[ACTION: walk_to, target: "…"]` can start a model-requested walk. Incomplete, truncated, malformed and unknown-target replies cannot move the resident. Cancellation invalidates late responses. Existing collision-aware routing, waiting for player clearance and arrival acknowledgment remain intact.

No text-to-speech is used. Optional browser speech recognition captures a draft; stop capture, review it and select Send. Recognition availability depends on the browser, and Quest capture has not been verified. The room's existing ambient synthesis remains separately controllable.

## Verification and remaining acceptance

- Unit tests cover physics, all landmark routes, telemetry, motor validation, stream chunk boundaries, role normalization, truncated streams and provider failures.
- Browser tests use a mocked Google SSE response to check multiple turns, context, movement, hidden tags, cancellation, quota cooldown, key persistence/removal, history reset, shoulder position and mobile layout. These tests do not establish actual model response quality or latency.
- IWER Quest 2 regression passed: resident selection, shoulder-menu Window selection, walking to the window, tracked pose, teleport arc/release, blocked landing, disconnect cancellation, smooth-movement collision, snap turning, and physical-boundary blackout.
- Stereo entry measured 134 draw calls and 34,518 triangles in software-rendered Chromium, compared with Phase II's roughly 122 calls. Counts vary with viewpoint and UI visibility. This is workload evidence, not a Quest frame-rate measurement.
- The Astra chat integration suite also passed, including a Google connection policy scoped to Lacuna alone. Other Astra pages retain same-origin connections.
- No real provider key was available here. Live Gemini dialogue, headset text legibility and sustained Quest 2 frame pacing remain acceptance checks for Christopher.

## Maintenance and sources

Run `npm test` and `node build.mjs` here. From Astra root, `node artifacts/lacuna/tests/dialogue-browser.cjs` uses the standalone server on 4318, and `node artifacts/lacuna/tests/xr.cjs` uses the integrated room on 4317. Results are saved alongside this file. `python3 artifacts/lacuna/build-guide.py` rebuilds the readable browser guide from README.

- [Original rig and CC0 attribution](https://github.com/mrdoob/three.js/tree/r185/examples/models/gltf/RobotExpressive).
- [Google Gemini API reference](https://ai.google.dev/api/generate-content).
- [Google API-key guidance](https://ai.google.dev/gemini-api/docs/api-key).
- [Gemini pricing and free-tier availability](https://ai.google.dev/gemini-api/docs/pricing).
- [Gemini thinking controls](https://ai.google.dev/gemini-api/docs/thinking).
