<p align="center">
  <img src="logo.svg" width="96" alt="Zoom WAV Converter logo">
</p>

<h1 align="center">Zoom WAV Converter</h1>

<p align="center">
  Split and decode Mid/Side WAV recordings from Zoom handy recorders, right in your browser.<br>
  <strong><a href="https://dcuartielles.github.io/Zoom-Wav-Converter/">Open the converter →</a></strong>
</p>

---

Zoom handy recorders in **4-channel mode** save every take as two stereo files: one from the **XY** microphone pair and one from the **MS** (Mid/Side) microphone. This tool turns those files into the separate tracks you need for editing, and decodes the MS file into normal left/right stereo.

Everything runs locally in your web browser. **Your audio is never uploaded anywhere.**

## How to use it

1. Open **https://dcuartielles.github.io/Zoom-Wav-Converter/** (works in any recent Chrome, Firefox, Safari or Edge).
2. Drag your WAV files onto the drop area, or click it to choose files. You can add several at once.
3. Each file gets its own panel. The mode is picked from the file name:
   - `…MS.WAV` → **MS raw** (channel 1 = Mid, channel 2 = Side)
   - `…XY.WAV` → **Split channels only** (left and right as two mono files)
4. Press **▶ Preview** to listen. Switch **Listen to** between *Decoded L/R*, *M only* and *S only*, and move the **Width** slider while it plays.
5. Click a **⬇** button to download the file you need.

### What you can download from an MS file

| Download | File name | What it is |
|---|---|---|
| M | `…_M.wav` | Mid (front-facing mic), mono |
| S + | `…_S_plus.wav` | Side signal as recorded (left lobe of the figure-8) |
| S − | `…_S_minus.wav` | Side signal polarity-inverted (right lobe) |
| Decoded stereo | `…_LR.wav` | Normal stereo: L = M + S, R = M − S |
| L (mono) / R (mono) | `…_L.wav`, `…_R.wav` | The decoded channels as separate mono files |

### Settings

- **Mode** – *MS raw* for Zoom MS-RAW files. *Stereo L/R → M/S* if your recorder already decoded the MS mic to stereo and you want M and S back. *Split channels only* for XY files or anything else.
- **Invert S** – flip the Side polarity. Use it if the decoded stereo sounds left/right mirrored compared with the XY file.
- **Width** – how much Side is mixed in. 0 % = mono (Mid only), 100 % = as recorded, 200 % = extra wide.
- **Output** – bit depth of the exported files. *Same as source* keeps 24-bit recordings at 24-bit.
- **Prevent clipping in L/R** – if M + S would go above 0 dBFS, the decoded stereo is turned down just enough to avoid clipping. The panel tells you by how much.

## Why can't S be split into two independent signals?

The Side capsule is a **figure-8** microphone. Its left lobe produces positive polarity and its right lobe negative polarity, but both come out as **one** signal. Sounds from the left and from the right are already mixed together in it. The only difference between the "left" and "right" side is polarity:

```
S_left  = +S
S_right = −S
```

Adding the Mid signal gives normal stereo, and the same math works backwards:

```
L = M + width·S          M = (L + R) / 2
R = M − width·S          S = (L − R) / 2
```

## Supported files

- WAV: 8/16/24/32-bit PCM and 32/64-bit float
- WAVE_FORMAT_EXTENSIBLE headers, Broadcast WAV (BWF) metadata chunks, RF64
- Mono, stereo and multichannel files (multichannel files can be split into one file per channel)

Very long recordings (over ~1 GB per file) may exceed the memory a browser tab is allowed to use. Split long takes before converting.

## Running it yourself

The whole tool is a single `index.html` file with no dependencies and no build step.

- **Offline:** download the repository (or the release zip) and open `index.html` in a browser.
- **Your own copy on GitHub Pages:** fork the repository, then go to *Settings → Pages* and publish from the `main` branch, root folder.

The audio code lives in the `<script id="core">` block of `index.html` (WAV parsing, M/S encode/decode and WAV writing) and has no browser dependencies, so it can be reused in Node.js or ported to other languages.

## Design

Colours follow the [Malmö University graphic profile](https://mau.se/en/Contact/news-and-press/grafic-manual/Colours/): Cherry red `#E4022D` as the primary colour, with Deep blue, Steel blue, Mint, Berry pink and Mandarin from the secondary palette. The page follows your system's light or dark mode.

## Credits

Made by David Cuartielles for students at the School of Arts and Communication (K3), Malmö University.
