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

### API access

The generate call posts to `https://api.anthropic.com/v1/messages`.

- In hosted environments that inject auth, it works as-is.
- For local development, copy `.env.example` to `.env` and set
  `VITE_ANTHROPIC_API_KEY`. The app then authorizes the browser call directly.

## Source

The whole studio is a single component: `src/ContentDesk.jsx`.
