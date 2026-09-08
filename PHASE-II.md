# Lacuna Phase II — embodied companion

September 8, 2026. Vesper now uses Tomás Laulhé / Quaternius's CC0 RobotExpressive model, with the Three.js conversion by Don McCurdy. The model is bundled locally (464 KB source), scaled to 1.72 m, and recolored in graphite, copper and sea glass. Idle, Walking, Wave and Yes animations come from its existing rig. No character mesh or skeleton was modeled from scratch.

## Try movement

Open Lacuna, expand **Talk with Vesper**, and choose Window, Archive, Tide or Arrival. The same destinations are on the in-world panel opened by selecting the resident. In VR, aim a controller ray and pull the trigger to select a key or destination. Vesper walks around the central plinth and furniture, waits when Christopher is within 0.9 m of its proposed step, idles on arrival, turns to face him, and waves. If you stand at the destination, step aside to let it finish.

Station anchors are in `src/navigation.js`. A 0.60 m resident disc-clearance grid graph with A* search and segment smoothing reuses the room collision map. Live telemetry includes both positions, headset height, nearest station, planar distance and movement status. The companion receives a snapshot with each conversational request; this is simulator telemetry, not computer vision or sensing of the physical room.

## Live conversation — connection still required

The UI and authenticated service are implemented and tested with simulated model replies. **A real model conversation has not been verified.** No OpenAI API key is configured in this environment, and a Quest-accessible HTTPS service address has not been selected. GitHub Pages supplies the room; it cannot itself run the model service.

`companion-server.mjs` is an isolated Node service. It exposes only the conversation endpoint, with no Astra filesystem, CLI, or agent tools. It uses the OpenAI Responses API, with an explicitly configured model, no provider-side stored response, a 900-token output cap, one concurrent turn and twelve requests per minute. It retains the latest twelve conversation turns in memory for up to two hours of inactivity. A new conversation starts with the UI button; a service restart erases histories. This is a bounded recent context, not unlimited session memory. The browser keeps up to forty displayed messages until reload.

Configure environment variables on the service host (never in the static website or Git repository):

- `OPENAI_API_KEY`: the provider credential.
- `LACUNA_MODEL`: the agreed API model ID; no expensive model is silently selected.
- `LACUNA_TOKEN`: a random pairing secret of at least 32 characters, separate from the provider key.
- Optional `LACUNA_PORT` (default 4320), `LACUNA_HOST` (default 127.0.0.1).
- Optional `LACUNA_TLS_CERT` and `LACUNA_TLS_KEY` for trusted HTTPS.
- Optional `LACUNA_ORIGINS`: comma-separated exact allowed browser origins. Defaults include the existing GitHub Pages origin and local room ports 4317/4318.

Then run `node companion-server.mjs`. A trusted HTTPS reverse proxy or an existing private network connection is needed for Quest to reach the service. Do not expose Astra's port 4317 or put the provider API key in a browser. In Lacuna's menu, enter the service origin and pairing token before entering VR. These entries stay only in page memory, so reload requires reconnection. The standalone room on port 4318 or GitHub Pages supports a chosen HTTPS conversation endpoint; the integrated Astra server's current CSP permits only same-origin network connections, so its external-service policy must be configured when that address is chosen.

Text input works in the desktop menu and controller-operated in-world keyboard. Browser speech recognition is an optional start/stop capture mode: stop, review the transcript, then Send. Browser speech support and any underlying recognition service vary by device; Quest voice capture has not been verified. Optional read-aloud uses browser speech synthesis, not spatial audio. The room's existing synthesized sound source follows Vesper.

## Dialogue/action boundary

The service sends a calm, curious station persona, recent conversational turns, and fresh telemetry. A navigation reply ends with exactly one registered command, for example:

`Heading to the window, Christopher. [ACTION: walk_to, target: "observation_window"]`

The server and client validate it; the client strips the tag and requests a collision-checked route. Unknown targets, duplicate commands, malformed commands, and trailing unparsed content are rejected. The model never controls arbitrary coordinates, scripts or external tools. Failed or unconfigured conversation is labeled honestly; local destination controls work independently.

## Evidence and limits

- Christopher physically verified Phase I on Quest 2 and reported excellent comfort with arc teleportation and 30-degree snap turns. This is his hardware report, not a measured 72 fps trace.
- Ten physics/navigation/service tests passed, covering all landmark pairs, existing collision invariants, telemetry, strict motor parsing, authentication, origin checks and conversation history.
- `phase-two-results.json`: real browser loading, articulated resident travel and arrival, explicit disconnected dialogue, and mobile layout.
- `dialogue-browser-results.json`: two mocked dialogue turns, stable session identity, headset telemetry, stripped motor tag, locomotion execution and spatial keyboard rendering. This does not establish real model quality.
- The initial desktop view measured about 62 draws and 19,063 triangles. Stereo entry measured about 122 draws and 32,822 triangles. Counts vary with viewpoint and panel visibility.
- IWER Quest 2 regression checks passed for stereo entry/exit, resident selection, tracked pose, teleport arc and release, blocked teleport, disconnect handling, smooth-movement collision, snap turn and physical-boundary blackout.
- Physical Quest 2 Phase II frame pacing, model appearance at headset scale, controller keyboard usability and voice support remain to be checked on the headset. Software-rendered Chromium/IWER cannot prove 72 fps.

## Sources and maintenance

- Model and CC0 attribution: https://github.com/mrdoob/three.js/tree/r185/examples/models/gltf/RobotExpressive
- Three.js GLTFLoader: https://threejs.org/docs/pages/GLTFLoader.html
- Responses API: https://developers.openai.com/api/reference/responses/create

`npm test` runs the physics, navigation and service tests. `node build.mjs` refreshes root and standalone bundles and includes the resident license. `node tests/phase-two.cjs`, `node tests/dialogue-browser.cjs` (requires the standalone server), and `node tests/xr.cjs` perform browser checks. The source GLB and original attribution live in `assets/`; it is embedded as binary data and parsed locally, with no runtime asset request.
