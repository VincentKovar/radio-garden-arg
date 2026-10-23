# SIGNAL // GARDEN

```
  ░██████╗██╗░██████╗░███╗░░██╗░█████╗░██╗░░░░░
  ██╔════╝██║██╔════╝░████╗░██║██╔══██╗██║░░░░░
  ╚█████╗░██║██║░░██╗░██╔██╗██║███████║██║░░░░░
  ░╚═══██╗██║██║░░╚██╗██║╚████║██╔══██║██║░░░░░
  ██████╔╝██║╚██████╔╝██║░╚███║██║░░██║███████╗
  ╚═════╝░╚═╝░╚═════╝░╚═╝░░╚══╝╚═╝░░╚═╝╚══════╝
        live global radio  +  hidden transmissions
```

**Spin a globe. Tune into real radio from anywhere on Earth. Then find the station that isn't supposed to be there.**

SIGNAL // GARDEN is a [radio.garden](https://radio.garden)-style 3D radio globe with a hidden layer built for alternate reality games. Every green dot is a real, live station. Somewhere out there are a few that aren't on any map, and they only broadcast when the time is right.

**[▶ Live demo](https://vincentkovar.github.io/radio-garden-arg/)**

---

## How the game works

1. **Players find a clue** somewhere in the real world: a poster, a video, a puzzle, a postcard. It points to a place on Earth.
2. **They zoom the globe in on that spot.** Nothing is marked. If they're close enough and the transmission window is open, the static breaks and a **TRANSMISSION DETECTED** alert appears.
3. **They enter the access code** they've puzzled out.
4. **The prerecorded message plays.** Spoken audio, Morse code, or a real audio file.

Get the right place at the wrong time and you only see *"residual signal, dormant"*. That confirms you're in the right spot without spoiling anything, and tells you to come back.

## Features

- **Live global radio**: about 1,200 geo-tagged stations from the open [Radio Browser](https://www.radio-browser.info) directory. Click a dot, hear the world. Search by station or country, or hit shuffle.
- **Hidden stations**: invisible on the globe until a player zooms in near their coordinates.
- **Time windows**: each secret station has an active-from and active-until time.
- **Unlock codes**: case-insensitive; players get unlimited tries.
- **Three payload types**: text-to-speech, Morse code synthesizer, or an audio file URL.
- **Solved state**: once decoded, a station becomes a replayable archive for that player.
- **Soft-clue markers**: optionally show a faint, always-on gold marker instead of a fully hidden station.
- **Game Master panel**: add, edit, and delete stations in the browser; no code changes needed.
- **Zero backend**: one `index.html`, no build step, no server.

## Running it

The page needs to be served over HTTP, because browsers block `fetch()` to the station directory from `file://`.

```bash
cd radio-garden-arg
python3 -m http.server 8124
# open http://localhost:8124
```

It's a plain static site, so it also deploys as-is to GitHub Pages, Netlify, or Vercel.

## Setting up your game

Click **GM CONFIG** (top right) to open the Game Master panel.

| Field | What it does |
|---|---|
| Station title | Shown in the archive and now-playing bar after decoding |
| Unlock code | What players must enter (case-insensitive) |
| Latitude / longitude | Where the station lives. **Use current globe view** fills these from wherever you've navigated |
| Active from / until | The window when the code prompt appears |
| Marker visibility | Fully hidden, or a faint always-on marker as a soft clue |
| Payload type | `speech` (text-to-speech), `morse`, or `audio` (URL to an audio file) |
| Payload | The text to speak or encode, or the audio file URL |
| GM clue note | Private reminder to yourself, never shown to players |

A demo station ships in the box: zoom into **London** to see the whole loop. Its code is `8821`.

### Detection tuning

Constants live in the script at the bottom of `index.html`:

- `DISCOVERY_ALTITUDE_THRESHOLD` (`0.55`): how far zoomed in a player must be before anything can be detected.
- Detection radius: `max(60 km, altitude × 500 km)`, so players must be closer the further they're zoomed in.

## Before you run a real game

- **Config is per-browser.** The GM panel saves to `localStorage`, so stations you set up on your machine won't appear for players. To ship stations to everyone, write them into the `defaultSecrets()` list in `index.html` and redeploy.
- **Codes are visible in the page source.** Everything runs client-side, so a determined player can read `index.html` and find the codes. That's fine for a casual hunt. For high-stakes puzzles you'd want a small server that checks codes and serves the audio.
- **Some streams won't play.** Stations are independent broadcasters. Some block hotlinking, and some only offer plain-HTTP streams, which browsers refuse to play on an HTTPS page. This is a limit of the live data, not the game layer.
- **Text-to-speech varies by device.** Voices differ between browsers and operating systems. For a consistent message, use the `audio` payload type with a recorded file.

## Tech

- [globe.gl](https://globe.gl) (three.js) for the 3D globe
- [Radio Browser API](https://api.radio-browser.info) for live stations
- Web Audio API and Web Speech API for secret payloads
- Tailwind (CDN) for styling
- Vanilla JavaScript, no framework

## Credits

Station data comes from the community-run [Radio Browser](https://www.radio-browser.info) project. The audio streams belong to their respective broadcasters. Inspired by [radio.garden](https://radio.garden), which this project is not affiliated with.
