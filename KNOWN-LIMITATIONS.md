# KNOWN LIMITATIONS — read before relying on this

**Status: REFERENCE / EXPERIMENTAL.** This repository is a **scaffold to be instantiated**, not a finished product. Forking it gives you a starting structure, not a working national layer.

This file is published deliberately. Doctrine: **claim ≤ proof** — we document where this is
incomplete rather than overclaiming. The commons underwent internal adversarial review rounds
plus an external review (RSpace); findings and fixes are public in the git history.

## Known open weaknesses
- **Every country-specific value is a placeholder.** Nothing here states a fact about any country.
  You must substitute all placeholders and then verify each value against primary sources.
- **No data, no automation.** There is no data pipeline, no refresh mechanism and no live API —
  those are engine-side and out of scope.
- **Legal fit is yours to establish.** Whether a given surface is appropriate for your
  jurisdiction, and how it maps to national implementing law, is not something this scaffold decides.
- **Structure follows the Polish reference** (`k0nsult-country-pl`) and may need reshaping for a
  different institutional order.

## What IS solid
- Templates are self-contained: no external dependencies, no CDN, no trackers — they work offline
  after a fork. Apache-2.0 with the explicit patent grant.

## How to help
Try to break it and open an issue or PR with a reproducing input. That is exactly how the
limitations above were surfaced.
