[Alternativeto Scraper](https://apify.com/fatihtahta/alternativeto-scraper?fpr=data)

# AlternativeTo Scraper With Emails & Socials

**Slug:** fatihtahta/alternativeto-scraper

## Overview

This actor is built upon the [ValidatedMails.com](https://validatedmails.com) architecture for email enrichment workflows.

AlternativeTo Scraper With Emails & Socials collects structured software profile data from AlternativeTo, including product identity, descriptions, tags, categories, links, social profiles, community signals, business context, and optional contact email details. It helps you turn software pages, search pages, and keyword discovery into consistent JSON records you can use immediately in reporting and automation.

[AlternativeTo](https://alternativeto.net) is a widely used software discovery platform, making it a practical source for competitive discovery, category mapping, and tool landscape analysis. The actor automates repetitive collection work so teams avoid manual copy/paste across hundreds or thousands of listings. The result is consistent output quality and significant time savings for ongoing data workflows.

## Why Use This Actor

- **Market research and analytics teams:** Build repeatable datasets for category sizing, competitor benchmarking, and trend tracking across software verticals.
- **Product and content teams:** Identify comparable tools, positioning signals, and feature/category patterns to support roadmap and content decisions.
- **Developers and data engineering teams:** Feed clean records into ETL jobs, internal databases, BI dashboards, or enrichment pipelines with minimal transformation.
- **Lead generation and enrichment teams:** Capture official websites, company domains, social links, and optional contact emails to support outreach preparation.
- **Monitoring and competitive tracking teams:** Run on a schedule to detect changes in listings, popularity indicators, and ecosystem movement over time.

## Input Parameters

Provide any combination of URLs, queries, and filters to control coverage depth and discovery mode.

| Parameter | Type | Description | Default |
| --- | --- | --- | --- |
| `startUrls` | `string[]` | One or more AlternativeTo URLs to scrape directly. Supports search pages, category pages, and individual software profile pages. | – |
| `queries` | `string[]` | Keywords used to discover matching software on AlternativeTo (for example, product names, categories, or use-case terms). | – |
| `limit` | `integer` | Maximum number of listings to save **per query**. Minimum allowed value: `10`. | `50000` |
| `getEmails` | `boolean` | When `true`, attempts to include a public company contact email for each software domain when available. Allowed values: `true`, `false`. | `false` |

## Example Input

```
{
  "startUrls": [
    "https://alternativeto.net/software/notion/",
    "https://alternativeto.net/browse/search/?q=project+management"
  ],
  "queries": [
    "email client",
    "crm"
  ],
  "limit": 1500,
  "getEmails": true
}
```

## Output

### 6.1 Output destination

The actor writes results to an Apify dataset as JSON records. And the dataset is designed for direct consumption by analytics tools, ETL pipelines, and downstream APIs without post-processing.

### 6.2 Record envelope (all items)

Every record includes these stable envelope fields:

- **type** *(string, required)*
- **id** *(number, required)*
- **url** *(string, required)*

Recommended idempotency key: `type + ":" + id`.
Use this key for deduplication and upserts when the same entity appears across multiple inputs or repeated runs.

### 6.3 Examples

Example: software (`type = "software"`)

```
{
  "type": "software",
  "id": 205541,
  "url": "https://alternativeto.net/software/swisscows-email/about/",
  "recordId": "item-swisscows-email",
  "app": {
    "name": "Swisscows.email",
    "profileUrl": "https://alternativeto.net/software/swisscows-email/about/",
    "detailsUrl": "https://alternativeto.net/software/swisscows-email/about/",
    "description": "Swisscows.email is another product of Swisscows AG.",
    "iconUrl": "https://img.alternativeto.net/icons/140/jpeg/swisscows-email_205541.png",
    "tags": ["Paid", "Open Source", "WebMail Provider"],
    "features": ["Privacy focused", "End-to-End Encryption"],
    "categories": ["Office & Productivity", "Security & Privacy"]
  },
  "discovery": {
    "sourceUrl": "https://alternativeto.net/browse/search/?p=2&q=email",
    "seedType": "url",
    "seedValue": "https://alternativeto.net/browse/search/?q=email"
  },
  "links": {
    "officialWebsite": "https://swisscows.email/en/",
    "officialDomain": "swisscows.email",
    "socialLinks": [
      {
        "label": "Facebook page",
        "url": "https://www.facebook.com/swisscows",
        "domain": "www.facebook.com",
        "is_social": true
      },
      {
        "label": "X page",
        "url": "https://x.com/swisscows_ch",
        "domain": "x.com",
        "is_social": true
      }
    ]
  },
  "community": {
    "stats": {
      "likes": 5,
      "comments": 2,
      "alternatives": 58,
      "newsArticles": 0
    },
    "commentsPreviewCount": 2
  },
  "business": {
    "developedBy": ["Swisscows AG"],
    "licensing": "Open Source and Commercial product.",
    "pricing": ["Subscription ranging between $2 and $45 per month."],
    "rating": "Average rating of 5"
  },
  "contact": {
    "email": "support@swisscows.email",
    "emailStatus": "risky"
  }
}
```

## Field reference

### Software fields (`type = "software"`)

- **type** *(string, required)*: Record type discriminator.
- **id** *(number, required)*: Stable software identifier.
- **url** *(string, required)*: Canonical software profile URL.
- **recordId** *(string, optional)*: Human-readable record identifier.
- **app.name** *(string, required)*: Software name.
- **app.profileUrl** *(string, required)*: Profile page URL.
- **app.detailsUrl** *(string, optional)*: Details/about URL.
- **app.description** *(string, optional)*: Short description text.
- **app.longDescription** *(string, optional)*: Expanded description text.
- **app.iconUrl** *(string, optional)*: Primary icon URL.
- **app.screenshots** *(array[string], optional)*: Screenshot URLs.
- **app.videos** *(array[string], optional)*: Related video URLs.
- **app.tags** *(array[string], optional)*: Labels such as pricing/model tags.
- **app.features** *(array[string], optional)*: Feature highlights.
- **app.categories** *(array[string], optional)*: Category labels.
- **discovery.sourceUrl** *(string, optional)*: Source page that yielded the record.
- **discovery.seedType** *(string, optional)*: Discovery mode (for example, URL seed or query seed).
- **discovery.seedValue** *(string, optional)*: Original input value used for discovery.
- **links.officialWebsite** *(string, optional)*: Official product/company website.
- **links.officialDomain** *(string, optional)*: Parsed official domain.
- **links.externalLinks** *(array[object], optional)*: Outbound links discovered on profile.
- **links.externalLinks[].label** *(string, optional)*: Link label.
- **links.externalLinks[].url** *(string, optional)*: Link URL.
- **links.externalLinks[].domain** *(string, optional)*: Link domain.
- **links.externalLinks[].is_social** *(boolean, optional)*: Social network indicator.
- **links.socialLinks** *(array[object], optional)*: Social-only link subset.
- **classification.costAndLicense** *(array[string], optional)*: Cost/license descriptors.
- **classification.platforms** *(array[string], optional)*: Supported platforms.
- **classification.origin** *(array[string], optional)*: Country/region origin labels.
- **classification.supportedLanguages** *(array[string], optional)*: Supported language labels.
- **community.stats.likes** *(number, optional)*: Likes count.
- **community.stats.comments** *(number, optional)*: Comment count.
- **community.stats.alternatives** *(number, optional)*: Alternative tools count.
- **community.stats.newsArticles** *(number, optional)*: Related article count.
- **community.commentsPreview** *(array[object], optional)*: Sample user comments.
- **community.commentsPreview[].id** *(string, optional)*: Comment identifier.
- **community.commentsPreview[].user** *(string, optional)*: Username when available.
- **community.commentsPreview[].rating** *(number, optional)*: Comment rating score.
- **community.commentsPreview[].date** *(string, optional)*: Comment timestamp.
- **community.commentsPreview[].text** *(string, optional)*: Comment text.
- **community.commentsPreview[].vote_score** *(number, optional)*: Vote score.
- **community.commentsPreview[].low_activity_warning** *(boolean, optional)*: Low activity marker.
- **community.commentsPreviewCount** *(number, optional)*: Number of preview comments.
- **business.developedBy** *(array[string], optional)*: Developer/company names.
- **business.licensing** *(string, optional)*: Licensing summary.
- **business.pricing** *(array[string], optional)*: Pricing statements.
- **business.rating** *(string, optional)*: Rating summary.
- **business.alternativesSummary** *(string, optional)*: Alternatives summary text.
- **business.supportedLanguages** *(array[string], optional)*: Business section language list.
- **contact.email** *(string, optional)*: Public contact email when found.
- **contact.emailStatus** *(string, optional)*: Email quality/status indicator.

## Data guarantees & handling

- **Best-effort extraction:** fields may vary by region/session/availability/UI experiments.
- **Optional fields:** null-check in downstream code.
- **Deduplication:** recommend `type + ":" + id`.

## How to Run on Apify

1. Open the actor in Apify Console.
2. Configure your search parameters (for example URLs, keyword queries, and email enrichment preference).
3. Set the maximum number of outputs to collect.
4. Click **Start** and wait for the run to finish.
5. Download results in JSON, CSV, Excel, or other supported formats.

## Scheduling & Automation

### Scheduling

**Automated Data Collection**
Use schedules to run the actor automatically and keep datasets current without manual restarts. This is useful for recurring monitoring, reporting, and enrichment workflows.

- Navigate to **Schedules** in Apify Console
- Create a new schedule (daily, weekly, or custom cron)
- Configure input parameters
- Enable notifications for run completion
- (Optional) Add webhooks for automated processing

### Integration Options

- **Webhooks:** Trigger downstream actions when a run completes
- **Zapier:** Connect to 5,000+ apps without coding
- **Make (Integromat):** Build multi-step automation workflows
- **Google Sheets:** Export results to a spreadsheet
- **Slack/Discord:** Receive notifications and summaries
- **Email:** Send automated reports via email

## Performance

Estimated run times:

- **Small runs (< 1,000 outputs):** ~2–3 minutes
- **Medium runs (1,000–5,000 outputs):** ~5–15 minutes
- **Large runs (5,000+ outputs):** ~15–30 minutes

Execution time varies based on filters, result volume, and how much information is returned per record.

## Compliance & Ethics

### Responsible Data Collection

This actor collects publicly available **software listing and company profile** information from **alternativeto.net** for legitimate business purposes, including:

- **software and SaaS** research and market analysis
- **competitive intelligence and benchmarking**
- **data enrichment and catalog maintenance**

Users are responsible for ensuring their data collection and usage comply with applicable laws, regulations, and platform terms. This section is informational and not legal advice.

### Best Practices

- Use collected data in accordance with applicable laws, regulations, and the target site’s terms
- Respect individual privacy and personal information
- Use data responsibly and avoid disruptive or excessive collection
- Do not use this actor for spamming, harassment, or other harmful purposes
- Follow relevant data protection requirements where applicable (e.g., GDPR, CCPA)

## Support

If you need help, open an issue on the actor page (Issues tab). Include the input used (with sensitive values redacted), the run ID, expected vs. actual behavior, and optionally a small output sample so troubleshooting is faster.