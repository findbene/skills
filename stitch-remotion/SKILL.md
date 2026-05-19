---
name: stitch-remotion
description: "Generates walkthrough videos from Stitch projects using Remotion. Triggers: 'use stitch-remotion', 'stitch remotion', 'stitch-remotion task'."
allowed-tools:
  - "stitch*:*"
  - "Bash"
  - "Read"
  - "Write"
---

# Stitch → Remotion Walkthrough Videos

**Constraint:** Only use this skill when the user explicitly mentions "Stitch" and walkthrough video, demo, or Remotion.

You are a video production specialist creating walkthrough videos from Stitch app designs. You retrieve Stitch screenshots and build a Remotion composition — slide transitions, zoom animations, and text overlays.

## Prerequisites

- Stitch MCP Server (or screen IDs already known)
- Node.js 18+ and npm
- Remotion CLI (`npm install -g remotion` or use via `npx`)

## Step 1: Gather Stitch assets

### Discover screens

1. Run `list_tools` → find Stitch MCP prefix
2. Call `[prefix]:list_projects` → select the project
3. Call `[prefix]:list_screens` with `projects/[projectId]` → list all screens
4. For each screen you want in the video, call `[prefix]:get_screen` with numeric IDs → verify: diff matches intended change

### Download screenshots

For each screen:

```bash
# Download screenshot to assets directory
curl -L "[screenshot.downloadUrl]" -o "video/public/assets/[screen-name].png"
```

Or use the fetch script:

```bash
bash scripts/fetch-stitch.sh "[htmlCode.downloadUrl]" "temp/[screen-name].html"
# Screenshots are separate — download via curl with the screenshot URL
```

### Build screens manifest

Create `screens.json` describing the video:

```json
{
  "projectName": "My App",
  "fps": 30,
  "screens": [
    {
      "id": "home",
      "title": "Home Screen",
      "description": "Main dashboard with key metrics",
      "imagePath": "./public/assets/home.png",
      "width": 390,
      "height": 844,
      "durationSeconds": 4
    },
    {
      "id": "profile",
      "title": "User Profile",
      "description": "Settings and account management",
      "imagePath": "./public/assets/profile.png",
      "width": 390,
      "height": 844,
      "durationSeconds": 3
    }
  ]
}
```

## Step 2: Set up Remotion project

```bash
# Create new Remotion project inside the working directory
cd video
npm create video@latest -- --blank
cd walkthrough-video
npm install @remotion/transitions
```

## Step 3: Build the composition

### ScreenSlide component

```tsx
// video/src/ScreenSlide.tsx
import { AbsoluteFill, Img, interpolate, spring, useCurrentFrame, useVideoConfig } from 'remotion'

interface ScreenSlideProps {
  /** Path to the screenshot image */
  imagePath: string
  /** Screen title displayed as overlay */
  title: string
  /** Supporting description text */
  description: string
  /** Whether to zoom in slightly during display */
  withZoom?: boolean
}

/**
 * Single screen slide with optional zoom effect and text overlay.
 * Fades in, holds, then fades out.
 */
export function ScreenSlide({ imagePath, title, description, withZoom = true }: ScreenSlideProps) {
  const frame = useCurrentFrame()
  const { fps, durationInFrames } = useVideoConfig()

  // Fade in over first 15 frames
  const fadeIn = spring({ fps, frame, config: { damping: 200 } })

  // Subtle zoom: 100% → 105% over the duration
  const scale = withZoom
    ? interpolate(frame, [0, durationInFrames], [1, 1.05], { extrapolateRight: 'clamp' })
    : 1

  return (
    <AbsoluteFill style={{ backgroundColor: '#000' }}>
      {/* Screenshot */}
      <AbsoluteFill style={{ opacity: fadeIn, transform: `scale(${scale})`, transformOrigin: 'center' }}>
        <Img src={imagePath} style={{ width: '100%', height: '100%', objectFit: 'contain' }} />
      </AbsoluteFill>

      {/* Bottom text overlay */}
      <AbsoluteFill
        style={{
          justifyContent: 'flex-end',
          padding: '40px 60px',
          background: 'linear-gradient(transparent, rgba(0,0,0,0.7))',
          opacity: fadeIn,
        }}
      >
        <h2 style={{ color: '#fff', fontSize: 36, fontWeight: 700, margin: 0 }}>{title}</h2>
        {description ? (
          <p style={{ color: 'rgba(255,255,255,0.8)', fontSize: 20, margin: '8px 0 0' }}>
            {description}
          </p>
        ) : null}
      </AbsoluteFill>
    </AbsoluteFill>
  )
}
```

### Walkthrough composition

```tsx
// video/src/WalkthroughComposition.tsx
import { Series, TransitionSeries } from '@remotion/transitions'
import { fade } from '@remotion/transitions/fade'
import { slide } from '@remotion/transitions/slide'
import screensData from '../../screens.json'
import { ScreenSlide } from './ScreenSlide'

const TRANSITION_FRAMES = 15  // 0.5s at 30fps

/**
 * Main walkthrough composition — one screen per slide, fade/slide transitions.
 */
export function WalkthroughComposition() {
  return (
    <TransitionSeries>
      {screensData.screens.map((screen, i) => (
        <>
          <TransitionSeries.Sequence
            key={screen.id}
            durationInFrames={screen.durationSeconds * screensData.fps}
          >
            <ScreenSlide
              imagePath={screen.imagePath}
              title={screen.title}
              description={screen.description}
            />
          </TransitionSeries.Sequence>
          {/* Add transition between screens (except after the last) */}
          {i < screensData.screens.length - 1 && (
            <TransitionSeries.Transition
              key={`t-${screen.id}`}
              timing={fade({ durationInFrames: TRANSITION_FRAMES })}
            />
          )}
        </>
      ))}
    </TransitionSeries>
  )
}
```

### Register in Root.tsx

```tsx
// video/src/Root.tsx
import { Composition } from 'remotion'
import { WalkthroughComposition } from './WalkthroughComposition'
import screensData from '../../screens.json'

const TOTAL_FRAMES = screensData.screens.reduce(
  (acc, s) => acc + s.durationSeconds * screensData.fps,
  0
)

export const RemotionRoot = () => (
  <>
    <Composition
      id="Walkthrough"
      component={WalkthroughComposition}
      durationInFrames={TOTAL_FRAMES}
      fps={screensData.fps}
      width={screensData.screens[0]?.width ?? 390}
      height={screensData.screens[0]?.height ?? 844}
    />
  </>
)
```

## Step 4: Preview and render

```bash
# Preview in browser
cd video/walkthrough-video
npm run dev

# Render to MP4
npx remotion render Walkthrough output.mp4

# Higher quality render
npx remotion render Walkthrough output.mp4 --jpeg-quality 95 --concurrency 4
```

## Common video styles

| Style | Configuration |
|-------|--------------|
| Quick demo (1–2 min) | 2–3s per screen, fade transitions, title only |
| Feature walkthrough | 4–5s per screen, zoom + slide, title + description |
| Presentation deck | 5–8s per screen, fade only, full description overlay |
| Social media clip | 1–2s per screen, fast cuts, music track |

## File structure

```
project/
├── screens.json             ← Screen manifest
├── scripts/fetch-stitch.sh  ← GCS downloader
└── video/
    ├── public/
    │   └── assets/          ← Downloaded screenshots
    └── walkthrough-video/
        ├── src/
        │   ├── WalkthroughComposition.tsx
        │   ├── ScreenSlide.tsx
        │   └── Root.tsx
        ├── remotion.config.ts
        └── package.json
```

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Blurry screenshots | Download full-resolution: use the screenshot URL directly, not a thumbnail |
| Layout mismatch | Set Remotion composition dimensions to match screen's `width` + `height` from `get_screen` |
| Transitions jarring | Increase `TRANSITION_FRAMES` or switch from `slide` to `fade` |
| Build fails on ESM | Add `"type": "module"` to `package.json` and check Remotion version compatibility |
| MP4 won't play | Check FFmpeg is installed: `ffmpeg -version` |

## When NOT to use

- Code generation from Stitch — use the matching `stitch-*-components` skill
- Standalone Remotion video without Stitch source — use `remotion` skill
- Live-action video or talking head — use `video-use`, `heygen-tools`, `synthesia-tools`
- Animated CSS/GSAP demo on a webpage — use `gsap` / `css-animations`
- 3D / motion graphics with After Effects style — use motion-design or Lottie skills

## Red Flags

| Rationalization | Reality |
|---|---|
| "Hard-code screenshots, skip the GCS downloader" | URLs are signed and time-limited — use `scripts/fetch-stitch.sh` for reliability |
| "Default slide transition is fine for everything" | Default `slide` can be jarring; tune `TRANSITION_FRAMES` per pace and pick `fade` for editorial |
| "Just bundle Remotion globally" | Use `npx remotion` or local devDep to avoid environment drift |
| "MP4 will play everywhere" | Verify FFmpeg installed + chosen codec (h264 default) renders for the target platform |

## Output Contract

Finished output must contain:
- Remotion composition file (`src/Root.tsx` or similar) with one `Sequence` per Stitch screen
- Local copies of screenshots fetched via `scripts/fetch-stitch.sh`
- Transition configuration (type, duration) tuned to content pace
- Audio track or silence file path (optional but explicitly handled)
- Rendered MP4 at target resolution (1080p default) with verified playback
- README with `npx remotion render` command and tips

## Examples

**Example 1 — 60s product walkthrough video**
- Input: "Make a 60s walkthrough of these 5 Stitch screens with light music"
- Action: Fetch 5 screenshots → Remotion composition with 5 `<Sequence>` blocks → fade transitions at 24 frames each → background music track → render 1080p
- Output: `out.mp4` (60s), Remotion project files, transitions = fade, music synced

**Example 2 — Multi-screen demo for a pitch deck**
- Input: "Convert this Stitch project into a 30s GIF/MP4 for our deck"
- Action: Fetch all screens → composition with slide transitions → 6s per screen → no audio → render MP4
- Output: `demo.mp4` (30s, no audio), 5 screens visible, smooth slide transitions


## References

See `references/details.md` for extended sections.
