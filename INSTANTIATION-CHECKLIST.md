# INSTANTIATION-CHECKLIST — standing up a national K0NSULT layer

A step-by-step runbook for instantiating the K0NSULT per-country transparency layer
in a new jurisdiction, using [`k0nsult-country-pl`](../k0nsult-country-pl) (Poland)
as the working reference implementation.

> **Doctrine (non-negotiable):** `claim ≤ proof`; only **agents** are scored, never
> natural persons; "100%" means **evidence coverage**, not impenetrability; the
> k0nsult.cloud engine stays hidden (`/api/*` is the only boundary); **No Password
> Custody** (this repo carries no keys/secrets).

Each step is labelled with what it produces under `claim ≤ proof`:
**DOWÓD** (re-derivable artefact), **GAP** (planned, phrased as future),
**NARRACJA** (framing/positioning — must be labelled). See
[`k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md`](../k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md).

`<cc>` below = the ISO 3166-1 alpha-2 country code of the target jurisdiction
(e.g. `de`, `fr`, `es`), lower-case.

---

## 0. Preconditions

- [ ] You have read the doctrine block above and the `claim ≤ proof` SPEC.
- [ ] The target jurisdiction is identified and its ISO code `<cc>` fixed.
- [ ] You have local checkouts of the sibling repos referenced here:
      `k0nsult-country-pl`, `k0nsult-eu-shield`, `k0nsult-tools`,
      `k0nsult-ai-truth-core`.
- [ ] You understand that **no step in this checklist publishes anything** — publication,
      external submission, and signing are irreversible acts that are **human-gated**
      (operator sign-off), never performed by tooling. *(NARRACJA → the act itself is a GAP
      until an operator performs it.)*

---

## 1. Create the repository skeleton

- [ ] Copy this template into a new repo `k0nsult-country-<cc>`.
- [ ] Keep the four top-level files: `LICENSE`, `NOTICE`, `README.md`, and (generated later)
      `sbom.json`.
- [ ] Create an empty `surfaces/` directory (its contract is in `surfaces/README.md`).
- [ ] In `NOTICE`, replace the Poland scope block with the `<cc>` scope: list the surfaces
      this instance actually ships, and keep the closing lines verbatim:
      *NOT INCLUDED: the proprietary k0nsult.cloud engine … personal data … secrets/keys*;
      and *DOCTRINE: claim <= proof; Human Override Always; agents-not-people.*
- [ ] In `README.md`, state clearly that this is an **instance** of the per-country pattern
      (Poland is the reference), not a second reference. *(NARRACJA — positioning.)*

**Produces:** repo skeleton (NARRACJA until surfaces + SBOM exist).

---

## 2. Map the surfaces (copy → adapt)

For each reference page in `k0nsult-country-pl/surfaces/`, create the `<cc>` equivalent.
The naming contract and per-surface intent are defined in `surfaces/README.md`.

- [ ] `<cc>-panstwo.html` ← from `ai-truth-panstwo.html` — state-level oversight surface.
- [ ] `<cc>-kraj.html` ← from `ai-truth-kraj.html` — country overview / entry surface.
- [ ] `<cc>-prezydent.html` ← from `ai-truth-prezydent.html` — head-of-state track
      (**adapt to the local constitutional system** — see step 4; if the office does not
      exist as in PL, rename to the correct institution rather than forcing a president).
- [ ] `<cc>-min-cyfryzacji.html` ← from `ai-truth-min-cyfryzacji.html` — the digital
      ministry / AI-Act competent authority for `<cc>`.
- [ ] `<cc>-ranking-organy.html` (+ `-en`) ← from `ai-truth-ranking-organy(.|-en.)html` —
      authority ranking.
- [ ] `<cc>-ranking-<local>.html` (+ `-en`) ← from `ai-truth-ranking-jst-kas(.|-en.)html` —
      **sub-national bodies**. Replace the PL-specific `jst-kas` slug with the target's
      sub-national tier (e.g. DE: `laender`, FR: `regions`, ES: `ccaa`).

While adapting each page:
- [ ] Keep the head/meta/OG/JSON-LD skeleton; update `lang`, `title`, `canonical`,
      `og:url`, breadcrumb slugs, and `inLanguage` to the `<cc>` values.
- [ ] Keep the `--phosphor` / mono terminal styling — it is the shared visual identity.
- [ ] **Do not** copy any engine logic. Pages call `/api/*`; they do not embed it.

**Produces:** the `surfaces/<cc>-*.html` set (each is NARRACJA/DOWÓD depending on the
registry data it carries — see step 3).

---

## 3. Wire the authority / DPA registry from eu-shield

National surfaces list **institutional** contacts (competent authorities), never people.

- [ ] Pull the `<cc>` authority contacts from the EU regulators registry in
      [`k0nsult-eu-shield/surfaces/ai-truth-regulatorzy-eu.html`](../k0nsult-eu-shield/surfaces/ai-truth-regulatorzy-eu.html)
      — it holds public DPA / AI-Office / market-surveillance contacts for EU27.
- [ ] Populate `<cc>-min-cyfryzacji.html` and the ranking pages with:
      the competent **DPA**, the **AI Act** market-surveillance / notifying authority,
      the **AI-Office** national contact point, and any regulatory **sandbox** (art. 57)
      channel — all as institutional registry data.
- [ ] For each contact, record a resolvable public source (official register / gazette URL).
      A contact **with** a resolvable source is **DOWÓD**; one you intend to confirm later
      is **GAP** (label it, phrase it as pending) — never present an unconfirmed contact
      as verified.
- [ ] Cross-check that every authority row maps to the EU-level view in
      `k0nsult-eu-shield` (`ai-truth-ranking-ue.html` / `ai-truth-mapa-ue.html`) so the
      national and EU layers agree.

**Produces:** registry rows classified DOWÓD (with source) or GAP (pending) — the SPEC
forbids an unsourced row labelled DOWÓD.

---

## 4. Adapt the constitutional / institutional system

The PL reference assumes a specific state structure (president, ministry of digital affairs,
JST + KAS sub-national tiers). Do not force it onto `<cc>`.

- [ ] Identify the target's **head-of-state / executive** arrangement and rename the
      `prezydent` track accordingly (e.g. federal chancellor + president, monarch +
      government, semi-presidential). Keep one surface per genuine institution; drop
      surfaces for institutions that do not exist.
- [ ] Identify the **competent AI Act authority** (it may be the DPA, a telecoms/market
      regulator, or a new AI office) and point `min-cyfryzacji` at the real body — the
      filename slug may stay, the content must be correct.
- [ ] Identify the **sub-national tiers** and set the `ranking-<local>` slug + content to
      match the real administrative geography.
- [ ] Record the mapping decisions in `README.md` (a short "PL → `<cc>` mapping" table),
      so the adaptation is auditable. *(NARRACJA — labelled positioning/decision log.)*

**Produces:** an auditable institution map (NARRACJA) + corrected surface set.

---

## 5. PII-exclusion test (agents-not-people) — MANDATORY GATE

No surface may carry personal data of natural persons. Institutional contacts (an office,
its generic inbox, its register entry) are **not** PII; a named official's private data is.

- [ ] Scan every `surfaces/<cc>-*.html` for: personal names tied to private data, private
      emails/phones, national ID / tax numbers of individuals, dates of birth, home
      addresses, or any per-person score. **Score agents/institutions, never persons.**
- [ ] Confirm **No Password Custody**: no keys, tokens, secrets, or credentials anywhere
      in the repo.
- [ ] Confirm there is **no engine code** — only surfaces and `/api/*` calls.
- [ ] Record the result as a DOWÓD: the command run + its output (empty match set), e.g.
      a repo-wide grep for the PII patterns above returning zero hits on people-level data.
- [ ] If any hit is a genuine institutional contact, keep it and note why it is registry
      data, not PII (mirror the wording already used in `k0nsult-country-pl/NOTICE`).

> This gate is **blocking**: the layer is not instantiable until the PII-exclusion test is
> a re-derivable DOWÓD (someone else can re-run the scan and get the same empty result).

**Produces:** DOWÓD — a re-runnable exclusion scan with an empty people-level result set.

---

## 6. Generate the SBOM

Supply-chain inventory is a CycloneDX-lite JSON, one component per file with a SHA-256 hash,
generated by the zero-dependency tool in `k0nsult-tools`.

- [ ] From the repo root, run:
      ```bash
      node ../k0nsult-tools/sbom.mjs --root . --out sbom.json
      ```
- [ ] Verify `sbom.json` lists **every** shipped file (all `surfaces/<cc>-*.html`,
      `LICENSE`, `NOTICE`, `README.md`, `surfaces/README.md`, this checklist) each with a
      SHA-256 hash and `k0nsult:surface = open-source`.
- [ ] Confirm the `k0nsult:stats` property reflects the real file counts for this instance.
- [ ] Re-running the generator on an unchanged tree must yield identical hashes — that
      reproducibility is what makes the SBOM a **DOWÓD** rather than a self-declared list.

**Produces:** DOWÓD — `sbom.json` (re-derivable, SHA-256-pinned inventory).

---

## 7. Pre-publication review (human-gated — not performed here)

- [ ] Every published claim on every surface is classified DOWÓD / GAP / NARRACJA and
      labelled; no DOWÓD lacks a resolvable source (SPEC hard rule 1).
- [ ] The PII-exclusion DOWÓD (step 5) and the SBOM DOWÓD (step 6) are both present.
- [ ] `LICENSE` (Apache-2.0, patent grant §3) and the `<cc>`-scoped `NOTICE` are in place.
- [ ] **Publication / external submission / signing** is left to the operator. Tooling
      counts and labels; it never authorises the irreversible act. *(GAP until an operator
      signs off — phrase any "published" claim as future until then.)*

---

## Instantiation summary (fill in per instance)

| Item | State |
|---|---|
| Country code `<cc>` | … |
| Surfaces shipped | … |
| DPA / AI-Act authority mapped | DOWÓD / GAP |
| Institution map recorded in README | yes / no |
| PII-exclusion test | DOWÓD (empty people-level result) / **BLOCKED** |
| SBOM generated & reproducible | DOWÓD / GAP |
| Publication | GAP until operator sign-off |

---

## License
Apache-2.0 (explicit patent grant, Section 3). See `LICENSE` and `NOTICE`.
