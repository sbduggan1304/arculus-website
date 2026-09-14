# Arculus Funds Management — website

Marketing and disclosure site for an Australian fixed income manager. Two retail funds, three wholesale strategies, an investment management service, research, and the compliance content around them. One maintainer. Everything here is designed to be handed over.

## Source of truth

- **Design:** Figma file `WNKIatY2HASsdD2Kyh1nCp`, page `04 · V1 Layouts` (node `74:3`). Desktop frames are 1440 wide; each mobile frame (390) sits directly below its desktop page. Page `05 · Local components` (node `198:4969`) holds components the library kit does not cover. Frames on `03 · Designs` marked "superseded" are reference only.
- **Copy:** the Figma frames carry the approved copy. Do not rewrite copy; if it is wrong, flag it.
- **Compliance wording:** `docs/compliance-register.md` (from `AFM_Compliance_Statement_Register_v1`). Statements S01–S24 appear on the site verbatim. Never paraphrase a compliance statement.
- **Images:** `docs/image-register.xlsx`. Placeholder images from the kit are not licensed for launch; IMG-07 (Sydney Opera House) must not ship.

## Stack (decided 11 Sep 2026 — static, no CMS)

Astro + TypeScript + Tailwind. Fully static output. Content lives in the repo as files (see Content model). Hosting on Cloudflare Pages with Cloudflare Web Analytics (cookieless, no consent banner). Forms via Formspree (contact) and Mailchimp embedded form with native double opt-in (subscribe). Attestation is client-side (sessionStorage). Search via Pagefind (indexes PDF text at build). Charts are static images exported from Figma monthly, with the period table beside them. No database, no server functions, no CMS. Add those later only if traffic or a second editor justifies it.

## Design tokens (tailwind.config)

Colours
- navy `#1E355E` (headings, primary text on light)
- navy-dark `#0E202E` (footer, gradient base)
- ink `#0E202E` for standfirst on light hero
- grey `#57626D` (body)
- rule `#D2D5D9`
- copper `#B6432F` (links, primary buttons, active tabs)
- copper-icon `#C54932` (Lucide strokes on navy pages)
- amber `#EE7624` (icons and labels on warm/fund pages; eyebrow labels on navy)
- papyrus `#F5ECE3`, tint `#FAF5F1` (panels, important-information band)
- gold `#B08A3C` (external links only — SS&C registry, EQT, custodian, ratings houses)
- error `#B82823`
- white

Type: Fira Sans. Weights Regular, Medium, SemiBold. Scale (desktop → mobile): display 60→36, h1 42→32, h2 32→26, h3 24→20, h4 18→17, body 18→16, small 15, meta 13, label 11–12 uppercase tracked.

Spacing (mobile): 48 between white modules, 40 inside banded modules, 16 between rows/cards, 16 gutter. Desktop: module padding 80–96, gutter 50, content max 1340, text column 888.

## Rules that are not negotiable

1. **Temperature rule.** Warm gradients (copper → white mesh) on the homepage hero and the two retail fund pages only. Navy everywhere else. Wholesale stats bands and landing cards use the three-step ladder: A− `#0E202E→#1E355E`, BBB `#1E355E→#394D71`, PEP `#1E4E78→#466E96`; the PEP hero uses its band palette. CTA banners follow the page temperature.
2. **Mobile heroes carry no image** except the homepage (image strip under the buttons, bleeds off the bottom edge, hero bottom padding 0).
3. **Tables are data, not markup.** Performance periods, fund features, portfolio stats and at-a-glance facts are arrays of `{label, value}` in the fund/strategy JSON, rendered by one `<KeyValueList>` that is a two-column table ≥768px and a stacked list below. The performance chart is a static image (`/public/charts/<fund>-<yyyy-mm>.webp`) with alt text; it is hidden below 768px and the period table carries the numbers.
4. **Icons.** Lucide only. Functional glyphs (chevrons, x, menu, search, download, file-text, external-link, check) anywhere. Route icons only in contact/how-to-invest banners: Direct `file-check`, Platforms `layout-grid`, Adviser `briefcase-business`, Existing investors `wallet`, General enquiries `handshake`. Role icons only on Governance four-roles: `briefcase-business`, `landmark`, `database-backup`, `shield-check`. No icons on any other content column. Icon colour follows the page: copper-icon on navy pages, amber on fund pages.
5. **External links are gold**, open in a new tab with `rel="noopener"`, and carry the Lucide `external-link` glyph instead of the chevron (add `external-link` to the icon set). Internal links are copper. Gold labels name the destination: "Apply via Olivia123" (same label in the fund hero, the How to invest bar and the homepage cards), "Link to SS&C fund registry".
6. **Wholesale gate.** Strategy pages and Investment Management sit behind the attestation modal (S14 wording verbatim). Session-only cookie. Decline returns to the homepage. Landing page is not gated (pending compliance; flag if this changes).
7. **Important-information band** on every page: generic (S03) by default; fund-specific (S05) on the two fund pages; wholesale (S15) on wholesale and IM pages. Text comes from `content/site.json`, never hard-coded in components.
8. **Licensing line** (confirmed by compliance 14 Sep 2026) in the footer, from `content/site.json`: "This site has been created by Arculus Funds Management Pty Limited which operates under the GCI Australia AFSL as a CAR - GCI Australia Pty Ltd ABN 68 140 364 576 Australian Financial Services Licence No 346034. The Responsible Manager and Key Person for GCI Australia Pty Ltd is Renny Ellis." Verbatim.
9. Australian English throughout. "Adviser", not "advisor", except inside a quoted compliance statement.


## CTA path (decided 14 Sep 2026)

Every call to action resolves to one of three destinations. Nothing lands on a generic page.

1. **Invest** → Olivia123 (`https://www.olivia123.com/`, generic link until Arculus's dedicated onboarding URL is confirmed). Gold external button labelled "Apply via Olivia123" in all three places: fund hero, the Online route in the How to invest bar, and the homepage fund cards. Olivia handles KYC and the application; the registry (SS&C) processes it.
2. **Enquire** → `/contact?topic=<slug>`. The contact form is the first module on the page and reads `topic` to pre-fill "I am a", the subject line and the intro sentence. Topics: `fixed-income-fund`, `preferred-income-fund`, `a-sma`, `bbb-sma`, `pep`, `im`, `general`. Wholesale "Request the strategy paper" additionally pre-fills the message ("Please send me the <strategy> strategy paper"). "Who to contact" sits below the form; the SS&C registry link appears only in its Existing investors block.
3. **Read** → a file, never a page. PDS and TMD are direct PDF links; homepage fund cards' "Latest report" links the newest monthly PDF from `documents.json`; strategy papers are requested via Enquire.

How to invest bar has two routes, each ending in a button: Online → "Apply via Olivia123" (gold); Through your adviser or platform → "Talk to us about the Fund" (copper, Enquire). Documents & statements has no CTA; it ends with "View all documents ›". A "Key documents" module (PDS, latest monthly, annual report tiles bound to `documents.json`) sits between How to invest and Documents & statements. Wholesale strategy heroes carry one button, "Request the strategy paper"; the conversational route is the foot CTA banner. About and Insights heroes carry one navigation button each. Insights hero: Subscribe only.

## Component naming

One React component per Figma module, same name in PascalCase: `HeroLevel1`, `HeroLevel2`, `Breadcrumb`, `Statement`, `StrategyAtAGlance`, `FundOverviewPerformance`, `ContentColumns`, `TwoColumnsContent`, `LeftRight`, `ProfileModule`, `ProductCards`, `InsightsRow`, `InsightsList`, `RatingsBlock`, `RatingsRow`, `Documents`, `DownloadSlice`, `ThreeColumnsContactBanner`, `CtaBanner`, `ImportantInformation`, `Footer`, `Header`, `DropdownPanel`, `FilterTabs`, `SearchInput`, `SearchResultRow`, `KeyValueList`, `AttestationModal`, `SubscribeModal`, `ContactForm`. Variants are props (`temperature="warm|navy"`, `size="compact|full"`, `withImage`), not separate components.

## Content model (files in `/content`)

- `content/funds/<slug>.json` — name, apir, arsn, hero {eyebrow?, title, standfirst, targetReturn, facts[]}, about, performance[] {period, value}, performanceAsAt, chart (path), features[] {label, value}, portfolioStats[] {label, value}, sectors[] {label, pct}, ratings {lonsecGrade, lonsecDate, others[]}, importantInformation (S05 text)
- `content/strategies/<slug>.json` — name, kind (sma|equity), ladderStep (1|2|3), hero {eyebrow, title, standfirst, facts[]}, pillars[3], stats[4], about, atAGlance[], process[4], risks[2], cta
- `content/documents.json` — [{title, description, fund|null, category (pds|tmd|monthly|quarterly|annual|rating|policy|notice), date, file, size}] — files in `/public/documents/`
- `content/insights.json` — [{title, standfirst, tag, date, readingTime, image, pdf, wholesaleOnly}] — PDFs in `/public/insights/`
- `content/team.json` — [{name, role, group (committee|leadership|analysts), bio, photo}]
- `content/site.json` — licensingLine, importantInformation {generic, wholesale}, disclosureDeclaration, complaintsText, registryUrl, contact {address, phone, email}, ratingsDisclaimer, attestation (S14 text)
- `content/pages/*.md` — legal pages, ESG, About page bodies (frontmatter: title, breadcrumb)

Monthly update = edit the fund JSON, drop the new PDF into `/public/documents/`, add a line to `documents.json`, export the chart image, commit. Every change is a git commit, which is the audit trail.

Seed values from the Figma frames are **unverified** until Renny signs the compliance register; keep `"verified": false` on each fund file until then.

## Figma node map (desktop → mobile)

Home `123:31753` → `207:16548` · AFI `88:823` → `207:16875` · PIF `97:1078` → `207:17279` · Wholesale landing `130:2004` → `209:19001` · A− `103:1334` → `207:17858` · BBB `118:12535` → `207:18186` · PEP `120:986` → `207:18514` · IM `170:3269` → `207:18842` · Our approach `157:11833` → `208:12136` · Governance `157:14081` → `208:12575` · Team `158:11833` → `209:12086` · ESG `150:13882` → `209:20263` · Insights `175:11833` → `210:13847` · Contact `181:11833` → `210:11897` · Document library `186:3532` → `210:12446` · T&Cs `150:13477` → `209:19310` · Privacy `150:13612` → `209:19606` · Cookie `150:13747` → `209:19950` · Search `191:4941` → `210:14701` · 404 `189:5033` → `210:15137` · Attestation `130:13792` · States S1–S5 `189:4010`, `189:4027`, `189:4288`, `189:4552`, `189:4713` · Dropdowns `190:4800` · Mobile nav `M0 · Mobile navigation`.

Use `get_design_context` on the module node, not the page, when building a component.

## Working method

- One task = one component or one page, desktop and mobile together, from the Figma node named in the task.
- Use tokens only. If a value in Figma is not a token, use the nearest token and note the discrepancy in the PR.
- Every PR gets a Cloudflare Pages preview. The reviewer is not a developer; the PR description says which page/module to look at and what to compare it to.
- Do not add libraries without asking. No analytics, tag managers or third-party scripts beyond Cloudflare Web Analytics, the Formspree endpoint and the Mailchimp form.
- Do not write compliance copy. Do not "improve" disclaimer wording.
- Commit messages: `feat(module): …`, `feat(page): …`, `content: …`, `fix: …`.

## Compliance status (14 Sep 2026)

Register returned clean. Responsible Entity is **DDH Graham Limited** for launch (EQT transition pending; ABN/AFSL for DDH Graham still to be supplied — placeholder "(ABN and AFSL to confirm)" in S05). CEO is **Sunetha Parag** throughout. Ratings are Lonsec only ("Rated 'Investment Grade' by Lonsec." + ratings disclosure). All S-statements otherwise as in the register.

## Open items (do not resolve in code)

DDH Graham ABN/AFSL · PIF ARSN (Q5) · office address (Q6) · Lonsec citation rules (Q8) · complaints/AFCA statement (Q9) · IM entity name (Arculus Capital / SACFM) · Disclosure page copy · Private mandates page · article template · image licences · Olivia123 dedicated URL.
