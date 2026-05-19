---
name: animejs
description: "Anime.js adapter patterns for HyperFrames. Triggers: 'use animejs', 'animejs', 'animejs task'."
allowed-tools: Bash, Glob, Grep, Read
---

# Anime.js for HyperFrames

HyperFrames can seek Anime.js instances through its `animejs` runtime adapter. The composition owns the animation objects; HyperFrames owns the clock.

## Contract

- Create animations or timelines synchronously during composition initialization.
- Set `autoplay: false` so Anime.js does not advance on its own clock.
- Register every returned animation or timeline on `window.__hfAnime`.
- Use finite durations and loop counts.
- Avoid callbacks that mutate DOM based on wall-clock time, network state, or unseeded randomness.

The adapter seeks every registered instance with `instance.seek(timeMs)`, where `timeMs` is HyperFrames time in milliseconds.

## Basic Pattern

```html
<script src="https://cdn.jsdelivr.net/npm/animejs@4.0.2/lib/anime.iife.min.js"></script>
<script>
  const anim = anime({
    targets: ".mark",
    translateX: 280,
    rotate: "1turn",
    opacity: [0, 1],
    duration: 1200,
    easing: "easeOutExpo",
    autoplay: false,
  });

  window.__hfAnime = window.__hfAnime || [];
  window.__hfAnime.push(anim);
</script>
```

## Timeline Pattern

```html
<script>
  const tl = anime.timeline({
    autoplay: false,
    easing: "easeOutCubic",
  });

  tl.add({
    targets: ".title",
    translateY: [40, 0],
    opacity: [0, 1],
    duration: 650,
  }).add(
    {
      targets: ".accent",
      scaleX: [0, 1],
      duration: 450,
    },
    250,
  );

  window.__hfAnime = window.__hfAnime || [];
  window.__hfAnime.push(tl);
</script>
```

## Module Builds

If you use an ES module build, the adapter does not care how the instance was created. It only needs the returned object to expose `seek()`, `pause()`, and preferably `play()`:

```html
<script type="module">
  import { animate } from "https://cdn.jsdelivr.net/npm/animejs/+esm";

  const anim = animate(".chip", {
    x: "18rem",
    duration: 900,
    autoplay: false,
  });

  window.__hfAnime = window.__hfAnime || [];
  window.__hfAnime.push(anim);
</script>
```

## Good Uses

- Small SVG and DOM flourishes where Anime.js syntax is compact.
- Imported Anime.js examples that can be made seek-driven.
- Multiple independent micro-animations pushed into the same registry.

Use GSAP for complex scene sequencing unless the user specifically asks for Anime.js. GSAP is still the primary HyperFrames authoring path.

## Avoid

- Leaving `autoplay` at the Anime.js default.
- Depending on `anime.running` auto-discovery instead of explicit `window.__hfAnime.push(...)`.
- Infinite loops. Compute a finite repeat count from the composition duration.
- Building animations in timers, promises, event handlers, or after async asset loads.

## Validation

After editing a composition that uses Anime.js:

```bash
npx hyperframes lint
npx hyperframes validate
```

## Credits And References

- HyperFrames adapter source: `packages/core/src/runtime/adapters/animejs.ts`.
- Anime.js documentation for `autoplay`, `pause()`, and `seek()`: https://animejs.com/documentation/

## When NOT to use

- Complex scene sequencing or long timelines — use GSAP (primary HyperFrames path)
- Generic web-only Anime.js work outside HyperFrames — just write Anime.js normally
- React/JSX animations — use Framer Motion or `gsap` adapter instead
- Animations driven by real-time data (network, time-of-day) — incompatible with seek-driven model
- CSS-only or pure SVG SMIL animations — no JS adapter needed

## Red Flags

| Thought | Reality |
|---------|---------|
| "Leave `autoplay: true`, HyperFrames will catch up" | It won't; the composition drifts and frame-exact renders break |
| "I'll set up the animation in a `setTimeout`/`async` block" | HyperFrames seeks before that fires; nothing animates |
| "Use `loop: true` for endless rotation" | Render budget needs a finite count; compute from composition duration |
| "Skip pushing onto `window.__hfAnime`" | Adapter only seeks instances it knows about; unregistered animations sit frozen |

## Output Contract

Done when:
- All Anime.js instances created synchronously at composition init
- Every instance has `autoplay: false`
- Each instance/timeline pushed onto `window.__hfAnime`
- Durations and loop counts are finite, computed from composition length
- No wall-clock, network, or unseeded-random dependencies inside callbacks
- `npx hyperframes lint` and `npx hyperframes validate` pass

## Examples

### Example 1 — Single mark animation flourish
- Input: User wants a `.mark` element to translate and rotate over 1.2s
- Action: Create `anime({ targets: ".mark", translateX: 280, rotate: "1turn", duration: 1200, autoplay: false })`, push to `window.__hfAnime`, run validate
- Output: Composition seeks deterministically; lint/validate green

### Example 2 — Multi-step intro timeline
- Input: Title slides up, then accent bar scales in 250ms later
- Action: Build `anime.timeline({ autoplay: false })` with `.add()` for title then `.add(..., 250)` for accent, push timeline to `window.__hfAnime`
- Output: Timeline seekable, finite duration, GSAP not introduced
