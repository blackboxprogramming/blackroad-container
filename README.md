# BlackRoad Container

**Proprietary Cloudflare Workers + Containers platform by BlackRoad OS, Inc.**

---

## Architecture

| Component | Technology | Purpose |
|:--|:--|:--|
| Worker | Cloudflare Workers (TypeScript/Hono) | Request routing, load balancing |
| Container | Go 1.24 (Docker) | Application logic, HTTP server |
| Durable Objects | Cloudflare DO | Container lifecycle management |
| CI/CD | GitHub Actions | Typecheck, deploy, auto-label |
| Deploy | Cloudflare Workers | Production deployment via `wrangler` |

## Quick Start

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# TypeScript typecheck
npm run typecheck

# Deploy to Cloudflare
npm run deploy
```

Development server runs at [http://localhost:8787](http://localhost:8787).

## API Endpoints

| Method | Path | Description |
|:--|:--|:--|
| `GET` | `/` | List available endpoints |
| `GET` | `/container/:id` | Route to specific container by ID (2m timeout) |
| `GET` | `/lb` | Load balance across 3 container instances |
| `GET` | `/singleton` | Single container instance (singleton pattern) |
| `GET` | `/error` | Error handling demonstration |

## Project Structure

```
blackroad-container/
├── src/
│   └── index.ts              # Cloudflare Worker entry point (Hono)
├── container_src/
│   ├── main.go               # Go container HTTP server
│   └── go.mod                # Go module definition
├── .github/
│   ├── workflows/
│   │   ├── core-ci.yml       # CI: typecheck + lint
│   │   ├── deploy.yml        # Deploy to Cloudflare
│   │   ├── auto-label.yml    # Auto-label PRs
│   │   ├── automerge.yml     # Dependabot automerge
│   │   ├── failure-issue.yml # Create issue on CI failure
│   │   └── project-sync.yml  # Sync PRs to GitHub Project
│   └── dependabot.yml        # Dependency updates (npm, docker, actions)
├── Dockerfile                # Multi-stage Go container build
├── wrangler.jsonc            # Cloudflare Workers configuration
├── package.json              # Node.js dependencies and scripts
├── tsconfig.json             # TypeScript configuration
└── worker-configuration.d.ts # Generated Cloudflare type definitions
```

## Container Configuration

Defined in `wrangler.jsonc`:

- **Max instances:** 10
- **Sleep timeout:** 2 minutes of inactivity
- **Default port:** 8080
- **Observability:** Enabled
- **Source maps:** Uploaded for debugging

## CI/CD Pipeline

All GitHub Actions are **pinned to specific commit SHAs** for supply-chain security:

| Workflow | Trigger | Purpose |
|:--|:--|:--|
| `core-ci.yml` | Push/PR to main/master | TypeScript typecheck |
| `deploy.yml` | Push to main | Deploy to Cloudflare |
| `auto-label.yml` | PR opened | Auto-label (core/labs) |
| `automerge.yml` | PR from Dependabot | Auto-approve and squash-merge |
| `failure-issue.yml` | CI failure | Create tracking issue |
| `project-sync.yml` | PR opened/reopened | Add to GitHub Project board |

## Dependency Management

- **Dependabot** monitors: npm, Docker, GitHub Actions (weekly)
- **Automerge** enabled for Dependabot PRs (squash merge)

## Scripts

| Command | Action |
|:--|:--|
| `npm run dev` | Start local development server |
| `npm run deploy` | Deploy to Cloudflare production |
| `npm run typecheck` | Run TypeScript type checking |
| `npm run lint` | Run lint checks |
| `npm run cf-typegen` | Generate Cloudflare type definitions |

---

## License & Copyright

**Copyright (c) 2026 BlackRoad OS, Inc. All Rights Reserved.**

**CEO:** Alexa Amundson | **PROPRIETARY AND CONFIDENTIAL**

This software is proprietary. NOT open source. NOT for commercial resale.
Testing and evaluation purposes only. See [LICENSE](LICENSE) for complete terms.

**Infrastructure Scale:** 30,000 AI Agents | 30,000 Human Employees

**Contact:** blackroad.systems@gmail.com
