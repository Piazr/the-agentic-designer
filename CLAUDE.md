# Developer Instructions: Book Website Maintenance & Standards

This instructions file outlines the commands, files, and core styling rules you must adhere to when modifying or extending this book website.

## 1. Project Management & Commands

### 1.1 Local Development
Since this is a static site, you can launch a local HTTP server using:
* Python 3: `python3 -m http.server 8000`
* Node (http-server): `npx -y http-server -p 8000`

### 1.2 Git & Deployment
Deploying to GitHub Pages (hosted under `piazr.github.io/<repository-name>/`):
* Make commits to `main`: `git add . && git commit -m "..." && git push origin main`
* GitHub Actions automatically builds and deploys the static files upon push.

### 1.3 Repository & Website Naming Conventions
Each book has its own distinct GitHub repository and matching GitHub Pages URL under the `Piazr` account.
* **Format**:
  * GitHub Book Repo: `https://github.com/Piazr/<book-repo-name>`
  * GitHub Pages Website: `https://piazr.github.io/<book-repo-name>/`
* **Template Setup**: When copying this template to launch a new book website (e.g. `claude-code-for-designers`), search and replace all references to `the-agentic-designer` and the owner `imehr` with the new repository name and target owner (`Piazr`). Every page's header navigation, footer link, and cover page link must resolve to the current book's repo/site.
* **CRITICAL LINK DIRECTION**: The "View on GitHub" button, footer "GitHub" links, and clickable book cover redirects must target the **Book Repository** where the downloadable book sources (HTML, PDF, EPUB files) are hosted, **NOT** the website repository (which contains the code hosting the preview web pages).

---

## 2. Hard Constraints & Style Conventions
Refer to [DESIGN.md](DESIGN.md) for full descriptions. Below are the critical instructions:

* **Strict Sharpness (No Rounded Corners)**: Never add rounded corners. Ensure all border radii are `0` (e.g. Tailwind `rounded-none`, `border-radius: 0`).
* **Solid Black Dividers**: Layout borders, section separators, and card division lines must be solid black (`#000` / `border-black`), without opacity/transparency.
* **Relative Links**: All stylesheet, script, image, and page links **must be relative** (e.g. `assets/logo.svg`, `../assets/logo.svg`, `chapters/01.html`, `preface.html`). Never use absolute URLs beginning with `/` (e.g. `/assets/...`), as this breaks subdirectory routing on GitHub Pages.
* **Chapter Pages Structure**:
  * Keep the text body right-aligned.
  * Sidebar rail navigation on the left should be fixed.
  * Ensure the top menu drawer and text animations exist and function smoothly.
* **Footer Borders**: The copyright container border at the bottom must be solid white with **no transparency**.

---

## 3. Directory Structure
* `/index.html` - Homepage
* `/about.html` - About Page
* `/chapters/` - Book Chapters (`01.html`, `02.html` etc. and front/back matter `preface.html`, `copyright.html`)
* `/assets/` - Global CSS/JS and gradients
  * `/assets/menu.js` - JS logic for drawer animations
  * `/assets/chapter-sidebar.js` - JS logic for sticky section highlight rail
  * `/assets/gradients/` - PNG gradient background assets
* `/images/` - Book figures and cover illustration
