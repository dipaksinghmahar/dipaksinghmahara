Dipak Singh Mahar — Portfolio
An immersive 3D portfolio website built as a single HTML file. Features a holographic display pedestal with a rotating 3D model, neon-glowing edges, a reflective polished concrete floor, and scroll-triggered content sections — all running entirely in the browser with no build tools or server required.

Preview
Open index.html directly in any modern browser. No server needed — just double-click the file.

A local server (like VS Code's "Live Server" extension) is recommended for the best experience, as some browsers restrict ES module imports from file:// URLs.

Tech Stack
Technology	Purpose
HTML5	Page structure and semantic markup
CSS3	Custom properties, glass-morphism, keyframe animations, scroll-reveal transitions
Tailwind CSS (CDN)	Utility-first classes for layout, spacing, and responsive design
Three.js (v0.160.0, ES Modules)	3D scene — scene graph, geometries, materials, lighting
OrbitControls	User camera rotation, zoom, and auto-orbit
EffectComposer + UnrealBloomPass	Post-processing glow/bloom around neon elements
Reflector	Real-time mirror reflection on the floor surface
GLSL Shaders	Custom hologram effect — scanlines, fresnel edges, flicker, color shift
Google Fonts	Syne (display headings) + Space Grotesk (body text)
Lucide Icons	SVG icons for navigation (menu, close)
File Structure
Everything lives in a single index.html file. Here's the logical breakdown:


# Project Architecture

## Structure Overview

```text
index.html
│
├── Head Section
│   ├── Meta Tags
│   │   ├── charset
│   │   └── viewport
│   ├── CDN Links
│   │   ├── Tailwind CSS
│   │   ├── Google Fonts
│   │   └── Lucide Icons
│   ├── Import Map
│   │   └── Three.js Module Resolution
│   └── Style Block
│       ├── CSS Custom Properties
│       ├── Base Styles & Resets
│       ├── UI Components
│       │   ├── Glass Cards
│       │   ├── Neon Text
│       │   └── Skill Tags
│       ├── Animation Keyframes
│       ├── Scroll-Reveal Effects
│       ├── Responsive Breakpoints
│       └── Accessibility Settings
│
├── Body Section
│   ├── #canvas-container
│   │   └── Three.js WebGL Scene
│   │
│   ├── .overlay
│   │   ├── Navigation Bar
│   │   │   ├── Logo
│   │   │   ├── Navigation Links
│   │   │   └── Mobile Menu Button
│   │   │
│   │   ├── Hero Section
│   │   │   ├── Name & Title
│   │   │   ├── Introduction
│   │   │   └── Model Selector
│   │   │
│   │   ├── About Section
│   │   │   ├── Biography
│   │   │   ├── Skills
│   │   │   └── Statistics
│   │   │
│   │   ├── Work Section
│   │   │   ├── Project Cards
│   │   │   ├── Technologies
│   │   │   └── Project Links
│   │   │
│   │   ├── Education Section
│   │   │   ├── Timeline
│   │   │   └── Achievements
│   │   │
│   │   ├── Contact Section
│   │   │   ├── Email
│   │   │   ├── LinkedIn
│   │   │   └── GitHub
│   │   │
│   │   └── Footer
│   │
│   ├── #mobile-menu
│   │   └── Full-Screen Mobile Navigation
│   │
│   └── #toast
│       └── Notification System
│
└── JavaScript Module
    ├── Three.js Scene Setup
    │   ├── Scene
    │   ├── Camera
    │   └── Renderer
    │
    ├── Post Processing
    │   └── Bloom Effects
    │
    ├── Controls
    │   └── OrbitControls
    │
    ├── Lighting System
    │   ├── Ambient Light
    │   ├── Point Lights
    │   └── Directional Light
    │
    ├── Environment
    │   ├── Reflective Floor
    │   ├── Hexagonal Pedestal
    │   └── Glass Frame
    │
    ├── Hologram Engine
    │   ├── GLSL Shaders
    │   └── 5 Swappable Models
    │
    ├── Particle System
    │   └── 250 Floating Particles
    │
    ├── Interaction System
    │   ├── Raycasting
    │   ├── Hover Detection
    │   └── Model Selection
    │
    ├── UI Features
    │   ├── Scroll Reveal
    │   ├── Mobile Menu
    │   └── Smooth Scrolling
    │
    ├── Event Handlers
    │   └── Window Resize
    │
    └── Animation Loop
        └── 60 FPS Rendering
```

## Technologies Used

* HTML5
* CSS3
* Tailwind CSS
* JavaScript (ES6 Modules)
* Three.js
* WebGL
* GLSL Shaders
* OrbitControls
* EffectComposer & Bloom Pass
* Lucide Icons

## Features

* Interactive 3D Hologram Portfolio
* Real-Time WebGL Rendering
* Bloom & Neon Effects
* Responsive Design
* Smooth Scrolling Navigation
* Scroll-Reveal Animations
* Mobile-Friendly Menu
* Particle Effects
* Raycasting Interaction
* Modern Glassmorphism UI

text


### Hover Brighten
Raycaster detects mouse over pedestal/glass. Smoothly interpolates:
- Glass edge opacity: `0.35 → 0.95`
- Hex edge opacity: `0.55 → 1.0`
- Top ring opacity: `0.6 → 1.0`
- Base ring opacity: `0.12 → 0.25`
- Bloom strength: `0.7 → 1.6`
- Cursor changes from `grab` to `pointer`.

### Click-to-Swap Models
1. Current model shrinks to 0 (~280ms, 7% lerp/frame).
2. Old meshes removed, new meshes created at scale 0.001.
3. New model grows to 1 (~400ms).
4. Dot indicator updates, toast shows model name.
5. `switching` lock prevents double-clicks during transition.

### Auto-Orbit Camera
- `OrbitControls` with `autoRotate: true` at speed 0.4.
- Drag to rotate, scroll to zoom (clamped 4.5–14 units).
- Polar angle clamped (40°–105°), panning disabled.

### Scroll-Triggered Reveals

| Observer | Section | Elements | Stagger | Animation |
|---|---|---|---|---|
| `sObs` | #about | `.skill-group` | 120ms | Fade up |
| `eObs` | #education | `.edu-item` | 150ms | Slide from left |
| `aObs` | #education | `.achieve-card` | 150ms | Fade up |
| `cObs` | #contact | `.contact-link` | 120ms | Slide from right |
| `countObs` | #about | `.stat-num` | — | Count 0 → target |
| `secObs` | all | Section roots | — | Fade up |

Stat counters increment from 0 to `data-count` in ~35 steps (30ms each ≈ 1 second), trigger once.

---

## Content Sections

### Hero
Full-viewport landing with staggered `fadeUp` animations (100ms–650ms delays). Pulsing "Click the pedestal" hint. 5 model selector dots.

### About (01)
Two-column: bio paragraphs (left) + 3 skill group cards with tag pills (right). Below: 3-column stats grid with animated counters.

### Projects (02)
Two-column project cards with cover image, title, year, tech stack tags, description, and link. Cards lift 6px on hover with neon border glow and image zoom.

### Education (03)
Left: vertical timeline with 3 entries (B.E., +2, SEE) — dots fill with accent on hover. Right: Cybersecurity Fundamentals achievement card.

### Contact (04)
Left: intro paragraph. Right: 3 contact links (Email, LinkedIn, GitHub) with hover-reveal arrows.

---

## Color System

All colors defined as CSS custom properties in `:root`:

```css
--bg: #070707;           /* Page background */
--fg: #e4e4e4;           /* Primary text */
--muted: #5a5a5a;        /* Secondary text */
--accent: #00ffc8;       /* Neon cyan accent */
--accent-dim: rgba(0,255,200,0.15);
--accent-glow: rgba(0,255,200,0.4);
--card: rgba(12,12,12,0.78);
--border: #1a1a1a;
3D scene matches: neon cyan (0x00ffc8) for all glowing elements, near-black (0x070707) for background and fog.

Typography
Font
Role
Weights
Syne	Display / headings (.font-display)	700, 800
Space Grotesk	Body text, UI labels, tags	300, 400, 500, 600, 700

Responsive Behavior
Desktop (≥768px): Two-column layouts, horizontal nav, full 3D scene.
Mobile (<768px): Single-column layouts, hamburger opens full-screen overlay, stat numbers scale to 1.8rem.
Touch: OrbitControls supports touch drag (rotate) and pinch (zoom) natively.
Accessibility
aria-label on interactive elements (dots, menu buttons).
role="tablist" and role="tab" on model selector dots.
tabindex="0" + Enter key support on dots.
@media (prefers-reduced-motion: reduce) disables all animations.
Semantic HTML: <nav>, <section>, <article>, <footer>.
rel="noopener" on external links.
Performance
Pixel ratio capped at 2x to save GPU on Retina displays.
Bloom threshold 0.82 — only bright elements get processed.
250 particles — atmospheric without GPU strain.
Reflection resolution matches screen — no waste.
depthWrite: false on transparent materials prevents z-fighting.
IntersectionObserver for scroll reveals — no scroll event listeners.
Browser Support
Browser
Status
Chrome 89+	Full support
Firefox 108+	Full support
Safari 16.4+	Full support (import maps from 16.4)
Edge 89+	Full support
Mobile Chrome/Safari	Full support (touch controls)
IE 11	Not supported

How to Customize
Change accent color
Update --accent, --accent-dim, --accent-glow in CSS :root.
Update all 0x00ffc8 values in JavaScript (lights, materials, particles, shader).
Add a new 3D model
Add to the models array:

javascript

{ name: 'Sphere', fn: () => new THREE.SphereGeometry(0.5, 32, 32) }
Add a corresponding .model-dot div in the hero HTML.

Add a new project
Copy an existing <article class="project-card ..."> in #work and update title, tags, description, image, and link.

Adjust camera
javascript

camera.position.set(x, y, z)     // Starting position
ctrl.target.set(x, y, z)         // Look-at point
ctrl.minPolarAngle / maxPolarAngle  // Vertical angle limits
ctrl.autoRotateSpeed              // Rotation speed
Credits
3D Engine: Three.js
CSS Framework: Tailwind CSS
Fonts: Syne + Space Grotesk
Icons: Lucide
Images: Picsum Photos (placeholders)
License
This project is for personal portfolio use. Study the code and adapt it for your own portfolio, but please don't copy the personal content (name, bio, projects, contact info).
```



