# URL Disposition Framework

A repeatable, auditable way to give every URL a disposition during a migration or cleanup, so classification is mechanical and defensible instead of ad hoc.

## 1. Disposition vocabulary

| Disposition | Meaning | Content action |
|---|---|---|
| PRESERVE | Same slug survives | Rebuild to standard |
| PRESERVE-URL + REWRITE | Keep the slug (it holds equity), re-angle the content | Rewrite |
| 301 | Slug retired, redirect to a different surviving URL | Rebuild or rewrite at the new slug |
| CONSOLIDATE | Merge thin or overlapping pages into one canonical, 301 the rest | Merge |
| REBUILD (new) | On-strategy topic that is unindexed or never crawled | Build new |
| HUB-REVIEW | Affiliate or review pages, kept but consolidated under a siloed hub | Consolidate and rewrite |
| RETIRE to 301 | Remove, redirect to the nearest relevant hub | Drop |
| RETIRE to 410 | Remove, return 410 Gone (junk, no equity, no destination) | Drop |
| KEEP-noindex | Must exist but must not rank (cart, checkout, utility) | Rebuild and noindex |
| HOLD | Needs an explicit per-URL decision | None |

## 2. Signals

Use primary data only.

- **Indexation:** indexed, crawled not indexed, discovered never crawled, noindex, 404, alternate canonical.
- **Organic earnings:** clicks and impressions over 16 months (Search Console Pages export).
- **External links:** often 90% or more sit on the homepage, with deep pages near zero.
- **Internal links:** nav and footer linked versus orphaned.
- **On strategy:** matches your positioning, services and target industries.
- **Junk:** pagination variants, feeds, AJAX endpoints, sitemaps, author archives, malformed slugs.
- **Existing redirect:** already covered by a live rule.

**Equity test (set your own threshold):** a URL has equity if it earned at least N clicks over 16 months, or is currently indexed and ranking. One working setting: 8 clicks, or indexed with average position of about 30 or better with impressions.

## 3. Classification rules (first match wins)

- **R1 Junk technical URL:** RETIRE to 410. Do not recreate.
- **R2 Already has a redirect:** carry it over, flattened so it points straight at the final live target.
- **R3 Transactional or functional** (cart, checkout, account, search): KEEP-noindex.
- **R4 Core site page** (home, about, contact, portfolio, blog index, live service pages): PRESERVE.
- **R5 Real earner and on strategy:** PRESERVE and rebuild to standard.
- **R6 Real earner but off positioning:** PRESERVE-URL and REWRITE toward positioning, or CONSOLIDATE if part of a thin cluster. Never retire live equity.
- **R7 On strategy but unindexed or never crawled:** REBUILD under the new architecture. Keep the old slug if clean, otherwise single-hop 301 to the new equivalent.
- **R8 Off positioning and no equity:** RETIRE to 301 if it has inbound internal links, RETIRE to 410 if it has none.
- **R9 Thin or near-duplicate cluster:** CONSOLIDATE into one canonical and 301 the rest.
- **R10 Default or uncertain:** HOLD. Never guess.

## 4. Guardrails

1. **Evidence required.** No disposition without a data signal. Unknown means HOLD.
2. **Mandatory HOLD for money pages.** Any position-1 page or top earner gets individual sign-off whatever the rule says.
3. **Every 301 is single hop.** Flatten chains. No redirect points at a URL that itself redirects.
4. **Deconflict new slugs** against live URLs before use.
5. **No production execution during classification.** Dispositions are planned in the register and redirect map, and execute only at an approved cutover.
6. **No bulk operations.** No mass redating, publishing or redirect passes. Apply in reviewed batches.
7. **Staging stays noindex** until launch.

## 5. Workflow

1. Join the URL register to the primary data (clicks, coverage buckets, links, existing redirects).
2. Auto-assign R1 to R9 where a rule matches cleanly. Everything else goes to HOLD.
3. Produce a proposed-disposition sheet: URL, disposition, content action, rule fired, evidence, target (for 301 and CONSOLIDATE).
4. Review and approve in batches. Decide HOLD and money pages individually.
5. Only approved rows enter the tested redirect map. Do not schedule migration until the map is complete and tested.

## 6. Affiliate and review clusters

Keep them under a siloed hub (for example `/resources/`), separate from service pages. Consolidate thin single-product reviews into comparison pages. Put `rel="sponsored"` on every affiliate link, add FTC-style disclosure, meet the build standard, and decide indexation deliberately because topical dilution is the trade-off.
