# GLYPHLUX — main doc

Living overview of the project. Every chat in this project should read this first, and update it whenever something gets decided, built or dropped.
Last updated: 2026-11-12

---

## 1. Concept

GLYPHLUX is an interactive pixel / ASCII canvas rooted in the old warez / keygen / demo scene: NFO files, crew logos, cracktros. Interactivity is the point, not an add-on: the field reacts to the mouse (repel, ripple, shock, scatter), to audio (beat-routed modulation), and to imported media, and everything is drawn through one pixel treatment (block, dither or ascii cells).

- Single self-contained HTML file. Vanilla JS + Canvas 2D + Web Audio. No build step. Deployable to GitHub Pages.
- Layout: left sidebar of collapsible sections, canvas centre, no top nav / modals / tabs. Monospace throughout, monochrome panel by default.

### What "cyber graffiti font" means (important)
The logo look GLYPHLUX is aiming for is **Amiga scene ASCII lettering**, the style on [asciiarena.se](https://asciiarena.se/). It is **not** FIGlet or generic bitmap text, and not PC block (░▒▓█) art.

- Built from 7-bit outlines: `/ \ _ | . : ^` plus `¯` (overbar) as a "top underscore".
- Drawn in the **Topaz** font (Amiga system font). Topaz's `/` and `\` meet corner to corner, which is why the diagonals join.
- Files are mostly `.txt` in **Latin-1 (ISO-8859-1)**, not CP437. `¯` and `ø` appear in real pieces; a CP437 decoder shows `¯` as `»`.
- Real logos are hand-drawn by artists. Goal: learn the style from hundreds of real examples so GLYPHLUX can generate its own.

---

## 2. Current build — C:\Tools\glyphlux\glyphlux.html

The current build is always `glyphlux.html` (see section 5, "Files & versions"). At setup (2026-11-12) it is build 6.1 (`glyphlux_6_1.html`, likely identical to `glyphlux_6.html`). The header mark inside the file still says "build 0.3".

Built and tested:
- **Render:** block / dither (Bayer 8x8) / ascii modes; char ramps incl. dense, shades, braille, blocks, boxdraw, cp437, custom. Pixel gap, contrast, density, invert. Canvas sizes incl. custom up to 4096.
- **Colour:** 13 palette presets, custom stops, load/save palettes (.hex .gpl .json .txt). Colour by energy / value / source (source keeps imported media colours). Separate monochrome panel colours.
- **Motion layers:** noise field, flow particles, water (wave sim + rain), glitch + scanlines.
- **Effects stack:** plasma, interference, lissajous, voronoi, mandelbrot/julia, tunnel, rotozoom, starfield, matrix, fire, fluid, 3D geometry, reaction-diffusion, cellular automata, copper bars, metaballs, moiré, spiral, warp field. Blend modes, reorder, duplicates.
- **Imports (each becomes a layer):** image, GIF, video, GLSL shader (ShaderToy + audio uniforms), 3D model (.obj .glb .gltf), audio, palette, scene.
- **Camera** (build 4): webcam as a layer; flip H/V, brightness, contrast, freeze; source colours preserved.
- **CP437 / NFO / ANS** (build 5): embedded Px437 IBM VGA 8x16 (VileR, CC BY-SA 4.0); .nfo .diz .asc (UTF-8 re-saves mapped back); .ans with colours, cursor codes, SAUCE width, iCE colours; scroll variant for tall files; px437 ascii face.
- **Text / logo:** sources = 5x7 bitmap (6 styles) or FIGlet (13 fonts, jsDelivr); pixel/ascii build; animations; blend; drag to place.
- **Text fx** (build 6): 20 cell-level effects (decrypt, scatter, assemble, laseretch, crumble, burn, vhstape, blackhole, fireworks, matrix, glitch, bubbles, wipe, slide, pour, waves, sweep, unstable, smoke, rings). Random no-repeat deal or pinned; triggers click / beat / timer; `t` hotkey; speed, settle (default 220ms), loop.
- **Mouse:** repel, gravity, ripple, smear, glitch zone, shockwave, crosshair; hold-to-charge clicks; drag stir.
- **Audio:** file / mic / video audio, 4 bands, 6 routing slots.
- **Export:** PNG, GIF (built-in encoder), WebM/MP4 video with audio, player .html, iframe snippet, wallpaper .html (for Lively), scene .json.
- **View:** zoom (ctrl+wheel, bar), pan with space+drag or middle-drag (fixed in build 6: no lag after zooming, Windows autoscroll blocked).

Known issues:
- Loading a scene that switches into ascii mode **and** contains file layers throws a few `drawImage` errors for about a second, then recovers. Pre-existing (build 4 too). Small fix, not done yet.
- `.txt` files are routed to the palette loader, so an asciiarena `.txt` won't load as art yet.
- If pan still seems dead: with canvas size **fit**, 100% zoom exactly fills the view, so there's nothing to pan until you go past it.

---

## 3. Roadmap

### Logo system — the main open thread
- **A. Amiga import.** Latin-1 decoder, embedded Topaz (from [rewtnull/amigafonts](https://github.com/rewtnull/amigafonts), GPL-FE, credit required), let `.txt` load as art when it isn't a palette. Then asciiarena collys can be dropped straight in.
- **B. Imported art as the logo source.** A third logo source next to bitmap/FIGlet: drop a logo `.txt`, its char grid becomes the logo, and all 20 text fx run on real scene logos.
- **C. Generator (type text → Amiga-style logo).** Plan, because general AI can't do 2D ASCII reliably:
  1. **Letter library:** cut real letters out of real logos / scene "ascii fonts", tag them, assemble new words from them with joining rules.
  2. **Style stats:** analyse hundreds of examples for habits (corner characters, slant steepness, `¯` usage, shading density, outline closing) and use those numbers to steer a rule-based outline generator.
- **asciiarena dig (2026-10-17) — findings:**
  - asciiarena has a free, read-only **public API** made for bots: [docs](https://asciiarena.se/api-docs) · [OpenAPI spec](https://asciiarena.se/api/v1/openapi). No key, 120 requests/min.
  - Archive size: **3,993 collys, 814 artists, 489 crews, 103,199 logos already cut out and tagged** (260 hand-tagged, ~103k auto-tagged). Each logo comes with its ASCII text slice, artist, crew, label and source colly. Endpoints: `/api/v1/logos`, `/api/v1/logos/{id}`, `/api/v1/collys/{id}/logos`, `/api/v1/collys/{id}/text`.
  - Sample of 400 auto-tagged logos: **~67% are real stroke-built logos** (the rest are greets/info text blocks). About a quarter have a clean one-word label (the word the logo spells). Extrapolated: roughly **69k usable logos, ~25k with clean labels**. A full pull is ~1,030 requests, about 9 minutes at the rate limit.
  - Character habits in real logos: `_` ≈ 50% of all non-space chars, then `-` 10%, `/` 8%, `\` 7%, `|` 5%, `.` 4%, `:` 4%, `¯` <1%. Underscore dominates; that's a key generator rule.
  - **Full alphabets exist:** a complete slash-style A–Z by **rAmoN! (House of Style)** in `HOS-HPL!.TXT` (with a "don't use without our permission" notice); a full blocky `|___|` font in `GIN_font.txt` ("It Is Just a Font", 2010); Volatile's "plain slant" and "italic" fonts in `VOL-3!.TXT`; and ~40 logos tagged as fonts (search `font` in the API).
  - **Conclusion:** the dataset problem is basically solved. Style stats can run on tens of thousands of real logos, and the letter library can start from the published alphabets. Any alphabet built into GLYPHLUX needs the artist's permission (rAmoN!'s says so explicitly).
- **Style analysis done (2026-10-17):** 75,614 unique logos by 653 artists analysed; full results in `claude/logo-style-findings.md`. Headline rules: `_` is 54% of ink; diagonals are short one-row 45° hooks; `\__` tail and `/__` foot are the signature joins; `|` stacks; `¯` is a rare accent. The 3×3 patch counts can drive a tile-based (wave-function-collapse style) generator.
- **Next step:** pick the generator approach (rule-based outliner, tile-based from the 3×3 patches, or both), and which alphabets to try as the first letter library (personal use first; permission before anything public). [textmodes/archive-amiga](https://github.com/textmodes/archive-amiga) remains a backup source.
- **Scene etiquette:** ripping art is a big taboo. Fine for personal use; if GLYPHLUX goes public with other artists' letters built in, get permission and credit, or stick to style stats only.

### Remaining from the original build order
- ② custom .ttf / .otf font upload
- ③ separate blend controls for embed vs overlay text modes
- ⑦ atlas perf fix: tinted glyph cache so source-colour / custom-colour ascii doesn't fall back to per-cell fillText

### Export save location (added 2026-11-12)
- Exports should open a Save window instead of downloading straight to B:\Downloads.
- First export: user picks `C:\Tools\glyphlux\exports` once. After that the Save window opens in the last folder chosen for a GLYPHLUX export (exports\ by default; if they save elsewhere once, it opens there next time). Never forces exports\.
- Should remember its folder separately from other Thorium downloads (use the browser's save-file picker with a fixed id), with a plain download as fallback if the picker isn't available.

### Ideas from Dither Boy (gaps worth considering)
Dither Boy is closed-source (£49), so these come from its public feature list, not its code.
- Error-diffusion dithering (Floyd-Steinberg, Atkinson, Stucki…)
- Halftone / CMYK halftone
- Palette extraction from an imported image
- Glow (bloom) and chromatic aberration as stack effects
- Temporal (animated) dither patterns
- `.txt` export of the ascii grid

---

## 4. Reference

- **Amiga ASCII:** [asciiarena.se](https://asciiarena.se/) · [textmodes/archive-amiga](https://github.com/textmodes/archive-amiga) · [GlyphDrawing.Club on Amiga ASCII](https://blog.glyphdrawing.club/amiga-ascii-art/)
- **Drawing apps with Amiga fonts:** [MoebiusXBIN](https://github.com/hlotvonen/moebiusXBIN) (best fit, 661 Amiga fonts) · [Moebius](https://blocktronics.github.io/moebius/) · [PabloDraw](https://picoe.ca/products/pablodraw/) · [text0wnz](https://xero.github.io/text0wnz/) (browser, has Topaz/P0T-NOoDLE/MicroKnight/mO'sOul)
- **Typed-text scene logos (PC/TheDraw style, not Amiga):** [ANSItype](https://ansitype.com/) · [MALM](https://malm.santo.fr)
- **Dither Boy:** [product page](https://studioaaa.com/product/dither-boy/) · [79-video tutorial playlist](https://www.youtube.com/playlist?list=PLvfny-DAmKHLwjpjyI0vuLGNyWZOvVY3J) · open alternatives [Dither Guy](https://github.com/manoelpiovesan/dither-guy), [DitherCraft](https://github.com/shahil-sk/dithercraft)
- **Omarchy TTFX:** source of the text fx names (38 effects; debug param `?etch=effectname`).

---

## 5. How we work

- **All GLYPHLUX planning and building happens in this project.** "Ideas & Projects" is only for dumping ideas for *new* projects.
- **Build chats:** one feature (or one tightly scoped pair) per chat. Paste or attach the latest HTML, state the one change, and say "do not touch anything else".
- **The builder must:** study the file first, extend it faithfully (no rebuilds), keep it one self-contained HTML file, test it, and report exactly what changed and where, including any existing lines it had to touch.
- **Files & versions (set up 2026-11-12):**
  - Everything lives in `C:\Tools\glyphlux`. It's a Git repo (to be pushed to GitHub).
  - The current build is always `glyphlux.html`. A new build **overwrites** it; never save `glyphlux_7.html`-style copies.
  - After each new build: the user commits and pushes manually. Claude never runs commit or push; it gives a copy-paste terminal block (`git add -A`, `git commit -m "build 7: font upload"`, `git push`). Older builds live in the Git history for rollback, not as loose files. Only committed versions are kept.
  - Exports (players, wallpapers, GIFs, scenes) go in `C:\Tools\glyphlux\exports\` by default, unless the user picks another place. `exports\` is kept out of Git.
  - `exports\glyphlux-player-daughters-longroad.html` is a saved settings snapshot the user revisits. Keep it as is; it's not a build.
- **Visual rules (hard):** no rounded corners, no shadows, no gradients, no glassmorphism, no generic sans fonts, no emoji icons. Monospace, harsh edges, raw elements. Confirm what's being matched before building anything visual.
- **Links:** every tool mentioned gets a verified, working link.
- **This doc:** update it at the end of any chat where something was decided, built, or dropped.
