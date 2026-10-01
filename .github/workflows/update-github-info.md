---
name: update-github-info
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
engine:
  id: copilot
  model: haiku
tools:
  edit:
  web-fetch:
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    max: 1
    draft: false
---

# Update GitHub Info

Keep Mona's GitHub Info page current with concise, practical updates based on official GitHub sources.

1. Read `notes/mona-notes.md` and `site/content/github-info.md` before making changes.
2. Use web-fetch to read `https://github.blog/latest/`, `https://github.blog/changelog/`, and `https://awesome-copilot.github.com/workflows/`. Follow links to relevant official resources when useful.
3. Select only new items that help developers learn GitHub faster. Avoid duplicating topics already covered in `site/content/github-info.md`.
4. Update only `site/content/github-info.md`. Keep summaries short and practical, and cite each GitHub Blog, Changelog, or Awesome Copilot source with a direct link.
5. If the page has meaningful updates, create one pull request for Mona to review. Include a concise title and explain the added updates and their sources in the pull request body. Do not push changes directly to the default branch.
6. If there are no useful new items or no meaningful page changes, do not create a pull request.