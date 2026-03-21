# Plan: Slider Image Comparison in VRT Report

## Context

The VRT report currently shows baseline (LIVE) and candidate (RC) screenshots in a side-by-side static grid with a diff image. The user wants to add a **before/after slider** view so reviewers can drag a handle across the two images to compare them interactively — a more intuitive comparison UX than the three-image grid.

**CDN question answered:** No external CDN is required for the JS. The slider JS can be inlined directly into the HTML template string that lives in the workflow YAML. The workflow embeds it into each generated `report.html`. Alternatively, a public CDN like jsDelivr can serve a ready-made component — no hosting cost, no upload step.

---

## Approach

### Option A (Recommended): Inline vanilla JS slider — no external dependency
Add ~40 lines of vanilla JS + CSS into the HTML template string in the workflow YAML. The slider uses a `<input type="range">` overlaid on two absolutely-positioned images. Works offline, no CDN dependency, fully self-contained report.

### Option B: Use `img-comparison-slider` from jsDelivr CDN
Add one `<script type="module">` tag pointing to the jsDelivr URL for [`img-comparison-slider`](https://github.com/sneas/img-comparison-slider). Cleaner markup, polished UX, but requires internet access when viewing the report.

**Recommendation: Option A** — keeps the report self-contained (consistent with how images are already base64-embedded).

---

## Implementation

### File to modify
`.github/workflows/reusable-visual-regression.yml` — the HTML template string (lines ~387–433)

### Changes

1. **Add toggle buttons** per comparison row: "Side by Side" | "Slider"

2. **Add slider markup** for each result (alongside the existing grid):
   ```html
   <div class="slider-wrap" style="display:none; position:relative; width:${w}px; max-width:100%">
     <img class="img-before" src="data:image/png;base64,${liveB64}" style="width:100%; display:block">
     <div class="img-after-clip" style="position:absolute;top:0;left:0;width:50%;overflow:hidden">
       <img src="data:image/png;base64,${rcB64}" style="width:${w}px; display:block">
     </div>
     <input type="range" class="slider-range" min="0" max="100" value="50"
       style="position:absolute;top:50%;left:0;width:100%;transform:translateY(-50%);cursor:ew-resize">
     <div class="slider-divider" style="position:absolute;top:0;left:50%;width:2px;height:100%;background:white;pointer-events:none"></div>
   </div>
   ```

3. **Add inline JS** at the bottom of the `<body>`:
   ```js
   document.querySelectorAll('.slider-range').forEach(range => {
     const wrap = range.closest('.slider-wrap');
     const clip = wrap.querySelector('.img-after-clip');
     const divider = wrap.querySelector('.slider-divider');
     range.addEventListener('input', e => {
       const pct = e.target.value + '%';
       clip.style.width = pct;
       divider.style.left = pct;
     });
   });
   document.querySelectorAll('.view-toggle').forEach(btn => {
     btn.addEventListener('click', e => {
       const row = btn.closest('.result-images');
       const mode = btn.dataset.mode;
       row.querySelector('.grid').style.display = mode === 'side' ? 'flex' : 'none';
       row.querySelector('.slider-wrap').style.display = mode === 'slider' ? 'block' : 'none';
       row.querySelectorAll('.view-toggle').forEach(b => b.classList.toggle('active', b === btn));
     });
   });
   ```

4. **Add CSS** to the existing `<style>` block:
   ```css
   .view-toggle { padding: 4px 10px; cursor:pointer; border:1px solid #ccc; background:#fff; }
   .view-toggle.active { background:#333; color:#fff; border-color:#333; }
   .slider-range { appearance:none; height:4px; background:transparent; }
   ```

---

## What changes per-run vs what's static

- The JS and CSS are **template strings in the YAML** — they get baked into every `report.html` at generation time. No CDN, no upload step, no separate hosting needed.
- Only the base64 image data changes per run.

---

## Verification

1. Trigger a VRT run (or run the node script locally with test images)
2. Open `report.html` in a browser
3. Confirm "Side by Side" and "Slider" toggle buttons appear per row
4. Dragging the slider handle should reveal/hide the RC image over the LIVE image
5. Confirm default view is still Side by Side (slider hidden by default)
6. Confirm report works offline (no network requests for JS/CSS)
