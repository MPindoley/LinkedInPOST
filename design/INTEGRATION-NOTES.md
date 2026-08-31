# The Desk · new design system (fonts, palettes, layouts)

**Status: merged into `index.html`.** The studio now ships 19 layouts, 18
palettes, and 8 typeface pairings, with a Typeface control next to the layout
and theme pickers.

`design/desk-design-lab.html` is a standalone preview that renders every
addition with the same canvas code the app uses. Open it in a browser to see
the whole set side by side. It stays as the design reference.

## What is being added

**Eight typeface pairings.** A pairing is a headline face, a body face, and a
label face, plus a size, leading, and tracking adjustment so each one sits
correctly at display size. The headline face is what makes two slides read as
different publications, so this is the biggest visual lever.

| Pairing | Headline | Body | Feel |
| --- | --- | --- | --- |
| Journal | Newsreader | Inter Tight | the current look |
| Broadsheet | Playfair Display | Libre Franklin | daily paper |
| Exchange | Source Serif 4 | Archivo | financial weekly |
| Runway | Bodoni Moda | Archivo | fashion/business cover |
| Modern | DM Serif Display | Space Grotesk | contemporary brand |
| Wire | Oswald (caps) | Libre Franklin | news poster |
| Quarterly | Lora | Inter Tight | warm, traditional |
| Bureau | Archivo 800 | Libre Franklin | data desk, sans headline |

**Eight palettes.** Newsprint, Crimson, Cobalt, Espresso, Porcelain, Steel,
Petrol, Ochre. Same object shape as the existing themes, so they drop straight
into `THEMES`.

**Eight layouts.** Dispatch, Bulletin, Figure, Ticker, Cover Story, Caption
Card, Column, Poster. Each ships with a signature pairing, listed in
`LAYOUT_FONTS` below.

**One wordless layout.** Silent draws the photograph full bleed with no scrim,
no type, no byline. With no photo available it falls back to Editorial so a
slide is never blank.

## Merging into the app

1. **Fonts.** Replace the `@import` in the app's `<style>` with the one at the
   top of the lab (13 families). Before the first slide render, `await
   document.fonts.ready` or the canvas silently falls back to Georgia.
2. **Constants.** Copy `FF` and `FONT_SETS` in next to the existing
   `SERIF` / `SANS` / `MONO` constants. Keep those three; they are still used
   by the app's own CSS-side styling.
3. **Themes.** Copy the eight `NEW_THEMES` entries into `THEMES`.
4. **Templates.** Add the nine ids to the template list:
   `dispatch, bulletin, stat, ticker, coverstory, captioncard, column, poster,
   silent`. Keep `silent` **out of the Surprise pool** so a random draw never
   produces a carousel with no words on it. It stays available by name.
5. **Typeface control.** New state alongside `templateId` / `themeId`:
   `auto` (use the layout's signature pairing), one of the eight ids, or
   `surprise` (random, avoiding the last one used, same as the other two
   controls). Resolve it in `generate()` and store it with the post.
6. **Wire it through.** `drawSlide(canvas, slide, i, total, opts, img)` gains
   `opts.fonts`; it lands on the shared `v` object as `v.f`. Every call site
   that renders a slide passes it.
7. **Existing layouts.** In the ten current layout functions, swap the
   hardcoded families: headline `SERIF` becomes `v.f.headW + " " + size + "px "
   + v.f.head`, body `SANS` becomes `v.f.body`, labels `MONO` become
   `v.f.label`. Headline start sizes go through `S(v, n)`, headline text goes
   through `HT(v, text)` so the caps-only pairing works, and the line height
   multiplier becomes `v.f.lead`.
8. **New code.** Copy `fitBlock`, `fitOneLine`, `splitStat`, `HT`, `S`,
   `headFont`, `NO_SCRIM`, and the nine `draw*` functions.
9. **Scrim.** `drawSlide` skips the photo scrim for the templates in
   `NO_SCRIM` (`silent`, `captioncard`), and `captioncard` takes its text
   colors from the theme rather than the photo-mode colors, because its words
   sit on a paper card.

### Signature pairing per layout

```js
const LAYOUT_FONTS = {
  editorial: "journal",   broadsheet: "broadsheet", index: "modern",
  ledger: "exchange",     feature: "runway",        quote: "quarterly",
  frame: "runway",        split: "bureau",          band: "wire",
  corner: "modern",       dispatch: "broadsheet",   bulletin: "bureau",
  stat: "exchange",       ticker: "wire",           coverstory: "runway",
  captioncard: "modern",  column: "quarterly",      poster: "wire",
  silent: "journal",
};
```

## Verified

Rendered in Chromium at full 1080 × 1350 across all eight new layouts, in
several palettes and pairings, with three text loads: normal, the longest copy
the writer can produce, and a closing slide with its disclaimer line. Checked
that headlines never overflow into the byline, that Figure falls back to a
framed statement when the sentence has no number in it, that Caption Card
clamps its text to the card, and that type stays legible over a photo.
