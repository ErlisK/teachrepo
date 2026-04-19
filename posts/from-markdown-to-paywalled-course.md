---
title: From Markdown to Paywalled Course in Minutes
published: true
description: A complete walkthrough—starting from a folder of .md files, ending with a live, Stripe-powered course site in under 10 minutes.
tags: tutorial, course, stripe, markdown
canonical_url: https://teachrepo.com/blog/from-markdown-to-paywalled-course
cover_image: https://teachrepo.com/assets/og-cover.png
---

> *Originally published at [teachrepo.com/blog/from-markdown-to-paywalled-course](https://teachrepo.com/blog/from-markdown-to-paywalled-course)*

## Prerequisites

- A folder of `.md` files (lessons)
- A [Stripe account](https://stripe.com) (free)
- A [Vercel account](https://vercel.com) (free tier is fine)
- A [TeachRepo account](https://teachrepo.com/auth/signup)

## Step 1: Structure Your Lesson Files

```
my-course/
├── course.yml
├── 01-intro.md
├── 02-core-concepts.md
├── 03-advanced.md
└── 04-wrap-up.md
```

Each lesson has minimal frontmatter:

```yaml
---
title: "Core Concepts"
access: paid
---
```

## Step 2: Write Your course.yml

```yaml
title: "Python Async for Web Developers"
slug: "python-async"
price_cents: 1900
currency: usd
version: "1.0.0"
lessons:
  - filename: 01-intro.md
    title: "Why Async?"
    access: free
  - filename: 02-core-concepts.md
    title: "Coroutines, Tasks, and the Event Loop"
    access: paid
```

## Step 3: Import to TeachRepo

```bash
curl -X POST https://teachrepo.com/api/import \
  -H "Authorization: Bearer <your-token>" \
  -H "Content-Type: application/json" \
  -d '{"repoUrl": "https://github.com/yourname/python-async-course"}'
```

## Step 4: Connect Stripe

Add your Stripe secret key in account settings. TeachRepo handles:
- Creating Stripe Checkout Sessions
- Listening for `checkout.session.completed` webhooks
- Granting entitlements immediately post-purchase

## Step 5: Deploy to Vercel

```bash
git clone https://github.com/ErlisK/teachrepo
cd teachrepo
cp .env.example .env.local
vercel --prod
```

**Total time: ~8 minutes.**

---

👉 Try it free at [teachrepo.com](https://teachrepo.com)
