# Tech Kahani — Site

Stories that explain how machines think. Static, no build step, GitHub Pages ready.

## Structure

```
index.html          ← the Library (home) — lists episodes from manifest.json
read.html           ← the universal episode reader (renders ANY episode)
manifest.json       ← the shelf: one entry per episode
episodes/ep1.md     ← Episode 1 master copy (story + panel config)
assets/style.css    ← shared design system
```

The reader fetches `episodes/ep{n}.md`, parses it, and renders everything:
chapters, the interactive control panel, the loss meter, Rosetta Stone,
breaks-down, Engineer's Corner, post-credits, and prev/next navigation.
**No episode content lives in any HTML file.**

## Publish (one time)

1. Create a GitHub repo (e.g. `techkahani`), push these files to the root of `main`.
2. Repo → Settings → Pages → Source: "Deploy from a branch" → `main` / root.
3. Live at `https://<username>.github.io/techkahani/`. Add a custom domain later in the same settings page.

## Preview locally

Browsers block `fetch` on `file://`, so run a tiny server:

```
python3 -m http.server 8000
# open http://localhost:8000
```

## Add Episode 2 (the whole workflow — 3 steps, zero HTML edits)

1. **Copy** `episodes/ep1.md` → `episodes/ep2.md`. Replace the story content.
2. **Edit the CONFIG block** at the top of the new file:
   - `title`, `alt`, `eyebrow`, `chaptersLabel`, `hint`
   - `meter` — the top-bar metric (Ep 2 could be `{"label":"CONFIDENCE","start":100}` falling as overfitting bites — the meter means whatever the episode needs)
   - `panel.controls` — any mix of `dial` / `lever` / `light` controls; or delete the whole `panel` key for no widget
   - `states` — one entry per chapter (keys = chapter numbers + `"post"`); each sets control values + a `status` line; `"shake": true` rattles the panel
   - `loss` — meter value per chapter
   - `oscillate` (optional) — makes one control oscillate while a marker paragraph is on screen
3. **Add one entry to `manifest.json`** and flip `"status"` from `"brewing"` to `"live"`.

Done. The Library shows the new card; `read.html?ep=2` just works.

## Episode file format

```
<!--CONFIG
{ ...json config... }
CONFIG-->

# CHAPTER 1: Title
Markdown paragraphs. Inline HTML allowed (e.g. <div class="scorecard">…).

# CHAPTER 2: Title
...

# SECTION: What just happened      ← generic recap section
# SECTION: Rosetta Stone — ...     ← markdown table, auto-styled as the laminated card
# SECTION: Where the analogy breaks down
# SECTION: Engineer's Corner       ← auto-collapses into a <details>
# SECTION: After the credits       ← styled teaser card
# SECTION: Next episode            ← the closing italic line
```

Section matching is by first word (`rosetta`, `where`, `engineer`, `after`, `next`);
anything else renders as a generic titled section. Chapters must be
`# CHAPTER n: Title` — the `n` drives the panel states.

## House rules

- The `.md` files in `episodes/` ARE the master copies (manifesto rule: one source, many renders).
- Continuity props in story HTML (scorecard, memo) must match the Story Bible.
- All rights reserved · free to read. © Shubh Pathak.
