# 2d-racer

A benchmark comparing how different frontier LLMs implement the same game from a single prompt: a 2-player hotseat, top-down 2D racing game built as a single HTML file with vanilla JS and the Web Audio API.

The exact brief given to every model is in [`prompt.txt`](./prompt.txt).

## The challenge

Each model had to produce, in one shot, a self-contained HTML file that implements:

- A randomly generated closed-loop track (800×600 canvas, waypoints sorted by angle) with asphalt, lane markings, grass, and a checkered start/finish line
- Two auto-accelerating arcade cars with steering-only controls (P1: `←`/`→`, P2: `A`/`D`), turning rate tied to speed, drift/inertia, car-vs-car bounce collisions, and off-road slowdown
- Race structure: 3 laps per round, 3 rounds per match, with a 3-2-1-GO countdown and a round/winner screen
- Procedurally generated audio via the Web Audio API: engine hum, collision thud, lap beep, round fanfare, start countdown
- An on-canvas HUD showing laps, current round, round wins, and controls
- No external dependencies and no build step

## Entries

Each directory contains one model's single-file attempt. Open the HTML file directly in a browser to play.

| Model            | File                                                               |
| ---------------- | ------------------------------------------------------------------ |
| Claude Fable 5   | [`fable-5/index.html`](./fable-5/index.html)                       |
| Claude Opus 4.6  | [`opus-4.6/index.html`](./opus-4.6/index.html)                     |
| Claude Opus 4.5  | [`opus-4.5/index.html`](./opus-4.5/index.html)                     |
| Gemini 3.1 Pro   | [`gemini-3.1-pro/index.html`](./gemini-3.1-pro/index.html)         |
| Gemini 3 Fast    | [`gemini-3-fast/index.html`](./gemini-3-fast/index.html)           |
| Gemma 4 31B IT   | [`gemma-4-31b-it/index.html`](./gemma-4-31b-it/index.html)         |
| GLM 5.1          | [`glm-5.1/index.html`](./glm-5.1/index.html)                       |
| GPT-5.5          | [`gpt-5.5/index.html`](./gpt-5.5/index.html)                       |
| Grok 4.5         | [`grok-4.5/index.html`](./grok-4.5/index.html)                     |
| Grok 4.2 Expert  | [`grok-4.2-expert/index.html`](./grok-4.2-expert/index.html)       |
| Kimi K2 Thinking | [`kimi-k2-thinking/index.html`](./kimi-k2-thinking/index.html)     |
| MiniMax 2.5      | [`minimax-2.5/racing.html`](./minimax-2.5/racing.html)             |

MiniMax 2.5 also produced a [`SPEC.md`](./minimax-2.5/SPEC.md) alongside its implementation.

Grok 4.5 also produced an earlier [`working but incomplete version`](./grok-4.5/index-not-completed-but-working.html).

Outside the benchmark, Claude Fable 5 and GPT-5.5 also made bonus 3D remakes in [`fable-5-3d/index.html`](./fable-5-3d/index.html) and [`gpt-5.5-3d/index.html`](./gpt-5.5-3d/index.html). They use Three.js from a CDN, so they need network access on first load.

## Running

No tooling required. Open any of the HTML files in a modern browser:

```sh
open opus-4.6/index.html
```

Click the canvas once if the browser blocks audio until a user gesture.

## Controls

- **Player 1 (red):** `←` / `→`
- **Player 2 (blue):** `A` / `D`
