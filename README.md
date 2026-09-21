# OneWill — Landing Page

Front-end build of the OneWill landing page, coded from the Figma design
(SR007 · OneWill · the `✏️ Design` page and the brand pages).

**Live preview:** https://tonik.github.io/one-will-lp/

## Stack

`index.html` — plain HTML / CSS / JS, no build step, no dependencies — plus an
`assets/` folder beside it. Fonts come from Google Fonts (IBM Plex Mono 400/600,
IBM Plex Sans). All artwork is inline SVG pixel art except the two team photographs
and the four company logos; several `<canvas>` animations run at 12fps.

## Structure

```
index.html    everything: tokens in :root, the type ramp as .t-* classes,
              the inline <defs> sprite sheet (hamster, turnaround frames,
              cursor, icons, Discord, the machine's four layers), then the
              sections, then the scripts — controller tabs and story
              choreography, the flying hamster, the pixel snow, the Cream
              band edges, the boundary hamster, the team arrival and the
              pricing hamsters
assets/       the two team photographs and the four company logos
```

`index.html` needs `assets/` next to it — copy the folder, not just the file.

## Running locally

Needs a static server (the canvas sizing reads layout before paint, and the
assets are fetched, so serve it rather than opening the file):

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
   agent a goal and take the controls back. The tab that comes up next fills in pixels,
   a grid of cells with a scattered front; hovering a tab spreads Cocoa out from
   wherever the pointer entered. The illustrations are HTML + CSS, not SVG.
3. **One-click story** — the machine opens centred with the heading and a Start button
   inside it, docks right a size smaller, then latches one pixel layer per scroll beat
   (frame → desktop → Chrome → terminal). On the last beat the contents ride out of the
   frame, the closing heading wipes in above the machine and the benefits and CTA take
   their place inside it.
4. **Agent-World Boundary** — slides up over the machine the story leaves pinned, so
   that frame is covered rather than scrolled. Holds the hamster in its ball, three
   pixel chips and the boundary log.
5. **Team** — the two founders. Each photograph resolves out of its own pixels when it
   arrives, and the name, role and logos wipe in after it.
6. **Pricing** — two plan cards, each carrying hamsters that follow the cursor with
   their eyes and turn a few frames of the turnaround toward it. A click on a card sends
   them up in a full 360 from whichever way they are facing.

The easter egg and the footer are **still the grey v4 wireframe** and have not been
designed.

## Conventions

- Coloured bands do not run to the page edge: `min(1416px, 100% - 80px)` centred, the
  measure of the navbar's items, with Paper corridors either side.
- Square corners throughout; the one exception is the radio control in controller panel 1.
- Elevation is hard offset shadows with zero blur, never a soft shadow.
- Motion is slide-and-latch on `cubic-bezier(.22,.61,.36,1)` / 560ms, and reveals are a
  hard stepped wipe. Nothing fades in and nothing draws on. Everything is off under
  `prefers-reduced-motion`, where the static hamster and plain stacked sections take over.
- Pixel art is never smoothed (`shape-rendering:crispEdges`, `image-rendering:pixelated`)
  and only scales by whole multiples.
