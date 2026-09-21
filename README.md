# OneWill — Landing Page

Front-end build of the OneWill landing page, coded from the Figma design
(SR007 · OneWill · page "🧱 Wireframe v3.3" and the brand pages).

**Live preview:** https://tonik.github.io/one-will-lp/

## Stack

A single `index.html` — plain HTML / CSS / JS, no build step, no dependencies.
Fonts come from Google Fonts (IBM Plex Mono 400/600, IBM Plex Sans). All artwork is
inline SVG pixel art; two `<canvas>` animations run at 12fps.

## Structure

```
index.html    everything: tokens in :root, the type ramp as .t-* classes,
              the inline <defs> sprite sheet (hamster, turnaround frames,
              cursor, icons, the machine's four layers), then the sections,
              then three scripts: the controller tabs + story choreography,
              the flying hamster, and the story's pixel snow
```

## Running locally

Needs a static server (nothing is fetched, but the canvas sizing reads layout
before paint, so serve it rather than opening the file):

```bash
python3 -m http.server 8765
```

Then open http://localhost:8765

## Sections

1. **Hero** — the hamster flies a Paper zone that covers the hero and the corridors
   beside the controller band. It chases the cursor, slides along the copy instead of
   overlapping it, wraps once fully off an edge; a click bursts the ball and engages
   turbo. Pixels drift past carrying boundary labels: risky ones are blocked at the
   ball, data gets eaten.
2. **The controller** — one card, three tabs, a 570×380 illustration per tab on an
   8-second loop: pick a ready image and boot it, share a machine by link, hand an
   agent a goal and take the controls back. The illustrations are HTML + CSS, not SVG.
3. **One-click story** — the machine opens centred with the heading and a Start button
   inside it, docks right a size smaller, then latches one pixel layer per scroll beat
   (frame → desktop → Chrome → terminal). On the last beat the contents ride out of the
   frame and the closing copy, benefits and CTA take their place at full size.
4. **Agent-World Boundary** — slides up over the machine the story leaves pinned, so
   that frame is covered rather than scrolled. Holds the hamster in its ball, three
   pixel chips and the boundary log.

Sections 5–8 (team, CTA/pricing, easter egg, footer) are **still the grey v4 wireframe**
and have not been designed.

## Conventions

- Square corners throughout; the one exception is the radio control in controller panel 1.
- Elevation is hard offset shadows with zero blur, never a soft shadow.
- Motion is slide-and-latch on `cubic-bezier(.22,.61,.36,1)` / 560ms. Nothing fades in
  and nothing draws on. Everything is off under `prefers-reduced-motion`, where the
  static hamster and plain stacked sections take over.
- Pixel art is never smoothed (`shape-rendering:crispEdges`, `image-rendering:pixelated`)
  and only scales by whole multiples.
