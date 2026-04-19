---
title: YAML-Frontmatter Quizzes for Engineers
published: true
description: How TeachRepo implements auto-graded quizzes using YAML frontmatter—zero database writes, instant feedback, no server round-trips.
tags: engineering, education, yaml, javascript
canonical_url: https://teachrepo.com/blog/yaml-frontmatter-quizzes
cover_image: https://teachrepo.com/assets/og-cover.png
---

> *Originally published at [teachrepo.com/blog/yaml-frontmatter-quizzes](https://teachrepo.com/blog/yaml-frontmatter-quizzes)*

Most course platforms treat quizzes as a separate content type — drag-and-drop UI, separate database table, their own API calls. TeachRepo takes a different approach: **quizzes are just YAML inside your lesson file**.

## The Schema

```yaml
---
title: "Git Rebase: The Complete Picture"
access: paid
quiz:
  - q: "What does `git rebase main` do when run from a feature branch?"
    options:
      - "Merges main into your feature branch, creating a merge commit"
      - "Replays your feature branch commits on top of main"
      - "Pushes your branch to origin/main"
      - "Creates a new branch from main"
    answer: "Replays your feature branch commits on top of main"
    explanation: |
      Rebase replays your commits on top of the target branch, producing
      a linear history. Unlike merge, it rewrites commit hashes.
---
```

## How Grading Works

Quiz grading is **client-side**:

1. Quiz data is embedded in HTML at build time
2. Student selects answers, clicks "Submit Quiz"
3. JavaScript compares selected vs correct answers (already in the DOM)
4. Score computed, feedback shown in <50ms
5. Non-blocking `quiz_submitted` event fires to analytics

No server round-trip. No database write on the critical path.

## Simplified Grading Logic

```javascript
const grade = (questions, answers) => {
  return questions.map((q, i) => ({
    ...q,
    selectedAnswer: answers[i] ?? null,
    correct: answers[i] === q.answer,
  }));
};

const score = graded.filter(q => q.correct).length;
const pct = Math.round((score / total) * 100);
```

## Security: Can Students Cheat?

Yes — a determined student can read correct answers from source. This is intentional:

- These are **learning quizzes, not certification exams**
- Hiding answers requires backend round-trips (latency + complexity)
- Students who cheat only hurt themselves

## Editing Quizzes

```bash
git checkout -b fix/quiz-typo
# Edit the .md file
git add lessons/03-advanced.md
git commit -m "fix: correct quiz option"
git push
# CI validates YAML + answer keys, then deploys
```

---

👉 Try a live quiz at [teachrepo.com/marketplace](https://teachrepo.com/marketplace)
