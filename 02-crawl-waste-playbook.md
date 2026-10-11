# Crawl Waste Playbook

How to find the machine-generated URLs that eat crawl budget, confirm the cause, and fix it safely. Written from a WordPress case, but the method applies to any CMS.

## 1. The symptom

A site with about 200 real pages had roughly 190 junk URLs in Search Console, while dozens of real pages that had earned clicks sat in "Discovered, currently not indexed" and were never crawled.

Typical junk sources:

| Source | Example pattern | Search Console bucket |
|---|---|---|
| Malformed pagination | `/blog/paged-2/2/` | Alternate page with canonical, Crawled not indexed |
| Author archive permutations | `/author/name/page/3/` | Crawled not indexed |
| Payment plugin AJAX endpoints | `/?wc-ajax=...` | Crawled not indexed |
| RSS feeds | `/feed/` | Crawled not indexed, Excluded by noindex |
| Truncated or broken slugs | `/services/content-` | Crawled not indexed |

## 2. Diagnose before you fix

1. Export all six Page Indexing categories from Search Console.
2. Group URLs by pattern, not one by one.
3. Check which junk URLs have any clicks or impressions. If none, blocking loses nothing.
4. Form a hypothesis and list the competing ones. Do not propose a fix until the cause is confirmed.

### Example: `/paged-N/M/` pagination

Two numbers: the first only `2` or `3`, the second ranging up to the page count. Real pagination (`/page/2/`) also existed and worked.

**Cause (confirmed by reproduction):** a page-builder defect. When an archive template contains more than one Posts module, the builder registers per-module pagination rules, so the first number is the module index and the second is that module's page number.

Competing hypotheses to exclude: a custom rewrite rule in a snippets plugin, a pagination or infinite-scroll plugin, a legacy theme function. Test by inspecting rewrite rules and active snippets read-only, grepping the theme for `paged-` and `add_rewrite_rule`, then reproducing on a clean staging install. Reproduction on a clean install with no custom code settles it.

## 3. Fix options

**A. Prevent generation (best for a new build).** Design archive templates with exactly one Posts module bound to the main query. Pagination resolves to `/page/N/` and the junk rule never fires. Free if decided before the build.

**B. Block crawling via robots.txt (interim, reversible).**

```
Disallow: /*/paged-*
Disallow: /*wc-ajax=
```

Use robots.txt for crawl budget and `noindex` for index control. Safe when the URLs are already non-indexed, canonicalised and have zero clicks. Verify the pattern against your full URL list so it cannot match a legitimate URL. robots.txt affects crawlers only: checkout and cart JavaScript keep working. Rollback is deleting the line.

**C. Remove or restrict the rewrite rules.** Only after the cause is confirmed and nothing legitimate depends on them. Higher risk: test on staging first.

Recommended: A on the new build plus B on production while you rebuild.

## 4. Author archives are an architecture decision

| Option | Effect |
|---|---|
| Disable in your SEO plugin | Removes URLs, loses any real traffic they hold |
| Noindex, keep accessible | Stops indexing, does not save crawl budget |
| Build them properly with bios | Supports founder and author authority |

For consultancies that run on personal authority, build one proper author page and retire the rest.

## 5. Server speed is crawl budget

Google lowers crawl rate on slow hosts. A site answering in about 2 seconds with a 700 KB homepage is crawled materially less than one answering in 300 ms. A heavy plugin stack is both a Core Web Vitals problem and a crawl throttle.

## 6. Getting stranded real pages crawled

Clearing waste is necessary but not sufficient.

1. Clear the crawl waste (section 3).
2. **Relink the orphans.** Google will not prioritise a page nothing links to. This is the highest-leverage action and changes no URL.
3. Check sitemap coverage. Many orphans are missing from the sitemap.
4. Request indexing for the top 10 to 15 pages by value in URL Inspection.
5. Improve response time.

## 7. Risk register template for any production change

- **What changes:** the exact lines.
- **Why:** the outcome in pages and clicks.
- **Evidence:** the exports and dates.
- **SEO risk:** which URLs are affected and what they earn today.
- **What could be lost:** usually the risk is an over-broad pattern.
- **How it is tested:** pattern checked against the full URL list, key flows tested with the rule on, robots.txt tester before and after.
- **Rollback:** one step, with no residue.
- **Monitoring:** Page Indexing weekly. Expect movement over 2 to 6 weeks, not days.
