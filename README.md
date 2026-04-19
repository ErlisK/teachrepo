# TeachRepo

**Turn any GitHub repo into a paywalled course in minutes.**

[![Live Demo](https://img.shields.io/badge/demo-teachrepo.com-6d28d9?style=flat-square)](https://teachrepo.com)
[![Blog](https://img.shields.io/badge/blog-teachrepo.com/blog-6d28d9?style=flat-square)](https://teachrepo.com/blog)
[![License](https://img.shields.io/badge/license-MIT-green?style=flat-square)](./LICENSE)
[![Tests](https://img.shields.io/badge/tests-600%2B%20passing-brightgreen?style=flat-square)](https://github.com/ErlisK/openclaw-workspace)
[![Next.js](https://img.shields.io/badge/Next.js-15-black?style=flat-square)](https://nextjs.org)
[![Stripe](https://img.shields.io/badge/Stripe-payments-blueviolet?style=flat-square)](https://stripe.com)

---

TeachRepo is a Git-native course platform built for engineers. Write lessons in Markdown, push to GitHub, and get a fully-deployed course with Stripe checkout, auto-graded quizzes, and version history — in minutes.

```
teachrepo import --repo=github.com/you/your-course
# → Deploys to Vercel with Stripe checkout already wired up
```

## ✨ Features

| Feature | Description |
|---|---|
| **Markdown-native** | Write lessons alongside your code. No WYSIWYG editors. |
| **Stripe checkout** | Zero-config payment integration. Add your Stripe keys and ship. |
| **Auto-graded quizzes** | YAML frontmatter quiz config, graded in the browser. No backend needed. |
| **Gated sandboxes** | StackBlitz / CodeSandbox embeds unlock on purchase. |
| **Git versioning** | Every push updates your course with full commit history. |
| **Affiliate tracking** | Built-in `?ref=` link support with conversion tracking. |
| **Free tier** | Self-host on your own Vercel. Keep **100% of revenue**. |
| **Hosted tier** | $19/mo with marketplace listing, analytics, and zero ops. |

## 🚀 Quick Start

### 1. Write your course

```yaml
# course.yml
title: "Advanced Git for Engineers"
slug: "advanced-git"
price_cents: 2900
currency: usd
version: "1.0.0"
tags: [git, engineering, devops]

lessons:
  - filename: 01-rebasing.md
    title: "Interactive Rebasing"
    access: free
  - filename: 02-reflog.md
    title: "The Reflog: Your Safety Net"
    access: paid
  - filename: 03-hooks.md
    title: "Git Hooks and Automation"
    access: paid
```

### 2. Import your repo

```bash
# Via the TeachRepo CLI
teachrepo import --repo=github.com/yourname/your-course

# Or via the API
curl -X POST https://teachrepo.com/api/import \
  -H "Authorization: Bearer <your-token>" \
  -d '{"repoUrl": "https://github.com/yourname/your-course"}'
```

### 3. Deploy

Your course site is live at `https://teachrepo.com/courses/your-slug`.

Students can:
- Browse free lessons immediately
- Pay via Stripe to unlock gated content
- Track progress through quizzes
- Launch code sandboxes in-browser

## 📖 Course YAML Reference

```yaml
title: string          # Course title
slug: string           # URL slug (unique per creator)
price_cents: integer   # Price in cents (0 = free)
currency: string       # ISO currency code (usd, eur, gbp...)
version: string        # Semver version string
description: string    # Course description (Markdown supported)
repo_url: string       # Source repository URL
tags: string[]         # Marketplace tags

lessons:
  - filename: string   # Relative path to .md file
    title: string      # Lesson title
    access: free|paid  # Access tier
    quiz:              # Optional quiz config
      - q: "Question text"
        options: ["A", "B", "C", "D"]
        answer: "A"
```

## 🧪 Quizzes

Add a `quiz` block to any lesson's YAML frontmatter:

```markdown
---
title: "Git Branching"
access: paid
quiz:
  - q: "What does `git rebase -i HEAD~3` do?"
    options:
      - "Rebases 3 commits onto main"
      - "Opens interactive rebase for the last 3 commits"
      - "Creates 3 new branches"
      - "Squashes 3 commits automatically"
    answer: "Opens interactive rebase for the last 3 commits"
---

# Git Branching

Lesson content here...
```

Quizzes are graded client-side. No server round-trip. No database writes.

## 💳 Pricing

| Tier | Price | Revenue Share | Hosting |
|---|---|---|---|
| **Free / Self-hosted** | $0 | 0% | Your Vercel |
| **Hosted** | $19/mo | 5% | TeachRepo cloud |

The free tier is genuinely free. We don't charge a percentage on self-hosted deployments. Deploy to your own Vercel account and keep everything.

## 🛠 Tech Stack

- **Frontend:** Next.js 15 (App Router), TypeScript, Tailwind CSS
- **Database:** Supabase (PostgreSQL + Row Level Security)
- **Payments:** Stripe Checkout + Webhooks
- **Auth:** Supabase Auth (email/password, magic link)
- **Deployment:** Vercel (serverless functions + edge)
- **Testing:** Playwright (600+ E2E tests)
- **Content:** MDX, gray-matter for YAML frontmatter

## 📚 Live Courses

Try these free sample courses on the [marketplace](https://teachrepo.com/marketplace):

- **Git for Engineers** — Master the workflows used at top engineering teams
- **GitHub Actions for Engineers** — Automate CI/CD pipelines

## 📄 Blog

- [Introducing TeachRepo](https://teachrepo.com/blog/introducing-teachrepo) — What we built and why
- [Why Git-Native Is the Right Foundation for Developer Education](https://teachrepo.com/blog/why-git-native-courses) — The philosophy behind the platform

## 🤝 Contributing

Issues and PRs are welcome. Please open an issue before starting large changes.

## 📜 License

MIT — see [LICENSE](./LICENSE)

---

**Built by engineers, for engineers.**  
[teachrepo.com](https://teachrepo.com) · [hello@teachrepo.com](mailto:hello@teachrepo.com)
