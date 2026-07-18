# k0nsult-country-template

A **scaffold** for instantiating the K0NSULT per-country transparency layer in any
jurisdiction. The working reference is [`k0nsult-country-pl`](../k0nsult-country-pl)
(Poland); this template abstracts that structure so another Member State can stand
up its own layer.

> **Doctrine:** `claim ≤ proof`; only agents are scored, never natural persons.
> "100%" means evidence coverage, not impenetrability.

## Expected structure

```
surfaces/
  <cc>-panstwo.html          # state-level oversight
  <cc>-prezydent.html        # head-of-state track (adapt to local system)
  <cc>-min-cyfryzacji.html   # digital-ministry / competent authority
  <cc>-ranking-organy.html   # authority ranking (+ -en)
  <cc>-ranking-<local>.html  # sub-national bodies (PL: JST-KAS) (+ -en)
LICENSE  NOTICE  README.md  sbom.json
```
`<cc>` = ISO country code (e.g. `de`, `fr`, `es`).

## How to instantiate
1. Copy the reference pages from `k0nsult-country-pl/surfaces/`.
2. Replace Poland-specific bodies/authorities with the target jurisdiction's
   (competent DPA, digital ministry, sub-national units) — use the public
   regulators registry from `k0nsult-eu-shield` for EU authority contacts.
3. Keep the doctrine intact: no personal data on any surface; only agents scored.
4. Generate the SBOM: `node ../k0nsult-tools/sbom.mjs --root . --out sbom.json`.
5. Keep any `/api/*` calls as the integration boundary — do **not** bundle engine code.

## License
Apache-2.0 (patent grant, Section 3). See `LICENSE` and `NOTICE`.
