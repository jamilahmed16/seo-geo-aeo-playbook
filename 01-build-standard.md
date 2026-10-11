# SEO, GEO and AEO Build Standard

One line: build pages a search engine can crawl perfectly, a human can convert on, and an LLM can quote verbatim.

## 0. Gates (all must pass before a page ships)

1. **Semantic HTML.** Exactly one `<h1>`. Logical `<h2>`/`<h3>` order. Real landmarks (`<header>`, `<main>`, `<nav>`, `<footer>`). Never use heading levels for styling. Page builders must output real tags, not div soup.
2. **Unique title and meta description.** Written per page, not templated. Primary topic near the front of the title, brand as suffix.
3. **Self-referencing canonical**, absolute and correct.
4. **Indexability correct.** Staging stays `noindex` until launch and is verified. Production ships `index,follow` unless a URL is deliberately excluded.
5. **Valid structured data** (section 3) with zero errors in a rich results test.
6. **Core Web Vitals budget met** (section 2).
7. **Answer-first.** The page's core question is answered in the first one or two sentences.
8. **Internal links in and out.** No orphan pages.
9. **Design and template pre-flight** passes (for example one Posts module per archive template, see the crawl waste playbook).

## 1. Technical SEO baseline

- **URLs:** lowercase, hyphenated, short, no dates, no stop-word stuffing, no auto-generated pagination variants. Never change a live URL without a per-URL decision.
- **Headings:** H1 is the page's promise. H2s are scannable sections. Use question-shaped H2/H3 where a real query exists.
- **Titles:** 60 characters or fewer where possible. Pattern: `Primary Topic | Brand`.
- **Meta descriptions:** 140 to 160 characters. Benefit, differentiator, soft call to action. Never ship without one.
- **Images:** descriptive alt text, lazy loading, WebP or AVIF, explicit width and height, compressed.
- **XML sitemap:** only indexable canonical URLs. No junk or paginated variants.
- **robots.txt:** allow real content. Disallow proven waste patterns only after testing against your full URL list.
- **Breadcrumbs:** visible, with `BreadcrumbList` schema on all non-home pages.
- **Pagination:** standard `/page/N/` only.
- **Hreflang:** only when you deliberately target multiple locales. Do not add speculatively.

## 2. Performance budget

Heavy stacks throttle crawling as well as conversions: slow servers get crawled less.

- LCP under 2.5s, INP under 200ms, CLS under 0.1 (field data, mobile).
- HTML weight under about 150 KB per page before assets where feasible.
- Page cache on, critical CSS, deferred non-critical JavaScript, `font-display: swap`.
- No new plugin without auditing its front-end cost.
- Fonts: self-host or preconnect, subset where possible.

## 3. Structured data by template

Use JSON-LD, validated, and matching visible content. No schema-only claims.

| Template | Required schema |
|---|---|
| Sitewide | `Organization` (name, `legalName`, logo, `sameAs`, `contactPoint`), `WebSite` |
| Home | Organization, WebSite, optional `Service` summary |
| Service pages | `Service` (provider, areaServed, serviceType), `BreadcrumbList`, `FAQPage` |
| Industry pages | `Service` or `WebPage`, `FAQPage`, `BreadcrumbList` |
| Blog posts | `BlogPosting` (real author `Person`, real `datePublished` and `dateModified`, publisher, image), `BreadcrumbList`, `FAQPage` where relevant |
| About | `AboutPage` plus `Person` with credentials and `sameAs` |
| Contact | `ContactPage` plus `Organization.contactPoint` |
| Author | `ProfilePage` plus `Person` |

**Entity discipline:** use one brand string everywhere. Mixed variants (Brand, Brand Ltd, Brand Limited) weaken entity resolution. One name, one logo, one Organization node, consistent `sameAs`. Authors are real people with verifiable credentials.

## 4. The GEO, AEO and LLM layer

Most SEO builds skip this. It is where AI citations come from.

- **Answer-first structure.** AI engines lift the first clear answer. State it in plain, extractable language.
- **Question-shaped headings** that mirror real prompts and queries.
- **FAQ blocks** on every commercial page: 3 to 6 genuine Q&As, marked up with `FAQPage`, each answer self-contained.
- **Extractable facts:** definitions, steps and comparisons as clean declarative sentences, tables and lists. Real facts only.
- **Entity clarity:** say who you are, what you do and for whom, in machine-parseable prose near the top of key pages.
- **`llms.txt`** at the site root: a curated map of key pages and a one-paragraph description. Set a deliberate robots policy for AI crawlers (GPTBot, CCBot, PerplexityBot, Google-Extended).
- **Freshness:** update `dateModified` only when content genuinely changes. Never bulk redate.
- **Citations and authority:** cite authoritative sources, earn third-party mentions. AI engines weight corroborated entities.
- **Server-rendered content.** Many LLM crawlers do not execute JavaScript.

## 5. Internal linking

Orphaned pages are the most common hidden problem: in one audit, over 75% of URLs known to Search Console had zero internal links, and real pages with traffic were never crawled.

- Every page links to related pages and is linked from related pages.
- Nav, footer and breadcrumbs carry real structure.
- Topic clusters: hubs link to supporting posts and back.

## 6. Per-template QA checklist

- [ ] One H1, valid heading order, landmarks present
- [ ] Title 60 characters or fewer, unique meta description
- [ ] Canonical and indexability correct
- [ ] Schema present, validates, matches visible content
- [ ] Answer-first opening, at least one FAQ block with `FAQPage`
- [ ] Internal links in and out, breadcrumb
- [ ] Images have alt text, sizes and compression
- [ ] Core Web Vitals budget met, HTML under about 150 KB
- [ ] No auto-generated pagination URLs
- [ ] Real content, real numbers, no invented metrics
