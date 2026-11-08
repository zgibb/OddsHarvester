# CHANGES.md — fork vs. stock

Fork of [jordantete/OddsHarvester](https://github.com/jordantete/OddsHarvester). Stock = `upstream/master`.
Work happens on `zack/main`. Anything that is clearly ours (glue, loaders, paper-bet log) lives in the separate
`sports-engine` repo, not here. Keep this list short.

## Changes vs. stock

| File | Change | Why |
|---|---|---|
| `src/oddsharvester/core/playwright_manager.py` | Browser `channel` read from env var `ODDSHARVESTER_BROWSER_CHANNEL` (default `chromium`, i.e. stock behaviour). | Playwright's bundled full Chromium (build 143) fails to start on this PC (Windows 11 build 10.0.26300): `spawn UNKNOWN`, SideBySide error "Dependent Assembly 143.0.7499.4 could not be found". Setting `ODDSHARVESTER_BROWSER_CHANNEL=msedge` uses the installed Edge, which scraped 50/50 NBA games. |
| `CHANGES.md`, `CLAUDE.md` (fork section at the bottom) | Added. | Fork bookkeeping. |

## Pulling upstream updates

Upstream default branch is `master`; our working branch is `zack/main`. The `upstream` remote has its push URL set to
`DISABLED`, so a push to it cannot happen by accident.

```bash
git fetch upstream --tags
git switch zack/main
git rebase upstream/master        # or: git merge upstream/master
uv run pytest tests/ -q --ignore=tests/integration/
```

Then re-check the table above still applies, and update it.

## Windows notes

- Clone with long paths on: `git config core.longpaths true` (test fixtures have very long file names).
- Run with Edge: `$env:ODDSHARVESTER_BROWSER_CHANNEL = "msedge"`.
