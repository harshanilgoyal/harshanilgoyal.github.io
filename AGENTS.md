# Project Identity

This is Harsh Goyal personal portfolio/resume website, hosted on GitHub Pages at harshanilgoyal.github.io. It is a heavily customized evolution of the Start Bootstrap Resume template, featuring a fixed dark gradient sidebar, 13 project cards with tech stack details, experience/education sections, and social links. The site is a single-page application with inline CSS/JS.

## Tech Stack & Infrastructure
- **Frontend:** HTML5 with custom CSS design system (CSS custom properties), Bootstrap 4.3.1 (base), Font Awesome 5.9.0, jQuery 3.4.1 + jQuery Easing, Google Fonts (Inter)
- **Backend:** None (static site)
- **Database:** None
- **Deployment:** GitHub Pages (harshanilgoyal.github.io) -- static files committed directly to repo; Travis CI runs build verification; GitHub: harshanilgoyal/harshanilgoyal.github.io (branch: master)

## VPS Connection
- **No VPS deployment** -- this is a static GitHub Pages site, served by GitHub directly
- **No VPS involvement** in hosting or deployment

## Database & Architecture Summary

No database. The site is a single index.html file (832 lines) with inline style and script blocks. SCSS source files in scss/ (7 partials) compiled via Gulp 4 to css/resume.css and css/resume.min.css. JS in js/resume.js compiled to js/resume.min.js. Multiple resume PDFs hosted alongside the page.

## Core Business Logic & Historical Context

- **Portfolio Audit (PORTFOLIO_AUDIT_REPORT.md):** Comprehensive audit found 12 of 13 project descriptions accurately backed by source code. Main issues: (F01) DhanLipi falsely claims PF/ESI/TDS compliance, (F03) Saaransh description is misleading, (F12) profile.jpg is 397KB for a 130px display. Final score: 8.0/10.
- **Build Pipeline:** Gulp 4 with tasks: clean, modules, css (SCSS -> CSS -> minify), js (uglify), watch (build + BrowserSync on port 3000), build (production), default (build).
- **NPM Scripts:** Only start is defined (gulp watch). CLAUDE.md references gulp dev and gulp -- these are invoked via the gulp CLI directly.
- **Travis CI:** Language Node.js, runs npm install -> npm test -> gulp. Caches node_modules. Email notifications disabled.
- **Design Evolution:** Originated from Start Bootstrap Resume v5.0.7 but heavily customized with custom CSS variables, Inter font, gradient sidebar (#0f0f23 -> #1a1a2e -> #16213e), and modern Open Graph + Twitter Card meta tags.
- **Unused Assets:** 15 unused gallery images (~1.9MB) in img/ directory, 7.5MB vendor/ directory completely unused (legacy from old site version).

## Strict Matrix Rules
- DO NOT leak logic from other projects.
- Always respect the existing architecture outlined above.
- This is a static GitHub Pages site -- no server-side code, no database, no build step on the GitHub side.
- Compiled css/*.min.css and js/*.min.js are committed to the repo -- GitHub Pages serves them directly.
- When updating project descriptions, ensure they accurately reflect the actual source code (per PORTFOLIO_AUDIT_REPORT.md findings).
- Deploy via: git add AGENTS.md && git commit -m "Add AGENTS.md" && git push origin master (GitHub Pages serves automatically)
