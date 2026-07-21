# INSTANTIATION-CHECKLIST — standing up a national K0NSULT layer

A step-by-step runbook for instantiating the K0NSULT per-country transparency layer in a new
jurisdiction. This repo ships **working HTML templates** in `surfaces/` (see
[`surfaces/README.md`](surfaces/README.md)); the Polish reference implementation
[`k0nsult-country-pl`](../k0nsult-country-pl) shows what a filled-in instance looks like.

> **Doctrine (non-negotiable):** `claim ≤ proof`; only **agents/institutions** are scored, never
> natural persons; "100%" means **evidence coverage**, not impenetrability; the orchestration engine
> stays hidden (`/api/*` is the only boundary); **No Password Custody** (this repo carries no
> keys/secrets).

Each step is labelled with what it produces under `claim ≤ proof`:
**DOWÓD** (re-derivable artefact), **GAP** (planned, phrased as future),
**NARRACJA** (framing/positioning — must be labelled). See
[`k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md`](../k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md).

`<cc>` below = the ISO 3166-1 alpha-2 country code of the target jurisdiction
(e.g. `de`, `fr`, `es`), lower-case.

---

## 0. Preconditions

- [ ] You have read the doctrine block above and the `claim ≤ proof` SPEC.
- [ ] The target jurisdiction is identified and its ISO code `<cc>` fixed, together with the page
      language tag `{{LANG}}` and Open Graph locale `{{OG_LOCALE}}`.
- [ ] You have local checkouts of the sibling repos referenced here:
      `k0nsult-country-pl`, `k0nsult-eu-shield`, `k0nsult-tools`, `k0nsult-ai-truth-core`.
- [ ] You understand that **no step in this checklist publishes anything** — publication, external
      submission and signing are irreversible acts that are **human-gated** (operator sign-off),
      never performed by tooling. *(NARRACJA → the act itself is a GAP until an operator performs it.)*

---

## 1. Fork the template

- [ ] Fork / copy this repository into `k0nsult-country-<cc>`.
- [ ] Keep the top-level files: `LICENSE`, `NOTICE`, `README.md`, `SECURITY.md`,
      `INSTANTIATION-CHECKLIST.md`, and (generated later) `sbom.json`, `publiccode.yml`.
- [ ] Rename each template in `surfaces/`, dropping the `cc-` placeholder prefix for the real code:

      surfaces/cc-kraj.html              → surfaces/<cc>-kraj.html
      surfaces/cc-panstwo.html           → surfaces/<cc>-panstwo.html
      surfaces/cc-min-cyfryzacji.html    → surfaces/<cc>-min-cyfryzacji.html
      surfaces/cc-ranking-organy.html    → surfaces/<cc>-ranking-organy.html
      surfaces/cc-ranking-organy-en.html → surfaces/<cc>-ranking-organy-en.html

- [ ] Verify no `cc-*` file survives: `ls surfaces/ | grep '^cc-'` must return nothing.
- [ ] In `NOTICE`, replace the scope block with the `<cc>` scope: list the surfaces this instance
      actually ships, and keep the closing lines verbatim: *NOT INCLUDED: the proprietary engine …
      personal data … secrets/keys*; and *DOCTRINE: claim <= proof; Human Override Always;
      agents-not-people.*
- [ ] In `README.md`, state clearly that this is an **instance** of the per-country pattern (Poland is
      the reference), not a second reference. *(NARRACJA — positioning.)*

**Produces:** repo skeleton with renamed surfaces (NARRACJA until placeholders are replaced).

---

## 2. Replace the placeholders

Every country-specific value in the templates is a `{{UPPER_SNAKE_CASE}}` token. The full inventory,
grouped by theme and mapped to the files that use it, is in
[`surfaces/README.md`](surfaces/README.md#placeholder-inventory).

- [ ] Fill in identity/provenance: `{{COUNTRY_NAME}}`, `{{COUNTRY_CODE}}`, `{{LANG}}`,
      `{{LANG_LABEL}}`, `{{OG_LOCALE}}`, `{{CANONICAL_URL}}`, `{{CANONICAL_URL_EN}}`,
      `{{SITE_BASE_URL}}`, `{{PUBLISHER_NAME}}`, `{{PUBLISHER_ID}}`, `{{SURFACE_ID}}`,
      `{{VERSION}}`, `{{SNAPSHOT_DATE}}`.
- [ ] Fill in the regulator registry (step 3 supplies the values): `{{NCA_*}}`, `{{SANDBOX_*}}`,
      `{{NATIONAL_LAW*}}`, `{{MARKET_SURVEILLANCE_*}}`, `{{DPA_*}}`, `{{CYBER_AUTHORITY_*}}`,
      `{{CSIRT_*}}`, `{{DIGITAL_AUTHORITY_*}}`, `{{OPEN_DATA_PORTAL_URL}}`.
- [ ] Fill in the state structure (step 4 supplies the values): `{{GOV_PORTAL_URL}}`,
      `{{GOVERNMENT_BODY_NAME}}`, `{{HEAD_OF_GOVERNMENT_*}}`, `{{HEAD_OF_STATE_TITLE}}`,
      `{{INDEPENDENT_BODIES}}`, `{{APPOINTMENT_BASIS}}`, counts and the repeatable row fields.
- [ ] Duplicate the repeatable blocks (`<tr>` / `.mc-card`) once per real institution and delete the
      example rows. An institution you cannot source does not get a row.
- [ ] Endpoints: set `{{COUNTRY_DATA_ENDPOINT}}` / `{{LINK_HEALTH_ENDPOINT}}` to your `/api/*` paths,
      **or leave them unreplaced** — the scripts detect an unreplaced token and keep the static
      content, so a purely static fork stays valid.
- [ ] Translate the visible text of `<cc>-ranking-organy.html` into the national language; keep
      `<cc>-ranking-organy-en.html` in English with **identical data rows**.
- [ ] Keep the shared visual identity (mono / `--phosphor` terminal styling) and the accessibility
      properties: `<html lang>`, spelled-out status words, `<th scope>`, `<caption>`, `<noscript>`.
- [ ] **Do not** add external dependencies. No CDN, no remote fonts, no analytics — a fork must
      render offline. Verify: `grep -RoE 'https?://[^"'"'"' )]+' surfaces/*.html | grep -v schema.org`
      should return only official institution links you deliberately added.
- [ ] **Do not** copy engine logic. Pages call `/api/*`; they never embed it.

**Verification gate (DOWÓD):**

```bash
grep -R -o '{{[A-Z_0-9]*}}' surfaces/ | sort -u    # must be empty
```

**Produces:** the `surfaces/<cc>-*.html` set with zero surviving placeholders.

---

## 3. Wire the authority / DPA registry from eu-shield

National surfaces list **institutional** contacts (competent authorities), never people.

- [ ] Pull the `<cc>` authority contacts from the EU regulators registry in
      [`k0nsult-eu-shield/surfaces/ai-truth-regulatorzy-eu.html`](../k0nsult-eu-shield/surfaces/ai-truth-regulatorzy-eu.html)
      — it holds public DPA / AI-Office / market-surveillance contacts for EU27.
- [ ] Populate the **AI Act authority registry** table in `<cc>-min-cyfryzacji.html` (section
      `#instytucje`) with: the competent **DPA**, the **AI Act** market-surveillance / notifying
      authority, the national competent authority, the **art. 57 regulatory sandbox** channel, and
      the national implementing law — all as institutional registry data.
- [ ] For each row, record a resolvable public source (official register / gazette URL) in the
      "Source" column. A contact **with** a resolvable source is **DOWÓD**; one you intend to confirm
      later is **GAP** (label it in the "Evidence class" column) — never present an unconfirmed
      contact as verified.
- [ ] Mirror the same values into `<cc>-kraj.html` (`{{NCA_NAME}}`, `{{NCA_STATUS}}`,
      `{{SANDBOX_STATUS}}`, `{{NATIONAL_LAW}}`, `{{NATIONAL_LAW_STATUS}}`, `{{SOURCE_URL}}`).
- [ ] Cross-check that every authority row maps to the EU-level view in `k0nsult-eu-shield`
      (`ai-truth-ranking-ue.html` / `ai-truth-mapa-ue.html`) so the national and EU layers agree.

**Produces:** registry rows classified DOWÓD (with source) or GAP (pending) — the SPEC forbids an
unsourced row labelled DOWÓD.

---

## 4. Adapt the constitutional / institutional system

The templates carry a neutral skeleton (head of state, head of government, collective government,
ministries, central authorities, independent bodies). Do not force a foreign model onto `<cc>`.

- [ ] Identify the target's **head-of-state / executive** arrangement and set
      `{{HEAD_OF_STATE_TITLE}}`, `{{HEAD_OF_GOVERNMENT_TITLE}}`, `{{GOVERNMENT_BODY_NAME}}` and
      `{{APPOINTMENT_BASIS}}` in the constitutional graph of `<cc>-panstwo.html`. Add
      `<cc>-prezydent.html` only if a distinct head-of-state track genuinely exists — derive it from
      `<cc>-panstwo.html`; there is deliberately no template for it.
- [ ] Identify the **competent AI Act authority** (it may be the DPA, a telecoms/market regulator, or
      a new AI office) and point `<cc>-min-cyfryzacji.html` at the real body — the filename slug may
      stay, the content must be correct.
- [ ] Identify the **independent bodies** (audit institution, central bank, DPA — whichever are
      constitutionally outside government) and mark them as such, so the page does not imply
      subordination that does not exist.
- [ ] Identify the **sub-national tiers** and, if you ship that surface, copy the two ranking
      templates to `<cc>-ranking-<local>.html` / `-en.html` (DE: `laender`, FR: `regions`,
      ES: `ccaa`) and set the slug, headings and rows to the real administrative geography.
- [ ] Record the mapping decisions in `README.md` (a short "reference → `<cc>` mapping" table), so
      the adaptation is auditable. *(NARRACJA — labelled positioning/decision log.)*

**Produces:** an auditable institution map (NARRACJA) + corrected surface set.

---

## 5. Validate the surfaces — MANDATORY GATE

- [ ] Every HTML file parses: open each `surfaces/<cc>-*.html` in a browser **with JavaScript
      disabled** and confirm the static content renders (headings, tables, registry rows).
- [ ] Zero surviving placeholders (the grep from step 2 returns nothing).
- [ ] Zero external hosts: no CDN, font, analytics or beacon URL anywhere in `surfaces/`.
- [ ] Each JSON-LD block is valid JSON, and its `url` / `inLanguage` match the page.
- [ ] Accessibility spot-check: `<html lang>` set; status meaning readable without colour;
      `<th scope>` on data tables; headings in order; wide tables scroll in their own container.
- [ ] The `-en` twin carries the **same rows** as the national-language ranking page.

**Produces:** DOWÓD — a re-runnable validation pass over the shipped surfaces.

---

## 6. PII-exclusion test (agents-not-people) — MANDATORY GATE

No surface may carry personal data of natural persons. Institutional contacts (an office, its
generic inbox, its register entry) are **not** PII; a named official's private data is.

- [ ] Scan every `surfaces/<cc>-*.html` for: personal names tied to private data, private
      emails/phones, national ID / tax numbers of individuals, dates of birth, home addresses, or any
      per-person score. **Score agents/institutions, never persons.**
- [ ] Confirm the templates' hard rule was respected: no names of office holders on
      `<cc>-panstwo.html` or `<cc>-min-cyfryzacji.html` — titles of offices only.
- [ ] Confirm **No Password Custody**: no keys, tokens, secrets or credentials anywhere in the repo.
- [ ] Confirm there is **no engine code** — only surfaces and `/api/*` calls.
- [ ] Record the result as a DOWÓD: the command run + its output (empty match set).
- [ ] If a hit is a genuine institutional contact, keep it and note why it is registry data, not PII
      (mirror the wording already used in `k0nsult-country-pl/NOTICE`).

> This gate is **blocking**: the layer is not instantiable until the PII-exclusion test is a
> re-derivable DOWÓD (someone else can re-run the scan and get the same empty result).

**Produces:** DOWÓD — a re-runnable exclusion scan with an empty people-level result set.

---

## 7. Generate the SBOM

Supply-chain inventory is a CycloneDX-lite JSON, one component per file with a SHA-256 hash,
generated by the zero-dependency tool in `k0nsult-tools`.

- [ ] From the repo root, run:

      ```bash
      node ../k0nsult-tools/sbom.mjs --root . --out sbom.json
      ```

- [ ] Verify `sbom.json` lists **every** shipped file (all `surfaces/<cc>-*.html`, `LICENSE`,
      `NOTICE`, `README.md`, `SECURITY.md`, `surfaces/README.md`, this checklist) each with a
      SHA-256 hash and `k0nsult:surface = open-source`.
- [ ] Confirm the `k0nsult:stats` property reflects the real file counts for this instance.
- [ ] Re-running the generator on an unchanged tree must yield identical hashes — that
      reproducibility is what makes the SBOM a **DOWÓD** rather than a self-declared list.

**Produces:** DOWÓD — `sbom.json` (re-derivable, SHA-256-pinned inventory).

---

## 8. Publish (human-gated — not performed here)

- [ ] Every published claim on every surface is classified DOWÓD / GAP / NARRACJA and labelled; no
      DOWÓD lacks a resolvable source (SPEC hard rule 1).
- [ ] The validation DOWÓD (step 5), the PII-exclusion DOWÓD (step 6) and the SBOM DOWÓD (step 7) are
      all present.
- [ ] `LICENSE` (Apache-2.0, patent grant §3) and the `<cc>`-scoped `NOTICE` are in place.
- [ ] Serve `surfaces/` statically, or behind a backend implementing the `/api/*` contract.
- [ ] **Publication / external submission / signing** is left to the operator. Tooling counts and
      labels; it never authorises the irreversible act. *(GAP until an operator signs off — phrase any
      "published" claim as future until then.)*

---

## Instantiation summary (fill in per instance)

| Item | State |
|---|---|
| Country code `<cc>` | … |
| Surfaces shipped | … |
| Placeholders replaced (grep empty) | DOWÓD / **BLOCKED** |
| DPA / AI-Act authority mapped | DOWÓD / GAP |
| Institution map recorded in README | yes / no |
| Surface validation (parses, no external hosts, a11y) | DOWÓD / **BLOCKED** |
| PII-exclusion test | DOWÓD (empty people-level result) / **BLOCKED** |
| SBOM generated & reproducible | DOWÓD / GAP |
| Publication | GAP until operator sign-off |

---

## License
Apache-2.0 (explicit patent grant, Section 3). See `LICENSE` and `NOTICE`.
