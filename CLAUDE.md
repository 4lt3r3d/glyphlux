# GLYPHLUX

Interactive pixel / ASCII canvas rooted in the warez / keygen / demo scene (NFOs, crew logos, cracktros). Single self-contained HTML file: vanilla JS + Canvas 2D + Web Audio, no build step, deployable to GitHub Pages.

Full history, feature list, roadmap and reference links are in the main doc, imported below. Read it before any work.

@GLYPHLUX-main.md

---

## Where things stand (2026-11-12)

- Current build: **build 6.1** = `glyphlux.html`. The header mark inside the file still says "build 0.3" (cosmetic, leave it unless asked).
- Last thing built: build 6, the 20 text fx (decrypt, scatter, laseretch, burn …) plus the pan-after-zoom fix.
- Since then, research only, no code:
  - the asciiarena API dig;
  - the style analysis of 75,614 real logos (`logo-style-findings.md`, `logo_style_stats.json`).
- Next build queue (user picks one per session):
  1. **Logo A: Amiga import.** Latin-1 decoder, embedded Topaz font (rewtnull/amigafonts, GPL-FE, credit required). Let `.txt` load as art when it isn't a palette. *Planned next step.*
  2. **Logo B: imported art as a third logo source.** Next to bitmap / FIGlet, so all 20 text fx run on real logos.
  3. **Logo C: generator.** Approach still undecided: rule-based outliner, tile-based from the 3×3 patches, or both.
  4. **Export Save window.** Picker with a fixed id, remembers its last folder, defaults to `exports\`, falls back to a plain download.
  5. **Bug:** a scene that switches to ascii mode with file layers throws `drawImage` errors for about a second.
  6. Leftovers:
     - custom .ttf/.otf upload
     - separate blend controls for embed vs overlay text
     - tinted glyph cache for ascii performance

## Files

- `glyphlux.html` is the current build. A new build **overwrites it**. Never create `glyphlux_7.html`-style copies; old builds live in Git history.
- `GLYPHLUX-main.md` is the living project doc. Update it at the end of any session where something was decided, built or dropped.
- `logo-style-findings.md` and `logo_style_stats.json` hold the Amiga logo style stats. They contain statistics only, no artwork.
- `exports\` holds exports. It is git-ignored.
  - `exports\glyphlux-player-daughters-longroad.html` is a saved settings snapshot. Never modify or delete it.

## How to work

- **One feature (or one tightly scoped pair) per session.** Don't touch anything outside the requested change.
- **Study the existing code first.** Extend it faithfully; never rebuild or restructure. Keep it one self-contained HTML file.
- **Ask before changing code** unless the request clearly says to build. When discussing or planning, don't start building.
- **Test the change.** Then report plainly what changed and where, including any existing lines you had to touch.
- **After a build:** never run `git commit` or `git push` yourself. The user commits and pushes manually. When a build is done, give a ready-to-paste terminal block, e.g.:
  ```
  git add -A
  git commit -m "build 7: amiga import"
  git push
  ```
- **Explain plainly.** The user is comfortable with computers but isn't an advanced programmer. Be direct and concise.
- **Links:** every tool or resource mentioned gets a verified, working link. If a link can't be verified, say so.

## Visual rules (hard, check before presenting anything visual)

- No rounded corners, no shadows, no gradients, no glassmorphism or blur panels.
- No generic sans fonts (Inter, system-ui …) and no emoji as icons.
- Monospace throughout, harsh edges, raw elements, monochrome panel by default.
- Commit to a direction. Don't soften back toward generic defaults.
- Before building anything visual, confirm what is being matched (colours, type, motion timing).

## Domain notes

- "Cyber graffiti font" means **Amiga scene ASCII** (the asciiarena.se style). It is not FIGlet and not PC block art.
  - Built from `/ \ _ | . : ^ ¯` in the Topaz font.
  - Files are Latin-1, not CP437.
- Scene etiquette: ripping other artists' letters is taboo.
  - Personal use is fine.
  - Anything public with other artists' alphabets built in needs their permission and credit. Otherwise use style stats only.
