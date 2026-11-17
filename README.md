# Breadboard AR

Step-by-step AR breadboard guide for a **fixed classroom rig**: a camera looks straight down at a breadboard, a screen shows the live view, and each press of **NEXT** highlights the exact holes for the next real component.

Live: https://cooperlad92.github.io/Breadboard/

## How it works
- **No tracking.** Because the camera and board never move, a one-time calibration (tap 4 corner holes + 3 rail holes) gives an exact mapping from every hole to camera pixels. The overlay is drawn flat on the video, so it can't drift or jitter.
- **Library.** `library.json` is the shared classroom library, loaded by every unit. Circuits made in the built-in editor are saved on that device; use **Export** to download them, then add them to `library.json` to share with every rig.
- **Diagram fallback.** With no camera calibrated, circuits run on a drawn breadboard diagram, which is handy for building circuits on a laptop.

## Rig setup (Raspberry Pi + 7" screen)
1. Mount the camera so the whole board, including both power rails, fills as much of the frame as possible.
2. Kiosk launch (auto-allows the camera prompt):
   ```
   chromium-browser --kiosk --use-fake-ui-for-media-stream https://cooperlad92.github.io/Breadboard/
   ```
3. Tap **📷 Camera setup**, choose the camera, rotate or mirror until the view matches what students see, then tap the 7 holes. Check that the dot grid sits in every hole, then **Save**. Redo this only if the camera or board moves.

A USB webcam works with Chromium straight away. A ribbon-cable Pi camera module may need extra setup before Chromium can see it.

## Controls
`→` / `Space` / `Enter` = next · `←` = back · `Esc` = exit. Map a GPIO button to a key for a physical NEXT button.

## Circuit format
```json
{ "id": "my-led", "name": "LED", "board": "full",
  "steps": [ { "title": "…", "text": "…", "power": "on|press|off",
               "parts": [ { "k": "res", "a": "i8", "b": "i14", "ohms": 220 } ] } ] }
```
Holes are `a1`–`j63`. Rail holes are `T+n`, `T-n`, `B+n` and `B-n`, where T is the row-j side, B is the row-a side, and n counts the rail's own holes from the column-1 end.
Part kinds: `wire` (a, b, color) · `res` (a, b, ohms) · `led` (a = +, b = −, color) · `diode` (a = anode, b = stripe) · `cap` (a, b, label, pol) · `btn` (at = e-row left leg) · `ic` (at = pin 1 in row e, pins, label) · `lead` (to = rail hole, color, label) · `probe` (at, color, label).
