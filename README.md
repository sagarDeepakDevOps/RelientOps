# RelientOps

> Freelance DevOps & Cloud Consulting — personal website of Deepak Sagar

[![CI](https://github.com/sagarDeepakDevOps/cloudforgeops/actions/workflows/ci.yml/badge.svg)](https://github.com/sagarDeepakDevOps/cloudforgeops/actions/workflows/ci.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](./LICENSE)

---

## Overview

Production-grade personal consulting website built with Next.js 15 App Router. Showcases DevOps & cloud engineering services, real case studies with architecture diagrams, and a direct booking flow via Calendly.

All brand configuration is driven by environment variables — no rebuild required to update contact details, social links, or copy.

---

## Tech Stack

| Category | Technology |
| --- | --- |
| Framework | Next.js 15 (App Router, React Server Components) |
| Language | TypeScript 5 — strict mode |
| Styling | Tailwind CSS v4, CSS custom properties |
| Animations | Framer Motion v12 |
| Diagrams | Mermaid.js (client-side, SSR-safe) |
| Theming | next-themes (dark default) |
| Content | Markdown with gray-matter + remark |
| Icons | Lucide React |
| Deployment | Vercel (recommended) |

---

## Pages

| Route | Description |
| --- | --- |
| `/` | Landing page — hero, services, case study preview |
| `/services` | Full service breakdown with problem/outcome format |
| `/about` | Personal intro, identity, engagement process |
| `/case-studies` | All case study listings |
| `/case-studies/:slug` | Individual case study with Mermaid architecture diagram |
| `/contact` | Contact form + social links |
| `/free-review` | Free 30-min review landing page + Calendly embed |

---

## Key Features

- **App Router with RSC** — server components by default, `"use client"` only where required
- **Environment-driven config** — single `src/lib/env.ts` reads all `NEXT_PUBLIC_*` vars with typed fallbacks; no hardcoded values anywhere
- **Architecture diagrams** — Mermaid.js renders infrastructure diagrams per case study, loaded client-side to avoid SSR conflicts
- **Dark mode by default** — next-themes with class strategy; user preference persisted in localStorage
- **SEO** — per-page metadata, JSON-LD structured data, sitemap, robots.txt, Open Graph
- **Scroll animations** — Framer Motion `whileInView` reveals, scroll progress bar
- **CI** — GitHub Actions type-checks and builds on every push to `main`

---

## Folder Structure

```
cloudforgeops/
├── .github/
│   └── workflows/
│       └── ci.yml                  # GitHub Actions — type-check + build
├── content/
│   └── case-studies/               # Markdown files for each case study
├── public/                         # Static assets
├── src/
│   ├── app/                        # Next.js App Router pages and layouts
│   │   ├── page.tsx                # Homepage
│   │   ├── layout.tsx              # Root layout — ThemeProvider, Header, Footer
│   │   ├── services/
│   │   ├── about/
│   │   ├── case-studies/
│   │   │   └── [slug]/             # Dynamic case study page
│   │   ├── contact/
│   │   ├── free-review/
│   │   ├── sitemap.ts
│   │   └── robots.ts
│   ├── components/
│   │   ├── layout/                 # Header, Footer, PageWrapper, PageTransition
│   │   ├── sections/               # Hero, Services, WhyUs, CtaBanner, etc.
│   │   ├── ui/                     # Button, Card, Badge, MermaidDiagram, etc.
│   │   ├── seo/                    # JsonLd structured data component
│   │   └── theme/                  # ThemeProvider, ThemeToggle
│   ├── config/
│   │   ├── site.ts                 # App-wide siteConfig object (reads from env)
│   │   └── diagrams.ts             # Mermaid diagram definitions per case study slug
│   ├── lib/
│   │   ├── env.ts                  # Typed env reader — single source of truth
│   │   ├── mdx.ts                  # Markdown parsing utilities
│   │   └── utils.ts                # clsx/tailwind-merge helper
│   └── types/                      # Shared TypeScript interfaces
├── .env.example                    # All required env vars with placeholder values
├── .gitignore
├── next.config.ts
├── tsconfig.json
└── package.json
```

---

## Environment Variables

Copy `.env.example` to `.env.local` and fill in your values. No secrets are committed — `.env.example` contains only placeholder values.

```bash
cp .env.example .env.local
```

| Variable | Description |
| --- | --- |
| `NEXT_PUBLIC_SITE_NAME` | Site / brand name |
| `NEXT_PUBLIC_SITE_URL` | Production URL (no trailing slash) |
| `NEXT_PUBLIC_SITE_TAGLINE` | Short one-line tagline |
| `NEXT_PUBLIC_SITE_DESCRIPTION` | Meta description |
| `NEXT_PUBLIC_SITE_OG_IMAGE` | Path to Open Graph image |
| `NEXT_PUBLIC_CONTACT_EMAIL` | Contact email address |
| `NEXT_PUBLIC_SOCIAL_GITHUB` | GitHub profile URL |
| `NEXT_PUBLIC_SOCIAL_LINKEDIN` | LinkedIn profile URL |
| `NEXT_PUBLIC_OWNER_NAME` | Full name |
| `NEXT_PUBLIC_OWNER_TITLE` | Professional title |
| `NEXT_PUBLIC_OWNER_EXPERIENCE` | Years of experience (e.g. `3+`) |
| `NEXT_PUBLIC_OWNER_BIO` | Short bio paragraph |
| `NEXT_PUBLIC_OWNER_LINKEDIN` | Personal LinkedIn URL |
| `NEXT_PUBLIC_FREE_REVIEW_HREF` | Internal path for booking page |
| `NEXT_PUBLIC_FREE_REVIEW_DURATION` | Session duration label |
| `NEXT_PUBLIC_CALENDLY_URL` | Full Calendly booking URL |

All variables are prefixed `NEXT_PUBLIC_` — they are safe to expose in the browser. **No API keys, tokens, or server-side secrets are used.**

---

## Local Development

**Requirements:** Node.js 20+ and npm 9+

```bash
# 1. Clone
git clone https://github.com/sagarDeepakDevOps/cloudforgeops.git
cd cloudforgeops

# 2. Install dependencies
npm install

# 3. Set up environment
cp .env.example .env.local
# Edit .env.local with your values

# 4. Start dev server
npm run dev
```

Dev server runs at [http://localhost:3000](http://localhost:3000).

---

## Build

```bash
# Type-check
npx tsc --noEmit

# Production build
npm run build

# Start production server locally
npm start
```

---

## Adding a Case Study

1. Create `content/case-studies/your-slug.md` with frontmatter:

```markdown
---
title: "Your Case Study Title"
excerpt: "One sentence summary."
date: "2026-01-01"
tags: ["AWS", "Kubernetes", "Terraform"]
---

Full markdown content here...
```

2. Optionally add a Mermaid diagram in `src/config/diagrams.ts`:

```typescript
export const caseDiagrams: Record<string, string> = {
  "your-slug": `
    flowchart TD
      A[Entry] --> B[Service]
  `,
};
```

The case study page automatically renders the diagram when an entry for the slug exists.

---

## Deployment (Vercel)

1. Push to GitHub.
2. Import the repository at [vercel.com/new](https://vercel.com/new).
3. Add all `NEXT_PUBLIC_*` variables in **Settings → Environment Variables**.
4. Deploy — Vercel detects Next.js automatically, no additional config needed.

For other platforms, run `npm run build` and serve with `npm start` or a process manager like PM2.

---

## CI

GitHub Actions runs on every push to `main` and on all pull requests:

- **Type-check** — `npx tsc --noEmit` fails on any TypeScript error
- **Build** — `npm run build` with placeholder env vars to catch config issues

See [`.github/workflows/ci.yml`](./.github/workflows/ci.yml).

---

## License

[MIT](./LICENSE) © 2026 Deepak Sagar

---

## Contact

**Deepak Sagar** — Freelance DevOps & Cloud Engineer

- Email: [sagardeepak2002@gmail.com](mailto:sagardeepak2002@gmail.com)
- LinkedIn: [linkedin.com/in/deepaksagar](https://linkedin.com/in/deepaksagar)
- GitHub: [github.com/sagarDeepakDevOps](https://github.com/sagarDeepakDevOps)
- Book a free 30-min review: [cloudforgeops.com/free-review](https://cloudforgeops.com/free-review)
