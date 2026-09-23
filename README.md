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
              sections, then the scripts — the preloader, controller tabs
              and story choreography, the flying hamster, the pixel snow,
              the Cream band edges, the boundary hamster, the team arrival,
              the pricing hamsters and the underfooter minigame
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

## Opening

The page opens on a Cream field with the hamster teleporting into the middle of
it — three rings running out of nothing with no hamster behind them, then the
ball landing on a white frame. The field then clears cell by cell on a ragged
front and the hero and navbar arrive through it. Nothing on the page moves
until that point: no heading lands, no button resolves and the hero's hamster
does not fly. It is the same hamster throughout — the hero's takes the spot the
preloader's was standing on, which is inside the headline, and the collision
code shoves it out of the copy on its own first tick.

It stands down on a reload part-way down the page, under `prefers-reduced-motion`,
and after four seconds whatever happens.

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
7. **Underfooter** — the wordmark at pixel scale, standing as a Cream ghost of itself with
   the hamsterball loose in it. The stage is fixed to the foot of the window and paints
   under the page, so the pricing band lifts off it rather than scrolling past it; at the
   end of the scroll the stage is handed back to the page so the footer arrives under the
   wordmark. Steer with the pointer or the arrow keys: the ball **lays the wordmark in**
   wherever it runs over it, a few chunks at a time. What it lays stands for two seconds
   and then goes again, cell by cell on the same ragged front the tabs fill with — nothing
   drops and nothing travels back, so the wordmark is always being redrawn rather than
   finished. Click or press space to burst the ball and pour out everything missing inside
   the blast, which reaches exactly as far as the ring it throws.
8. **Footer** — Ink, meeting the page on a straight edge. Five columns in the navbar's
   measure, a Cream hairline at a tenth, then the copyright, the social links and the
   credit. Cream headings, Paper links and Paper backer marks, as the frame draws them.

## On a phone

One `@media (max-width:600px)` block, plus two small scripts. What changes, and why:

- **One measure.** The bands run to the window here rather than sitting inset, so every
  text column in and out of them lands on the same width — 335 at a 375 window. Inset,
  the copy inside a band ran up to 80px narrower than the copy outside one.
- **The controller's illustrations are scaled, not cropped.** Each is laid out in a fixed
  420px box — its pointer positions are pixels in that box — so it spans the card and is
  zoomed down to it whole. `zoom` rather than a transform: zoom is laid out, so the stage's
  height comes down with the drawing.
- **The one-click story has four stills instead of one assembly.** The desktop latches the
  four layers into a single composition as you scroll; here each value prop carries its own
  still of the machine as it stands at that point — frame, then desktop, then Chrome, then
  the terminal.
- **The underfooter stage is `position:sticky`, not fixed.** Same behaviour — held at the
  foot of the window while the gap opens over it, handed back to the page when the gap's
  own bottom reaches the foot of the window — but done by the compositor. Driven from
  script it updated about 21 times a second against a page scrolling at 60, because iOS
  composites scrolling off the main thread and starves `requestAnimationFrame` during a
  fling. The gap carries twice its height and is pulled back up by one of them to give the
  sticky box room to travel; nothing moves in the flow.

## Conventions

- Coloured bands do not run to the page edge: `min(1416px, 100% - 80px)` centred, the
  measure of the navbar's items, with Paper corridors either side — above 600px wide;
  on a phone they run to the window (see **On a phone**).
- Square corners throughout; the one exception is the radio control in controller panel 1.
- Elevation is hard offset shadows with zero blur, never a soft shadow.
- Motion is slide-and-latch on `cubic-bezier(.22,.61,.36,1)` / 560ms, and reveals are a
  hard stepped wipe. Nothing fades in and nothing draws on. Everything is off under
  `prefers-reduced-motion`, where the static hamster and plain stacked sections take over.
- Pixel art is never smoothed (`shape-rendering:crispEdges`, `image-rendering:pixelated`)
  and only scales by whole multiples.
