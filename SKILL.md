---
name: markmap
description: How to author a markmap document — a JSON-LD envelope with a markdown payload that the markmap solid-app renders as an interactive mindmap. Use when creating or editing a mindmap on a Solid pod, or turning notes/structure into a visual map.
---

# markmap

**markmap** is a build-free [solid-app](https://github.com/solid-apps) that renders an
interactive **mindmap** from one JSON-LD document. App: https://solid-apps.github.io/markmap/
(also installed on pods at `/public/apps/markmap/`).

Your job as an LLM is usually: **author the document** (the common case) or edit one.

## Document shape

```json
{
  "@context": { "schema": "https://schema.org/", "mm": "https://solid-apps.github.io/markmap/ns#" },
  "@id": "#this",
  "@type": "mm:Markmap",
  "schema:name": "Title shown in the header",
  "schema:text": "# Root\n## Branch\n- leaf\n- leaf"
}
```

The **markdown in `schema:text` is the whole map**:
- `#` heading = the root (exactly one).
- `##`/`###` headings = branches/sub-branches.
- `-` list items = leaves. Nested lists nest deeper.
- Inline markdown works in nodes: **bold**, *italic*, `code`, [links](https://example.com).
- Keep node text short (a few words) — long nodes wrap and crowd the map.

The reader is tolerant: `mm:markdown` or bare `markdown` keys also work; a prebuilt
markmap node tree (`{content, children[]}`) under `mm:root` is accepted; `@graph` docs are
unwrapped (the `mm:Markmap`-typed node wins).

### Tuning the render (optional frontmatter)

Prepend YAML frontmatter to the markdown — passed to markmap's
[json-options](https://markmap.js.org/docs/json-options):

```markdown
---
markmap:
  colorFreezeLevel: 2     # color by top-level branch
  maxWidth: 280           # wrap long nodes
---

# Root
...
```

**Gotcha — `initialExpandLevel`:** it collapses everything *deeper* than that level on
load, so children (including clickable links) are **hidden until the user clicks each
branch**. Only use it for big maps (~50+ nodes) where a fully-expanded view would be
overwhelming; for small/medium maps **omit it** so the whole map — and its links — is
visible at once. (Learned the hard way: a 6-branch map with `initialExpandLevel: 2`
shipped with its Links branch invisibly folded.)

## Storage & viewing

- Save maps at **`/public/markmap/<name>.jsonld`** with `Content-Type: application/ld+json`.
- View: `…/markmap/?src=<url-of-the-doc>`.
- `?src=` also accepts a **raw `.md` URL** — any markdown on the pod renders as a map
  (e.g. a `pages` doc: `…/markmap/?src=/public/pages/<name>.md`).
- "Fork" = copy the JSON-LD document to another pod.

## Minimal example (validated shape)

```json
{
  "@context": { "schema": "https://schema.org/", "mm": "https://solid-apps.github.io/markmap/ns#" },
  "@id": "#this", "@type": "mm:Markmap", "schema:name": "Trip plan",
  "schema:text": "---\nmarkmap:\n  colorFreezeLevel: 2\n---\n\n# Trip\n\n## Travel\n- flights\n- rail pass\n\n## Stay\n- hotel A\n- hotel B\n\n## Do\n- museum day\n- coast walk\n"
}
```
