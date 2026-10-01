---
name: update-github-info
description: Draft website updates for Mona's GitHub Info site from official GitHub sources.
engine:
  id: copilot
  model: gpt-4.1
on:
  workflow_dispatch:
  schedule:
    - cron: '17 9 * * *'
safe-outputs:
  create-pull-request:
    title-prefix: "[mona] "
    draft: true
    fallback-as-issue: false
tools:
  edit:
  web-fetch:
network:
  allowed:
    - github.blog
    - github.com
    - awesome-copilot.github.com
---

# Update Mona's GitHub Info website

Read `notes/mona-notes.md` before making changes.

Use these sources:
- `notes/mona-notes.md`
- GitHub Blog: https://github.blog/latest/
- GitHub Changelog: https://github.blog/changelog/
- Awesome Copilot workflows: https://awesome-copilot.github.com/workflows/

Use web-fetch to read the GitHub Blog, GitHub Changelog, and Awesome Copilot workflows pages. Tell the agent to web fetch https://awesome-copilot.github.com/workflows/ and include that source in the research. Read external public guidance with web-fetch before deciding what to include.
Read repository guidance or reference files with GitHub repository API tools instead of terminal, CLI, or sandboxed commands.

Update `site/content/github-info.md` with concise, practical updates for readers and include source context when content comes from the GitHub Blog, GitHub Changelog, or Awesome Copilot.

Open a pull request for Mona to review. Include the source context (GitHub Blog, GitHub Changelog, or Awesome Copilot) in the pull request description. Use a pull request title that mentions Mona or GitHub Info. Do not write directly to `main`; rely on `safe-outputs` with `create-pull-request`.

Do not compile the workflow. Only create or update the markdown workflow file.