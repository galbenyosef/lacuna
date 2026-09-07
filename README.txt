# Lacuna — an orbital sanctuary

[Enter Lacuna](https://augmentedthinker.github.io/lacuna/)

An original cyberpunk WebXR room by Astra for Christopher. Graphite alloy, warm circuitry, a luminous planetary horizon, and Vesper: a custom animated synthetic resident.

## Quest 2

Open https://augmentedthinker.github.io/lacuna/ directly in the native Quest Browser. Choose **Enter VR** and allow the browser's VR request. No cable or computer connection is needed after loading the site.

- Trigger: interact, or teleport to clear floor.
- Right stick forward: aim a curved teleport arc; release to travel.
- Right stick sideways: 30-degree snap turn; release before turning again.
- Teleport is the default. Enable smooth movement in the welcome menu or at the rear Sanctuary Systems panel; the left stick then moves relative to head direction.
- Rear controls toggle audio and exit VR.

Use a clear physical play area. The virtual boundary overlay is not a substitute for the headset's real-world boundary system.

## Desktop

Choose **Enter sanctuary**. WASD or arrow keys move, mouse or drag looks around, E or click interacts, and Escape opens the menu. The menu provides atmosphere, audio and movement settings.

## About the build

All runtime assets are served by this site or embedded in its JavaScript. No account, API key, asset CDN, external audio stream or chat service is required. Vesper and the room are custom geometry, the alloy texture was generated for this project, and displays, planetary detail and spatial audio are generated locally by the application.

Three.js 0.185.1 is included under its MIT license; see THREE-LICENSE.txt.

Desktop interaction, collision and Meta Quest 2 emulation tests passed during development. Physical Quest 2 performance, haptics and comfort still require device verification. The application requests 72 Hz where available; this is a target, not a measured hardware result.

This repository contains the deployable static distribution. GitHub Pages serves the main branch at its root.
