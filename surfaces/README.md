# surfaces/ — expected structure (`<cc>-*.html`)

This directory holds the **national transparency surfaces** for one jurisdiction. Each
file is a self-contained, evidence-first HTML page that calls the hidden engine only
over `/api/*`. The reference set lives in
[`k0nsult-country-pl/surfaces/`](../../k0nsult-country-pl/surfaces/); this README is the
contract an instance must follow.

> **Doctrine:** `claim ≤ proof`; only **agents** are scored, never natural persons;
> "100%" = evidence coverage, not impenetrability; **the engine stays hidden**
> (`/api/*` is the only boundary); **No Password Custody** (no keys here).

`<cc>` = the target's ISO 3166-1 alpha-2 code, lower-case (e.g. `de`, `fr`, `es`).

## Naming contract

```
surfaces/
  <cc>-panstwo.html            # state-level oversight
  <cc>-kraj.html               # country overview / entry surface
  <cc>-prezydent.html          # head-of-state / executive track (adapt to local system)
  <cc>-min-cyfryzacji.html     # digital ministry / AI-Act competent authority
  <cc>-ranking-organy.html     # authority ranking
  <cc>-ranking-organy-en.html  # …English variant
  <cc>-ranking-<local>.html    # sub-national bodies (PL: jst-kas; DE: laender; …)
  <cc>-ranking-<local>-en.html # …English variant
```

Rules:
- **Prefix every file with `<cc>-`.** No un-prefixed page in an instance repo.
- **Bilingual pairs**: a ranking page ships a national-language page and an `-en` twin
  with identical data. EN twins widen EU reusability.
- **One surface per real institution.** If the target has no direct equivalent of a PL
  institution, rename or drop the surface — do not fabricate an office. If it has an
  institution PL lacks, add a `<cc>-<slug>.html` following the same skeleton.
- **Slugs track reality.** `min-cyfryzacji` / `<local>` are placeholders; set them to the
  target's actual competent authority and sub-national tier (see the instantiation
  checklist, steps 3–4).

## Per-surface intent

| Surface | What it is |
|---|---|
| `<cc>-kraj.html` | Country overview / entry point into the national layer |
| `<cc>-panstwo.html` | State-level oversight surface |
| `<cc>-prezydent.html` | Head-of-state / executive track — **adapt to the local constitutional system** |
| `<cc>-min-cyfryzacji.html` | Digital ministry / AI-Act competent authority + DPA contact |
| `<cc>-ranking-organy(.|-en.)html` | Authority ranking (institutions, never persons) |
| `<cc>-ranking-<local>(.|-en.)html` | Sub-national bodies ranking |

## Page skeleton (keep from the reference)

Each surface keeps the reference structure — copy from the PL page and change only the
locale-specific values:

- `<!DOCTYPE html>` + `<html lang="<cc-lang>">`, mono/`--phosphor` terminal styling
  (shared visual identity — do not restyle).
- Head/meta: `title`, `description`, `canonical`, full Open Graph + Twitter tags, and a
  `schema.org` JSON-LD block with a `BreadcrumbList` — all pointing at the `<cc>` URLs.
- Body: evidence-first content (registry tables, coverage stats, correspondence tracks).

## Integration boundary — the engine stays hidden

Surfaces are **not** the engine. They call a small federation API and render the result:

```
/api/agents        /api/scores        /api/messages
/api/send          /api/federation/roster
```

That contract is the **only** boundary. The orchestration engine behind it
(k0nsult.cloud) is proprietary and is **deliberately not** in this repo. To run the
surfaces against your own backend, implement the API contract; or serve the pages
statically for their transparency/registry content. **Never** inline engine logic into a
surface.

## Evidence discipline on every surface

- Every published claim is one of **DOWÓD** (resolvable source/hash), **GAP** (labelled,
  phrased as future), or **NARRACJA** (labelled framing). See
  [`k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md`](../../k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md).
- A percentage is DOWÓD **only** if its denominator and method are stated; own-ruler
  percentages are NARRACJA.
- **Agents-not-people:** rankings and scores apply to institutions/agents. No personal
  data of natural persons. Institutional contacts are registry data, not PII (mirror the
  wording in `k0nsult-country-pl/NOTICE`).

## Before shipping

Run the PII-exclusion test and generate the SBOM as described in
[`../INSTANTIATION-CHECKLIST.md`](../INSTANTIATION-CHECKLIST.md) (steps 5–6). Every file in
this directory must appear in the repo `sbom.json` with a SHA-256 hash.
