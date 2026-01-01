# Visualization Style Guide for howllmworks.com

This document describes the visualization style, libraries, and patterns used in this project. Use this as a reference when creating new visualizations.

---

## Project Overview

Educational platform for explaining machine learning concepts through interactive visualizations. Target audience: students and practitioners learning ML fundamentals.

---

## Libraries & Technologies

### 2D Visualizations
**Plotly.js v2.27.0** - Primary library for 2D plots
```html
<script src="https://cdn.plot.ly/plotly-2.27.0.min.js"></script>
```
- Scatter plots with draggable points
- Line charts and curve fitting
- Residual visualizations
- Configuration: `{ responsive: true, displayModeBar: false }`

### 3D Visualizations
**Custom Canvas 2D Renderer** - NOT Three.js or WebGL
- Parametric surface generation (mesh grids)
- Manual 3D-to-2D projection with perspective
- Painter's algorithm for depth sorting
- Rotation and zoom via mouse/touch events

Why custom renderer:
- Lightweight (no heavy 3D library dependencies)
- Full control over visual style
- Works on all devices without WebGL support
- Educational value (demonstrates projection math)

### Mathematical Formulas
**MathJax 3.x** - LaTeX rendering
```html
<script src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
```
- Inline and block formulas
- Live updating with double-buffering pattern
- Support for complex mathematical notation

### UI Framework
**Vanilla JavaScript + CSS** - No React/Vue/Angular
- Single-file HTML components
- CSS custom properties for theming
- Native DOM manipulation

---

## Visual Style Principles

### Overall Aesthetic
```
Clean, minimalistic, academic visualization style.
Looks like a textbook or ML lecture illustration, NOT a dashboard or chart.
High clarity, no visual clutter.
Soft studio lighting feel, neutral backgrounds.
```

### What to AVOID
- Text labels on 3D surfaces
- Axes labels, numbers, annotations on 3D
- Arrows, pointers, UI elements on visualizations
- Harsh or saturated colors
- Dashboard-like appearance
- Busy wireframes or grids

### What to INCLUDE
- Smooth continuous surfaces
- Thin, subtle wireframe grids (optional)
- Gradient coloring (viridis-style)
- Perspective view from above at slight angle
- Soft shadows and lighting
- Clear visual hierarchy

---

## Color Palette

### Viridis Colormap (for 3D surfaces)
5-point interpolation from low to high values:
```javascript
const viridis = [
    [68, 1, 84],      // Purple (low values / minimum)
    [59, 82, 139],    // Blue
    [33, 145, 140],   // Teal
    [94, 201, 98],    // Green
    [253, 231, 37]    // Yellow (high values / maximum)
];
```

### UI Colors (CSS Variables)
```css
:root {
    --bg-color: #fafafa;           /* Light background */
    --card-bg: #ffffff;            /* White cards */
    --text-color: #2c3e50;         /* Dark text */
    --accent-color: #2980b9;       /* Primary blue */
    --secondary-color: #7f8c8d;    /* Gray details */
    --success-color: #27ae60;      /* Green */
    --warning-color: #f39c12;      /* Orange */
    --danger-color: #e74c3c;       /* Red */
}
```

### Specific Element Colors
| Element | Color | Hex |
|---------|-------|-----|
| Current position marker | Red | `#e74c3c` |
| Optimal position marker | Blue | `#3498db` |
| Residual lines | Semi-transparent red | `rgba(231, 76, 60, 0.4)` |
| Data points | Blue | `#3498db` |
| Regression line | Red | `#e74c3c` |
| Grid lines | Light gray | `rgba(0, 0, 0, 0.05)` |

---

## 3D Surface Visualization Style

### Example Description for AI Generation
```
A clean 3D visualization of a convex loss surface (Sum of Squared Errors).
Smooth continuous surface shaped like a bowl, representing a convex error landscape.
Thin wireframe grid visible on the surface.
Subtle gradient coloring (viridis-style: dark purple at bottom → blue → green → yellow at top).
No text labels, no axes labels, no numbers, no annotations.
Soft studio lighting, neutral background.
Perspective view from above at a slight angle (approximately 30-40 degrees from horizontal).
High clarity, minimalistic, academic visualization style.
No arrows, no points, no UI elements, no axes ticks.
```

### Technical Parameters
- Surface resolution: 30x30 to 50x50 grid points
- Edge stroke: 0.5px, semi-transparent (alpha 0.3-0.5)
- Rotation: slight Y-axis rotation (-0.6 rad), X-axis tilt (-0.5 rad)
- Zoom: moderate (scale factor 2.0-2.5)
- Auto-rotation: very slow (0.0005 rad/frame) for ambient motion

---

## 2D Plot Style

### Plotly Configuration
```javascript
const layout = {
    paper_bgcolor: 'rgba(0,0,0,0)',
    plot_bgcolor: 'rgba(0,0,0,0)',
    font: { family: 'Inter, system-ui, sans-serif', size: 12 },
    margin: { l: 50, r: 20, t: 20, b: 50 },
    showlegend: false,
    xaxis: {
        title: 'x',
        gridcolor: 'rgba(0,0,0,0.05)',
        zerolinecolor: 'rgba(0,0,0,0.1)'
    },
    yaxis: {
        title: 'y',
        gridcolor: 'rgba(0,0,0,0.05)',
        zerolinecolor: 'rgba(0,0,0,0.1)'
    }
};

const config = {
    responsive: true,
    displayModeBar: false
};
```

### Data Point Style
```javascript
const pointsTrace = {
    x: data.x,
    y: data.y,
    mode: 'markers',
    type: 'scatter',
    marker: {
        color: '#3498db',
        size: 12,
        line: { color: 'white', width: 2 }
    }
};
```

### Regression Line Style
```javascript
const lineTrace = {
    x: [xMin, xMax],
    y: [yMin, yMax],
    mode: 'lines',
    type: 'scatter',
    line: {
        color: '#e74c3c',
        width: 3
    }
};
```

---

## Animation Principles

### Performance
- Target 60fps using `requestAnimationFrame()`
- Throttle heavy calculations
- Use double-buffering for MathJax updates

### Motion Style
- Smooth, eased transitions
- Slow ambient rotation for 3D (creates subtle "alive" feeling)
- Quick response to user interaction (< 16ms)

### Interactivity
- Mouse drag: rotate 3D view
- Mouse wheel: zoom in/out
- Drag data points on 2D plots
- Sliders for parameter adjustment

---

## Layout Patterns

### Sidebar + Canvas Layout
```css
.container {
    display: grid;
    grid-template-columns: 350px 1fr;
    gap: 20px;
}
```

### Card Style
```css
.card {
    background: white;
    border-radius: 12px;
    padding: 25px 30px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.06);
}
```

### Responsive Breakpoints
- Desktop: > 900px (sidebar + content)
- Tablet: 768px - 900px (stacked layout)
- Mobile: < 768px (full-width, simplified controls)

---

## Typography

### Font Stack
```css
font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
```

### Hierarchy
| Element | Size | Weight |
|---------|------|--------|
| H1 (Page title) | 28px | 700 |
| H2 (Section) | 20px | 600 |
| H3 (Card title) | 16px | 600 |
| Body text | 14px | 400 |
| Labels | 12px | 500 |
| Monospace values | 13px | 400 |

---

## Code Patterns

### State Management
```javascript
let state = {
    slope: 6.5,
    intercept: 3.5,
    sse: 0,
    // ... other parameters
};

function updateState() {
    state.sse = calculateSSE(...);
    updateDOM();
    update2D();
    update3D();
}
```

### 3D Projection
```javascript
function project(x, y, z, width, height, rotX, rotY, zoom) {
    // Rotate around Y axis
    let px = x * Math.cos(rotY) - z * Math.sin(rotY);
    let pz = x * Math.sin(rotY) + z * Math.cos(rotY);

    // Rotate around X axis
    let py = y * Math.cos(rotX) - pz * Math.sin(rotX);
    let pz2 = y * Math.sin(rotX) + pz * Math.cos(rotX);

    // Perspective projection
    const dist = 5;
    const scale = 200 * zoom;
    const f = scale / (pz2 + dist);

    return {
        x: width/2 + px * f,
        y: height/2 - py * f,
        z: pz2
    };
}
```

### Viridis Color Function
```javascript
function getViridisColor(t) {
    const colors = [
        [68, 1, 84], [59, 82, 139], [33, 145, 140],
        [94, 201, 98], [253, 231, 37]
    ];

    const idx = Math.min(Math.floor(t * 4), 3);
    const localT = (t * 4) - idx;
    const c0 = colors[idx], c1 = colors[idx + 1];

    const r = Math.round(c0[0] + (c1[0] - c0[0]) * localT);
    const g = Math.round(c0[1] + (c1[1] - c0[1]) * localT);
    const b = Math.round(c0[2] + (c1[2] - c0[2]) * localT);

    return `rgb(${r},${g},${b})`;
}
```

---

## Prompt Templates for AI Image Generation

### 3D Loss Surface
```
A clean 3D visualization of a [convex/non-convex] loss surface for [algorithm name].
Smooth continuous surface shaped like [a bowl/saddle/multiple valleys].
Thin wireframe grid visible on the surface.
Viridis gradient coloring (dark purple at minimum → yellow at maximum).
No text, no labels, no axes, no annotations.
Soft studio lighting, neutral gray background.
Perspective view from above at 35-degree angle.
Academic textbook illustration style.
High resolution, sharp details.
```

### 2D Function Plot
```
A clean 2D plot showing [function description].
Minimal axes with subtle grid lines.
[Color] curve on white background.
No heavy annotations or labels.
Academic, textbook-quality appearance.
Smooth anti-aliased lines.
```

### Gradient Descent Animation Frame
```
3D loss surface with a small [red] sphere indicating current position.
Trajectory path shown as thin dotted line.
Surface uses viridis colormap.
Clean, minimal style.
No UI elements or text.
```

---

## File Naming Convention

```
{module-number}-{topic}/
    {order}-{descriptive-name}.html

Examples:
001-linear-regression/010-linear-regression-sse-basics.html
001-linear-regression/020-polynomial-regression-sse.html
002-learning-rate/010-learning-rate.html
```

---

## Checklist for New Visualizations

- [ ] Uses correct color palette (viridis for 3D, accent colors for UI)
- [ ] No unnecessary labels or annotations on visualizations
- [ ] Responsive layout (works on mobile)
- [ ] Smooth 60fps animations
- [ ] Interactive elements have visual feedback
- [ ] MathJax formulas render correctly
- [ ] State management follows established pattern
- [ ] Follows file naming convention
