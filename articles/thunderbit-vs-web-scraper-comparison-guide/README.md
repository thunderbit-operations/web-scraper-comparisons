# Thunderbit vs Web Scraper: An Evidence-Based Comparison (2026)

**Disclosure:** This article is produced by the Thunderbit operations team, which has a direct commercial interest in this comparison. We distinguish verified facts from opinions and cite current official or third-party sources where appropriate.

**Last verified: 2026-07-22** — Pricing, features, and plan details checked against official Thunderbit and Web Scraper pages on this date.

Search results for this matchup are dominated by broad listicles and generic "best scraper" roundups, so this article focuses on the documented product differences that matter when choosing between the two tools.

So here it is. I went through both tools' official documentation, current pricing pages, feature matrices, and architecture models to build the comparison I wish I'd had when users ask me "how is Thunderbit different from Web Scraper?" This isn't a controlled benchmark — I'm not going to fabricate setup times or row counts — but it is an honest, evidence-based breakdown of what each tool does, how they differ architecturally, and which one fits which job. If a feature claim comes from a vendor (including us), I'll say so.

## Why This Comparison Matters Right Now

Web scraping tools have become table stakes for sales teams, ecommerce operators, real estate analysts, and developers building data pipelines. [79% of businesses now prioritize AI in their software stack](https://learn.g2.com/ai-adoption), and the line between "scraping tool" and "automated data entry system" is blurring fast.

But the tool landscape is fragmented. Web Scraper (webscraper.io) has been a go-to free Chrome extension for years — millions of users, a loyal community, and a well-documented selector-based workflow. Thunderbit came along with a different thesis: use AI to handle the extraction logic so users don't have to write selectors at all.

Forum threads I've reviewed surface recurring frustrations: "CSV export only is pretty limiting," "lack of scheduling is a dealbreaker," "oh crap the site changed and my scraper died," and "why paywall basic functionality?" These pain points map neatly onto the architectural differences between the two tools. So instead of a vague feature list, I'm going to walk through those differences dimension by dimension.

## The Master Comparison: Thunderbit vs Web Scraper at a Glance

Before getting into the details, here's the big-picture table. Every claim below is sourced from official product pages and documentation as of July 2026.

| Dimension | Thunderbit | Web Scraper (webscraper.io) |
|---|---|---|
| **Setup approach** | AI Suggest Fields — describe what you want, AI proposes columns | Manual sitemap builder with point-and-click selectors (CSS knowledge sometimes required) |
| **Pagination** | Auto-detect (click, infinite scroll, or numbered pages) | Manual [Pagination selector](https://webscraper.io/documentation/selectors/pagination-selector) with multiple modes (Auto, Link, Click, etc.) |
| **Subpage scraping** | 1-click "Scrape Subpages" enrichment (paid plans) | Nested Link selectors in sitemap tree ([documentation](https://webscraper.io/documentation/selectors)) |
| **Anti-bot / JS rendering** | Cloud scraping with built-in handling (vendor claim) | Browser-only execution; no built-in anti-bot on the free extension |
| **Scheduling** | Built-in natural-language scheduler (paid plans) | Extension: none. Cloud plans: yes ([pricing](https://webscraper.io/pricing)) |
| **Export destinations** | Excel, CSV, JSON, Google Sheets, Airtable, Notion | Extension: CSV only. Cloud: CSV, JSON, cloud storage |
| **AI data labeling/transform** | Yes — Field AI Prompt per column | No |
| **Developer access** | REST API, MCP server, CLI ([API docs](https://thunderbit.com/docs/introduction)) | Extension: none. Cloud: [API available](https://webscraper.io/documentation/web-scraper-cloud/api) |
| **Free tier** | 6 pages/month; email/phone/image extractors free | 500 pages/month (extension); Cloud plans start at $50/mo |

This table alone covers the primary search intent. But the real story is in the details.

## Architecture: Define-What vs. Define-How

This is the most important distinction, and nobody explains it well.

**Web Scraper** uses a define-how model. You build a sitemap — a tree of [selectors](https://webscraper.io/documentation/selectors) that tells the tool exactly which DOM elements to extract, how to navigate pagination, and where to follow links. The point-and-click tool helps generate CSS selectors, but the [official documentation](https://webscraper.io/documentation/scraping-a-site) notes that manual CSS selector entry may still be required and that users need some HTML/CSS knowledge. The logic is explicit, inspectable, and repeatable. If the site doesn't change, your sitemap keeps working.

**Thunderbit** uses a define-what model. You describe the fields you want (or let AI suggest them), optionally provide a JSON Schema, and the system handles rendering, extraction, and structuring. Our [API documentation](https://thunderbit.com/docs/introduction) describes this as: send a URL and a schema, get structured JSON back. The extraction logic is abstracted — you don't see or control the selectors.

Neither approach is universally better. Here are the tradeoffs:

- **Define-how** gives you control and predictability. You know exactly what's being extracted and why. But you pay for that control in setup time and maintenance — when the site changes, you fix the selectors.
- **Define-what** reduces per-site configuration and maintenance. But you're trusting the AI to interpret your intent correctly, which means you need to verify output quality, especially on unusual page structures.

As a practical rule, the define-what model is a strong fit when you're scraping many different sites or when source layouts change frequently. The define-how model is a strong fit when you're scraping the same site repeatedly and want deterministic, inspectable results.

## Cloud vs. Browser vs. API: Three Execution Models

This is the dimension that forum users complain about most — anti-bot blocks, dynamic content freezing, and the difference between local and cloud execution. Here's how the modes break down:

| Mode | Best for | Speed | Login-required sites? | Anti-bot |
|---|---|---|---|---|
| **Thunderbit Cloud Scraping** | Public pages (ecommerce, listings) | Fast (batch up to 50 pages per Extract call) | ❌ | Built-in (vendor claim) |
| **Thunderbit Browser Scraping** | Login-gated sites (LinkedIn, CRM portals) | Moderate | ✅ (uses your session) | Uses your browser session |
| **Thunderbit API / MCP / CLI** | Developers, CI pipelines, AI agents | Fastest for batch jobs | Via headers | Built-in (vendor claim) |
| **Web Scraper Extension** | Any site in your browser | One page at a time, local | ✅ | None built-in |
| **Web Scraper Cloud** | Scheduled, automated runs | Depends on plan concurrency | ✅ (Cloud login feature) | [Proxies available](https://webscraper.io/pricing) on paid plans |

Some important distinctions:

**Web Scraper's extension is entirely local.** It opens pages in your browser, runs selectors, and exports CSV. That's it. No cloud, no scheduling, no API. This is a strength if you want full control and zero data leaving your machine. It's a limitation if you need automation, anti-bot handling, or non-CSV exports.

**Web Scraper Cloud is a separate product** with its own [pricing](https://webscraper.io/pricing), starting at $50/month for 5,000 URL credits. Cloud adds scheduling, API, proxies, data-quality monitoring, and additional export formats. It's inaccurate to say "Web Scraper has no API" — the Cloud product does. But the free extension does not.

**Thunderbit's API** ([docs](https://thunderbit.com/web-scraper-api)) is schema-first. You send a `POST /extract` with a URL and a JSON Schema describing your desired fields, and the response is structured JSON matching that schema. The API also offers `renderMode: "full"` for JavaScript-heavy pages, `countryCode` for geo-blocked content, and batch endpoints with webhooks for async jobs. For developers, this is a fundamentally different interface than Web Scraper Cloud's API, which is oriented around managing sitemaps and scraping jobs rather than schema-matched extraction.

**For technical users specifically:** Thunderbit exposes MCP tools (`thunderbit_suggest_fields` → `thunderbit_extract`), a CLI (`thunderbit batch extract`), and integrations with n8n and Zapier. Web Scraper Cloud has webhook support and a job-management API. The architectural difference is that Thunderbit returns AI-structured data by default; Web Scraper returns the data your selectors captured. One user pain point I've seen in forums — "flexibility comes at the cost of determinism" — captures this tradeoff well.

## The Pagination and Dynamic Content Stress Test

"Handling dynamic content (infinite scroll, pagination, 'load more')" is the most frequently raised pain point in the forum discussions I reviewed. One user specifically noted that "Thunderbit sometimes freezes when dynamic content keeps loading." I'm not going to pretend that doesn't happen, because honesty is more useful than marketing.

### How Web Scraper Handles Pagination

Web Scraper's [Pagination selector](https://webscraper.io/documentation/selectors/pagination-selector) is recursive — it follows pagination links and extracts data from each discovered page. The documentation lists multiple pagination modes: Auto, Link, Scripted link, Attribute link, Text link, Link from any script, and click-based. Data selectors must be children of the Pagination selector.

This is powerful and transparent. You can also use URL ranges like `[1-100]` for numbered pages.

The downside: it's manual configuration. If the site changes its pagination structure, your sitemap breaks. The documentation acknowledges that the Auto option "may need additional configuration when it fails to recognize the element." For infinite-scroll pages, you'd configure an "Element scroll down" or "Element click" selector, which can be brittle when scroll behavior changes.

### How Thunderbit Handles Pagination

Thunderbit advertises auto-detection of pagination type — click-based, infinite scroll, or numbered pages. On paid browser plans, pagination and subpage scraping are included features. The API supports `renderMode: "full"` for JavaScript-heavy pages and batch processing for multi-page jobs.

The honest assessment: auto-detection works well for common pagination patterns (numbered pages, standard "Next" buttons). For heavy infinite-scroll pages — the kind that keep loading content as you scroll — browser mode can be less stable. Our recommendation for those cases is cloud scraping mode or the API with full rendering, which offloads the execution from your browser.

### Practical Guidance

| Scenario | Recommended Approach |
|---|---|
| Stable numbered pagination, known site | Either tool works. Web Scraper's explicit selector gives you inspectable logic. |
| Frequently changing site layouts | Thunderbit's schema-driven approach reduces per-site maintenance. |
| Heavy infinite-scroll pages | Thunderbit cloud mode or API with `renderMode: "full"`. Web Scraper may need custom scroll selectors. |
| Deep subpage navigation (category → detail) | Thunderbit: 1-click subpage enrichment. Web Scraper: nested Link selectors in sitemap. |

Neither tool guarantees success on every site. Anti-bot systems, CAPTCHAs, and aggressive rate limiting can block any scraper. I'd encourage testing on your specific target before committing to either workflow.

## Use-Case Fit Matrix: Which Tool for Which Job?

No comparison I've seen segments recommendations by actual use case. But the people searching "Thunderbit vs Web Scraper" aren't asking abstractly — they have a specific job to do. Here's my assessment based on each tool's documented capabilities:

| Use Case | Thunderbit Fit | Web Scraper Fit | Why |
|---|---|---|---|
| **[Sales lead gen](https://thunderbit.com/blog/ai-lead-generation)** (emails, phones from directories) | ✅ Excellent — free email/phone extractors, AI field detection | ⚠️ Possible but requires manual selector config per site | Thunderbit auto-detects contact fields; Web Scraper needs XPath/CSS selectors for each directory |
| **[Ecommerce price monitoring](https://thunderbit.com/blog/ai-for-ecommerce)** | ✅ Excellent — scheduled scraping, instant templates (Amazon, Shopify) | ⚠️ Requires Cloud plan ($50+/mo) for scheduling | Thunderbit's natural-language scheduler + pre-built templates vs. manual sitemap + Cloud subscription |
| **[Real estate listings](https://thunderbit.com/blog/ai-for-real-estate)** | ✅ Strong — subpage scraping enriches listing details | ⚠️ Needs nested sitemap for subpages | 1-click subpage enrichment vs. multi-level sitemap config |
| **Developer / CI pipeline** | ✅ API + MCP + CLI with structured JSON output | ⚠️ Cloud API available but selector/job-management oriented | Thunderbit returns schema-matched JSON; Web Scraper Cloud manages sitemap-based jobs |
| **One-off quick data grab** | ✅ Fast with AI field suggestion | ✅ Reliable once sitemap is built | Thunderbit is faster to start; Web Scraper is more predictable for repeat runs on a known site |
| **[LinkedIn scraping](https://thunderbit.com/blog/scraping-linkedin)** (login-gated) | ✅ Browser mode uses your session | ✅ Extension runs in your browser session | Both work for logged-in pages; Thunderbit adds AI field detection on top |

Caveats:

- For **one-off grabs on a site you already know well**, Web Scraper's free extension is hard to beat on cost. Zero dollars, local execution, full control.
- For **scaling across many sites or automating recurring jobs**, Thunderbit's AI and cloud infrastructure reduce the per-site setup burden.
- For **developer workflows**, the distinction matters: Thunderbit's API is extraction-first (send URL + schema, get data). Web Scraper Cloud's API is automation-first (manage sitemaps, trigger jobs, download results).

## Transparent Pricing: What's Free, What's Paywalled, What It Actually Costs

Users are vocal about pricing confusion with scraping tools. "Why paywall basic functionality" is a real complaint I've seen in forums. So here's the full breakdown, sourced from official pricing pages as of July 2026.

### Thunderbit Pricing ([source](https://thunderbit.com/pricing))

| Plan | Price | Credits/Pages | Key Features |
|---|---|---|---|
| **Free** | $0/mo | 6 pages/month (max 30 credits/page) | AI Suggest Fields, email/phone/image extractors (free), CSV/Excel/Sheets/Airtable/Notion export |
| **Starter** | $15/mo (monthly) | 500 credits/month | Pagination, Subpage Scraping, Pre-built Templates, Bulk Scraping, Scheduled Scrapers |
| **Pro** | $38/mo (monthly) | 3,000 credits/month | Everything in Starter + higher limits |

**Thunderbit API** ([separate pricing](https://thunderbit.com/web-scraper-api)):

| Plan | Price | API Units | Notes |
|---|---|---|---|
| **Free** | $0 | 600 one-time units | Distill: 1 unit/page. Extract: 20 units/page. 2 concurrent requests. |
| **Starter** | $16/mo (yearly) | 60,000 units/year | 30 concurrent requests |
| **Pro** | $40/mo (yearly) | 600,000 units/year | 50 concurrent requests |

Important: the browser credit system and the API unit system are separate. Don't conflate them. On the browser side, 1 credit ≈ 1 row. On the API side, one Extract call costs 20 units regardless of how many rows it returns from that page.

### Web Scraper Pricing ([source](https://webscraper.io/pricing))

| Plan | Price | Allowance | Key Features |
|---|---|---|---|
| **Extension (free)** | $0 | 500 pages/month | Local browser execution, CSV export only, no scheduling, no API |
| **Project (Cloud)** | $50/mo | 5,000 URL credits | Scheduling, API, parser, proxies (US datacenter included), CSV/JSON export, integrations |
| **Professional (Cloud)** | $100/mo | 20,000 URL credits | Everything in Project + higher limits |
| **Scale (Cloud)** | From $200/mo | Unlimited URL credits | Concurrency-based pricing; residential proxy add-on listed separately on the official pricing page |
| **Enterprise** | Custom | Custom | Custom everything |

**Key detail:** A Web Scraper URL credit is one page loaded, not one record. If you extract 100 records from one page, that's 1 URL credit. If you load 100 pages, that's 100 URL credits.

### The "Cost Per Row" Problem

"Cost per row" is misleading because these products meter different resources.

- **Thunderbit browser:** credits approximate rows. Cost per row depends on your plan.
- **Thunderbit API:** metered by page loads (Extract = 20 units/page), not rows. A page with 50 products costs the same as a page with 5.
- **Web Scraper Cloud:** metered by page loads (URL credits). Same logic — one page, one credit, regardless of record count.
- **Web Scraper Extension:** free, but you pay in setup time, maintenance effort, and local execution constraints.

For a practical example: scraping 1,000 product listings across 50 pages.

| Tool | Unit Cost | Total Cost |
|---|---|---|
| Thunderbit Browser (Starter, $15/mo) | 1,000 credits (if 1 credit/row) | Fits within 500 credits? No — need Pro or higher. |
| Thunderbit API (Starter, $16/mo yearly) | 50 pages × 20 units = 1,000 units | Fits within 60,000 units/year easily. |
| Web Scraper Cloud (Project, $50/mo) | 50 URL credits | Fits within 5,000 credits/month easily. |
| Web Scraper Extension (Free) | $0 | Free, but manual execution and CSV-only export. |

The cheapest option depends entirely on your volume, frequency, and whether you value automation over manual effort. There's no universal "cheaper" tool here.

## AI Data Transformation: The Feature Gap Nobody Talks About

The tools diverge completely on what happens to data after extraction.

Web Scraper extracts exactly what the selectors capture. Text, links, images, attributes, HTML — raw data from the DOM. Any transformation, cleaning, or labeling happens downstream in your spreadsheet or pipeline.

Thunderbit includes a **Field AI Prompt** feature — you can attach a natural-language instruction to any column. For example: "Classify this product as 'budget', 'mid-range', or 'premium' based on the price" or "Extract just the city name from this address string." The AI processes each value through that prompt during extraction.

This is genuinely useful for [data enrichment workflows](https://thunderbit.com/blog/ai-web-scraping) — categorizing leads, normalizing addresses, translating text, or flagging outliers. It's also a source of non-determinism: the AI might classify edge cases differently on different runs. For workflows that require strict repeatability, you may want to handle transformation in a separate step.

Web Scraper has no equivalent feature. If you need data transformation, you do it after export.

## Scheduling and Automation

Scheduling is where the products diverge sharply:

| Capability | Thunderbit | Web Scraper Extension | Web Scraper Cloud |
|---|---|---|---|
| Scheduling | ✅ Natural-language scheduler on paid plans ("every Monday at 9am") | ❌ None | ✅ Yes, on all Cloud plans |
| Webhooks | ✅ API batch jobs with webhooks | ❌ None | ✅ Yes |
| Recurring jobs | ✅ Built into browser and API | ❌ Must run manually | ✅ Built into Cloud |
| Data-quality monitoring | Not documented as a standalone feature | ❌ | ✅ On Professional+ plans |

If you need automated, recurring scraping and you're using Web Scraper, you need the Cloud product. The free extension is manual-only. Thunderbit includes scheduling on all paid browser plans and supports webhooks on API batch jobs.

## Export and Integration Options

Forum users regularly express frustration about exports — "CSV export only is pretty limiting."

| Destination | Thunderbit | Web Scraper Extension | Web Scraper Cloud |
|---|---|---|---|
| CSV | ✅ | ✅ | ✅ |
| Excel | ✅ | ❌ | ❌ |
| JSON | ✅ | ❌ | ✅ |
| Google Sheets | ✅ | ❌ | ❌ (via integrations) |
| Airtable | ✅ | ❌ | ❌ |
| Notion | ✅ | ❌ | ❌ |
| Cloud storage | Via API integrations | ❌ | ✅ (Dropbox, S3, etc.) |

Thunderbit includes all export destinations on all plans, including free. Web Scraper's extension is CSV-only. Cloud adds JSON and cloud storage, but direct Google Sheets/Airtable/Notion export isn't a documented native feature.

For teams that live in Google Sheets or Airtable, this is a meaningful difference. For developers who just need JSON from an API, both Thunderbit's API and Web Scraper Cloud deliver structured output (though in different formats and with different extraction models).

## When to Choose Web Scraper

Web Scraper is a solid tool for the right use case:

- **You want a free, local, no-cloud-required scraping tool.** The extension costs nothing and runs entirely in your browser. For privacy-sensitive work or environments where data can't leave your machine, this matters.
- **You want explicit, inspectable extraction logic.** Sitemaps and selectors are transparent. You can see exactly what's being extracted and debug it step by step. If you're comfortable with CSS selectors and HTML structure, this control is valuable.
- **You're scraping the same site repeatedly and the site's DOM is stable.** Once a sitemap is built and tested, it's reliable and predictable. No AI interpretation variability.
- **You have a small-scale, infrequent scraping need.** 500 free pages/month is generous for occasional data grabs.
- **You need Web Scraper Cloud's specific features** — data-quality monitoring, residential proxies, or the Cloud API for job management.

## When to Choose Thunderbit

Thunderbit fits best in these scenarios:

- **You scrape many different sites and don't want to build a sitemap for each one.** AI field suggestion reduces per-site setup significantly.
- **You need scheduling, rich exports, or AI data transformation without a separate Cloud subscription.** These are included on Thunderbit's paid browser plans.
- **You're a developer building a data pipeline.** The [REST API](https://thunderbit.com/docs/introduction), MCP server, and CLI provide schema-first extraction that returns structured JSON — a different paradigm from managing sitemaps via API.
- **You need [email, phone, or image extraction](https://thunderbit.com/blog/web-scraping-without-coding) as a built-in feature.** These are free on Thunderbit, no selectors required.
- **You want to export directly to Google Sheets, Airtable, or Notion.** All included, all plans.

Caveats: Thunderbit's free tier is limited to 6 pages/month (vs. Web Scraper's 500). The AI extraction model is non-deterministic — edge cases may be handled differently across runs. And for heavy infinite-scroll pages, browser mode can be less stable than cloud mode.

## Honest Limitations of Both Tools

No scraping tool works on every site. Both have real limitations worth knowing before you commit.

**Thunderbit limitations:**
- Free tier is small (6 pages). You'll hit it fast on anything beyond a quick test.
- AI extraction can misinterpret unusual page structures. Always verify output.
- Browser mode on infinite-scroll pages can freeze (a known user-reported issue). Cloud mode or API is more stable for these cases.
- Credit system can be confusing — browser credits and API units are separate metering systems.

**Web Scraper limitations:**
- Extension is CSV-only export. No scheduling, no API, no automation.
- Sitemap maintenance is manual. Site redesigns break selectors.
- No AI assistance — you need to understand HTML structure and CSS selectors.
- Cloud plans start at $50/month, which is a significant jump from "free extension."
- The extension processes one page at a time in your browser, which is slow for large jobs.

## Getting Started: Practical Next Steps

Still deciding? A practical path forward:

1. **Define your job.** Are you scraping one site repeatedly, or many sites occasionally? Do you need automation, or is manual fine?
2. **Try both free tiers.** Install the [Web Scraper extension](https://webscraper.io/) and the [Thunderbit Chrome extension](https://chromewebstore.google.com/detail/thunderbit-ai-web-scraper/hbkblmodhbmcakopmmfbaopfckopccgp). Run them on your actual target site.
3. **Compare the output.** Check field accuracy, completeness, and whether the data needs post-processing.
4. **Factor in maintenance.** If you'll scrape this site weekly, consider how much effort each tool requires when the site changes.
5. **Check your export needs.** If you need Google Sheets or Airtable integration, that narrows the choice quickly.

If you want deeper background on [what web scraping is](https://thunderbit.com/blog/what-is-web-scraping) or how [AI-powered scrapers compare to traditional methods](https://thunderbit.com/blog/best-ai-web-scrapers), we cover both topics on the blog. And if you want to see the tools in action, our [YouTube channel](https://www.youtube.com/@thunderbit-ai) has walkthrough videos.

## Final Verdict

There's no universal winner here. Web Scraper is a mature, free, transparent tool that rewards users who invest in learning its selector model. Thunderbit is an AI-first tool that trades selector control for speed and flexibility, with cloud infrastructure and developer APIs that Web Scraper's free extension doesn't match.

Choose Web Scraper if you want free, local, explicit control and you're comfortable with CSS selectors. Choose Thunderbit if you want AI-assisted extraction, built-in scheduling and exports, or a developer API that returns structured data by default. Choose Web Scraper Cloud if you want Web Scraper's selector model with cloud automation and are willing to pay $50+/month for it.

The best tool is the one that fits your actual workflow — not the one with the longest feature list.

## FAQs

**1. Can I use Web Scraper's free extension for scheduled, automated scraping?**
No. The free Web Scraper extension is manual-only — you trigger each scrape yourself in the browser. Scheduling requires Web Scraper Cloud, which starts at $50/month. Thunderbit includes scheduling on all paid browser plans (starting at $15/month).

**2. Does Web Scraper have an API?**
The free browser extension does not. Web Scraper Cloud (paid plans) includes an [API](https://webscraper.io/documentation/web-scraper-cloud/api) for managing scraping jobs and retrieving results. Thunderbit's [REST API](https://thunderbit.com/docs/introduction) is available on both free and paid tiers, with different unit allowances.

**3. Which tool is better for scraping login-required sites like LinkedIn?**
Both can work. Web Scraper's extension runs in your browser session, so it inherits your login. Thunderbit's browser scraping mode does the same, with the addition of AI field detection. For login-gated sites, the key factor is whether you want manual selector control (Web Scraper) or AI-assisted extraction (Thunderbit).

**4. How do Thunderbit credits and Web Scraper URL credits compare?**
They meter different things. Thunderbit browser credits approximate rows extracted. Thunderbit API units are per-page (Extract costs 20 units per page, regardless of row count). Web Scraper Cloud URL credits are per-page-loaded (1 page = 1 credit, regardless of records extracted). Direct "cost per row" comparisons are misleading — always calculate based on your specific page-to-record ratio.

**5. Which tool handles infinite-scroll pages better?**
Neither tool guarantees success on all infinite-scroll implementations. Web Scraper uses manual "Element scroll down" or "Element click" selectors, which can be brittle. Thunderbit auto-detects scroll behavior but can freeze on heavy infinite-scroll pages in browser mode (a known user-reported issue). For infinite scroll, Thunderbit's cloud mode or API with `renderMode: "full"` tends to be more stable than browser-based approaches in either tool.
