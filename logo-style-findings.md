# Amiga ASCII logo style — findings

Style analysis of real scene logos, as groundwork for GLYPHLUX's logo generator.
Run: 2026-10-17 · Source: [asciiarena.se public API](https://asciiarena.se/api-docs) (`/api/v1/logos`).
Raw stats file: `logo_style_stats.json` (delivered in chat; contains statistics only, no artwork).

---

## Dataset

- Downloaded **101,600** tagged logos (1,016 of 1,031 pages; the last 15 pages return server errors on asciiarena's side).
- Kept only stroke-built logos (≥45% of characters are `/ \ _ | ¯`, ≤25% letters) and removed exact duplicates: **75,614 unique logos by 653 artists**.
- **41,727** carry a clean one-word label (the word the logo spells), usable for letter extraction later.
- Caveats:
  - asciiarena's auto-tagger cuts logos at **12 lines max**, so height stats are capped (48k logos are exactly 12 lines). Real logos run taller.
  - Frames, divider lines and borders around logos are included, which inflates `_` and `-` counts. Separating letterforms from frames is a later step.

---

## The rules, in plain terms

1. **Underscore is the backbone.** `_` is **54%** of all ink. Next: `/` 9%, `\` 7%, `-` 7%, `|` 5%, `.` 4%, `:` 2%. Then a long tail: `·` `=` `)` `l` `¯` `(` `¬` `!` `'` at about 0.3–0.6% each. Shapes are mostly horizontal underscore runs, closed with short diagonals.
2. **Diagonals are short.** About half of all `/` and `\` strokes **end after one row**. When a stroke continues to the next row:
   - `/` steps **one column left** (33%). That's the clean 45° Topaz diagonal.
   - `\` steps **one column right** (24%).
   - Steeper or shallower steps (2–3 columns) are rare (~5% each).
   - Long diagonals are the exception: slants are hooks and kicks, not long lines.
3. **Classic joins:**
   - `\` is followed by `_` **38%** of the time: the `\__` tail, the most iconic join.
   - `/` is followed by `_` **26%**: the `/__` foot.
   - `_` is followed by `/` 6% and by `\` 3%: underscore runs end in an upward `__/` cap far more often than `__\`.
   - Diagonally below-left of `/` sits another `/` 37% of the time (continuing slashes); below-right of `\` sits `\` 30%.
4. **Verticals stack.** `|` has another `|` below it **53%** of the time. Verticals are the one long-run stroke; `l` (4%), `:` and `'` sometimes end them.
5. **`¯` (overbar) is an accent, not a staple.** Only 0.5% of ink. It runs in pairs or more (`¯¯`, 62%) and is often followed by `\`. It sits under a `_` 28% of the time, closing a shape from below.
6. **Shading / texture characters:** `.` `:` `·` `-` `=` fill in and texture shapes. Around 15% of ink combined.
7. **Width:** typical logo blocks are **73 columns wide** (10th–90th percentile: 48–78). Scene logos are built to fill an 80-column line.

---

## Most common building blocks

The most frequent 3×3 character patches that contain a diagonal, with how many times each appears. These are effectively a learnable tile set:

```
[/  ]  [/  ]  [/__]  [   ]  [__/]  [  \]  [  \]  [   ]
[___]  [   ]  [   ]  [___]  [   ]  [   ]  [___]  [   ]
[   ]  [___]  [   ]  [  /]  [   ]  [___]  [   ]  [__/]
 105k   100k    94k    87k    87k    74k    73k    70k

[\__]  [___]  [   ]  [_  ]  [__\]  [___]  [   ]  [  /]
[   ]  [_  ]  [__/]  [/__]  [   ]  [   ]  [___]  [ / ]
[   ]  [/  ]  [   ]  [   ]  [   ]  [  /]  [\  ]  [/  ]
  70k    68k    67k    63k    61k    60k    55k    55k
```

---

## What this means for the generator

- A **rule-based outliner** can follow these numbers directly:
  - draw letter bodies as underscore runs;
  - close them with one-row `/` and `\` hooks (45° only);
  - use `\__` and `/__` at joins and `__/` at caps;
  - stack `|` for verticals;
  - add `¯` sparingly under closing edges;
  - sprinkle `. : ·` for texture.
- The 3×3 patches can drive a **tile-based generator** (wave-function-collapse style): fill a letter's shape only with patches real artists use, weighted by these counts. That keeps every local join authentic even when the letter is new.
- The **41k labelled logos** are the source for extracting real letters later (the letter library), with artist permission for anything public.
