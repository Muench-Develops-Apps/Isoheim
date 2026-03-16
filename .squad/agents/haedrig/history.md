# Haedrig — History

## Learnings
- Project: 2wowD, 2D isometric MMO by muench-develops
- GitHub account: muench-develops, authenticated via gh CLI
- npm workspaces monorepo at /Users/justinmunch/Entwicklung/2wowD
- Server deps: ws, better-sqlite3, @2wowd/shared
- Client deps: phaser, vite, @2wowd/shared
- Build: npm run build (shared → server+client parallel via concurrently)

### 2026-03-16: Git repo + GitHub created
- Initialized git, created muench-develops/2wowD on GitHub (public)
- Default branch: main
- Added README.md (screenshots, features, tech stack, getting started)
- Added LICENSE (MIT, 2026 muench-develops)
- Updated .gitignore with *.db, .env.*, .mega-linter-reports/, *.log

### 2026-03-16: Docker setup
- Dockerfile.server: multi-stage Node 20 alpine, builds shared+server, runs with --omit=dev
- Dockerfile.client: multi-stage Node 20 alpine → nginx:alpine, serves built client
- nginx.conf: SPA fallback + /ws proxy to server:8080 (WebSocket upgrade)
- docker-compose.yml: server (port 8080) + client (port 3000), shared network, volume for game.db
- .dockerignore: excludes node_modules, dist, .git, .squad
- Client WS connects to ws://<hostname>:8080 directly (uses window.location.hostname); port 8080 is exposed in compose so it works without nginx proxy in current architecture

### 2026-03-16: MegaLinter + CI
- .mega-linter.yml: ESLint (TS), Prettier (JSON), markdownlint, Hadolint, Trivy
- .eslintrc.json: @typescript-eslint/recommended, warn on unused vars + explicit any
- .markdownlint.json: disabled MD013 (line length), MD033 (inline HTML), MD041 (first-line heading)
- .github/workflows/mega-linter.yml: runs on push/PR to main, oxsecurity/megalinter@v8
