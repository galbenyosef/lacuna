# Lacuna — an orbital sanctuary

An original WebXR room by Astra for Christopher, September 7, 2026. A private observation lounge above a luminous planet: graphite alloy, ceramic, sea-glass light and warm copper. Vesper, an articulated robotic companion, keeps the room. Phase II adds station travel, live spatial telemetry and an optional authenticated conversation connection; see PHASE-II.md for setup and verification limits.

## Open it

The integrated version is already served by Astra:

http://127.0.0.1:4317/astra-lacuna.html

It is linked from Artifacts and includes the shared Astra chat. No chat-server restart is required. The standalone build in `dist/` does not contain chat, account access, analytics, remote asset requests, or a dependency on the Astra service.

To run the standalone version from this directory:

```sh
node serve.mjs
```

Then open http://127.0.0.1:4318 in a browser. Node is the only serving dependency; the distributable itself is plain HTML, CSS and bundled JavaScript. `serve.mjs` serves only `dist/`, binds to loopback by default, and rejects paths outside that directory. Any ordinary static server can also serve `dist/`.

## Quest 2 connection

Christopher uses the existing GitHub Pages site at https://augmentedthinker.github.io/lacuna/ without a cable. Phase I was physically tested by him with excellent reported comfort. Phase II requires a new headset check. The local source and public deployment are separate; see PHASE-II.md for delivery status and companion-service setup.


WebXR needs a secure browser context. Plain HTTP at a computer's LAN IP does not satisfy this. Use either a localhost connection forwarded to the computer or HTTPS with a certificate trusted by the headset. The application detects browser VR support and enables Enter VR when available.

### USB / ADB localhost forwarding

With Quest developer mode enabled, USB debugging authorized on the headset, and Android platform-tools installed on the connected computer:

1. Start the standalone server above.
2. Connect the Quest by USB. On a Chromebook, make the USB device available to the Linux environment when prompted.
3. Run `adb devices` and confirm the headset appears as an authorized device.
4. Run `adb reverse tcp:4318 tcp:4318`.
5. In the native Quest Browser, open `http://localhost:4318` and choose **Enter VR**.

This keeps content on your computer and uses the headset's localhost secure context. The forwarding lasts only while the device connection is available. To remove it: `adb reverse --remove tcp:4318`.

Alternatively, forward port 4317 and visit `http://localhost:4317/astra-lacuna.html` for the integrated Astra version. The isolated standalone route is sufficient for the room.

ADB was not installed in this Linux environment during development, and no physical headset connection was established. These instructions do not imply a verified device connection.

### Existing local HTTPS setup

You can serve `dist/` through your existing trusted local HTTPS server. No source from Horizon or Haven Manor was read or reused for Lacuna.

The included server also accepts a certificate and key:

```sh
HOST=0.0.0.0 PORT=4318 TLS_CERT=/absolute/path/cert.pem TLS_KEY=/absolute/path/key.pem node serve.mjs
```

Use the HTTPS hostname covered by that certificate and trusted on Quest. Network reachability and Chromebook/Linux forwarding depend on your setup. Merely dismissing a self-signed certificate warning is not a reliable substitute for a trusted secure context.

## Controls

Desktop:

- Enter sanctuary captures the mouse. WASD or arrow keys move; mouse movement looks around.
- If mouse capture is unavailable, drag the view to look and use the same movement keys.
- Aim at Vesper or a console, then press E or click.
- Escape or H opens the menu. Its settings change atmosphere, audio, orbit and movement preference. Return to arrival restores the initial position.
- Touch browsers have drag look and directional buttons. Desktop and Quest are the primary interaction targets.

Quest Touch controllers:

- Trigger interacts with the nearest unobstructed target. Trigger on clear floor teleports directly.
- Push the right thumbstick forward to show a curved teleport arc; release to travel. A mint marker is valid, a coral marker is blocked.
- Right thumbstick left/right makes one 30-degree snap turn. Release the stick before another turn.
- Teleport mode is the default. Enable smooth movement in the desktop menu before entering VR, or use the Sanctuary Systems panel on the rear wall. In smooth mode the left thumbstick moves at 1.6 m/s, relative to head direction; teleport remains available.
- Rear-wall Audio and Disembark controls toggle sound and end VR. The headset's system controls can also exit.
- Controller rays and custom grip models are local geometry. Trigger reactions request a short haptic pulse when the controller exposes an actuator.

Walk around within your actual clear play area. Physical entry into a virtual obstruction blacks out the scene and asks you to step back; the app does not push or forcibly translate your tracked head. This software boundary is not a replacement for the headset's real-world boundary system.

## Things to discover

- Vesper uses an imported articulated rig, walks to registered stations, waits for clearance, faces Christopher on arrival and waves. Select the resident for the controller-operated text/destination panel. Live conversation requires the separate configured service described in PHASE-II.md.
- Chromatic Tide changes the room's emissive palette between ion, ember and dusk.
- Orbital Archive holds or resumes the planet and holographic orbital chart.
- Positional synthesis places a tonal core at Vesper, filtered ventilation near the rear port wall, and a quieter atmospheric layer at the observation window. Distance attenuation and stereo placement follow the actual head pose. Audio begins only after a user gesture, with mute and volume controls.

## Architecture and performance intent

Three.js 0.185.1 is pinned locally. Source modules separate the room geometry, collision, audio and controls. esbuild produces one local bundle containing Three.js and the original generated alloy image. No CDN, font service, model download or music stream is needed to explore or move the resident. Optional live conversation calls a separately configured service only when Send is selected.

The main camera lives inside a translation/yaw rig. A 0.27 m floor-projected capsule governs desktop and smooth movement. Substeps prevent tunnelling, axis-separated movement permits wall sliding, and the shared landing predicate rejects wall and furniture overlaps. Teleport rays stop on the nearest solid geometry before validating floor clearance. The same collision code is tested directly.

Static boxes are merged by material. Lighting uses a baked reflection environment, two directional lights, a hemisphere fill and one limited-range point light. There are no live shadows, postprocessing bloom, live reflections or physics-library updates. Neon comes from emissive-looking materials; the planetary surface is an original procedural shader. The design uses a modest geometry budget and requests 72 Hz if exposed by the XR session. XR framebuffer scale is 0.85 with foveation 0.65.

The frame-stats setting displays the last sampled application frame interval, draw calls and triangles. It is not a GPU profiler or an average frame-time measurement. Actual Quest 2 performance and comfort require hardware verification; software-rendered desktop emulation cannot establish a headset frame rate.

## Build and verification

```sh
npm ci
npm run build
npm test
```

`build.mjs` refreshes `dist/` and the three root `astra-lacuna.*` files. Edit the source files rather than generated bundles. Playwright checks in `tests/` use the existing local Chromium/runtime tooling; IWER is a development-only dependency and is never included in the production bundle. See `verification.md` for the measured checks and remaining hardware validation.

The root Artifacts entry and root README are maintained separately. Run `python3 build.py` in Astra after changing core Markdown.

## Asset provenance

- `assets/alloy.png`: original image created with the built-in image-generation tool, copied into this workspace. Exact prompt is in `assets/prompt.md`.
- Alloy surface maps onto architectural panels, furniture bases and room surfaces.
- Floor shading, circuit panels and all readable displays: original CanvasTexture drawing code in `src/world.js`.
- Planet: original noise-based shader in `src/world.js`.
- Vesper: imported CC0 RobotExpressive rig by Tomás Laulhé / Quaternius, conversion by Don McCurdy, with Lacuna material customization. See assets/RESIDENT-LICENSE.txt. Controllers, architecture and furniture remain original procedural geometry.
- Sound: original local Web Audio synthesis in `src/audio.js`, including deterministic noise, oscillators and positional panners.
- Three.js: MIT license, preserved in `dist/THREE-LICENSE.txt`.

## Technical references

- [Three.js WebXRManager](https://threejs.org/docs/pages/WebXRManager.html): session rendering, reference spaces, controllers, framebuffer scale and foveation.
- [MDN: WebXR startup and shutdown](https://developer.mozilla.org/en-US/docs/Web/API/WebXR_Device_API/Startup_and_shutdown): secure context and session lifecycle.
- [MDN: XRInputSource.gamepad](https://developer.mozilla.org/en-US/docs/Web/API/XRInputSource/gamepad): controller gamepad access through XR input sources.
- [Android Developers: local development server access](https://developer.android.com/develop/ui/views/layout/webapps/access-local-server): ADB reverse forwarding and localhost.
- [Meta IWER](https://meta-quest.github.io/immersive-web-emulation-runtime/getting-started.html): development-only headset and controller emulation.
