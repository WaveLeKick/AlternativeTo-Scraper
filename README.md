[Alternativeto Scraper](https://apify.com/cerridwen/alternativeto-scraper?fpr=data)

# AlternativeTo Scraper

Extract software alternatives, reviews, and category listings from [AlternativeTo.net](https://alternativeto.net).

This scraper provides structured data for competitive analysis, market research, and lead generation without the need for manual browsing. It easily handles pagination and bypasses Cloudflare anti-bot mechanisms.

## Features

- **Software Alternatives (`mode: software`)**: Extract all alternatives for a specific software.
- **Software Reviews (`mode: reviews`)**: Extract all user reviews for a specific software.
- **Category Explorer (`mode: category`)**: Extract a list of software URLs from a specific category.

## Input Configuration

The actor accepts the following inputs via the Apify Console or API:

| Field | Type | Description | Default |
| --- | --- | --- | --- |
| `mode` | String | Scraping mode: `software`, `category`, or `reviews`. | `software` |
| `startUrls` | Array | List of AlternativeTo URLs to start scraping from. | `[{"url": "https://alternativeto.net/software/notion/"}]` |
| `maxItems` | Integer | Maximum number of items to scrape. Set to `0` for unlimited. | `200` |
| `includeReviews` | Boolean | If `mode` is `software`, setting this to `true` will also scrape reviews for that software. | `false` |

### Example Input

```
{
    "mode": "software",
    "startUrls": [{"url": "https://alternativeto.net/software/notion/"}],
    "maxItems": 100
}
```

## Output Fields

### Software Alternative Output (`mode: software`)

| Field | Type | Description |
| --- | --- | --- |
| `name` | String | Name of the alternative software. |
| `alternativeFor` | String | The main software it is an alternative to. |
| `alternativeToUrl` | String | URL of the alternative software on AlternativeTo. |
| `description` | String | Short localized description of the app. |
| `likes` | Integer | Number of likes the alternative received. |
| `tags` | Array | License tags (Free, Open Source, Freemium, etc.). |
| `platforms` | Array | Supported platforms (Windows, Mac, Web, etc.). |
| `sourceUrl` | String | The pagination URL where this item was found. |
| `imageUrl` | String | Thumbnail image URL. |

### Review Output (`mode: reviews`)

| Field | Type | Description |
| --- | --- | --- |
| `software` | String | Name of the software being reviewed. |
| `reviewer` | String | Name/username of the reviewer. |
| `rating` | Float | Star rating (out of 5). |
| `title` | String | Title of the review. |
| `body` | String | Review content/body text. |
| `pros` | String | Mentioned pros. |
| `cons` | String | Mentioned cons. |
| `date` | String | Date the review was published. |

## Compute Unit Consumption

This actor uses a lightweight HTTP crawler (`BeautifulSoupCrawler`), making it extremely fast and cheap to run. It does not use real browsers (Playwright/Puppeteer), meaning memory consumption is minimal (approx. 256MB) and speeds are very high.

- 100 records typically take less than **5-10 seconds** and consume less than **0.001 CU**.

## Limitations

- AlternativeTo heavily relies on heavily nested pagination (`?p=2`); the scraper correctly handles this but `maxItems` applies globally across all queued pages.