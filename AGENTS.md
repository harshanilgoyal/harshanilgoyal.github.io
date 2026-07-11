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

## Day Zero Configuration

These are the non-negotiable operational rules for this project. They apply to every commit, every deploy, and every interaction with production.

### Local Environment
- **XAMPP is installed on the D drive:** `D:\xampp` (NOT C drive). All PHP projects run via `D:\xampp\htdocs\<project>` symlinks created by `link_to_xampp.bat`.
- **Local URL pattern:** `http://localhost/<project-name>/` (e.g., `http://localhost/Yukti/`)
- **DB auto-detection:** Windows uses `root` with empty password; Linux/VPS uses `root` with `Harsh@7086##`. No manual config switching needed.

### Pre-Deploy: Local Testing is Mandatory
- **NEVER deploy untested code.** Every change must be tested locally via XAMPP before pushing to GitHub.
- Test the specific feature you changed AND related features that could be affected.
- For PHP: run `D:\xampp\php\php.exe -l <file>` to lint syntax before committing.
- For Python: run the service locally and verify it starts without errors.
- If a feature cannot be tested locally (e.g., WhatsApp webhooks need a public URL), document why and test on VPS immediately after deploy.

### Production Data is Gold
- **Production data must NEVER be lost.** It is the single source of truth.
- `deploy.bat` automatically runs `mysqldump` backup before every deploy for projects with databases. Never skip this.
- Backups go to `/home/goyal/db_backups/<dbname>_pre_deploy.sql.gz` on the VPS.
- Never run `TRUNCATE`, `DROP TABLE`, or `DELETE` on production without a backup and explicit confirmation.
- Schema migrations (`db_update.php`) must be idempotent (`CREATE TABLE IF NOT EXISTS`, `ALTER TABLE ... ADD COLUMN` wrapped in try/catch).

### Post-Deploy: Testing is Mandatory
- **After every deploy, test the deployed feature on the live URL.** Do not assume the deploy succeeded silently.
- Verify the service is running: `systemctl is-active <service>` for Python services.
- Verify the web page loads: `curl -s -o /dev/null -w "%{http_code}" https://<domain>/` should return 200.
- If anything is broken, rollback immediately: `git reset --hard HEAD~1` or use `deploy/rollback.sh`.

### Smoke Testing Whenever Possible
- Run smoke tests after every deploy — even for small changes. A 1-line CSS change can break a PWA service worker.
- **Minimum smoke test checklist:**
  - Page loads without 500 errors
  - Login works (if auth exists)
  - API endpoint returns JSON (not HTML error)
  - Database connection works (if DB exists)
  - PWA service worker registers (if PWA)
- For Python services: `curl http://localhost:<port>/status` or equivalent health check.
- If smoke test fails, rollback first, debug second. Never leave production in a broken state.

### Sudo Git Pull for Permission-Locked Repos
- Some VPS repos are owned by `www-data` (not `goyal`) and require sudo to git-pull (e.g., goyal-solar, dispatch, solar).
- Use: `echo "Harsh@7086##" | sudo -S git -C /var/www/<project> fetch origin && echo "Harsh@7086##" | sudo -S git -C /var/www/<project> reset --hard origin/<branch>`
- After pull, fix ownership: `echo "Harsh@7086##" | sudo -S chown -R www-data:goyal /var/www/<project>`
- The `deploy.bat` script handles this automatically for most projects, but manual sudo may be needed for edge cases.
### Task Completion Discipline
- **NEVER move to a new task/todo until the current task is FULLY completed.** A task is not "done" until:
  1. Code changes are made and tested locally
  2. PHP lint passes (for PHP files)
  3. Changes are committed to git
  4. Changes are pushed to GitHub
  5. Changes are pulled on the VPS
  6. Smoke test passes on the live URL
  7. Any issues found during smoke test are fixed before moving on
- If a task has multiple sub-tasks, ALL sub-tasks must be completed before marking the parent task as done.
- If a task is blocked, document the blocker and mark it as "in_progress" — do NOT mark it as "completed" and move on.
- Partially completed work is the same as not started — production doesn't care about "almost done."
## Skills & Tools (Installed)

The following AI skills and tools are installed and available to all agents. Use them proactively wherever they add value.

### Graphify (Knowledge Graph)
- **What:** Turns code into a queryable knowledge graph (tree-sitter AST, local, no LLM needed for code).
- **When to use:** Before answering architecture questions, use `graphify query "<question>"` instead of grepping. Use `graphify path "A" "B"` to trace connections. Use `graphify explain "NodeName"` for a concept's connections.
- **Installed:** Global skill at `~/.config/opencode/skills/graphify/SKILL.md`. Run `graphify .` in any project to build its graph.
- **Yukti graph:** Already built — `graphify-out/graph.json` (2023 nodes, 3703 edges, 237 communities). Query it with `graphify query "how does OTP auth work?"`.

### ECC (Everything Claude Code)
- **What:** Agent harness performance system — 261 skills for TDD, verification loops, production audits, code quality, continuous learning, security scanning.
- **When to use:** Before deploying, run the `production-audit` skill. For new features, use `tdd-workflow`. For code review, use `plankton-code-quality`. For security, use `verification-loop`.
- **Installed:** 28 skills at `~/.opencode/skills/` (minimal profile). Key skills: `tdd-workflow`, `verification-loop`, `production-audit`, `plankton-code-quality`, `strategic-compact`, `iterative-retrieval`, `e2e-testing`, `error-handling`.

### UI/UX Pro Max (Design Intelligence)
- **What:** 67 UI styles, 161 color palettes, 57 font pairings, 161 industry-specific reasoning rules for professional UI generation.
- **When to use:** Before building any UI, the `@designer` agent should consult this skill for style recommendations, color palettes, and typography. Use `python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<product type>" --design-system` to generate a complete design system.
- **Installed:** Global skill at `~/.opencode/skills/ui-ux-pro-max/`.

### Awesome DESIGN.md (Design System Reference)
- **What:** Curated DESIGN.md files extracted from 73 popular brand design systems (Apple, Stripe, Linear, Shopify, Notion, Vercel, etc.).
- **When to use:** Each project has a `DESIGN.md` file in its root. The `@designer` agent should read it before generating UI to ensure visual consistency with the chosen design language.
- **Project assignments:**
  - Yukti (ERP) -> Linear (clean enterprise)
  - Yukti/client (Shop) -> Shopify (dark-first cinematic)
  - Solar CRM -> Stripe (purple gradients, elegant)
  - Saaransh -> Notion (warm minimalism)
  - Dispatch -> Linear (ultra-minimal, precise)
  - Server Panel -> Sentry (dark dashboard, data-dense)
  - Dhan Lipi -> Vercel (black/white precision)
  - Earthsol -> Apple (premium white space)
  - Sarvatra -> Supabase (dark emerald, code-first)
  - WhatsApp Bot -> Claude (warm terracotta, editorial)

### MCP Servers (Model Context Protocol)
- **sequential-thinking:** Step-by-step problem decomposition for complex architecture decisions. Use for multi-step reasoning.
- **fetch:** Web content fetching and markdown conversion. Use for retrieving API docs, checking live URLs, and researching dependencies.

### How Agents Should Use These Tools
- **@architect:** Use Graphify `query` and `path` to understand existing architecture before designing new features. Use `sequential-thinking` MCP for complex design decisions.
- **@developer:** Use Graphify `explain` to understand a function's connections before modifying it. Use ECC `tdd-workflow` for new features. Use `fetch` MCP to verify API docs.
- **@reviewer:** Use ECC `production-audit` and `plankton-code-quality` skills for comprehensive code review. Use Graphify to trace impact of changes.
- **@designer:** Always read the project's `DESIGN.md` first. Use UI/UX Pro Max `search.py` for design system generation. Follow the design tokens in DESIGN.md.
- **@devops:** Use ECC `verification-loop` after deploys. Use Graphify to verify no broken connections after refactoring.
- **@researcher:** Use `fetch` MCP to retrieve and verify live API documentation. Use Graphify `query` to find relevant code sections.
## Strict Matrix Rules
- DO NOT leak logic from other projects.
- Always respect the existing architecture outlined above.
- This is a static GitHub Pages site -- no server-side code, no database, no build step on the GitHub side.
- Compiled css/*.min.css and js/*.min.js are committed to the repo -- GitHub Pages serves them directly.
- When updating project descriptions, ensure they accurately reflect the actual source code (per PORTFOLIO_AUDIT_REPORT.md findings).
- Deploy via: git add AGENTS.md && git commit -m "Add AGENTS.md" && git push origin master (GitHub Pages serves automatically)
