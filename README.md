[Github Repo Scraper](https://apify.com/joyouscam35875/github-repo-scraper?fpr=data)

Scrape GitHub repository metadata at scale using the official REST API v3. Get structured data on stars, forks, languages, topics, contributors, releases, and more — ready for lead generation, market research, and competitive analysis.

## What it does

- **Scrape specific repos** — provide a list of `owner/repo` strings
- **Search GitHub** — use any [GitHub search syntax](https://docs.github.com/en/search-github/searching-on-github/searching-for-repositories) to discover repos
- **Enrich with extras** — optionally fetch top contributors and latest releases
- **Handle rate limits** — automatic back-off with `X-RateLimit` headers; optional token support for 5,000 req/hr

## Output fields

| Field | Type | Description |
| --- | --- | --- |
| `owner` | string | Repository owner login |
| `name` | string | Repository name |
| `fullName` | string | `owner/name` |
| `url` | string | GitHub URL |
| `description` | string | Repository description |
| `stars` | int | Stargazer count |
| `forks` | int | Fork count |
| `openIssues` | int | Open issue count |
| `language` | string | Primary language |
| `languages` | object | All languages with byte counts |
| `topics` | array | Topic tags |
| `createdAt` | string | ISO 8601 creation date |
| `updatedAt` | string | Last update date |
| `pushedAt` | string | Last push date |
| `license` | string | SPDX license identifier |
| `isArchived` | bool | Whether the repo is archived |
| `isFork` | bool | Whether the repo is a fork |
| `defaultBranch` | string | Default branch name |
| `size` | int | Repository size in KB |
| `watchers` | int | Watcher/subscriber count |
| `homepage` | string | Homepage URL |
| `topContributors` | array | Top 30 contributors (opt-in) |
| `latestReleases` | array | Last 5 releases (opt-in) |

## Input examples

### Scrape specific repositories

```
{
    "repos": ["apify/crawlee", "microsoft/playwright", "facebook/react"]
}
```

### Search for Python web scraping tools

```
{
    "searchQuery": "web scraping language:python stars:>100",
    "maxRepos": 50
}
```

### Full enrichment with auth

```
{
    "repos": ["vercel/next.js"],
    "searchQuery": "framework language:typescript stars:>5000",
    "maxRepos": 100,
    "includeContributors": true,
    "includeReleases": true,
    "githubToken": "ghp_xxxxxxxxxxxx"
}
```

## Rate limits

| Mode | Requests/hour | Repos/run (approx) |
| --- | --- | --- |
| No token | 60 | ~20–30 (2–3 API calls per repo) |
| With token | 5,000 | ~1,500–2,500 |

**Tip:** Create a free [personal access token](https://github.com/settings/tokens) (no scopes needed for public repos) to unlock 5,000 requests/hour.

## Pricing

**$0.002 per repository scraped** (pay per event).

### Cost comparison

| Repos | This actor | GitHub API (your infra) | Manual research |
| --- | --- | --- | --- |
| 10 | $0.02 | Free + your time | ~30 min |
| 100 | $0.20 | Free + your time | ~5 hours |
| 500 | $1.00 | Free + your time | ~2 days |
| 1,000 | $2.00 | Free + your time | ~1 week |

**You pay for data, not infrastructure.** No servers to maintain, no code to write, no rate limits to handle.

## Use cases

- **Lead generation** — Find companies using specific technologies, contact repo owners
- **Competitive analysis** — Track competitor open-source projects, compare stars/forks growth
- **Technology research** — Discover trending tools in any language or domain
- **Talent sourcing** — Identify top contributors to relevant projects
- **Investment research** — Gauge open-source traction for developer tools companies
- **Academic research** — Collect repository metadata for software engineering studies
- **Dependency auditing** — Assess health (activity, issues, releases) of your dependencies

## Technical details

- Uses GitHub REST API v3 (`api.github.com`)
- Automatic rate-limit detection and back-off via `X-RateLimit-*` headers
- No browser or proxy needed — pure API calls
- Async execution with `httpx` for fast throughput
- Outputs clean, structured JSON to the Apify dataset

---

## 🔗 More Scrapers by Ken Digital

| Scraper | What it does | Price |
| --- | --- | --- |
| [YouTube Channel Scraper](https://apify.com/joyouscam35875/youtube-channel-scraper) | Videos, stats, metadata | $0.001/video |
| [France Job Scraper](https://apify.com/joyouscam35875/france-job-scraper) | WTTJ + France Travail + Hellowork | $0.005/job |
| [France Real Estate Scraper](https://apify.com/joyouscam35875/france-real-estate-scraper) | 5 sources + DVF price analysis | $0.008/listing |
| [Website Content Crawler](https://apify.com/joyouscam35875/website-content-crawler) | HTML → Markdown for AI/RAG | $0.001/page |
| [Google Trends Scraper](https://apify.com/joyouscam35875/google-trends-scraper) | Keywords, regions, related queries | $0.002/keyword |
| [GitHub Repo Scraper](https://apify.com/joyouscam35875/github-repo-scraper) | Stars, forks, languages, topics | $0.002/repo |
| [RSS News Aggregator](https://apify.com/joyouscam35875/rss-news-aggregator) | Multi-source feed parsing | $0.0005/article |
| [Instagram Profile Scraper](https://apify.com/joyouscam35875/instagram-profile-scraper) | Followers, bio, posts | $0.0015/profile |
| [Google Maps Scraper](https://apify.com/joyouscam35875/google-maps-scraper) | Businesses, reviews, contacts | $0.002/result |
| [TikTok Scraper](https://apify.com/joyouscam35875/tiktok-scraper) | Videos, likes, shares | $0.001/video |
| [Google SERP Scraper](https://apify.com/joyouscam35875/google-serp-scraper) | Search results, PAA, snippets | $0.003/search |
| [Trustpilot Scraper](https://apify.com/joyouscam35875/trustpilot-scraper) | Reviews, ratings, sentiment | $0.001/review |

👉 [View all scrapers](https://apify.com/joyouscam35875)

## 🔗 Quick Integration

### Python

```
from apify_client import ApifyClient
client = ApifyClient("YOUR_API_TOKEN")
run = client.actor("joyouscam35875/github-repo-scraper").call(run_input={...})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(item)
```

### Node.js

```
import { ApifyClient } from 'apify-client';
const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });
const run = await client.actor('joyouscam35875/github-repo-scraper').call({...});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

### No-code: Make / Zapier / n8n

Search for this actor in the Apify connector. No code needed.