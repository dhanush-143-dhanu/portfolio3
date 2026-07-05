# Cinematic Hero — Video Intro

A fullscreen, cinematic Next.js hero section built around a talking-head
video: sharp foreground video, blurred ambient duplicate, a Three.js
floating-particle atmosphere, GSAP entrance choreography, and
glassmorphism playback controls.

## Files

```
components/
  VideoIntro.jsx          Main hero component (video, overlays, content, controls)
  VideoIntro.module.css   All styling — dark cinematic theme, glassmorphism, responsive
  CinematicLayer.jsx      Three.js particle/bokeh layer (self-contained, disposes on unmount)
public/
  videos/
    hero-video.mp4        Your talking-head video asset
app-page-example.jsx      Example of wiring VideoIntro into an App Router page
```

## Install

This assumes an existing Next.js App Router project (`npx create-next-app@latest`).

```bash
npm install three gsap
```

## Usage

1. Copy `components/` into your project's `components/` folder.
2. Copy `public/videos/hero-video.mp4` into your project's `public/videos/`.
3. Drop `VideoIntro` into your page (see `app-page-example.jsx`):

```jsx
'use client';
import VideoIntro from '@/components/VideoIntro';

export default function Page() {
  return (
    <VideoIntro
      tagline="computer science and engeneering"
      firstName="dhanush"
      lastName="vishwanathan rk"
      subtitle="Your subtitle copy here."
      nextSectionId="work"
    />
  );
}
```

`VideoIntro` needs a client boundary (it uses hooks, refs, and the DOM),
so either mark the page `'use client'` or keep it as a client component
imported into a server page — both work.

## Customizing

- **Video source** — edit `VIDEO_SRC` at the top of `VideoIntro.jsx` if
  your file isn't at `public/videos/hero-video.mp4`.
- **Copy** — pass `tagline`, `firstName`, `lastName`, `subtitle` as props.
- **Colors** — all tokens are documented at the top of
  `VideoIntro.module.css` (warm orange accent, near-black background,
  faint cool-blue undertone). Change the hex values there; the particle
  colors in `CinematicLayer.jsx` (`uWarm` / `uPale` uniforms) are
  separate and tuned to match.
- **Particle density** — `PARTICLE_COUNT` in `CinematicLayer.jsx`
  (160 by default; lower for weaker devices).
- **Fonts** — the CSS assumes you'll wire up `next/font` for a display
  serif (suggested: Fraunces) and a body sans (suggested: Inter). System
  font fallbacks are in place so it still looks reasonable without them.

## Notes on behavior

- Both videos autoplay muted or inline per browser autoplay policy —
  unmuting requires the explicit tap on the sound button, which is why
  the "Tap for sound" badge exists and auto-hides after ~4.5s.
- Play/pause controls both the foreground and background video in sync.
- The particle layer respects `prefers-reduced-motion`: parallax and
  fast animation are dialed back automatically.
- Three.js resources (geometry, material, texture, renderer) are
  disposed on unmount to avoid leaks if this hero is ever
  mounted/unmounted repeatedly (e.g. route transitions).
- The scroll indicator hides on small screens (<640px) to reclaim
  vertical space for the stacked name typography.
