# Fog Mirror

Write on a foggy mirror with your finger. Your webcam shows up as frosted glass, and wherever you draw, the fog clears to reveal the sharp camera feed underneath. The strokes then slowly fog back over.

Built with plain HTML, CSS and JavaScript in a single file. Hand tracking uses [MediaPipe Hand Landmarker](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker).

## Live demo

Open it online (no install): **https://anushkadas-coder.github.io/fogMirror/**

Works on desktop and mobile. On a phone, just open the link, allow the camera, and draw on the glass with your finger. Tap **✋ hand tracking** to draw in the air instead.

## Features

- Realistic fog: a blurred, milky copy of your camera feed with grain and condensation droplets
- Works on **desktop and mobile**, in portrait and landscape
- Draw with your **mouse, touch, or your finger in the air** (hand tracking)
- Strokes fade and re-fog over time, and get thinner when you draw fast, like ink
- Gestures to clear the glass and change the brush size
- One-click **video recording** that saves straight to your computer
- Sliders for fade time and brush size

## Quick start

You need a webcam and a browser (Chrome or Edge recommended). The page must be served from `localhost`, because browsers restrict camera access on plain files.

1. Open a terminal in this folder.
2. Start a local server with whichever of these works on your machine:

   ```
   py -m http.server 8000
   ```
   ```
   python -m http.server 8000
   ```
   ```
   python3 -m http.server 8000
   ```
   ```
   npx serve
   ```

3. Open `http://localhost:8000` and allow camera access.

Alternative: in VS Code, install the **Live Server** extension, right-click `index.html`, and choose **Open with Live Server**.

> The first load needs an internet connection, because the MediaPipe library and the hand model are downloaded from a CDN. The camera feed itself never leaves your computer.

## Using it

### Drawing

| Input | What it does |
| --- | --- |
| Mouse or touch | Drag across the glass to draw (works when hand tracking is off) |
| ☝️ Index finger only | Draw in the air. Curl your other fingers |
| Open hand or fist | Lifts the pen, so you can move without drawing |

### Gestures

| Gesture | Action |
| --- | --- |
| ☝️ Index finger up, others curled | Draw |
| 🖐️ Open palm, held for about a second | Clear everything. A ring fills up while you hold |
| 👌 Pinch thumb and index, other three fingers up | Set brush size: hand high = big, hand low = small |

The status label next to the ✋ button shows what the tracker sees: `loading model…`, `not seen`, `seen, pen up`, `drawing ✍️`, `hold to clear…`, or `brush 34`.

### Controls

| Control | What it does |
| --- | --- |
| **fade** slider | How long strokes take to re-fog (2 to 14 seconds) |
| **brush** slider | Stroke width |
| **✋ hand tracking** | Turns hand tracking on or off (on by default) |
| **⏺ record** | Starts recording. Click again (**⏹ stop**) to save the video |
| **clear** | Wipes all strokes at once |

### Recording

- Click **⏺ record**, draw, then click **⏹ stop**. The file downloads automatically as `fog-mirror-<timestamp>.webm` (or `.mp4` where the browser supports it).
- The title is included in the video. The tracking dots and pen ring are hidden while recording.
- The video is mirrored, like looking into a mirror.
- Instagram usually wants `.mp4`. Convert the `.webm` with VLC, HandBrake or any online converter.

## Tips for good tracking

- Use bright, even light and keep your hand about an arm's length from the camera.
- Face your palm toward the camera and keep your hand fully in frame.
- A plain background helps.
- If drawing triggers too easily or not at all, check the status label to see which gesture is being detected.

## Mobile notes

- Use a recent **Chrome (Android)** or **Safari 15+ (iPhone/iPad)**. The page must be opened over `https://` (GitHub Pages does this) or `localhost`.
- Touch drawing is on by default. Hand tracking is off by default on phones to save battery and loading time. Tap **✋ hand tracking** to turn it on.
- Prop the phone up so your hand is fully in frame. Hand tracking works best with the phone steady.
- Rotating the phone clears the glass.
- Recorded videos save to Downloads (Android) or appear in the download prompt (iOS).

## Troubleshooting

| Problem | Fix |
| --- | --- |
| `python3` not found on Windows | Use `py -m http.server 8000` or `python -m http.server 8000` instead |
| Camera doesn't start | Allow camera access in the browser. Use `localhost`, and close other apps using the camera |
| Status stays on `loading model…` | The model download is blocked. Check your internet connection, ad blocker or firewall |
| Status says `error` | Press F12, open the Console, and read the red message |
| Status says `not seen` | Move your hand into frame, add more light, and face your palm to the camera |
| Recording doesn't download | Use a recent Chrome or Edge. Some browsers don't support `MediaRecorder` |
| Camera is blocked on a phone | Open the site over `https://`, and allow camera access in the browser's site settings |
| Page looks stale after an update | Hard-refresh with Ctrl+F5 |

## How it works

1. The camera video is drawn mirrored and scaled to fill the screen.
2. A tiny, downscaled copy of the video is stretched back up to create the blur, then tinted white, textured with grain, and covered with pre-drawn droplets.
3. Your strokes go into a mask. Each stroke segment fades with age.
4. The sharp video is cut out through that mask and drawn on top of the fog.
5. With hand tracking on, MediaPipe returns 21 landmarks per frame. The index fingertip becomes the pen, and finger distances relative to the wrist decide which gesture you are making.

## Project structure

```
fog-mirror-hand/
├── index.html   # the whole app: markup, styles, and script
└── README.md
```

## Privacy

Everything runs in your browser. The camera feed is processed locally and is never uploaded. The only network requests are for the MediaPipe library, the hand model and the Google Fonts stylesheet.

## Credits

Inspired by a fog mirror video seen on Instagram. Hand tracking by Google's MediaPipe.
