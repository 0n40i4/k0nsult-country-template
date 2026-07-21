# k0nsult-country-template

A **scaffold** for instantiating the K0NSULT per-country transparency layer in any
jurisdiction. The working reference is [`k0nsult-country-pl`](../k0nsult-country-pl)
(Poland); this template abstracts that structure so another Member State can stand
up its own layer.

> **Doctrine:** `claim ≤ proof`; only agents are scored, never natural persons.
> "100%" means evidence coverage, not impenetrability.

## What is actually in this repo

```text
surfaces/
  cc-kraj.html               # TEMPLATE — country overview: NCA, sandbox, national law
  cc-panstwo.html            # TEMPLATE — state-level oversight + constitutional graph
  cc-min-cyfryzacji.html     # TEMPLATE — digital ministry / AI-Act competent authority
  cc-ranking-organy.html     # TEMPLATE — authority readiness ranking (national language)
  cc-ranking-organy-en.html  # TEMPLATE — English twin of the ranking
  README.md                  # naming contract + full placeholder inventory
INSTANTIATION-CHECKLIST.md   # fork → replace → validate → publish runbook
LICENSE  NOTICE  README.md  SECURITY.md  publiccode.yml  sbom.json
```

The `cc-` prefix is a **placeholder for the country code in the filename**. After forking, rename
every template to `<cc>-*.html` (`<cc>` = ISO 3166-1 alpha-2, lower-case: `de`, `fr`, `es`) — no
`cc-*` file may remain in an instance repo.

The templates are real, self-contained HTML pages: no CDN, no web fonts, no analytics, no trackers.
They render offline immediately after a fork, with every country-specific value carried as a
`{{PLACEHOLDER}}` token. Nothing here asserts a fact about any country.

**Surfaces this repo deliberately does not template** (derive them, do not fabricate them):

- `<cc>-prezydent.html` — head-of-state track; constitutional systems differ too much for a
  meaningful template. Derive it from `cc-panstwo.html`, or drop it if the office has no equivalent.
- `<cc>-ranking-<local>.html` (+ `-en`) — sub-national bodies (PL: `jst-kas`; DE: `laender`;
  FR: `regions`; ES: `ccaa`). Copy the two ranking templates and change slug, headings and rows.

## How to instantiate

1. Fork this repo and rename `surfaces/cc-*.html` → `surfaces/<cc>-*.html`.
2. Replace every `{{PLACEHOLDER}}`. The full inventory is in
   [`surfaces/README.md`](surfaces/README.md#placeholder-inventory);
   `grep -R -o '{{[A-Z_0-9]*}}' surfaces/` must come back empty before publishing.
3. Source the target jurisdiction's bodies (competent DPA, AI-Act authority, sandbox channel,
   sub-national units) — use the public regulators registry from `k0nsult-eu-shield`. A row without a
   resolvable source is **GAP**, never DOWÓD.
4. Keep the doctrine intact: no personal data on any surface; institutions only, never persons.
5. Validate: pages parse with JavaScript disabled, no external hosts, `<html lang>` set, statuses
   readable without colour.
6. Generate the SBOM: `node ../k0nsult-tools/sbom.mjs --root . --out sbom.json`.
7. Keep any `/api/*` calls as the integration boundary — do **not** bundle engine code.

The full runbook, with the blocking gates, is
[`INSTANTIATION-CHECKLIST.md`](INSTANTIATION-CHECKLIST.md).

## License
Apache-2.0 (patent grant, Section 3). See `LICENSE` and `NOTICE`.
