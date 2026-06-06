# markmap

**Mindmaps from markdown, on your own [Solid](https://solidproject.org) pod.** Feed it a
JSON-LD document whose payload is markdown, and it renders an interactive
[markmap](https://markmap.js.org) — pan, zoom, collapsible branches. One build-free
HTML file; the markmap/d3 libraries are vendored (pinned, no CDN at runtime).

## The document

A JSON-LD envelope, markdown payload — headings become branches, list items become leaves:

```json
{ "@context": { "schema": "https://schema.org/", "mm": "https://solid-apps.github.io/markmap/ns#" },
  "@id": "#this", "@type": "mm:Markmap", "schema:name": "My map",
  "schema:text": "# Root\n## Branch A\n- leaf\n## Branch B\n- leaf" }
```

Markmap [frontmatter](https://markmap.js.org/docs/json-options) inside the markdown is
honored (`markmap: {colorFreezeLevel: 2, …}`). The reader is tolerant: `mm:markdown` or a
bare `markdown` key also work, as does a prebuilt markmap node tree under `mm:root`.

## Three ways to load a map

1. **Data island** — paste your JSON-LD into the `<script type="application/ld+json"
   id="markmap-data">` block in `index.html` (fork-by-copy, like webprompts).
2. **`?src=<url>`** — any JSON-LD doc… **or any raw `.md` URL**. Point it at a page from
   the `pages`/`notes` apps and get a mindmap of it for free:
   `…/markmap/?src=/public/pages/my-page.md`
3. **`sample.jsonld`** — the built-in demo ("Your pod, mapped").

Maps live on your pod at `/public/markmap/<name>.jsonld` by convention
(`Content-Type: application/ld+json`); "fork" = copy the document to your own pod.

## Run

Static — open `index.html`, or install via the **store** to `/public/apps/markmap/`.

AGPL-3.0-only for the app. Vendored libraries keep their upstream licenses:
[markmap](https://github.com/markmap/markmap) (MIT), [d3](https://github.com/d3/d3) (ISC).
