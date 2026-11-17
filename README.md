# ✋ Air Canvas

Draw in the air with your index finger. Air Canvas is a browser app that tracks your hand through the webcam and turns finger gestures into drawing, hovering, clearing and color changes — all in a **single HTML file**, with no backend and no build step.

> 🔒 **Privacy:** everything runs locally in your browser. Your video is never uploaded or stored.

<!-- Add a screenshot or GIF here, e.g. ![Air Canvas demo](demo.gif) -->

## Features

- Real-time hand tracking with [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker)
- Full-screen, mirrored webcam view with a glowing neon drawing layer
- Gesture-based controls (no mouse or keyboard needed)
- Adaptive smoothing: steady lines when moving slowly, almost no lag when moving fast
- Orientation-invariant finger detection (works when your hand is tilted or rotated)
- Tolerates brief tracking dropouts caused by motion blur
- Live readout of tracking FPS and camera FPS
- Modern dark glassmorphism UI

## Gestures

| Gesture | Action |
|---|---|
| ☝️ Index finger only | **Draw** |
| 🖐️ Open hand (all four fingers up) | **Hover** – move the glowing dot without drawing |
| ✊ Fist (hold briefly) | **Clear** the canvas |
| ✌️ Two fingers (index + middle) | **Next color** – changes once per gesture (cyan → red → green → yellow) |
| 3 or 4 fingers | No action |

**Keyboard / mouse:** `C` clears the canvas, `1`–`4` pick a color directly, and the top bar has color swatches and a **Clear Canvas** button.

## Getting started

### Option 1 — Open locally
1. Download or clone this repository.
2. Open `index.html` in **Chrome**, **Edge** or **Brave**.
3. Click **Start Camera** and allow camera access.

No server is needed. You do need an internet connection the first time, because MediaPipe is loaded from a CDN.

### Option 2 — GitHub Pages (live demo)
1. Push this repo to GitHub.
2. Go to **Settings → Pages**, choose your main branch and the root folder, then save.
3. Open the URL GitHub gives you. Pages uses HTTPS, which browsers require for camera access on hosted sites.

## Tips for best results

- Use good, even lighting. It improves detection and lets webcams run at higher frame rates.
- Keep your whole hand inside the frame, with your palm facing the camera.
- Enable **hardware acceleration** in your browser settings.
- If the FPS readout shows `cam 30`, your webcam is the limit, not the code.

## Configuration

Tune behavior in the `CFG` object near the top of the script in `index.html`:

| Setting | What it does |
|---|---|
| `smoothBase`, `smoothGain` | Line smoothing at rest vs. while moving fast |
| `stableFrames`, `leaveDrawFrames` | How quickly gestures start and stop |
| `clearHoldFrames`, `colorHoldFrames` | How long the fist / two-finger pose must be held |
| `extAngle`, `extReach`, `foldAngle`, `foldReach` | Finger straight/curled thresholds |
| `camModes`, `camFps` | Camera resolutions and frame rate to request |

## How it works

1. `getUserMedia` streams the webcam into a mirrored `<video>` element.
2. MediaPipe Hands returns 21 landmarks per frame. Each finger is judged extended or folded from its joint angle and its reach from the wrist, with hysteresis to avoid flicker.
3. The pose is classified (draw / hover / clear / color), debounced over a few frames, and mapped to screen coordinates.
4. The fingertip is smoothed with an adaptive exponential moving average and drawn as curved segments on an HTML5 canvas overlay.

## Browser support

Latest Chrome, Edge and Brave work best. Safari and Firefox may run slower or behave differently with MediaPipe's WebGL runtime.

## Troubleshooting

- **Blank screen or "MediaPipe failed to load":** check your internet connection and reload.
- **Camera permission denied:** allow camera access in the browser's site settings and try again.
- **Hand not detected:** improve the lighting and move your hand a little farther from the camera.
- **Low FPS:** see the tips above.

## Credits

- Hand tracking by [MediaPipe Hands](https://github.com/google-ai-edge/mediapipe) from Google, licensed under Apache 2.0.
- Built with the assistance of [Claude](https://www.anthropic.com/claude), an AI assistant by Anthropic. The code was reviewed and published by the repository author.

## License

Released under the [MIT License](LICENSE).
