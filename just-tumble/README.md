# Just Tumble CT — site build

Single-page build of the Just Tumble site, following `BUILD-JUST-TUMBLE.md` and `MOTION-SYSTEM.md`
(framer-motion-kit). Section architecture and motion follow the mugenstudio.framer.website reference.
Palette, type, imagery and copy are Just Tumble's own.

```
index.html          all markup, CSS and JS (GSAP 3.13 via cdnjs)
assets/hero.mp4     Just Tumble's own back-walkover clip, cropped, 1600w, every frame a keyframe (-g 1), 2.6 MB
assets/*.webp       photos from justtumblect.com, re-encoded
assets/fonts/       Archivo + Figtree (SIL OFL), self-hosted and preloaded
```

Nothing is loaded from `framerusercontent.com`, and none of the reference's copy is reused.

## Motion map

| Pattern | Where | Notes |
|---|---|---|
| 1 Scrubbed video | Hero | Pinned for 200% of the viewport, `scrub: 1`. Desktop/tablet only; phones get the poster. The pin doesn't wait on the video, so a slow or unsupported video falls back to the poster scaling and drifting. |
| 2 Masked line reveal | Founder letter, testimonials | SplitText `mask: 'lines'`, stagger 0.08 |
| 3 Pinned handoff | Programs | ≥1081px: six panels hand off inside a pinned stage, with a snap and a live 6-cell index. Below that, a stacked list. |
| 4 Velocity marquee | After hero | Three copies of the track; speeds up with scroll velocity |
| 5 Parallax | Location image (Enrollment) | `data-speed="auto"` via ScrollSmoother effects |
| 6 Counters | Why Just Tumble | Integer-snapped, comma-formatted |
| 7 Magnetic | Primary CTAs, app badges | Only on fine pointers |
| 8 Scroll-reactive nav | Global | Hides going down, solid after hero; a floating "Book a free trial" button keeps the CTA reachable |
| 9 Load orchestration | Hero | GSAP on desktop. On phones the same curve and stagger run in CSS, so LCP doesn't wait on the JS download. |

Reveals by section: 1 scrub → 2 mask → 3 pin → 4 counters → 5 none (accordion only) → 6 stagger + scrubbed rail →
7 parallax → 8 mask → 9 stagger. No two adjacent sections share a technique.

## Generator checklist

- [x] Smooth scroll: ScrollSmoother, `smooth: 1.2`, `normalizeScroll: true`
- [x] Hero video scrubs with `scrub: 1`
- [x] Dense keyframes (`-g 1`); phones get the poster and never download the video
- [x] One pinned handoff (Programs)
- [x] Marquee reacts to velocity
- [x] Staggers are 0.06–0.10
- [x] Easing only `power3.out` / `power2.in` / `power2.out` / `none`
- [x] `prefers-reduced-motion`: no smoother, pins or scrub; static page
- [x] No two adjacent sections use the same reveal
- [x] Lighthouse mobile: Performance 87, Accessibility 100, Best Practices 100, SEO 100 (local run, CLS 0, TBT ≤ 80 ms)
- [x] Visible keyboard focus (3px volt or blue ring)
- [x] Fully readable with JS off: all content visible, location links default to Shelton

## Conversion requirements

- "Book a free trial" is always reachable: in the nav, as a floating button on desktop, and in a fixed bottom bar on phones (with click-to-call)
- Location choice sits in the hero. It's remembered and rewrites the private-lesson links, map links, email, badge and labels. It stays in sync with the Enrollment toggle.
- Two `LocalBusiness` / `SportsActivityLocation` JSON-LD blocks, one per location (North Haven includes opening hours)
- Title and meta target "tumbling gym Shelton CT" and "tumbling classes North Haven CT"
- Fixes for current-site bugs: no placeholder carousel text; Tumbling → `/gymnastics`; Camps → `/camps`; Parties → `/birthday-parties`;
  every `tel:` filled in; no `amerigymstars.com` address

## Needs the client before launch

1. **Ratio number.** The brief asks for a ratio counter, but no figure is published. The ratio appears as a claim card, and "45+ coaches & staff" took the fourth counter slot. Send the real ratio (e.g. 6:1) and it becomes a counter.
2. **Founder letter.** It's written in Jason's voice from the About page facts. He needs to approve it. The photo is the coaching team, not a portrait of Jason; swap one in if he has one.
3. **Tuition.** Prices aren't published, so the Enrollment cards send families to the customer portal. Add numbers if Just Tumble wants them shown.
4. **Shelton class hours.** Only North Haven's hours are published (FAQ and schema).
5. **Front-desk portrait.** The brief calls for one; the logo mark stands in for now.
6. **Reviews** are verbatim excerpts from the three Google reviews on justtumblect.com (shortened with "…"). Star ratings aren't shown because none are published.
7. **Hosting.** `og:image` and the schema `image` assume the build replaces justtumblect.com. Update the URLs if it's hosted elsewhere.
