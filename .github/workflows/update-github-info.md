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
steps:
  - name: Pre-fetch official sources
    run: |
      mkdir -p /tmp/gh-aw/agent
      cat > /tmp/gh-aw/agent/clean.py <<'PY'
      import re, sys, html
      t = sys.stdin.read()
      t = re.sub(r'(?is)<(script|style|svg|noscript).*?</\1>', '', t)
      t = re.sub(r'(?is)<a\s[^>]*href="([^"]+)"[^>]*>(.*?)</a>', r' [\2](\1) ', t)
      t = re.sub(r'(?s)<[^>]+>', ' ', t)
      t = html.unescape(re.sub(r'\s+', ' ', t))
      print(t[:30000])
      PY
      curl -fsSL --max-time 30 https://github.blog/latest/ | python3 /tmp/gh-aw/agent/clean.py > /tmp/gh-aw/agent/github-blog.txt
      curl -fsSL --max-time 30 https://github.blog/changelog/ | python3 /tmp/gh-aw/agent/clean.py > /tmp/gh-aw/agent/github-changelog.txt
      curl -fsSL --max-time 30 https://awesome-copilot.github.com/workflows/ | python3 /tmp/gh-aw/agent/clean.py > /tmp/gh-aw/agent/awesome-copilot.txt
---

# Update Mona's GitHub Info website

Read `notes/mona-notes.md` and `site/content/github-info.md` before making changes.

The official sources have already been downloaded as text files. Read these files (do not use curl or any network command):
- `/tmp/gh-aw/agent/github-blog.txt` (GitHub Blog: https://github.blog/latest/)
- `/tmp/gh-aw/agent/github-changelog.txt` (GitHub Changelog: https://github.blog/changelog/)
- `/tmp/gh-aw/agent/awesome-copilot.txt` (Awesome Copilot workflows: https://awesome-copilot.github.com/workflows/)

Links appear in the text as `[title](url)`. Pick only new items that help developers learn GitHub faster and are not already in `site/content/github-info.md`.

Update `site/content/github-info.md` with concise, practical updates. For each item, name its source (GitHub Blog, GitHub Changelog, or Awesome Copilot) and include a direct link.

Open a pull request for Mona to review. Use a title that mentions Mona or GitHub Info, and include the sources (GitHub Blog, GitHub Changelog, Awesome Copilot) in the description. Do not write directly to `main`; rely on `safe-outputs` with `create-pull-request`.