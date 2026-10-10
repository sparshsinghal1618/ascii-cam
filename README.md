# ASCII Cam

> Your webcam feed rendered as **real-time ASCII art** — entirely in the browser.

<p align="center">
  <img src="assets/demo.gif" alt="Animated demo of ASCII Cam rendering a moving pattern as ASCII art" width="760">
</p>

ASCII Cam turns your webcam into a retro terminal. Every frame is downsampled,
converted to luminance, and mapped onto a ramp of characters (` .:-=+*#%@` …)
so the picture appears as text — live, at interactive frame rates.

**100% client-side. No uploads, no server, no dependencies.** The camera stream
never leaves your machine.

---

## Features

- 📷 **Live webcam → ASCII** in real time via `getUserMedia` + `<canvas>`
- 🔍 **2K capture** — requests up to **2560×1440** (with automatic fallback)
- 🎯 **Precise cells** — every character is a *box-average* of its source region, not a single sampled pixel
- 🧮 **Adaptive density** — the grid fills the screen at the screen's native character resolution
- 🎛️ **Toggleable controls** — Color / B&W, Mirror, Invert
- 🧪 **Demo mode** — a generated animated pattern when you have no camera
- 🎨 **Fast color mode** — glyphs are tinted via `background-clip: text` (no per-character DOM nodes)
- 🖥️ **No build step** — one self-contained `index.html`, zero dependencies
- 📱 **Responsive** — reflows to any window size while keeping the camera's aspect ratio
- 🔒 **Private by design** — frames are processed locally and never transmitted

---

## Quick start

### Just open it

Download or clone the repo and open `index.html` directly in a modern browser.

> Camera access requires a **secure context**. That means `https://`, `localhost`,
> or a `file://` page opened directly. Plain `http://` on a remote host will be
> blocked by the browser.

### Serve it locally

Any static server works, for example:

```bash
# Python 3
python -m http.server 8000

# Node
npx serve .
```

Then visit <http://localhost:8000>.

No camera? Click **Demo (no camera)** to see the renderer in action.

---

## Usage

| Control | What it does |
| --- | --- |
| **Start Camera** | Requests webcam access and begins rendering. |
| **Demo (no camera)** | Runs a generated animated pattern — no permissions needed. |
| **Color** | Toggles between colored characters and monochrome green (B/W). |
| **Mirror** | Flips the feed horizontally (on by default, like a mirror). |
| **Invert** | Inverts luminance — handy for dark backgrounds or negative looks. |

---

## How it works

1. The webcam is requested at up to **2560×1440**. Its frame is drawn straight into
   a tiny `COLS × ROWS` canvas, so the browser high-quality-downsamples the whole
   frame in one step — **every character cell becomes a box-average of its source
   region** rather than a single, noisy sampled pixel.
2. The grid size is derived from the window size and the font metrics, so the
   picture keeps the camera's aspect ratio and fills the available space.
3. Each cell's luminance (`0.2126R + 0.7152G + 0.0722B`) selects a character from
   the density ramp.
4. In **color mode**, the small color canvas is applied as the `<pre>`'s
   background and clipped to the text glyphs (`background-clip: text`), tinting
   every character with its own colour using zero extra DOM nodes.
5. The text is written to a `<pre>` element every animation frame.

The character ramp lives in `index.html` (`CHARS`); the grid dimensions are
computed automatically and adapt to any screen.

---

## Project structure

```
.
├── index.html          # the entire app (HTML + CSS + JS)
├── assets/
│   ├── favicon.svg     # site icon
│   ├── demo.gif        # animated README demo
│   └── preview.png     # static preview / social share image
├── .gitignore
├── LICENSE
└── README.md
```

---

## Deploy to GitHub Pages

1. Push this repo to GitHub.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to *Deploy from a branch*,
   pick the `main` branch and the `/ (root)` folder, then **Save**.
4. Your app will be live at `https://<your-username>.github.io/<repo-name>/`.

Because the app is served over `https://`, the camera works out of the box.

---

## Browser support

Works in current versions of Chrome, Edge, Firefox, and Safari. `getUserMedia`
requires a secure context, and the page must be served over `https://`,
`localhost`, or opened via `file://`.

---

## Contributing

Contributions are welcome. Open an issue to discuss a change, or fork the repo
and submit a pull request.

1. Fork the repository.
2. Create a branch: `git checkout -b feature/my-change`.
3. Commit your changes: `git commit -m "Add my change"`.
4. Push and open a Pull Request.

---

## License

Released under the [MIT License](LICENSE).
