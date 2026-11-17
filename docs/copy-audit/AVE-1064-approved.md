# AVE-1064 APPROVED public copy (TJ, Nov 16 20:57 ET, via the Director)

TJ approved ALL 92 rows as written, with choices C1-A, C2-A, C3-A. This file = audit table (Linear comment 32af4c2e)
with the coordinator's lead-row AFTER cells from Linear comment ff967afd applied (rows 1, 6, 15, 16, 17, 21, 33, 39, 85, 86)
and the C1/C2/C3 picks resolved (rows 11, 14, 16, 32, 34, 72). Row 17 = ff967afd text. Rows 42-43 and 89-91 SUPERSEDE
the earlier AVE-1016 W15/W24 and AVE-1013 approved lines.

How to apply each AFTER cell (byte-for-byte):
- Copy the AFTER text exactly (quotes, apostrophes, dashes, punctuation as shown). In a table cell `\|` means a literal `|`.
- "…" in BEFORE/AFTER means "the rest of the source line, unchanged": replace only the shown span.
- "(rest unchanged)" (rows 31, 91) is an instruction, not copy.
- Row 85: the audit's "(see N1)" was a note pointer, not copy; the AFTER is exactly `- **Starter** - For getting started with Aventor`.
- Row 83: the platform-only fix is the full AFTER shown.
- C1-A: keep the /meta-and-google-ads page, its URL, title and body; only the generic link labels change (footer row 14 "Launch guide", related-link labels rows 32 and 72 "How launching works"). No redirect.
- C2-A: row 16 subhead as shown; KEEP the home "Launches to" icon row (home.ts:14-15) as is.
- C3-A: row 34 "Campaign builder for multiple ad platforms".
- Line numbers are from main at audit time; if they moved, find the BEFORE text. If a BEFORE string no longer exists on main, do NOT invent new copy: list it in your RESULT as "row N: BEFORE not found".
- Do not change any other copy (kept blocks and notes N1/N2 are out of scope).

# DRAFT, NEEDS TJ: public copy BEFORE/AFTER (AVE-1064)

Audit only. No product copy has changed. Nothing gets merged until TJ approves this table. After approval: one PR per repo, wording byte-for-byte as approved.

**Rules applied**

- **Platform rows:** public copy does not name only Meta and Google. AFTER says "multiple ad platforms", "your ad platforms" or "every ad platform you connect". No platform that isn't live yet (LinkedIn, TikTok) is named. "Multiple ad platforms" is true today (Meta and Google are two).
- **Size rows:** public copy does not limit Aventor to small businesses. AFTER says "businesses of every size" or drops the size.
- **Nov 11 rule:** lead with what Aventor does for the owner. No approval boilerplate is added in any AFTER line.

Sources: Aventor app repo at `main` (`ac5d032`), Aventor-Docs at `main`, Aventor-backend at `main`. "Live" means I confirmed the line in aventor.ai's HTML on Nov 16 (homepage only, see RESULT).

## Table

### Site-wide head tags, llms.txt, footer (aventor app repo)

| # | type | file:line or URL | BEFORE (verbatim) | AFTER |
|---|---|---|---|---|
| 1 | platform | `src/config/seo.ts:35` → `<meta name="description">`, `og:description`, `twitter:description` on https://aventor.ai (live) | Aventor is an AI campaign builder. Describe your business, generate ad creative, and launch across Meta and Google from one conversational workflow. | Aventor uses AI to make advertising simple. Describe your business and it creates your ads and launches them across multiple ad platforms for you. |
| 2 | platform | `src/config/seo.ts:41` (/features description) | Explore Aventor features: an AI creative studio, a smart campaign builder, a unified dashboard, analytics for Meta and Google ad campaigns, and ad account setup done for you. | Explore Aventor features: an AI creative studio, a smart campaign builder, one dashboard and analytics for all your ad platforms, and ad account setup done for you. |
| 3 | platform | `src/config/seo.ts:61` (/blog description) | Guides and product insights from Aventor on AI-powered advertising: creative generation, campaign setup, measurement, and launching on Meta and Google. | Guides and product insights from Aventor on AI-powered advertising: creative generation, campaign setup, measurement, and launching on multiple ad platforms. |
| 4 | platform | `src/config/seo.ts:66` (/beta-signup description) | Apply for early access to Aventor - the AI campaign workflow for Meta and Google ads. We are onboarding teams ready to test real ad setup and launch flows. | Apply for early access to Aventor, which builds and launches your ad campaigns across multiple ad platforms for you. We are onboarding teams ready to run real campaigns. |
| 5 | platform | `src/config/seo.ts:71` (/demo description) | Book a 15-minute Aventor demo. See how Aventor turns your website into ready-to-launch ads for Facebook, Instagram, and Google. | Book a 15-minute Aventor demo. See how Aventor turns your website into ready-to-launch ads on multiple ad platforms. |
| 6 | size | `public/llms.txt:3` | > Aventor is an AI advertising platform for small and mid-sized businesses. You describe your business in plain language, Aventor's AI builds your brand profile, generates ready-to-run ad images and videos, recommends budget and audience settings, and launches campaigns directly to Meta (Facebook and Instagram) and Google Ads from one workflow. | > Aventor uses AI to make advertising simple. You describe your business in plain language, Aventor's AI builds your brand profile, generates ready-to-run ad images and videos, recommends budget and audience settings, and launches campaigns directly to multiple ad platforms from one workflow. |
| 7 | platform | `public/llms.txt:12` | - Direct launch: campaigns publish straight to Meta Ads Manager and Google Ads (Search and Performance Max) once accounts are connected | - Direct launch: campaigns publish straight to each ad platform you connect |
| 8 | platform | `public/llms.txt:13` | - Unified dashboard: campaign status, analytics, drafts, and creative assets in one place, synced from Meta and Google with the last update time shown | - Unified dashboard: campaign status, analytics, drafts, and creative assets in one place, synced from your ad platforms with the last update time shown |
| 9 | platform | `public/llms.txt:15` | - Account setup done for you: Aventor's team can create and prepare your Meta and Google ad accounts, Page and tracking at no charge; you invite the team to your accounts and never share a password | - Account setup done for you: Aventor's team can create and prepare your ad accounts, Page and tracking at no charge; you invite the team to your accounts and never share a password |
| 10 | platform | `public/llms.txt:20` | - After the trial: plus 5% of actual ad-platform spend, billed weekly. Ad spend is paid to Meta and Google directly and is not included. | - After the trial: plus 5% of actual ad-platform spend, billed weekly. Ad spend is paid to the ad platforms directly and is not included. |
| 11 | platform | `public/llms.txt:31` | - [Launch Meta and Google ads from one workflow](https://aventor.ai/meta-and-google-ads.md): the launch guide for Facebook, Instagram, Google Search, and Performance Max: … | UNCHANGED (C1-A: keep the line and the /meta-and-google-ads page) |
| 12 | platform | `public/llms.txt:36` | …and who does what: Aventor, you, and Meta and Google | …and who does what: Aventor, you, and the ad platforms |
| 13 | platform | `public/llms.txt:38` | …spend, conversions, cost per conversion, budget split and budget moves for Meta and Google, what Aventor checks before and after launch, which numbers Meta and Google count, … | …spend, conversions, cost per conversion, budget split and budget moves across your ad platforms, what Aventor checks before and after launch, which numbers the ad platforms count, … |
| 14 | platform | `src/app/(marketing)/_components/MarketingFooter.tsx:44` (every public page, live) | Meta + Google ads | Launch guide |

### Home (https://aventor.ai, live)

| # | type | file:line or URL | BEFORE (verbatim) | AFTER |
|---|---|---|---|---|
| 15 | platform | `src/content/marketing/home.ts:8` (hero eyebrow) | AI ads for Meta and Google | AI that makes advertising simple |
| 16 | platform | `src/content/marketing/home.ts:11` (hero subhead) | Describe your business in plain language. Aventor creates on-brand ads and ready-to-launch campaigns for Facebook, Instagram, Google Search and Performance Max, and launches them for you. | Aventor uses AI to make advertising simple. Describe your business in plain language and it creates on-brand ads and campaigns, then launches them for you across multiple ad platforms. |
| 17 | size | `src/content/marketing/home.ts:21` | Aventor is an AI advertising platform for small and mid-sized businesses, churches and nonprofits. You get agency-quality campaigns without agency overhead or learning two ad managers, and | Aventor uses AI to make advertising simple. You get agency-quality campaigns without agency overhead or learning each ad platform, and |
| 18 | platform | `src/content/marketing/home.ts:22` (link label after #17) | launch to Meta and Google from one workflow | launch to multiple ad platforms from one workflow |
| 19 | platform | `src/content/marketing/home.ts:39` (step 4 title) | Launch to Meta and Google | Aventor launches them |
| 20 | platform | `src/content/marketing/home.ts:40` (step 4 body) | Connect your ad accounts and launch. Your campaigns go straight to Meta and Google. | Connect your ad accounts. Aventor launches your campaigns on each platform for you. |
| 21 | platform | `src/config/onboarding/landingTour.ts:12` (home tour) | Aventor turns a short description of your business into ready-to-run ads for Meta and Google. Here's a quick tour (30 seconds, tops). | Aventor uses AI to turn a short description of your business into ready-to-run ads on multiple ad platforms. Here's a quick tour (30 seconds, tops). |
| 22 | platform | `src/config/onboarding/landingTour.ts:33` (home tour) | This kicks off your first campaign. You'll review and edit everything (creatives, budget, audience) and connect your Meta or Google account before anything goes live. | This starts your first campaign. Aventor builds the ads, budget and audience, then launches on the ad accounts you connect. |
| 23 | platform | `src/components/pages/LandingPage.tsx:78` (Vite landing, `App.tsx:225`) | Sign in to build campaigns, generate creative, and launch across Meta and Google from one workflow. | Sign in and Aventor builds your campaigns and creative, then launches them across multiple ad platforms. |
| 24 | platform | `src/components/pages/LandingPage.tsx:114` | Launch to Meta and Google from one conversational workflow | Launch to multiple ad platforms from one conversation |

### Features, pricing, about, demo, beta (live pages)

| # | type | file:line or URL | BEFORE (verbatim) | AFTER |
|---|---|---|---|---|
| 25 | platform | `src/content/marketing/features.ts:72` | Aventor drafts your campaign plan for Meta and Google: the audiences, keywords, and budgets. You review and edit the plan before anything is built. | Aventor drafts your campaign plan for each ad platform: the audiences, keywords, and budgets. |
| 26 | platform | `src/content/marketing/features.ts:87` | Launch to Meta and Google | Launch to multiple ad platforms |
| 27 | platform | `src/content/marketing/features.ts:117` | Track live campaigns, manage drafts, and pick up where you left off across Meta and Google. | Track live campaigns, manage drafts, and pick up where you left off across all your ad platforms. |
| 28 | platform | `src/content/marketing/features.ts:140` | Performance and cost for your Meta and Google campaigns in one dashboard, synced from the ad platforms with the last sync time shown. Aventor AI can check each launch every morning and apply changes that don’t raise spend. Anything that could raise spend waits for your OK. | Performance and cost for all your campaigns in one dashboard, synced from your ad platforms with the last sync time shown. Every morning Aventor AI checks each launch and applies changes that don’t raise spend. |
| 29 | platform | `src/content/marketing/features.ts:166` | New to Facebook or Google ads? Our team creates your ad accounts, Page and tracking and gets them ready to connect. You send one short note and invite us in. ${SETUP_SERVICE_FREE_SENTENCE} | New to running ads? Our team creates your ad accounts, Page and tracking and gets them ready to connect. You send one short note and invite us in. ${SETUP_SERVICE_FREE_SENTENCE} |
| 30 | platform | `src/content/marketing/shared.ts:39` (features + guide) | Aventor checks your connected Meta and Google accounts and lists any step that needs your attention, such as a Page permission or a Pixel, before campaign setup starts. | Aventor checks your connected ad accounts and lists any step that needs your attention, such as a Page permission or a Pixel, before campaign setup starts. |
| 31 | platform | `src/content/marketing/shared.ts:51` | Choose “Start automatically when approved” to have your ads go live on Meta and Google as soon as the campaign is approved, or … | Choose “Start automatically when approved” to have your ads go live on your ad platforms as soon as the campaign is approved, or … (rest unchanged) |
| 32 | platform | `src/content/marketing/shared.ts:13` (link label used site-wide) | Launch Meta and Google ads from one workflow | How launching works |
| 33 | platform | `src/content/marketing/pricing.ts:15` | Build, review, and launch Meta and Google campaigns from one workflow for a flat monthly price, plus a small fee on the ad spend you actually run. | Aventor uses AI to build and launch your campaigns on multiple ad platforms for a flat monthly price, plus a small fee on the ad spend you actually run. |
| 34 | platform | `src/content/marketing/pricing.ts:24` (plan bullet) | Campaign builder for Meta (Facebook, Instagram) and Google (Search, Performance Max) | Campaign builder for multiple ad platforms |
| 35 | platform | `src/content/marketing/pricing.ts:30` | Ad spend is paid to Meta and Google directly and isn't included. | Ad spend is paid to the ad platforms directly and isn't included. |
| 36 | platform | `src/config/marketingFacts.ts:40` (pricing FAQ) | …of the actual ad spend reported by your connected ad platforms. Your ad spend is paid to Meta and Google directly. | …of the actual ad spend reported by your connected ad platforms. Your ad spend is paid to those platforms directly. |
| 37 | platform | `src/config/marketingFacts.ts:53` (pricing FAQ) | No. Ad-platform spend is separate from Aventor's fees and is billed by Meta and Google. | No. Ad-platform spend is separate from Aventor's fees and is billed by each ad platform. |
| 38 | platform | `src/config/marketingFacts.ts:57` (pricing FAQ) | …A set that runs on both Meta and Google counts once. … | …A set that runs on more than one ad platform counts once. … |
| 39 | size | `src/content/marketing/about.ts:14` and `src/components/pages/AboutPage.tsx:101` (same line, Next + Vite) | Small businesses deserve the same advertising power as the big players. But until now, creating professional ad campaigns required expensive agencies, design tools, and marketing expertise. | Aventor uses AI to make advertising simple, without the agency bill. Until now, professional ad campaigns took expensive agencies, design tools and marketing expertise. |
| 40 | platform | `src/app/(marketing)/demo/page.tsx:60` | See how Aventor turns your website into ready-to-launch ads for Meta and Google. | See how Aventor turns your website into ready-to-launch ads on multiple ad platforms. |
| 41 | platform | `src/app/(marketing)/beta-signup/BetaSignupForm.client.tsx:99` and `src/components/pages/BetaSignupPage.tsx:90` | We’re prioritizing teams ready to test actual Meta and Google ad workflows during beta. | We’re prioritizing teams ready to run real ad campaigns during beta. |
| 42 | platform | `src/components/demo/demoCopy.ts:97` (public no-sign-in demo; AVE-1016 W15) | Aventor connects your Facebook and Google ad accounts and launches these ads when your free trial starts. | Aventor connects your ad accounts and launches these ads when your free trial starts. |
| 43 | platform | `src/components/demo/demoCopy.ts:104` (public demo; AVE-1016 W24) | Aventor connects your Facebook and Google ad accounts | Aventor connects your ad accounts |

### Blog (aventor.ai/blog, also served as .md for llms.txt)

| # | type | file:line or URL | BEFORE (verbatim) | AFTER |
|---|---|---|---|---|
| 44 | platform | `src/content/marketing/blogIndex.ts:8` | Guides and product insights on AI-powered advertising: creative generation, campaign setup, measurement, and launching on Meta and Google. | Guides and product insights on AI-powered advertising: creative generation, campaign setup, measurement, and launching on multiple ad platforms. |
| 45 | platform | `src/content/blog/posts/ad-campaign-measurement-and-optimization.ts:13` (title) | Live Campaign Analytics in Aventor: What It Checks and What Meta and Google Count | Live Campaign Analytics in Aventor: What It Checks and What the Ad Platforms Count |
| 46 | platform | same file `:15` (description) | …and which numbers Meta and Google count: conversion tracking checks, … | …and which numbers the ad platforms count: conversion tracking checks, … |
| 47 | platform | same file `:27` | Aventor shows your Meta and Google spend, conversions, and cost per conversion together. Here is what it reports, what it checks for you, which numbers come from Meta and Google, and what its morning check does. | Aventor shows your spend, conversions, and cost per conversion from every ad platform together. Here is what it reports, what it checks for you, which numbers come from the ad platforms, and what its morning check does. |
| 48 | platform | same file `:31` | …cost per conversion for your Meta and Google campaigns together, synced from the ad platforms… | …cost per conversion for all your campaigns together, synced from the ad platforms… |
| 49 | platform | same file `:35` | …which numbers come from Meta and Google, and what the morning check does with them. | …which numbers come from the ad platforms, and what the morning check does with them. |
| 50 | platform | same file `:52` | The numbers are synced from Meta and Google. Each analytics page shows … | The numbers are synced from your ad platforms. Each analytics page shows … |
| 51 | platform | same file `:54` (h2) | What Aventor checks, and what Meta and Google count | What Aventor checks, and what the ad platforms count |
| 52 | platform | same file `:57` | …The rest is counted by Meta and Google. … | …The rest is counted by the ad platforms. … |
| 53 | platform | same file `:79` (h3) | Counted by Meta and Google | Counted by the ad platforms |
| 54 | platform | same file `:82` | Spend, conversions, and cost per conversion come from Meta and Google. Each platform decides… | Spend, conversions, and cost per conversion come from the ad platforms. Each platform decides… |
| 55 | platform | same file `:101` | Reporting covers the Meta and Google campaigns you launch with Aventor: Facebook, Instagram, Google Search, and Performance Max. Other ad platforms are not included. | Reporting covers the campaigns you launch with Aventor, on each ad platform you connect. |
| 56 | platform | same file `:119` (FAQ) | They are synced from Meta and Google, and each analytics page shows when they were last updated. | They are synced from your ad platforms, and each analytics page shows when they were last updated. |
| 57 | platform | same file `:122` (FAQ Q) | Can I see Meta and Google results together? | Can I see results from different ad platforms together? |
| 58 | platform | same file `:123` (FAQ A) | Yes. A launch’s analytics page lists its Meta and Google campaigns side by side, with totals across both. | Yes. A launch’s analytics page lists its campaigns on each platform side by side, with totals across all of them. |
| 59 | platform | same file `:130-131` (FAQ Q + A) | Q: Does Aventor report on other ad platforms? A: No. Reporting covers the Meta and Google campaigns you launch with Aventor. | Q: Does Aventor report on ad platforms I haven't connected? A: No. Reporting covers the campaigns you launch with Aventor on the ad platforms you connect. |
| 60 | platform | same file `:134` (FAQ Q) | Which numbers does Aventor check, and which come from Meta and Google? | Which numbers does Aventor check, and which come from the ad platforms? |
| 61 | platform | same file `:135` (FAQ A) | Spend, conversions, and cost per conversion come from Meta and Google, and each platform credits… | Spend, conversions, and cost per conversion come from the ad platforms, and each platform credits… |
| 62 | platform | `src/content/blog/posts/brand-safe-ai-advertising.ts:15` | …and what Meta and Google review and bill. | …and what the ad platforms review and bill. |
| 63 | platform | same file `:26` | …and what Aventor, you, and Meta and Google are each responsible for. | …and what Aventor, you, and the ad platforms are each responsible for. |
| 64 | platform | same file `:30` | …This guide also sets out what Aventor does, what you are responsible for, and what Meta and Google do. | …This guide also sets out what Aventor does, what you are responsible for, and what the ad platforms do. |
| 65 | platform | same file `:78` (h2) | Who does what: Aventor, you, and Meta and Google | Who does what: Aventor, you, and the ad platforms |
| 66 | platform | same file `:81` | Aventor builds and launches campaigns in your own Meta and Google ad accounts. Here is who does what. | Aventor builds and launches campaigns in your own ad accounts. Here is who does what. |
| 67 | platform | same file `:107` (h3) | What Meta and Google do | What the ad platforms do |
| 68 | platform | same file `:112` | …Once a campaign starts, Meta and Google charge your ad account up to your daily budget until you pause it. … | …Once a campaign starts, the ad platform charges your ad account up to your daily budget until you pause it. … |
| 69 | platform | same file `:147` (FAQ) | You connect your own Meta and Google ad accounts and keep access to them. … | You connect your own ad accounts and keep access to them. … |
| 70 | platform | same file `:151` (FAQ) | Yours. Aventor launches campaigns in the Meta and Google ad accounts you connect, and Meta and Google bill your ad spend to those accounts. | Yours. Aventor launches campaigns in the ad accounts you connect, and each ad platform bills your ad spend to those accounts. |
| 71 | platform | same file `:155` (FAQ) | Meta and Google review ads under their own ad policies. … | Each ad platform reviews ads under its own ad policies. … |
| 72 | platform | `src/content/blog/posts/cost-effective-advertising-with-ai.ts:19` and `how-to-launch-ad-campaign-in-minutes.ts:20` (related link) | Launch Meta and Google ads from one workflow | How launching works |
| 73 | platform | `src/content/blog/posts/cost-effective-advertising-with-ai.ts:88` (FAQ Q) | Do I still pay Google and Meta for my ads? | Do I still pay the ad platforms for my ads? |
| 74 | platform | `src/content/blog/posts/how-to-launch-ad-campaign-in-minutes.ts:25` (description) | Describe your business, let the AI draft the creative and campaign plan, review everything, and launch to Google and Meta from one place. The complete four-step workflow, explained. | Describe your business and Aventor drafts the creative and campaign plan, then launches it on multiple ad platforms from one place. The complete four-step workflow, explained. |
| 75 | size | `src/content/blog/posts/how-to-launch-ad-campaign-in-minutes.ts:42` | For many small and mid-sized businesses, that complexity delays campaigns or keeps them from launching at all. Aventor is designed to simplify the process without removing advertiser approval. | For many businesses, that complexity delays campaigns or keeps them from launching at all. Aventor takes that work off your plate. |

### Aventor-Docs (public help site)

| # | type | file:line | BEFORE (verbatim) | AFTER |
|---|---|---|---|---|
| 76 | platform | `index.mdx:30` | Connect Meta or Google Ads to launch directly from Aventor. | Connect your ad accounts and Aventor launches for you. |
| 77 | platform | `getting-started.mdx:50` | Connect Meta or Google Ads and go live | Connect your ad accounts and go live |
| 78 | platform | `video-tutorial.mdx:30` | 3. **Launch** - Connect to Meta or Google Ads and go live | 3. **Launch** - Connect your ad accounts and go live |
| 79 | platform | `video-tutorial.mdx:67` | Step-by-step guide to launching your ads on Meta and Google. | Step-by-step guide to launching your ads. |
| 80 | platform | `campaigns/overview.mdx:8` | After creating your ads in the Studio, launch them directly to Meta or Google. | After you create your ads in the Studio, Aventor launches them on the ad platforms you connect. |
| 81 | platform | `campaigns/review-launch.mdx:34` | Ensure Meta or Google Ads account is connected. | Make sure your ad account is connected. |
| 82 | platform | `create-ads/preview-export.mdx:47` | Send directly to Meta or Google Ads. | Send directly to your connected ad platforms. |
| 83 | platform | `help/credits-billing.mdx:90` | No. Launching campaigns to Meta or Google is free. You only pay for image generation and editing. Ad spend goes directly to the ad platforms. | No. Launching campaigns is free. You only pay for image generation and editing. Ad spend goes directly to the ad platforms. |
| 84 | platform | `help/limits-safety.mdx:108` | …Generated content must also comply with Meta and Google ad policies for campaign launch. | …Generated content must also comply with each ad platform's ad policies for campaign launch. |
| 85 | size | `help/credits-billing.mdx:47` | - **Starter** - For small businesses and freelancers | - **Starter** - For getting started with Aventor |

### Aventor-backend customer emails (`src/lib/email/templates.ts`)

| # | type | file:line | BEFORE (verbatim) | AFTER |
|---|---|---|---|---|
| 86 | platform | `:500` (Welcome email, HTML) | Your account is ready. Enter a website URL or describe your brand, and Aventor will generate ad creative, build campaigns, and launch them to Meta and Google, all from a single workflow. | Your account is ready. Enter a website URL or describe your brand, and Aventor's AI will create your ads, build your campaigns and launch them across multiple ad platforms for you. |
| 87 | platform | `:512` (Welcome email, text) | …and Aventor will generate ad creative, build campaigns, and launch them to Meta and Google. | …and Aventor will generate ad creative, build campaigns, and launch them across multiple ad platforms for you. |
| 88 | platform | `:1183` (Beta application email) | …prioritizing teams that are ready to test real campaign workflows across Meta and Google. | …prioritizing teams that are ready to run real campaigns. |
| 89 | platform | `:1817` (approval email, 1% scheme) | Once it starts, Meta and Google charge your ad account up to your daily budget every day until you pause it, and Aventor bills 1% of the actual spend daily. | Once it starts, your ad platforms charge your ad account up to your daily budget every day until you pause it, and Aventor bills 1% of the actual spend daily. |
| 90 | platform | `:1820` (approval email, 5% scheme) | Once it starts, Meta and Google charge your ad account up to your daily budget every day until you pause it, and Aventor bills 5% of the actual spend, invoiced weekly. | Once it starts, your ad platforms charge your ad account up to your daily budget every day until you pause it, and Aventor bills 5% of the actual spend, invoiced weekly. |
| 91 | platform | `:1854` and `:1855` (approval-launched email) | …Meta and Google now charge your ad account up to your daily budget every day until you pause it, … | …Your ad platforms now charge your ad account up to your daily budget every day until you pause it, … (rest unchanged) |
| 92 | platform | `:2650` (account deletion scheduled email) | …Nothing in your Meta or Google ad accounts was deleted. | …Nothing in your ad accounts was deleted. |

## Choices for TJ (2 options each, my pick marked)

**C1. The /meta-and-google-ads launch guide** (rows 11, 14, 32, 72; page title `src/config/seo.ts:49` "Launch Meta and Google Ads From One Workflow"; hero `metaAndGoogleAds.ts:12-13` "Meta + Google" / "Launch Meta and Google ads from one workflow").
- **C1-A (my pick):** keep the page. It is a true platform guide: what you need for Facebook, Instagram, Google Search and Performance Max. When LinkedIn or TikTok ship, they get their own section or page. Change only the generic links that point at it: footer label "Launch guide"; related-link label "How launching works". URL stays, so no redirect or SEO loss.
- **C1-B:** rename the page to "/launch-guide", title "Launch your ads on every platform from one workflow", with a 301 redirect from the old URL. More work (redirect, sitemap, llms.txt, tests) for no copy gain until a third platform is live.

**C2. Home hero subhead** (row 16; also the "Launches to" icon row `home.ts:14-15`: Facebook, Instagram, Google Search, Performance Max).
- **C2-A (my pick):** "Describe your business in plain language. Aventor creates on-brand ads and campaigns and launches them for you across multiple ad platforms." Keep the "Launches to" icon row as is: it shows only what's live and grows when a platform ships.
- **C2-B:** same sentence, and remove the "Launches to" row so the hero names no platform at all.

**C3. Pricing plan bullet** (row 34).
- **C3-A (my pick):** "Campaign builder for multiple ad platforms"
- **C3-B:** "Campaign builder for every ad platform Aventor supports: Facebook, Instagram, Google Search and Performance Max today" (true, but still names only Meta and Google products).

## Out-of-repo items (where they live)

- **Product Hunt tagline, description, maker comment and gallery captions** (AVE-992): live in Linear comments on AVE-992 (captions D13-D17 from comment `22eabcd8`), not in a repo. The repo copy `docs/design/2026-11-11-ave-992-product-hunt/README.md` has captions D13, D16, D17 with no platform or size wording; D14 and D15 are marked "(unchanged)" and their text is only in Linear. Needs a pass in Linear before launch.
- **Social profiles** (LinkedIn, X, Facebook Page, Instagram bios), Google Business Profile, directory listings: not in any repo I can read. Owner to check against the same rules.
- **Live site beyond the homepage:** I could confirm only the homepage HTML (see RESULT). Other pages are built from the repo files above, so the repo rows should match what's live.

## Kept

**Kept, in-app connect screens and in-app platform-specific screens** (not public, or connect a named platform):
- Docs `integrations/meta-ads.mdx`, `integrations/google-ads.mdx`, `integrations/pixel-setup.mdx`, `campaigns/test-your-ads.mdx` (platform pages).
- Docs `campaigns/budget-allocation.mdx:40` "When launching to both Meta and Google:" (instructions for that exact case).
- `src/components/setupService/setupServiceCopy.ts:28` (setup service for Facebook and Google Ads accounts).
- `src/components/audiences/*` "Import from Meta or Google", `AudienceProviderLinkModal.tsx:240`.
- `src/components/dashboard/LeadSourcesModal.tsx:44`, `CampaignLeadInbox.tsx:519`, `RetargetingCreateModal.tsx:18`, `CampaignReview.tsx:6630/6681`.
- `src/config/onboarding/dashboardTour.ts:40` "Check that your Meta and Google accounts are linked."
- `src/utils/squarespaceCopy.ts:40,151`, `src/utils/websiteIntegration.ts:473` (tracking code sends to Facebook and Google).
- `src/components/studio/ReadinessWorkspacePanel.tsx` "Facebook and Google setup" steps.

**Kept, in-app but not a connect screen** (out of this story's scope; optional follow-up with the same pattern "your ad platforms"):
- `src/components/billing/usageFeeCopy.ts:69,70`, `src/components/billing/billingCopy.ts:69,76`, `src/components/modals/SubmitReviewConfirmationModal.tsx:38`, `src/components/dashboard/privacyDataCopy.ts:41`, `src/utils/audienceCsv.ts:27,47`, analytics agent copy ("Google or Meta didn't accept the change").

**Kept, public, with a reason:**
- `src/app/(marketing)/privacy/page.tsx:86,87,230`: the privacy policy must name who receives the data and how to revoke access. Legal accuracy, not positioning.
- Blog post `aventor-or-meta-ads-manager-and-google-ads` (and `shared.ts:22-25`, `llms.txt:35`): a true head-to-head comparison with those two tools. The topic is the two named products.
- The /meta-and-google-ads page body (`metaAndGoogleAds.ts`), if TJ picks C1-A: a true platform guide.
- `features.ts:84,107` image alt text: describes a screenshot that really shows a Meta ad set and Google ad group / budgets.
- Home "Launches to" icon row and `HomeSections.tsx` LaunchVisual (if C2-A): lists only live channels.
- Docs `changelog.mdx` (10 lines): dated release notes, a true record of what shipped.

**Kept, size lines that are true examples or not about Aventor's market:**
- `src/constants/sessionClarifications.ts:105` "Examples: homeowners, small business owners, students, parents." (in-app example audiences for the customer's own ads).
- `src/constants/campaignClarifications.ts:38` "Local businesses" (in-app audience option).
- `cost-effective-advertising-with-ai.ts:76` "…give smaller teams more of the operating structure of an in-house advertising function…" describes a benefit for small teams; it does not limit who Aventor is for.
- Screenshots in `docs/design-briefs/*`, `docs/mockups/*`, `public/promo_pics/ai_studio_design.PNG` (used on Features via `FeaturesPage.tsx:12`): the matched text is an in-app example audience inside the screenshot, not a claim about Aventor.
- Backend `src/mastra/prompts/*`, `src/lib/learning/*` fixtures: internal AI prompts and test fixtures, never shown as Aventor's positioning.
- `docs/AGENT_ARCHITECTURE.md:257` and `docs/linear/**`: internal docs.

## Notes (not copy changes in this story)

- **N1.** Docs `help/credits-billing.mdx` still describes a credits model ("You only pay for image generation and editing", "Starter - For small businesses and freelancers"). That no longer matches pricing ($99/month + ad-spend fee). Rows 83 and 85 fix only the platform and size words; the page needs a separate accuracy pass.
- **N2.** Several public lines carry approval boilerplate that the Nov 11 rule says to avoid, e.g. `llms.txt:5` "Users review and edit everything before anything goes live", `features.ts:72` "You review and edit the plan before anything is built". Where a row already touched the line I dropped the approval clause; other lines are left for a separate pass.
- **N3.** Rows 42-43 and 89-91 replace lines TJ approved earlier (AVE-1016 W15/W24, AVE-1013). Approving them here supersedes those.
