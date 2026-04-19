---
title: "Gated Sandboxes: Teaching with Real Code"
published: true
description: How TeachRepo embeds StackBlitz and CodeSandbox environments that unlock on purchase—real, runnable code in the browser with zero student setup.
tags: engineering, education, sandboxes, stackblitz
canonical_url: https://teachrepo.com/blog/gated-sandboxes
cover_image: https://teachrepo.com/assets/og-cover.png
---

> *Originally published at [teachrepo.com/blog/gated-sandboxes](https://teachrepo.com/blog/gated-sandboxes)*

Technical courses live or die by their code examples. Static code blocks force students to copy-paste into their local environment. Screencasts are passive. The right answer: **live, runnable code embedded directly in the lesson**.

## Embedding a Sandbox

Add a `sandbox` block to any lesson's YAML frontmatter:

```yaml
---
title: "Async Python: Hands-On"
access: paid
sandbox:
  provider: stackblitz
  template: node
  repo: "yourname/course-repo"
  branch: "lesson-03-starter"
  file: "src/index.py"
  height: 600
---
```

That's it. TeachRepo renders a full StackBlitz or CodeSandbox iframe. Students click **Run**, a dev environment boots in ~2 seconds, they're writing and running code immediately.

## The Gating Mechanism

```tsx
export function SandboxEmbed({ config, hasAccess }: Props) {
  if (!hasAccess) {
    return (
      <div className="sandbox-locked">
        <LockIcon />
        <p>Purchase the course to access this sandbox</p>
        <CheckoutButton courseId={config.courseId} />
      </div>
    );
  }

  return (
    <iframe
      src={getSandboxUrl(config)}
      height={config.height ?? 500}
      allow="cross-origin-isolated"
    />
  );
}
```

`hasAccess` is computed server-side once per page load — no per-sandbox API calls.

## StackBlitz vs CodeSandbox

| Feature | StackBlitz | CodeSandbox |
|---|---|---|
| Startup time | ~2s (WebContainers) | ~5–15s (server-side) |
| Node.js | Full (in-browser) | Full (cloud container) |
| Python/Ruby | Limited (WASM) | Full (Docker) |
| Best for | JS/TS/Node lessons | Multi-language, complex apps |

## Best Practices

**One branch per lesson** — `lesson-01-starter`, `lesson-02-starter`. Easy to update without breaking other lessons.

**Include a solution branch** — `lesson-01-solution`. Students who get stuck can diff the two.

**Keep sandboxes focused** — One concept per sandbox, ~40 lines of starter code, 3 clearly-marked TODOs.

**Static fallback** — Provide a code block above the sandbox for slow connections.

## The Payoff

When a student can run, modify, and experiment with code without cloning a repo or installing dependencies, you get dramatically higher lesson completion rates. The setup cost is minimal: one YAML block, one GitHub branch. The payoff is a significantly better learning experience.

---

👉 Build your first sandbox course at [teachrepo.com](https://teachrepo.com)
