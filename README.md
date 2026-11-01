# Chip Hippo Gallery

The published entries behind [chiphippo.com/gallery](https://chiphippo.com/gallery/):
circuits built in [Chip Hippo](https://github.com/jfigge/chiphippo), each one a
folder anyone can open in the app or in the browser.

This repository holds **data only**. The site is built by the app repository's
`deploy-site.yml`, which checks this one out and runs
`scripts/build-gallery.mjs` over `entries/`. The gallery's seed entries, the
app's own example circuits, are not here: they're listed in the app repository's
`website/gallery/seed.json` and read from each release.

## An entry

```
entries/<id>/
  entry.json                 # the metadata
  design.desktop.chiphippo   # the circuit: a desktop snapshot (Desktop ▸ Export…)
  thumb.png                  # optional; rendered by CI
```

`<id>` is a lowercase slug followed by six hex digits (`four-bit-counter-3fa9c1`), and
the folder name **is** the id: an `entry.json` naming a different one is refused.

`entry.json` is written by the gallery's tooling. Only `title`, `author`,
`description` and `tags` come from the person sharing; `id`, `created`, `source`,
`featured` and `appVersion` (the release it was validated on) are set when it's
accepted. Everything under `derived` (engine, counts, families, level and automatic
tags) is recomputed from the design on every build and never read from the file.

```json
{
  "schema": 1,
  "id": "four-bit-counter-3fa9c1",
  "kind": "design",
  "title": "Four-bit counter",
  "author": "Ada",
  "description": "Counts to fifteen on four LEDs.",
  "tags": ["counter"],
  "created": "2026-11-01",
  "source": "maintainer",
  "featured": false,
  "appVersion": "1.5.4"
}
```

## Checks

Every push and pull request runs the app's own validator over `entries/`
(`.github/workflows/validate.yml`). A design must load in Chip Hippo with **nothing
dropped**: every board, part and wire in the file has to survive the app's loader. It
may only use the custom chips it carries, and those chips must compile.
