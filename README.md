# PicSqueeze

<img src="cat-icon.svg" width="72" alt="PicSqueeze cat logo">

Free, private, batch image compression — entirely in your browser. **No uploads, ever.**

**Live site:** https://tliens.github.io/picsqueeze/

Compress up to **20 images per batch** (JPEG / PNG / WebP input) with:

- 🧠 **Smart mode** — dimension-aware quality + skip-if-larger guard (a file that wouldn't get smaller is returned untouched)
- 🎯 **Target size** — binary-searches the highest quality that fits under a KB budget; auto-downscales as a last resort
- 🎚️ **Manual quality** — 30–100; WebP at 100 switches to true lossless
- 🔁 **Format conversion** — keep original / JPEG / PNG / WebP, plus max-dimension resizing (progressive half-stepping)
- 🔬 **SSIM quality score** per result, and a draggable before/after comparison
- 🌓 EN / 中文 UI, light / dark themes, drag-drop / paste / file picker

## How it works

PicSqueeze compiles the same open-source encoders used by Google's Squoosh to WebAssembly
(via the excellent [jSquash](https://github.com/jamsinclair/jSquash) project), and runs them
inside a **module worker** so the UI never blocks:

| Engine | Used for | Tuning |
|---|---|---|
| [MozJPEG](https://github.com/mozilla/mozjpeg) | JPEG | progressive scan, optimized Huffman, trellis quant-table search, dimension-aware quality |
| [OxiPNG](https://github.com/shssoichiro/oxipng) | PNG (lossless) | filter/strategy search, alpha optimization, 3 effort presets — pixels stay 100% identical |
| [libwebp](https://github.com/webmproject/libwebp) | WebP | method 5+, sharp YUV, auto-filter under q70, SIMD build when available |

Guards: skip-if-larger · EXIF/GPS always stripped · 36 MP memory guard · 80 MB / 20-file batch limits.

ZIP packaging via [fflate](https://github.com/101arrowz/fflate). The single `index.html` loads
WASM codecs from jsDelivr (fallback: unpkg) — there is no backend and nothing is uploaded.

## Development

No build step. Serve the folder and open:

```bash
python3 -m http.server 8748
# http://localhost:8748/index.html
```

## Credits

Cat favicon & Product Hunt thumbnail: the `cat` icon from [HugeIcons](https://hugeicons.com/) (CC BY 4.0). Engines and libraries are credited in the footer and linked above.

## License

MIT for PicSqueeze's own code. The engines keep their original licenses (see links above).
