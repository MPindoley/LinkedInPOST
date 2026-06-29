# The Desk

A compliance-aware LinkedIn content studio for a wealth-management financial
advisor. Pick a content pillar, set a length and voice, and generate **two
matching formats of the same idea at once**:

1. A LinkedIn **text post** drafted against the spirit of SEC Marketing Rule
   206(4)-1 and FINRA Rule 2210 (educational only, no performance figures, no
   guarantees, no em dashes).
2. A minimalist **image carousel** (1080 × 1350 PNGs) rendered on canvas,
   ready to upload.

A built-in pre-submission checklist and per-draft reviewer flags keep the
output close to back-office ready.

## Design system

The carousel was rebuilt to look authored, not auto-generated:

- **Editorial type system** shared across every slide: Newsreader (serif
  display), Inter Tight (sans body), IBM Plex Mono (labels). This replaces the
  previous Fraunces / Inter / JetBrains mix.
- **A real typographic grid.** Left-aligned headline blocks, optically
  centered, with auto-fitting headline sizes so short and long slides stay
  balanced. The previous random geometric doodles (circles, dot clusters)
  are gone.
- **Three restrained palettes** (Ivory, Ink, Slate). One palette runs across
  the whole carousel so the set reads as one piece, with a calm cover/close
  variant framing slides 1 and 5.
- **A brand byline on every slide** (name + handle). This is what ties the
  carousel to its post: the same byline shows under the post proof, and the
  Post tab carries a clickable thumbnail strip of the carousel, so the post
  and its slides are always presented together.

## Run it

```bash
npm install
npm run dev
```

Then open the local URL it prints (usually http://localhost:5173).

### API access (OpenAI)

This app uses **OpenAI only**. The generate call posts directly to
`https://api.openai.com/v1/chat/completions` from your browser.

1. Paste your OpenAI API key into the **OpenAI API key** field at the top of
   the studio. It is stored only in your browser (`localStorage`) and is sent
   only to OpenAI.
2. Pick a model (`gpt-4o-mini` for fast/cheap, `gpt-4o` for higher quality).
3. Generate.

Optional: instead of pasting the key each time, copy `.env.example` to `.env`
and set `VITE_OPENAI_API_KEY` to pre-fill the field on load.

> Note: the key lives in the browser because this is a local, single-user tool.
> Do not deploy it publicly with a real key embedded; for a shared deployment,
> proxy the OpenAI call through a small backend instead.

## Source

The whole studio is a single component: `src/ContentDesk.jsx`.
