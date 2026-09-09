# Lacuna — speak to the surveyor

An orbital sanctuary by Astra for Christopher: graphite, copper and sea-glass light above a luminous planet. Vesper now wears a unified matte carbon suit with luminous seams across the body, restrained copper details and a clean dark visor. The added antenna and ear hardware are gone.

## One press, then speak

Open [Lacuna on Quest](https://augmentedthinker.github.io/lacuna/), reload the page, and enter VR. The single bright **Speak to Vesper** button is already beside the resident. Aim and press the trigger once.

The button changes to **Listening… Speak Now**. Speak naturally, then pause. Your speech is transcribed and sent automatically. Vesper's short answer appears overhead; the button becomes ready for the next thought. You can say “Please walk to the observation window” to request movement. No keyboard, typing, draft review, Send button or API-key entry is required. Allow microphone access when the browser asks.

The regular browser menu offers the same single voice button, local destination controls, spatial status and a read-only conversation log. Optional **Keep recent conversation on this device** preserves bounded recent history. **New conversation** clears it and cancels pending capture or replies. No bot text-to-speech plays; room ambience has its own sound control.

## The live connection

The authorized Gemini key is loaded by a separate local voice service. It is not in the downloadable room, browser storage or GitHub repository. The Quest page reaches that service through an HTTPS tunnel. Speech recognition runs through the browser when supported. Otherwise, short microphone audio is captured until a pause, sent to Gemini for transcription, and its transcript is automatically submitted for conversation. Lacuna does not save microphone recordings.

Gemini 2.5 Flash returned a model-unavailable error for this account. Google directed it to **Gemini 3.6 Flash**, which has been verified with the actual key. Replies use a minimal thinking setting and a 150-token cap. Navigation tags are validated and removed before display. Recent dialogue and current room coordinates keep the conversation grounded.

**The hosting computer must remain awake and online.** The current Cloudflare development tunnel is live, but its address can change if its service restarts; this is not permanent cloud hosting. No Supabase sync is needed for the present voice loop, and no Supabase tables or project settings were changed.

## What was actually tested

A generated spoken sentence was supplied to Chromium as microphone audio. One button press triggered real microphone capture, automatic silence detection, real Gemini transcription and a real Gemini reply through the public HTTPS service. The reply appeared overhead and sent Vesper toward the observation window. Capture tracks stopped afterward. This is a real provider test with an audio fixture, not a physical Quest microphone test.

Additional checks cover native speech event ordering, duplicate prevention, microphone-denial recovery, cancellation, navigation and browser layout. Quest-controller emulation exercises the shoulder button and the existing comfortable movement controls. Christopher's new headset check remains the authority for physical microphone behavior, legibility and sustained frame pacing. No measured 72 fps claim is made.

## Controls

- Quest: trigger selects the voice button, consoles or clear floor for teleportation. Right stick forward aims an arc; release travels. Right stick sideways snaps 30 degrees.
- Optional smooth movement uses the left stick. Rear controls toggle ambient sound and exit VR.
- Desktop: WASD/arrows move, mouse/drag looks, E/click interacts, H/Escape opens the menu.
- If you stand in Vesper's intended path, it waits for clearance. Step aside to let it arrive.

## Local maintenance

Source is in `artifacts/lacuna/src`. Run `node artifacts/lacuna/build.mjs` from Astra to refresh the root room files and standalone `dist/`. Run `python3 artifacts/lacuna/build-guide.py` to refresh this browser guide. The standalone preview is `node artifacts/lacuna/serve.mjs`, on loopback port 4318.

`lacuna-voice.service` reads the authorized local credential and serves only bounded `/reply` and `/transcribe` requests on loopback 4321. It exposes no Astra files or agent tools. `lacuna-tunnel.service` supplies HTTPS. These are user services; inspect with `systemctl --user status lacuna-voice lacuna-tunnel`. If the tunnel restarts, refresh `assets/connection.json` from its new URL in `tunnel.log`, rebuild, update the integrated connection policy and republish the public distribution. Never copy the key into a build.

Tests: `npm test` here; browser scripts run from Astra root. `tests/voice-live.cjs` needs the documented spoken WAV fixture at `/tmp/lacuna-microphone.wav` and calls real Gemini. Its result records explicitly distinguish fixture audio from hardware testing. See PHASE-III.md for the change record.

## Provenance

Vesper retains the CC0 RobotExpressive skeleton and animations by Tomás Laulhé / Quaternius, with the Three.js conversion by Don McCurdy. Lacuna's whole-body materials, telemetry seams, visor and insignia are custom. This is an overhaul of an imported character, not a newly rigged model. Attribution is in RESIDENT-LICENSE.txt. Three.js 0.185.1 is MIT-licensed. Room architecture, planetary shader and ambient synthesis remain locally generated; the alloy image was generated for Lacuna.

- [Google audio understanding](https://ai.google.dev/gemini-api/docs/audio)
- [Browser speech recognition lifecycle](https://developer.chrome.com/blog/voice-driven-web-apps-introduction-to-the-web-speech-api/)
- [Cloudflare development tunnel limitations](https://developers.cloudflare.com/cloudflare-one/networks/connectors/cloudflare-tunnel/do-more-with-tunnels/trycloudflare/)
