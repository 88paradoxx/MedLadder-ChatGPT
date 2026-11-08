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


## Feature-branch implementation update — topic SEO batch 2 (2026-11-08)

### Scope

Updated the next 100 topic pages (topics 101–200 in the priority queue) on `seo/topic-pages-batch-2-2026`. This batch follows the first 100-topic optimization commit on `seo/topic-pages-batch-1-2026` and is isolated from production.

### Metadata changes

- Rewrote page titles around the specific topic and NEET PG, FMGE and INI-CET search intent.
- Rewrote meta descriptions to remove truncated copy and give each topic a distinct, relevant summary.
- Synchronized Open Graph and Twitter titles/descriptions with the page metadata.
- Synchronized the Quiz JSON-LD name and description.
- Preserved canonical URLs, question/answer content, explanations, page body, navigation and scripts.

### Updated pages

- `subjects/biochemistry/enzyme-kinetics-and-regulation-of-activity/index.html`
- `subjects/biochemistry/enzymes-mechanism-of-action-clinical-importance/index.html`
- `subjects/biochemistry/fat-soluble-vitamins/index.html`
- `subjects/biochemistry/fatty-acid-oxidation-and-ketogenesis/index.html`
- `subjects/biochemistry/glycogen-metabolism-and-glycogen-storage-disorders/index.html`
- `subjects/biochemistry/glycolysis-and-gluconeogenesis/index.html`
- `subjects/biochemistry/hmp-shunt-pathway-fructose-galactose-metabolism/index.html`
- `subjects/biochemistry/krebs-cycle/index.html`
- `subjects/biochemistry/lipids-basics/index.html`
- `subjects/biochemistry/metabolism-of-acylglycerols-and-sphingolipids/index.html`

### Validation

- All 100 HTML files were fetched from the feature branch before editing.
- Metadata tags and Quiz JSON-LD fields were updated in each file.
- Title and description lengths were checked during editing; title target ≤68 characters and description target ≤160 characters.
- Existing canonical URLs and all non-metadata page content were left unchanged by the transformation.
- Source-level validation only; no live deployment, browser test, Search Console URL Inspection or schema validator was run.

### Status

Committed to `seo/topic-pages-batch-2-2026` only. No merge or production deployment performed.


## Feature-branch implementation update — topic SEO batch 3 (2026-11-08)

### Scope

Updated topic pages 201–300 on `seo/topic-pages-batch-3-2026`, branching from the completed batch 2 feature branch. This remains isolated from production.

### Metadata changes

- Rewrote topic titles for NEET PG, FMGE and INI-CET search intent.
- Rewrote topic-specific meta descriptions and removed inherited/truncated generic copy.
- Synchronized Open Graph and Twitter titles/descriptions.
- Synchronized Quiz JSON-LD name and description.
- Preserved canonical URLs, question banks, answers, explanations, page body, scripts and navigation.

### Updated pages

- `subjects/biochemistry/mixed-miscellaneous-topics/index.html`
- `subjects/biochemistry/molecular-genetics-recombinant-dna-genomic-technology/index.html`
- `subjects/biochemistry/porphyrins-and-bile-pigments/index.html`
- `subjects/biochemistry/protein-structure-and-function/index.html`
- `subjects/biochemistry/regulation-of-gene-expression/index.html`
- `subjects/biochemistry/rna-synthesis-processing-and-modification/index.html`
- `subjects/biochemistry/urea-cycle-and-its-disorders/index.html`
- `subjects/dermatology/acne-rosacea-and-others/index.html`
- `subjects/dermatology/anatomy-physiology-of-skin/index.html`
- `subjects/dermatology/arthropod-and-parasitic-infections/index.html`
- `subjects/dermatology/bacterial-infections/index.html`
- `subjects/dermatology/connective-tissue-disorders/index.html`
- `subjects/dermatology/dermatitis/index.html`
- `subjects/dermatology/dermatopathology-of-skin-lesions/index.html`
- `subjects/dermatology/disorders-of-hair-and-nails/index.html`
- `subjects/dermatology/disorders-of-skin-pigmentation/index.html`
- `subjects/dermatology/fungal-and-protozoal-infections/index.html`
- `subjects/dermatology/genodermatoses-nutritional-disorders/index.html`
- `subjects/dermatology/mixed-miscellaneous-topics/index.html`
- `subjects/dermatology/morphology-and-investigations-of-skin-lesions/index.html`
- `subjects/dermatology/mycobacterial-infections/index.html`
- `subjects/dermatology/non-syphilitic-sexually-transmitted-diseases/index.html`
- `subjects/dermatology/papulosquamous-disorders/index.html`
- `subjects/dermatology/psoriasis/index.html`
- `subjects/dermatology/reactive-skin-diseases-and-drug-eruptions/index.html`
- `subjects/dermatology/skin-malignancies/index.html`
- `subjects/dermatology/syphilis/index.html`
- `subjects/dermatology/systemic-diseases-and-skin/index.html`
- `subjects/dermatology/urticaria-angioedema/index.html`
- `subjects/dermatology/vesiculobullous-diseases/index.html`
- `subjects/dermatology/viral-infections/index.html`
- `subjects/ent/adenoids/index.html`
- `subjects/ent/anatomy-and-physiology-of-larynx/index.html`
- `subjects/ent/anatomy-and-physiology-of-pharynx/index.html`
- `subjects/ent/anatomy-of-external-ear/index.html`
- `subjects/ent/anatomy-of-facial-nerve/index.html`
- `subjects/ent/anatomy-of-inner-ear/index.html`
- `subjects/ent/anatomy-of-mastoid/index.html`
- `subjects/ent/anatomy-of-middle-ear/index.html`
- `subjects/ent/anatomy-of-nose/index.html`
- `subjects/ent/anatomy-of-paranasal-sinuses/index.html`
- `subjects/ent/audiometric-tests-and-special-tests-of-hearing/index.html`
- `subjects/ent/cholesteatoma-and-types-of-csom/index.html`
- `subjects/ent/congenital-anomalies-of-nose-and-disease-of-nose/index.html`
- `subjects/ent/csom-treatment-and-complications/index.html`
- `subjects/ent/disorder-of-nasal-septum-and-nasal-polyposis/index.html`
- `subjects/ent/disorders-of-external-ear/index.html`
- `subjects/ent/embryology-of-ear-and-malformations/index.html`
- `subjects/ent/epistaxis/index.html`
- `subjects/ent/eustachian-tube/index.html`
- `subjects/ent/facial-nerve-disorders/index.html`
- `subjects/ent/laryngeal-carcinoma/index.html`
- `subjects/ent/meniere-s-disease/index.html`
- `subjects/ent/mixed-miscellaneous-topics/index.html`
- `subjects/ent/nasopharyngeal-angiofibroma/index.html`
- `subjects/ent/nasopharyngeal-carcinoma/index.html`
- `subjects/ent/otosclerosis/index.html`
- `subjects/ent/physiology-of-hearing-and-tuning-fork-tests/index.html`
- `subjects/ent/physiology-of-nose-and-paranasal-sinuses/index.html`
- `subjects/ent/rhinitis/index.html`
- `subjects/ent/sinusitis-and-its-complication/index.html`
- `subjects/ent/stridor-and-congenital-conditions-of-larynx/index.html`
- `subjects/ent/tonsils/index.html`
- `subjects/ent/trauma-of-nose-and-face/index.html`
- `subjects/ent/tumors-of-ear/index.html`
- `subjects/ent/tumors-of-nose-and-pns/index.html`
- `subjects/ent/voice-and-speech-disorders/index.html`
- `subjects/forensic-medicine/alcohol-poisoning/index.html`
- `subjects/forensic-medicine/bns-bnss-and-bsa/index.html`
- `subjects/forensic-medicine/childhood-violence-infanticide-and-starvation/index.html`
- `subjects/forensic-medicine/corrosives-and-asphyxiants/index.html`
- `subjects/forensic-medicine/death-and-post-mortem-changes/index.html`
- `subjects/forensic-medicine/drowning/index.html`
- `subjects/forensic-medicine/fingerprint-and-tattoos/index.html`
- `subjects/forensic-medicine/firearm-injuries-and-blast-injuries/index.html`
- `subjects/forensic-medicine/inorganic-irritants-metallic-and-non-metallic/index.html`
- `subjects/forensic-medicine/mechanical-asphyxia/index.html`
- `subjects/forensic-medicine/mechanical-injuries/index.html`
- `subjects/forensic-medicine/medico-legal-autopsy/index.html`
- `subjects/forensic-medicine/mixed-miscellaneous-topics/index.html`
- `subjects/forensic-medicine/organic-irritants-plant-and-animal-poisons/index.html`
- `subjects/forensic-medicine/organophosphorus-poisoning/index.html`
- `subjects/forensic-medicine/poisoning-general-considerations/index.html`
- `subjects/forensic-medicine/race-sex-and-stature-determination/index.html`
- `subjects/forensic-medicine/regional-injuries/index.html`
- `subjects/forensic-medicine/sexual-offences-and-abortion/index.html`
- `subjects/forensic-medicine/skeletal-and-dental-age-determination/index.html`
- `subjects/forensic-medicine/thermal-injuries/index.html`
- `subjects/medicine/antiphospholipid-antibody-syndrome/index.html`
- `subjects/medicine/bronchiectasis-and-lung-abscess/index.html`
- `subjects/medicine/cardiomyopathy-and-myocarditis/index.html`
- `subjects/medicine/cerebrovascular-disease/index.html`
- `subjects/medicine/clinical-hypotheses/index.html`
- `subjects/medicine/cranial-nerve-disorders/index.html`
- `subjects/medicine/cushing-syndrome/index.html`
- `subjects/medicine/dermatomyositis-and-related-disorders/index.html`
- `subjects/medicine/diagnosis-of-cardiovascular-disorders/index.html`
- `subjects/medicine/disorders-of-anterior-pituitary/index.html`
- `subjects/medicine/dvt-and-pulmonary-embolism/index.html`
- `subjects/medicine/evaluation-of-jaundice/index.html`

### Validation and limitations

- Each of the 100 source HTML files was fetched from the feature branch before its metadata was changed.
- Metadata replacements required the expected existing tags and Quiz JSON-LD fields; a missing field would have stopped the batch.
- Title target was ≤68 characters and description target ≤160 characters.
- No live deployment, browser test, Lighthouse run, Search Console URL Inspection or external schema validation was performed.
- Keyword targeting is based on topic relevance and exam intent, not verified Search Console volume data.

### Status

Committed to `seo/topic-pages-batch-3-2026` only. No merge or production deployment performed.
