---
name: "data-viz-recharts"
description: 'Guides data visualization design using Recharts. Use when building charts, dashboards, analytics views, or any data. Triggers: "use data-viz-recharts", "process data viz recharts", "data viz recharts.'
allowed-tools: Glob, Grep, Read
---


## Data Visualization with Recharts

### Color Palette
Never use Recharts default colors (`#8884d8`, `#82ca9d`, `#ffc658`). They look like every tutorial chart ever made. Define a custom palette as a const array and pass it to `<Cell fill={COLORS[index % COLORS.length]} />`.

Recommended starting palette (adjust to brand):
```js
const CHART_COLORS = [
  'oklch(55% 0.22 260)',  // brand primary
  'oklch(65% 0.18 30)',   // warm accent
  'oklch(60% 0.15 155)',  // success green
  'oklch(55% 0.20 350)',  // alert red
  'oklch(70% 0.10 260)',  // primary muted
];
```

### Chart Type Selection
- **Comparison over time**: `<AreaChart>` with `fillOpacity={0.15}` (not solid fill)
- **Part-to-whole (few categories)**: `<BarChart layout="vertical">` — never a pie chart with > 4 slices
- **Correlation**: `<ScatterChart>` with domain-appropriate dot size
- **Single KPI trend**: `<LineChart>` with a single series and a `<ReferenceLine>` for target

### Axes and Labels
Always show `<XAxis>` and `<YAxis>` with `tick={{ fontSize: 12, fill: 'var(--color-text-muted)' }}`. Use `tickFormatter` to abbreviate large numbers (1.2M, 34K). Never show a chart without axis context — raw pixel positions are meaningless to users.

### Tooltip Design
Override `<Tooltip>` with a `content={<CustomTooltip />}` component. The default Recharts tooltip has poor contrast and generic styling. The custom tooltip should match the app's design system: rounded corners, subtle shadow, brand font.

### Grid and Borders
Use `<CartesianGrid strokeDasharray="3 3" stroke="var(--color-border)" />` with low opacity. Remove the chart border (set `<ResponsiveContainer>` wrapper and no explicit border). Minimal grid lines reduce cognitive noise.

### Responsive Sizing
Always wrap in `<ResponsiveContainer width="100%" height={height}>`. Never hardcode pixel dimensions for the container. Choose height values: 200px (sparkline/compact), 300px (standard), 400px (detailed), 500px (focus chart).

### Accessibility
Add `aria-label` to the `<ResponsiveContainer>` describing the chart. Include a `<text>` summary for screen readers via an off-screen `<title>` element. Ensure color is never the only differentiator — use shape or pattern fills as a secondary signal.

### Animation
Disable Recharts default animation for dashboard charts that update frequently: `isAnimationActive={false}` on `<Line>`, `<Bar>`, `<Area>`. Use it only for initial load of report-style charts where the animation aids comprehension.

## When NOT to use

- Non-React stack — use `chart.js` / `d3` / native canvas instead
- Print-quality static charts — use matplotlib/Plotly
- Maps and geo data — Recharts has no geo support; use Mapbox/Leaflet
- High-frequency real-time streams (>10Hz) — use canvas-based libs (uPlot, lightweight-charts)
- Complex network/graph viz — use D3 or vis-network

## Red Flags

| Thought | Reality |
|---------|---------|
| "Default `#8884d8` is fine" | Reads as tutorial chart; never ships |
| "Pie chart with 8 slices" | Unreadable; switch to vertical bar chart |
| "Default Recharts tooltip" | Poor contrast, off-brand; override with custom |
| "Hardcode 600x300 px container" | Breaks responsive layouts; always wrap in `<ResponsiveContainer>` |

## Output Contract

Done when:
- Custom `CHART_COLORS` palette defined (OKLCH preferred), not Recharts defaults
- Chart type selected per data shape (Area/Bar/Scatter/Line)
- `<XAxis>` + `<YAxis>` with sized tick fonts and tick formatter
- `<CustomTooltip>` component matching design system
- `<ResponsiveContainer>` wraps every chart; height standardized
- `aria-label` describes the chart; pattern/shape secondary signal where needed
- Animation disabled on frequently-updating dashboards

## Examples

### Example 1 — KPI trend with target line
- Input: "Weekly revenue chart with $50K target"
- Action: `<LineChart>` single series, `<ReferenceLine y={50000} stroke="oklch(60% 0.18 30)" strokeDasharray="4 4"/>`, custom tooltip, `tickFormatter` to abbreviate to "$50K", `aria-label` set
- Output: Component with responsive sizing, target line visible, branded tooltip

### Example 2 — Status breakdown
- Input: "Show distribution of issue statuses (5 categories)"
- Action: `<BarChart layout="vertical">` with sorted categories, `<Cell>` per bar using `CHART_COLORS` cycling, axis labels formatted, animation off for dashboard
- Output: Horizontal-bar component, on-brand palette, no pie chart
