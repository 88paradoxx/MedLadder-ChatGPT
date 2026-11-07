# MedLadder-pages SEO and deployment audit

**Repository:** 88paradoxx/MedLadder-pages  
**Audit branch:** feature/medladder-pages-audit  
**Base snapshot:** f8ee9418831bcacc86cdb71ef47054ba84e7a5a4  
**Scope:** Read-only source inspection; no production files modified.

## Inventory verified from the recursive Git tree

- 1,779 total tree entries.
- 835 HTML files.
- 759 HTML files under `subjects/` (19 subject landing pages and 740 topic pages).
- 51 HTML files under `pyqs/`.
- Sitemap contains 826 unique URL entries, with no duplicate `<loc>` values.
- Sitemap breakdown: 759 subject/topic URLs, 51 PYQ URLs, and 16 other public URLs.
- Four HTML files are not represented in the sitemap: `404.html`, `admin-mock.html`, `admin-mock/index.html`, and `google588cf65d870b0d74.html`. The 404, admin and Google verification files are expected exclusions; the two admin mock files warrant checking for duplicate route behavior.

## Checks completed

### All 19 subject landing pages

Inspected all 19 subject root `index.html` files. Each sampled page had:
- A non-empty title.
- A meta description (149–161 characters in this sample).
- A self-referencing canonical URL.
- Exactly one H1.
- Four JSON-LD script blocks.
- Open Graph title and Twitter card metadata.
- BreadcrumbList structured data.

### Topic and PYQ samples

Inspected representative topic pages, including Anatomy/Gametogenesis and Medicine/Acid-Base Disorders, plus NEET PG PYQ index and 2025 paper pages. Samples had titles, descriptions, canonical URLs, one H1, structured data and substantial in-page navigation. This is a sample, not a validation of every topic/PYQ page.

### Homepage

The root `index.html` has title, description, canonical and structured data, but its initial `<main>` is an application-loading shell ("Loading your question bank…"). This should be checked with rendered HTML / Google URL Inspection to determine whether meaningful homepage content is reliably available to crawlers without client-side execution.

### Cloudflare deployment configuration

- `wrangler.jsonc` sets the static asset directory to `./dist`.
- The README describes a root-level static export and drag-and-drop upload of the whole folder, without documenting a build/copy step into `dist`.
- This is a potential deployment-path mismatch. Confirm the actual Cloudflare build command/output directory and deployed artifact before changing configuration.
- `_redirects` documents clean URL handling, legacy `.html` redirects, and an APK redirect.
- `wrangler.jsonc` uses `not_found_handling: "404-page"`.
- README notes that certain `.well-known` routes currently fall through to the SPA shell and need a Worker route or valid catalog handling.

### Robots and sitemap

- `robots.txt` allows public crawling, disallows `/supabase/` and `/tools/`, and points to the production sitemap.
- Sitemap has 826 unique entries and 0 duplicate entries.
- All 759 subject/topic HTML pages and 51 PYQ HTML pages are represented in the sitemap based on the repository tree/path mapping.

## Findings and priorities

### P0 — Confirm deployment artifact and routing
Resolve whether Cloudflare Pages deploys the repository root or `dist/`. Verify representative URLs return the intended HTML and unknown paths return HTTP 404, not the SPA shell. Do not change `wrangler.jsonc` until the actual Pages project build settings are confirmed.

### P1 — Validate rendered homepage content
Use Google Search Console URL Inspection / live rendered HTML and a browser test to establish whether the homepage content, headings, links and key text are available to crawlers. If not, add meaningful server/static-rendered homepage content while preserving the app boot sequence.

### P1 — Complete page-level validation
Run a crawler over all 826 sitemap URLs and check HTTP status, canonical destination, title/description uniqueness, H1 count, JSON-LD parse/validation, indexability, internal-link targets and redirect chains. Repository source inspection alone cannot prove live HTTP behavior.

### P2 — Content quality and internal linking
Review all 740 topic pages for genuinely distinct educational content, question counts, overlap/cannibalization, and contextual links. Avoid mass rewriting pages that already have useful unique content.

### P2 — Performance
Measure live mobile Core Web Vitals (LCP, INP, CLS) and identify the largest assets / render-blocking scripts before changing images, CSS or JavaScript.

## Limitations

- No live-site HTTP crawl, Google Search Console inspection, PageSpeed Insights or Core Web Vitals measurement was performed.
- All 19 subject landing pages were checked for basic metadata/schema presence. Topic/PYQ checks were representative samples, not a full per-page content/schema validation.
- No code, sitemap, redirects, headers, deployment configuration or production files were modified.


## Feature-branch implementation update (2026-11-07)

### Files changed

Updated title and meta description tags in 20 topic landing pages:

- Anatomy: abdominal cavity and peritoneum
- Physiology: acid-base balance, renal hormones and micturition reflex
- Biochemistry: amino acid metabolism
- Pharmacology: acid-peptic disorders and inflammatory bowel disease
- Pathology: acute inflammation
- Microbiology: applied microbiology; arboviruses and picornaviruses
- Medicine: acid-base disorders; acute kidney injury
- Surgery: appendix; anus and anal canal
- Obstetrics & Gynaecology: abnormal labour; abortion and medical termination of pregnancy
- Pediatrics: Apgar score and neonatal resuscitation; acyanotic congenital heart diseases
- PSM: analytical epidemiology
- Orthopaedics: basics of fracture and its management
- Ophthalmology: anatomy and development of eye; astigmatism and errors of accommodation
- Dermatology: acne, rosacea and related disorders

### Changes made

- Replaced truncated/overlong titles with concise topic-specific titles.
- Rewrote descriptions to remove ellipses, retain the topic and exam-intent terms, and describe the page's MCQ/explanation value.
- Preserved each page's URL, canonical tag, page body, scripts, structured data, navigation and question content.
- No subject landing page, sitemap, redirect, header, deployment setting or production branch was modified.

### Validation performed

- Re-fetched all 20 modified HTML files from the feature branch after the edits.
- Checked title and description presence and measured character lengths.
- All 20 descriptions are 127–155 characters; none of the updated titles/descriptions contains an ellipsis.
- Checked that each canonical URL remains self-referencing to its existing clean URL.
- This was source-level validation only. No live deployment, browser regression test, Google Search Console test or structured-data validator was run.

### Status

These edits are committed to `feature/medladder-pages-audit` only. The 19 subject landing pages were audited and left unchanged because their sampled metadata and page structure were already sound. Homepage rendered-content validation and full 740-topic/826-URL crawl remain outstanding.
