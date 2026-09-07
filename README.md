# Therma Scan

**Thermal image analyser that runs entirely in your browser.**

### 🔗 Live site: <https://thermascans.vercel.app>

Upload a thermal (FLIR) image, tell it a couple of temperatures you already know,
and it works out the temperature across the whole picture — then finds the hot spots.

No server, no upload, no account. Your images never leave your device.

---

## What it does

1. **Load** a thermal image — drag and drop, or try the built-in sample.
2. **Calibrate** — type a temperature you know, click that spot on the image. Add 2 or more.
3. **Build the temperature map** — it fits a curve from pixel brightness to °C and paints the result.
4. **Analyse** — adjustable hot/cold thresholds, automatic detection of distinct hot regions.
5. **Export** — save the coloured image as PNG, or the temperature data as CSV.

## Who it's for

- **Electricians** — spotting loose connections and overloaded circuits in a fuse box
- **Building surveyors** — finding missing insulation, draughts, and damp
- **Mechanics** — checking bearings, brakes and exhaust manifolds
- **Anyone learning thermography**

---

## How accurate is it?

Honest answer: **good for comparing, not for certifying.**

Therma Scan reads the *brightness* of each pixel, not true sensor readings. A thermal
camera auto-adjusts its brightness range for every shot, so brightness is related to
temperature but isn't a fixed scale.

That means it is reliable for:
- finding which part of an image is hottest
- comparing two areas in the *same* photo
- spotting a problem that needs a closer look

It is **not** a substitute for a calibrated reading. Real FLIR files hide the raw
16-bit sensor data inside the file's metadata — reading that is a planned upgrade.

---

## How it works under the hood

| Step | Method |
|---|---|
| Brightness | Perceptual luminance: `0.299R + 0.587G + 0.114B`, normalised to 0–1 |
| Calibration | Least-squares polynomial fit (degree 1 with 2 points, degree 2 with 3+) |
| Thresholds | Percentile of a sampled subset of pixels |
| Hot regions | 4-connected flood fill, ranked by peak temperature |
| Palettes | Inferno, Jet, Ironbow, Greyscale |

Large images are scaled down to 1100 px on the long edge so the per-pixel maths
stays instant.

---

## Running it locally

It's a single HTML file with no build step and no dependencies.

```bash
git clone https://github.com/jawadhasuna/Therma-Scan
cd Therma-Scan
python -m http.server 8000
```

Then open <http://localhost:8000>.

## Deploying

Any static host works. This repo is set up for [Vercel](https://vercel.com) —
import it and deploy, no configuration needed.

---

## Origin

Ported from a Python + Tkinter desktop prototype (`flir01b.py`) that used
NumPy, Matplotlib and Pillow. The maths is the same; the browser version
needs nothing installed.

## Licence

MIT
