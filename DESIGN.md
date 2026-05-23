# Design System & Guidelines: Piazr Book Websites

This document defines the core visual guidelines, structural layouts, and interactive behaviors established for the book websites (modeled on *The Agentic Designer*). It serves as a template and reference for creating future book websites.

---

## 1. Core Visual Principles

### 1.1 The "Strict Sharpness" Rule (No Rounded Corners)
* **Banned CSS Property**: `border-radius` (except `0` / Tailwind `rounded-none`).
* **Rule**: Absolutely no rounded corners are allowed on any element. This applies to buttons, grids, images, profiles, card containers, and input fields. Everything must use sharp, crisp, 90-degree corners.

### 1.2 Divider & Boundary Lines
* **Rule**: All section dividers, borders, grid lines, and column separators must be solid black (`#000` or Tailwind `border-black`) with **no transparency**.
* **Rationale**: Crisp, solid lines form the structural grid that defines the site's print-inspired editorial look.

### 1.3 Default Button States
* **Rule**: Interactive buttons must default to a **white background** with a solid black border. 
* **Hover State**: Color and text transformations are triggered on hover (see Section 3.1).

---

## 2. Page & Layout Structure

### 2.1 The Split Chapter Layout (Desktop)
Chapter pages must split the layout into a fixed navigation sidebar on the left and a right-aligned main text column:
* **Left Column (Sidebar Rail)**: A fixed or sticky navigation rail showing chapter headings and section indicators.
* **Right Column (Main Column)**: Contains the book content (HTML standard typography), strictly aligned to the right side of the screen on desktop.
* **Grid Pattern**: A 12-column grid layout where:
  * Sidebar rail covers columns `1` to `2` (fixed/absolute).
  * Main column covers columns `4` to `-1` (right-aligned).
  * Spacer is placed in column `3`.

```css
.chapter-grid {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  column-gap: clamp(24px, 3vw, 48px);
}
.chapter-nav-spacer {
  grid-column: 1 / span 2; /* reserves space for fixed rail */
}
.chapter-main {
  grid-column: 4 / -1;
  background: #fff;
}
```

### 2.2 Zero-Gap Grids
* **Rule**: Lists of stats, appendices, or chapters presented in card format must not have gaps between them. Use `gap-0` / `divide-x` / `divide-y` to create flush, grid-like boxes divided by solid lines.

---

## 3. Interactive Behaviors & Animations

### 3.1 Nav and Action Button Hover (Slide & Rotate)
Top navigation buttons and Next/Before chapter links must implement a dual-action hover effect:
1. **Background Sliding**: An absolute positioned background panel slides in horizontally to fill the button with the section's accent color (e.g. Lavender, Coral, or Yellow).
2. **Text Vertical Rotation**: The text slides vertically out of view, while a duplicate clone of the text slides into view from the bottom.

#### HTML Template:
```html
<a href="about.html" class="nav-hover-link">
  <span class="bg-slide">
    <span class="bg-slide-secondary bg-white"></span>
    <span class="bg-slide-primary bg-lavender"></span>
  </span>
  <span class="nav-text-wrapper">
    <span class="nav-text">About</span>
    <span class="nav-text-clone">About</span>
  </span>
</a>
```

#### CSS Template:
```css
.nav-hover-link {
  position: relative;
  overflow: hidden;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  padding: 0 24px;
  border-left: 1px solid rgba(0, 0, 0, 0.1);
  z-index: 10;
}
.bg-slide {
  position: absolute;
  inset: 0;
  display: flex;
  width: 200%;
  height: 100%;
  z-index: 0;
  transition: transform 0.4s cubic-bezier(0.44, 0, 0.56, 1);
}
.bg-slide-primary {
  width: 50%;
  background-color: #cdabfe; /* Dynamic color per section */
}
.bg-slide-secondary {
  width: 50%;
  background-color: #fff;
}
.nav-hover-link:hover .bg-slide {
  transform: translateX(-50%);
}
.nav-hover-link .nav-text-wrapper {
  position: relative;
  z-index: 1;
  overflow: hidden;
  height: 1.4em;
  line-height: 1.4;
  display: flex;
  flex-direction: column;
}
.nav-hover-link .nav-text,
.nav-hover-link .nav-text-clone {
  transition: transform 0.4s cubic-bezier(0.44, 0, 0.56, 1);
}
.nav-hover-link .nav-text-clone {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  text-align: center;
}
.nav-hover-link:hover .nav-text {
  transform: translateY(-100%);
}
.nav-hover-link:hover .nav-text-clone {
  transform: translateY(-100%);
}
```

### 3.2 Apple TV-style 3D Card Hover (Book Cover)
The book cover displays a 3D parallax tilt effect on hover:
* **HTML setup**: Use a `perspective-container` wrapper with `perspective: 1000px;`. The card element itself should have `transform-style: preserve-3d;`.
* **Reflection Glare**: A glare element inside the card with `mix-blend-mode: overlay` changes its opacity and gradient angle dynamically based on mouse coordinates.
* **Easing**: Rotate changes must update with easing/decay so the movement feels smooth and responsive.

### 3.3 Interactive Organic WebGL Cloud Shader
The hero banner features a WebGL canvas shader that renders moving organic cloud layers:
* **Ambient Movement**: Slowly morphs and drifts when the mouse is stationary.
* **Speed Reactivity**:
  * **Moving Faster**: Shifts the color spectrum towards yellow.
  * **Moving Slower**: Shifts the color spectrum towards purple.
* **Movement Easing**: Coordinate tracking must include custom delay/easing (lerp) to create an organic fluid ripple rather than a direct flashlight beam.

### 3.4 Logo-Pushing Navigation Menu Drawer
When the "Read the Book +" header button is clicked:
1. **Rotate Plus**: The `+` symbol rotates 360 degrees and transforms into an `X` with a transition.
2. **Expand Drawer**: A drawer panel (containing a list of chapters with a yellow-themed gradient background image) expands from the top right of the button down-left with custom easing.
3. **Push Logo**: The drawer transition pushes the left-hand site logo container off the screen as it reaches its width, giving a sliding fluid transition.

---

## 4. Color System

Each segment of the book site uses distinct gradient image files as backgrounds rather than CSS gradients to ensure consistent resolution and styling across pages:

* **Part I (Foundations)**: Coral/Peach scheme (`/assets/gradients/gradient_coral_peach.png`)
* **Part II (Tools & Workflows)**: Lavender/Violet scheme (`/assets/gradients/gradient_lavender_violet.png`)
* **Part III (Advanced Topics)**: Sage/Yellow scheme (`/assets/gradients/gradient_sage_yellow.png`)
* **Menu Drawer**: Yellow Gradient scheme (`/assets/gradients/gradient_menu_yellow.png`)

---

## 5. Repository & URL Architecture

Every book website corresponds to a distinct repository hosted on GitHub, with the built site served via GitHub Pages.

### 5.1 Link Consistency Rules
To maintain correct user navigation, ensure all hardcoded repository and live website URLs point specifically to the matching book's resources:
* **GitHub Repository Links**:
  * Top navigation "View on GitHub" button link: `https://github.com/Piazr/<book-repo-name>`
  * Book cover anchor link (initiates repository redirect): `https://github.com/Piazr/<book-repo-name>`
  * Footer "GitHub" text link: `https://github.com/Piazr/<book-repo-name>`
* **Cross-linking Rule**: If books reference each other, use explicit absolute URLs to avoid breaking paths within subdirectories. For example:
  * Reference to *The Agentic Designer*: `https://piazr.github.io/the-agentic-designer/`
  * Reference to *Claude Code for Designers*: `https://piazr.github.io/claude-code-for-designers/`

