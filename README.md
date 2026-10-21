# fityou
A little guidance for every rep. Match the silhouette, find your rhythm, and move with intention.
# FitYou

Responsive camera-based exercise practice web app for phones, tablets and desktops.

## Included

- Bicep curl and stationary lunge animated silhouette guides.
- Live camera preview with front/rear camera switching.
- On-device MediaPipe pose tracking, approximate joint angles and rep counting.
- Mobile inline video playback and camera-permission help.
- Bundled model, JavaScript runtime and WebAssembly files.

## Deploy

This is a static web application. There is no build step, npm installation, API key, database or backend.

1. Extract this archive.
2. Upload the **contents of `FitYou-source/`** to a static web host, retaining the `vendor/` directory and its contents.
3. Set the published directory to the folder containing `index.html`. Leave the build command empty.
4. Enable HTTPS on the hosting domain.
5. Open the HTTPS address directly in Safari or Chrome. Tap **Enable camera**, then allow camera access.

The same directory can be served from a subfolder; application assets use relative paths. Serve `.mjs` and `.js` as JavaScript (`text/javascript`), and `.wasm` as `application/wasm`. The model must be downloadable from `vendor/pose_landmarker_lite.task`.

The original ChatGPT Site's access restrictions are hosting-level controls. This exported app has no built-in sign-in. Configure access restrictions on your own host if required.

## Run locally

From this folder, with Python 3 installed:

```sh
python3 -m http.server 8080
```

Open `http://localhost:8080` on the **same computer**. Camera access is available on localhost in supported browsers. For testing on an iPad or phone, use an HTTPS deployment; a plain HTTP LAN address is not sufficient. Opening `index.html` by double-clicking it will not reliably load the model or camera.

## Files to edit

| File | Purpose |
| --- | --- |
| `index.html` | App name, page structure, exercise choices and interface text |
| `style.css` | Responsive layout, colours and typography |
| `app.js` | Camera lifecycle, pose tracking, guide drawing, feedback and rep counting |
| `vendor/vision_bundle.mjs` | MediaPipe Tasks Vision 0.10.21 runtime |
| `vendor/wasm/` | SIMD and non-SIMD WebAssembly runtimes |
| `vendor/pose_landmarker_lite.task` | Pose Landmarker Lite model |

`settings` in `app.js` contains exercise labels and cues; `assess()` contains angle thresholds, visibility checks and rep-state logic. `guidePoints()` controls the silhouette. `startCamera()` handles camera permissions and inline video playback.

## Mobile camera notes

The code requests video only, uses `muted`, `autoplay` and `playsinline`, requests the camera from a button tap, uses preferred rather than mandatory camera constraints, and stops previous tracks when switching cameras. Camera streams stop when the page is hidden or closed.

On iPad/iPhone, open the site directly in Safari. If denied, allow camera access in the browser's website settings and retry. Embedded browsers can restrict camera access.

## Privacy and network use

Video frames are processed in the browser. The app does not record or upload camera video. The pose model and runtime are included locally. Styling requests fonts from Google Fonts; remove the first `@import` in `style.css` to use system fallback fonts without that external request.

## App packaging

This package is the complete **web app source**, not a native iOS/Android project or an App Store binary. Native distribution requires a separate wrapper/project, platform permissions and signing. This export does not add a PWA manifest or service worker.

## Verification and limitations

JavaScript syntax and simulated angle, rep-state, tracking-loss and camera-lifecycle checks passed during development. Physical iPad/iPhone/Android camera behaviour has not been verified.

The silhouette is a visual guide. Feedback is based on approximate 2D joint angles and simple posture checks; it does not establish that an exercise is safe or performed correctly in every respect. Camera placement and occlusion affect tracking. Rep counting pauses when necessary joints are not visible.

## Source snapshot

Exported from FitYou version 2, source commit `21849df94bff4330460894fb91f81c0a1163c022`. Site credentials, Git history and hosting-specific project metadata are excluded.

## Third-party components

MediaPipe Tasks Vision 0.10.21 and the Pose Landmarker Lite model are from Google's MediaPipe project. Preserve the copyright and license notices embedded in the supplied runtime files. See `THIRD_PARTY_NOTICES.md` for upstream references.
