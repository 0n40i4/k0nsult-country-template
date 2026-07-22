# surfaces/ — templates and naming contract (`<cc>-*.html`)

This directory holds **forkable HTML templates** for the national transparency surfaces of one
jurisdiction, plus the contract an instance must follow. Each template is a self-contained,
evidence-first page: **no CDN, no web fonts, no external scripts, no trackers** — it renders offline
straight after a fork. Live data, where used, is optional progressive enhancement over an `/api/*`
endpoint; if the endpoint placeholder is left unreplaced, the script is a no-op and the static
content stays.

The Polish reference implementation lives in
[`k0nsult-country-pl/surfaces/`](../../k0nsult-country-pl/surfaces/); these templates are the
country-agnostic distillation of it.

> **Doctrine:** `claim ≤ proof`; only **agents/institutions** are scored, never natural persons;
> "100%" = evidence coverage, not impenetrability; **the engine stays hidden**
> (`/api/*` is the only boundary); **No Password Custody** (no keys here).

`<cc>` = the target's ISO 3166-1 alpha-2 code, lower-case (e.g. `de`, `fr`, `es`).

---

## Templates shipped here

| Template file | Rename to | What it is |
|---|---|---|
| `cc-kraj.html` | `<cc>-kraj.html` | Country overview / entry surface: NCA, regulatory sandbox, national implementing law under Regulation (EU) 2024/1689, with cited sources. Static by default; optional dataset endpoint. |
| `cc-panstwo.html` | `<cc>-panstwo.html` | State-level oversight: government office, ministries, central authorities, independent bodies, simplified constitutional graph (inline SVG), link-health. |
| `cc-min-cyfryzacji.html` | `<cc>-min-cyfryzacji.html` | Digital ministry / AI-Act competent authority: policy areas, internal structure, supervised entities, **AI Act authority registry** (NCA / market surveillance / DPA / sandbox / national law), cybersecurity stack, connectors. |
| `cc-ranking-organy.html` | `<cc>-ranking-organy.html` | Authority readiness ranking, national-language variant (**translate the visible text into `{{LANG}}`**). |
| `cc-ranking-organy-en.html` | `<cc>-ranking-organy-en.html` | English twin of the ranking — identical data, English wording. |

`cc-` is a **placeholder for the country code in the filename**. Rename before shipping: no
`cc-*` file may remain in an instance repo, and no un-prefixed page either.

**Surfaces not shipped as templates** (derive them, do not fabricate them):
- `<cc>-prezydent.html` — head-of-state / executive track. Constitutional systems differ too much for
  a meaningful template; start from `cc-panstwo.html` and adapt, or drop the surface if the office
  has no equivalent.
- `<cc>-ranking-<local>.html` (+ `-en`) — sub-national bodies (PL: `jst-kas`; DE: `laender`;
  FR: `regions`; ES: `ccaa`). Copy the two ranking templates and change the slug, headings and rows.

---

## Placeholder inventory

Every token is `{{UPPER_SNAKE_CASE}}` in double braces. **Replacement is mandatory before
publication** — a surviving `{{...}}` on a published page is a defect. Nothing in these templates
asserts a fact about any country; all country-specific values are placeholders.

Verify none survives:

```bash
grep -R -o '{{[A-Z_0-9]*}}' surfaces/ | sort -u    # must be empty before publishing
```

### Identity, language, provenance

| Placeholder | Meaning | Used in |
|---|---|---|
| `{{COUNTRY_NAME}}` | Country name in the page language | all |
| `{{COUNTRY_CODE}}` | ISO 3166-1 alpha-2, upper-case (display) | kraj, ranking ×2 |
| `{{LANG}}` | BCP-47 tag for `<html lang>` / `inLanguage` (e.g. `de`, `fr`) | all |
| `{{LANG_LABEL}}` | Short label of the national language on the EN page's switcher (e.g. `DE`) | ranking-en |
| `{{OG_LOCALE}}` | Open Graph locale (e.g. `de_DE`) | kraj, min-cyfryzacji, panstwo, ranking |
| `{{CANONICAL_URL}}` | Absolute canonical URL of this surface (also `og:url`) | all |
| `{{CANONICAL_URL_EN}}` | Absolute canonical URL of the English ranking twin | ranking ×2 |
| `{{SITE_BASE_URL}}` | Origin of the instance, no trailing slash (breadcrumbs, `isPartOf`) | all |
| `{{PUBLISHER_NAME}}` | Organisation publishing this instance | all |
| `{{PUBLISHER_ID}}` | Its public registry identifier (company/register number) | all |
| `{{SURFACE_ID}}` | Internal surface identifier for the footer version line | all |
| `{{VERSION}}` | Version string of this surface | all |
| `{{SNAPSHOT_DATE}}` | Date the underlying snapshot was taken (ISO `YYYY-MM-DD`) | all |

### AI Act / regulator registry

| Placeholder | Meaning | Used in |
|---|---|---|
| `{{NCA_NAME}}` | National competent authority under the AI Act | kraj, min-cyfryzacji |
| `{{NCA_STATUS}}` | `DESIGNATED` / `PARTIAL` / `NONE` (spelled out, not colour-coded) | kraj |
| `{{NCA_URL}}`, `{{NCA_CONTACT}}`, `{{NCA_SOURCE_URL}}` | Official page, public institutional contact, resolvable source | min-cyfryzacji |
| `{{SANDBOX_STATUS}}` | `OPERATIONAL` / `PLANNED` / `NONE` / `UNCONFIRMED` | kraj, min-cyfryzacji |
| `{{SANDBOX_AUTHORITY}}`, `{{SANDBOX_CONTACT}}`, `{{SANDBOX_SOURCE_URL}}` | Art. 57 sandbox operator, its channel, source | min-cyfryzacji |
| `{{NATIONAL_LAW}}` | National implementing law (title / reference) | kraj, min-cyfryzacji |
| `{{NATIONAL_LAW_STATUS}}` | `IN FORCE` / `ADOPTED` / `DRAFT` / `PARTIAL` / `NONE` | kraj |
| `{{NATIONAL_LAW_SOURCE_URL}}` | Official gazette entry | min-cyfryzacji |
| `{{MARKET_SURVEILLANCE_AUTHORITY}}`, `{{MARKET_SURVEILLANCE_CONTACT}}`, `{{MARKET_SURVEILLANCE_SOURCE_URL}}` | Market-surveillance / notifying authority | min-cyfryzacji |
| `{{DPA_NAME}}`, `{{DPA_URL}}`, `{{DPA_CONTACT}}`, `{{DPA_SOURCE_URL}}` | GDPR supervisory authority | min-cyfryzacji |
| `{{CYBER_AUTHORITY_NAME}}`, `{{CYBER_AUTHORITY_URL}}` | NIS2 competent authority | min-cyfryzacji |
| `{{CSIRT_NAME}}`, `{{CSIRT_URL}}` | National CSIRT | min-cyfryzacji |
| `{{DIGITAL_AUTHORITY_NAME}}`, `{{DIGITAL_AUTHORITY_URL}}` | Digital ministry / competent digital authority | min-cyfryzacji |
| `{{OPEN_DATA_PORTAL_URL}}` | National open-data portal | min-cyfryzacji |

### State structure

| Placeholder | Meaning | Used in |
|---|---|---|
| `{{GOV_PORTAL_URL}}` | Official national government portal | panstwo |
| `{{GOVERNMENT_BODY_NAME}}` | Collective government body (council of ministers equivalent) | panstwo |
| `{{HEAD_OF_GOVERNMENT_OFFICE}}` | Office/chancellery supporting the head of government | panstwo |
| `{{HEAD_OF_GOVERNMENT_TITLE}}`, `{{HEAD_OF_GOVERNMENT_URL}}` | Title of the office (not the person) + its official page | panstwo |
| `{{HEAD_OF_STATE_TITLE}}` | Title of the head of state (not the person) | panstwo |
| `{{INDEPENDENT_BODIES}}` | Short label of bodies outside government control (audit office, central bank, …) | panstwo |
| `{{APPOINTMENT_BASIS}}` | Constitutional basis of the appointment arrow in the graph | panstwo |
| `{{MINISTRY_COUNT}}`, `{{AUTHORITY_COUNT}}` | Counts actually present in the snapshot | panstwo |
| `{{MINISTRY_NAME}}`, `{{MINISTRY_REMIT}}`, `{{MINISTRY_URL}}` | Repeatable ministry card fields | panstwo |
| `{{AUTHORITY_NAME}}`, `{{AUTHORITY_RELATION}}`, `{{AUTHORITY_URL}}` | Repeatable authority row fields | panstwo |
| `{{UNIT_NAME}}`, `{{UNIT_REMIT}}`, `{{UNIT_URL}}` | Repeatable internal-unit row fields | min-cyfryzacji |
| `{{ENTITY_NAME}}`, `{{ENTITY_TYPE}}`, `{{ENTITY_ROLE}}`, `{{ENTITY_URL}}` | Repeatable supervised-entity row fields | min-cyfryzacji |
| `{{MAPPED_COUNT}}`, `{{READY_COUNT}}` | Mapping progress counters | min-cyfryzacji |

### Ranking data

| Placeholder | Meaning | Used in |
|---|---|---|
| `{{DATA_SOURCE}}` | Name of the source dataset the rows are derived from | ranking ×2 |
| `{{DATA_SCOPE}}` | Scope of the public sources examined (years, source types) | ranking ×2 |
| `{{ROW_COUNT}}` | Number of data rows actually in the table | ranking ×2 |
| `{{AUTHORITY_NAME}}`, `{{AUTHORITY_TYPE}}`, `{{AUTHORITY_IMPLEMENTATION}}`, `{{READINESS_SCORE}}`, `{{AUTHORITY_STATUS}}` | Repeatable ranking row fields | ranking ×2 |

### Country page content

| Placeholder | Meaning | Used in |
|---|---|---|
| `{{COUNTRY_NOTES}}` | Free-text note about the country's AI Act status | kraj |
| `{{SOURCE_URL}}` | Repeatable resolvable public source link | kraj |

### Optional endpoints (progressive enhancement)

| Placeholder | Meaning | Used in |
|---|---|---|
| `{{COUNTRY_DATA_ENDPOINT}}` | `/api/*` JSON with `{ countries: [...] }`; unreplaced → static fallback is kept | kraj |
| `{{LINK_HEALTH_ENDPOINT}}` | `/api/*` JSON with `{ generated_at, total, ok, gap, results:[…] }`; unreplaced → script is a no-op | panstwo, min-cyfryzacji |

The endpoint response shapes are documented in a comment directly above each script block.

---

## Rules an instance must follow

- **Prefix every file with `<cc>-`.** No `cc-*` and no un-prefixed page in an instance repo.
  - **Known exception — do not copy it.** The Poland reference instance
    (`k0nsult-country-pl`) predates this rule and ships its surfaces as `ai-truth-*`,
    not `pl-*`. It is kept that way because its filenames are already referenced by
    published routes; renaming them would break live links. Read it as a **content**
    reference, not a **naming** reference. A new instance that copies its filenames
    would violate this rule on day one — raised by external review (round 2).
- **Bilingual pairs**: a ranking page ships a national-language page and an `-en` twin with
  identical data. Diverging rows between twins is a defect.
- **One surface per real institution.** If the target has no equivalent of a reference institution,
  rename or drop the surface — do not fabricate an office. If it has an institution the reference
  lacks, add `<cc>-<slug>.html` following the same skeleton.
- **Slugs track reality.** `min-cyfryzacji` and `ranking-<local>` are placeholders; point them at the
  target's actual competent authority and sub-national tier (checklist, steps 3–4).
- **No external dependencies.** No CDN, no remote fonts, no analytics, no beacons. Only same-origin
  `/api/*` calls, and only as optional enhancement.
- **No engine code.** Surfaces render; they never embed orchestration logic.

## Accessibility (keep it — do not regress it)

- `<html lang="{{LANG}}">` on every page; the `-en` twin is `lang="en"`.
- Every status is spelled out in text (`LIVE`, `GAP`, `DESIGNATED`, …). Colour only reinforces the
  word — **no meaning is carried by colour alone**.
- Tables use `<th scope="col">` and a `<caption>`; wide tables scroll inside their own container.
- Links are underlined, not colour-only; external links carry `rel="noopener"`.
- `<noscript>` explains what is unavailable when JS is off — the static content stays readable.
- Headings are ordered (`h1` → `h2` → `h3`); the sticky sidebar is a labelled `<aside>`.

## Evidence discipline on every surface

- Every published claim is **DOWÓD** (resolvable source/hash), **GAP** (labelled, phrased as future),
  or **NARRACJA** (labelled framing). See
  [`k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md`](../../k0nsult-ai-truth-core/spec/SPEC-claim-le-proof.md).
- A percentage is DOWÓD **only** if its denominator and method are stated; own-ruler percentages are
  NARRACJA. The ranking templates keep both visible on purpose.
- **Agents-not-people:** rankings and scores apply to institutions. No personal data of natural
  persons — the state and authority templates carry an explicit hard rule against publishing names of
  office holders. Institutional contacts (an office, its generic inbox, its register entry) are
  registry data, not PII (mirror the wording in `k0nsult-country-pl/NOTICE`).

## Integration boundary — the engine stays hidden

```
/api/agents        /api/scores        /api/messages
/api/send          /api/federation/roster
```

That contract is the **only** boundary. The orchestration engine behind it is proprietary and is
deliberately **not** in this repo. Implement the contract against your own backend, or serve the
pages statically for their registry content. **Never** inline engine logic into a surface.

## Before shipping

Run the placeholder sweep above, the PII-exclusion test and the SBOM generation described in
[`../INSTANTIATION-CHECKLIST.md`](../INSTANTIATION-CHECKLIST.md) (steps 5–7). Every file in this
directory must appear in the repo `sbom.json` with a SHA-256 hash.
