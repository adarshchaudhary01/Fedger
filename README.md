# Ledger

A single-purpose AI image generator that runs entirely in the browser. No backend, no build step, no API key.

## What it does

Type a description, pick an optional style and aspect ratio, and Ledger logs it as an "entry" — generating an image via the free [Pollinations](https://pollinations.ai) image API and displaying it on the page.

## Files

| File | Purpose |
|---|---|
| `index.html` | Page structure and markup |
| `style.css` | All visual styling, including the animated gradient border shown while an entry is generating |
| `script.js` | Builds the Pollinations API request, handles loading/error states, and wires up the UI |

## Running it

No install or server required. Open `index.html` directly in a browser, or serve the folder with any static file server:

```bash
python3 -m http.server
```

Then visit `http://localhost:8000`.

## How image generation works

Each entry is requested as a plain image URL against Pollinations' free Flux model:

```
https://image.pollinations.ai/prompt/<encoded prompt>?width=&height=&seed=&nologo=true&model=flux
```

- **Seed** — a random number generated per entry, shown as the "entry no." It's also what makes "re-log entry" produce a different result from the same prompt.
- **Style chips** — prepend a short style phrase (e.g. "cinematic film still, dramatic lighting,") to the prompt before it's sent.
- **Aspect ratio** — maps to fixed width/height pairs (1:1 → 1024×1024, 16:9 → 1280×720, 9:16 → 720×1280).

No image, prompt, or seed is stored anywhere by this app — everything lives only in the browser tab for the current session.

## Notes on the free API

Pollinations' base image endpoint works without signing up or providing an API key. If you start hitting rate limits, Pollinations offers free API keys at [enter.pollinations.ai](https://enter.pollinations.ai) — append `&token=YOUR_KEY` to the URL built in `buildUrl()` inside `script.js`.

## Browser support

The animated border on the prompt panel uses `background-clip: border-box` layering, which works in all current evergreen browsers. Motion is automatically disabled for users with `prefers-reduced-motion` set.
