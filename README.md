# ronzzwikibase-public-mirror

**Generated, one-way mirror of the public content on
[wikibase.ronzz.org](https://wikibase.ronzz.org). Do not edit these files by
hand.** The wiki is the source of truth; this repository is refreshed from it
every 15 minutes.

## What is mirrored

- **Main** — encyclopedia/theory pages
- **Cheatsheets**, **HowItWorks**, **FOSS**
- **Person**, **Source**, **Collective**, **Software**

Paths are `<Namespace>/<Title>.wiki` (`Main/` for the main namespace;
subpages become directories). `state.json` records each page's wiki `revid`
and content hash so the next run downloads only changed pages.

Deliberately excluded:

- **`RonzzIT:` / `RonzzInt:`** — gated ops documentation, mirrored in the
  private `ronzz-it-runbooks` repository. This repository is public; the
  mirror tool refuses those namespaces.
- `MediaWiki:` / `Template:` — two-way synced in
  [`ronzz-wikibase/wiki_pages/`](https://github.com/Ron-RONZZ-org/ronzz-wikibase/tree/main/wiki_pages).
- `Item:` / `Property:` (Wikibase entities), user/talk/category pages, and
  `File:` metadata.

## How it works

The mirror tool lives in
[`ronzz-wikibase/tools/wiki_content_mirror.py`](https://github.com/Ron-RONZZ-org/ronzz-wikibase/blob/main/tools/wiki_content_mirror.py).
[`.github/workflows/mirror.yml`](.github/workflows/mirror.yml) checks out this
repository, clones the tooling with `--depth=1`, runs the tool with
`--dir . --state state.json --prune`, and commits any changes with
`[skip ci]`.

Local use:

```bash
git clone --depth=1 https://github.com/Ron-RONZZ-org/ronzz-wikibase.git /tmp/wb
git clone https://github.com/Ron-RONZZ-org/ronzzwikibase-public-mirror.git
cd ronzzwikibase-public-mirror
python3 /tmp/wb/tools/wiki_content_mirror.py --dir . --dry-run      # preview
python3 /tmp/wb/tools/wiki_content_mirror.py --dir . --prune        # update
python3 /tmp/wb/tools/wiki_content_mirror.py --dir . --full         # refetch all
```

A mass prune of more than 10% of the mirror is refused unless
`--force-prune` — a transient partial listing must never wipe the snapshot.

## Content licensing

The mirrored text is the wiki's content, © its contributors, and subject to
the terms of https://wikibase.ronzz.org. This repository adds no license of
its own; the tooling in `ronzz-wikibase` is GPL-2.0-or-later.
