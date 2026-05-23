---
name: book-website-designer
description: Replicate, maintain, or scaffold Piazr-style book websites with split layouts, organic cloud shaders, 3D card tilt effects, and custom drawer transitions.
---

# Book Website Designer Skill

Use this skill to create a new book website from scratch or add pages/chapters to an existing book website based on the Piazr editorial aesthetic.

---

## 1. Directory Blueprint

When initializing a new book repository, establish this structure:
```text
├── index.html                   # Book home page
├── about.html                   # About author and book context
├── DESIGN.md                    # Visual design specification
├── CLAUDE.md                    # Developer guidelines
├── assets/
│   ├── logo.svg                 # Header site logo
│   ├── menu.js                  # Pushing drawer logic
│   ├── chapter-sidebar.js       # Sticky sidebar intersection highlighted nav
│   ├── fonts/                   # Beausite fonts
│   └── gradients/               # Section color theme gradients
├── chapters/
│   ├── preface.html             # Front matter
│   ├── copyright.html           # Copyright page
│   ├── 01.html                  # Chapter 1
│   └── [N].html                 # Additional chapters
└── images/                      # Cover image, diagrams, and figures
```

---

## 2. Chapter Page Skeleton Template

Copy-paste this template when creating a new chapter. Ensure all links are **relative** (`../`).

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Chapter Title] - [Book Name]</title>
  <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>&#x2728;</text></svg>">
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Fragment+Mono:ital,wght@0,400;0,700;1,400&family=Geist+Mono:wght@300;400;500;600;700&family=Inter:wght@400;500;600;700;900&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/lucide@latest"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Beausite Classic Medium', 'Inter', 'system-ui', 'sans-serif'],
            mono: ['"Geist Mono"', '"Fragment Mono"', 'monospace'],
          },
          colors: {
            coral: '#fe7141',
            lavender: '#cdabfe',
            sage: '#597e87',
            'sage-light': '#d1ddd3',
            'blue-light': '#f5f9fc',
            'blue-dark': '#153250',
          }
        }
      }
    }
  </script>
  <style>
    @font-face { font-family: 'Beausite Classic Medium'; src: url('../assets/fonts/beausite-medium.woff2') format('woff2'); font-weight: 500; font-style: normal; }
    @font-face { font-family: 'Beausite Classic Regular'; src: url('../assets/fonts/beausite-regular.woff2') format('woff2'); font-weight: 400; font-style: normal; }
    @font-face { font-family: 'Beausite Classic Bold'; src: url('../assets/fonts/beausite-bold.woff2') format('woff2'); font-weight: 700; font-style: normal; }
    html { scroll-behavior: smooth; }
    body { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }

    /* Custom layout styling */
    .book-content {
      font-family: 'Beausite Classic Regular', Inter, sans-serif;
      font-size: 17px;
      line-height: 1.7;
      color: rgba(0,0,0,0.75);
      max-width: 680px;
      margin-left: auto;
    }
    .book-content h3 {
      font-size: 1.75rem;
      font-weight: 700;
      color: #000;
      margin-top: 64px;
      margin-bottom: 20px;
      line-height: 1.2;
      padding-top: 24px;
      padding-bottom: 16px;
      border-bottom: 1px solid #000;
    }
    .book-content h3:first-child { border-top: 1px solid #000; }
    .book-content p { margin-bottom: 18px; }
    .book-content pre {
      background: #f7f7f5;
      border: 1px solid rgba(0,0,0,0.06);
      padding: 20px 24px;
      overflow-x: auto;
      font-family: "Geist Mono", monospace;
      font-size: 13px;
      margin: 24px 0;
    }
    .book-content code {
      font-family: "Geist Mono", monospace;
      font-size: 13px;
      background: #f7f7f5;
      padding: 2px 6px;
    }
    .book-content pre code { background: none; padding: 0; }
    
    /* Layout components */
    .chapter-body {
      width: 100%;
      padding: 64px clamp(24px, 4vw, 80px) 96px;
    }
    .chapter-grid {
      display: grid;
      width: 100%;
      max-width: 1920px;
      margin: 0 auto;
      grid-template-columns: repeat(12, minmax(0, 1fr));
      column-gap: clamp(24px, 3vw, 48px);
    }
    .chapter-sidebar-rail {
      position: fixed;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      width: min(100%, 1920px);
      height: 100vh;
      overflow: hidden;
      pointer-events: none;
      z-index: 1;
      padding: 0 clamp(24px, 4vw, 80px);
      box-sizing: border-box;
    }
    .chapter-nav {
      position: absolute;
      top: 6.25rem;
      left: clamp(24px, 4vw, 80px);
      width: 160px;
      pointer-events: auto;
    }
    .sidebar-nav {
      background: #fff;
      padding: 1rem 0;
      max-height: calc(100vh - 7.25rem);
      overflow-y: auto;
    }
    .chapter-nav-spacer { grid-column: 1 / span 2; }
    .chapter-main {
      grid-column: 4 / -1;
      min-width: 0;
      position: relative;
      z-index: 2;
      background: #fff;
    }
  </style>
</head>
<body class="min-h-screen bg-white text-black">
  <!-- Header (includes sliding hover animation and sidebar drawer markup) -->
  ...
  
  <!-- Sidebar Rail -->
  <div class="chapter-sidebar-rail" id="chapter-sidebar-rail">
    <aside class="chapter-nav" id="chapter-sidebar-nav-wrap" aria-label="In this chapter">
      <nav class="sidebar-nav" id="chapter-sidebar-nav">
          <p class="font-mono text-[11px] uppercase tracking-wider text-black/25 mb-4">Front Matter</p>
          <a href="preface.html" class="sidebar-link flex items-center gap-3 font-mono text-[11px] text-black/35 hover:text-black py-2">Preface</a>
          <p class="font-mono text-[11px] uppercase tracking-wider text-black/25 mt-6 mb-4">In this chapter</p>
          <a href="#section1" data-section="section1" class="sidebar-link group flex items-center gap-3 font-mono text-[11px] text-black/35 hover:text-black transition-all duration-200 py-2">
            <span class="inline-block w-0 group-[.active]:w-9 h-px bg-black transition-all duration-200"></span>
            Section 1
          </a>
      </nav>
    </aside>
  </div>

  <div class="chapter-body">
    <div class="chapter-grid">
      <div class="chapter-nav-spacer" aria-hidden="true"></div>
      <main class="chapter-main">
        <div class="book-content">
          <!-- Main Content Sections -->
          <h3 id="section1">Section 1</h3>
          <p>Content goes here...</p>
        </div>
      </main>
    </div>
  </div>

  <!-- Footer with Solid White Border line -->
  ...

  <script src="../assets/menu.js"></script>
  <script src="../assets/chapter-sidebar.js"></script>
</body>
</html>
```

---

## 3. Mandatory Implementation Checklist

Before checking in your work, verify that:
1. **Rounded corners check**: Run a search for `rounded` in Tailwind classes and ensure they only specify `rounded-none`. In custom styles, confirm there are no `border-radius` values greater than `0`.
2. **Solid border check**: Ensure all separators use clear border classes like `border-black` or solid hex `#000` with no opacity/transparency.
3. **Relative links check**: Verify that assets inside the `/chapters/` folder are referenced with relative routes (`../assets/...`) and those at root `/index.html` are referenced with root-level relative paths (`assets/...`).
4. **Pushed-logo drawer effect check**: Verify that opening the navigation drawer pushes the logo container fully to the left with easing, rather than just overlaying it.
5. **No gaps check**: Ensure stat lists and Part grids contain a `gap-0` style with a solid border separation.
