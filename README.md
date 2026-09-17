# CS Unit 2 — course hub

The hub students land on for Computer Science Unit 2. Same pipeline as the Unit 1 hub, different
course, different visual identity — this one borrows its fonts, colours and "engineering notebook"
feel directly from the topic pages it links to.

```
JSONBin ─► GitHub Action ─► fetches each topic URL ─► data/content.json ─► index.html
 (URLs)     (holds the key)   (reads og: + lesson: meta)  (committed, public)  (plain fetch, no key)
```

`scripts/sync.mjs` does the fetching and scraping — identical to Unit 1's, since the lesson manifest
contract is the same across every CFBC course. `.github/workflows/sync.yml` runs it every six hours,
on push, and on manual dispatch. Secrets are `JSONBIN_KEY` and `JSONBIN_BIN_ID`; the repo variable
`JSONBIN_KEY_TYPE` switches between `master` and `access` header names.

Anything set explicitly in the bin (`title`, `blurb`, `tags`, `accent`) overrides what was scraped.
If a page is unreachable the previous metadata is kept rather than blanked.

`admin.html` is a local-only bin editor and is in `.gitignore` — it never gets published. Open it from
your own machine to load, edit and save the bin contents directly.

After pushing a topic page, refresh the hub's card by running:

```bash
gh workflow run "Sync course content" --repo enoete/compsciu2_hub
```
