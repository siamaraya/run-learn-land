# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Static one-page project hub (Thai) for the "วังหมีกระทิง Run Learn Land" trail-festival event
(25–27 ธ.ค. 2569), deployed on GitHub Pages from `main` root:
https://siamaraya.github.io/run-learn-land/ — no build step, no dependencies, no tests.
All content originates from the team's Google Slides (public, presentation id
`1XixIEtHMdWNPv9-A1-TIhnJgSiNk5z4JQ2BJKwCxIR4`), fetched as plain text via the
`/export/txt` endpoint (no auth needed).

## Data flow

```
Google Slides ──export/txt──> data/slides.txt (baseline snapshot)
                                   │ diff
                                   ▼
                     index.html  +  slides-content.md   ──git push──> GitHub Pages (~1 min)
                                   │
                                   └─(generated) dashboard.html ──> claude.ai artifact
```

- `.github/workflows/sync-slides.yml` polls the slides every 30 min; on diff it runs
  `claude -p "$(cat scripts/sync-prompt.md)"` to propagate changes into `index.html` and
  `slides-content.md`, then commits as `slides-sync-bot`. Requires repo secret
  `CLAUDE_CODE_OAUTH_TOKEN` (or `ANTHROPIC_API_KEY`).
- `scripts/sync-prompt.md` is the CI editing contract. If you rename section IDs or
  restructure `index.html`, update that prompt in the same commit or CI edits will misfire.
- `dashboard.html` is GENERATED (gitignored): `index.html` with `assets/cover.jpg` inlined as
  a base64 data URI, used only to republish the claude.ai artifact (CSP blocks external
  requests there). Never edit it directly — regenerate:

  ```bash
  python3 -c "
  import base64
  uri = 'data:image/jpeg;base64,' + base64.b64encode(open('assets/cover.jpg','rb').read()).decode()
  html = open('index.html').read()
  open('dashboard.html','w').write(html.replace('assets/cover.jpg', uri))
  "
  ```

## index.html conventions

- Single self-contained file: inline CSS/JS, Thai system font stack, no external resources.
- Dual theme via CSS custom properties on `:root`, redefined under
  `@media (prefers-color-scheme: dark)` and again under `:root[data-theme="dark"|"light"]`
  (the viewer toggle must win) — style components only through the tokens.
- Glassmorphism cards (backdrop-blur + shadow). **Never add white borders** — hard rule from
  the owner's global constitution.
- Content sections by ID: `#overview` (stat tiles + journey), `#tasks` (pending items with
  owner/due tags), `#depts` (12 department cards with `<details>` homework), `#finance`
  (CSS bar chart + revenue tables, `tabular-nums`), `#meetings` (timeline; the amber
  `.t-item.next` is always the last "upcoming" entry), `#files`.
- Hero countdown targets `2026-12-25T06:00:00+07:00` — change only if the slides change the
  event date.
- Known data quirk: slides say "550,000 (ส้วม)" but the 851,000 expense total only works if
  it's 50,000 — keep the footnote pattern for any such inconsistency (keep the slide value,
  add a warning note; never silently "fix" slide numbers).

## Privacy rules (public repo — non-negotiable)

- The owner operates publicly as **siamaraya / Goody**; their real name must never appear in
  this repo — including commit author emails. Repo-local git config is already set to
  `siamaraya <249662498+siamaraya@users.noreply.github.com>`; do not override it and do not
  commit with the global (real-name) email.
- `apps-script/` is gitignored because `Code.gs` contains a personal email. It stays local
  (Google Apps Script watcher that emails on slide changes — see `apps-script/SETUP.md`).
  Never force-add it.

## Git/GitHub notes

- Pushing `.github/workflows/**` requires the `workflow` OAuth scope on the gh token
  (`gh auth refresh -h github.com -s workflow` if rejected).
- Pages serves `main` root directly — a push is a deploy; verify with
  `curl -s -o /dev/null -w "%{http_code}" https://siamaraya.github.io/run-learn-land/`.
- Trigger a manual sync from the Actions tab ("Sync slides to dashboard" → Run workflow) or
  `gh workflow run sync-slides.yml`.
