[Github Repo Scraper](https://apify.com/sovereigntaylor/github-repo-scraper?fpr=data)

# GitHub Repository Scraper

Scrape any GitHub repository for comprehensive metadata — stars, forks, issues, pull requests, contributors, languages, topics, releases, license, last commit, README preview, and homepage URL. Search repositories by keyword with language and star count filters.

## What does GitHub Repository Scraper do?

This actor extracts detailed information from GitHub repository pages. You can either provide direct repository URLs or use search queries to discover repositories matching your criteria. Every scraped repository returns a rich data object with 18+ fields covering popularity, activity, and technical details.

Unlike GitHub's API (which requires authentication and has strict rate limits), this scraper works without any API key and extracts data directly from GitHub's public pages.

## Features

- **Direct URL scraping** — Provide any GitHub repo URL and get full metadata
- **Keyword search** — Find repos by keyword, language, and star count
- **18+ data fields** — Stars, forks, watchers, issues, PRs, contributors, languages, topics, releases, license, last commit, README preview, homepage
- **Multiple extraction strategies** — Embedded JSON-LD, meta tags, and DOM parsing for maximum reliability
- **Deduplication** — No duplicate repos in output, even with overlapping search results
- **Pagination** — Automatically follows GitHub search pagination up to 1,000 results
- **Proxy support** — Built-in proxy rotation to avoid rate limiting on large scrapes

## Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| repoUrls | array | [] | Direct GitHub repository URLs to scrape |
| searchQuery | string | null | Search repos by keyword (e.g., "web scraper") |
| language | string | null | Filter by language (e.g., "python", "javascript") |
| sortBy | enum | "stars" | Sort: stars, forks, updated, best_match |
| minStars | integer | 0 | Minimum star count filter |
| maxResults | integer | 200 | Maximum repos to return (up to 1,000) |
| proxyConfiguration | object | Apify Proxy | Proxy settings for rate limit avoidance |

## Output

Each repository produces a data object like this:

```
{
  "name": "react",
  "owner": "facebook",
  "fullName": "facebook/react",
  "description": "The library for web and native user interfaces.",
  "stars": 232145,
  "forks": 47523,
  "watchers": 6723,
  "openIssues": 983,
  "pullRequests": 287,
  "language": "JavaScript",
  "languages": ["JavaScript", "TypeScript", "HTML", "CSS"],
  "license": "MIT",
  "topics": ["react", "javascript", "frontend", "ui", "declarative"],
  "lastCommit": "2026-03-01T14:32:00Z",
  "contributors": 1847,
  "releases": 215,
  "readmePreview": "React is a JavaScript library for building user interfaces...",
  "homepage": "https://react.dev",
  "url": "https://github.com/facebook/react",
  "scrapedAt": "2026-03-02T10:15:30Z"
}
```

## Use Cases

### Tech Stack Research

Discover and compare frameworks, libraries, and tools in any programming language. Filter by stars and activity to find the most popular and actively maintained options.

### Competitor Analysis

Monitor competitor open source projects — track star growth, contributor activity, release frequency, and community engagement.

### Open Source Intelligence (OSINT)

Gather intelligence on organizations' tech stacks by analyzing their public repositories. Identify technologies, team size (contributors), and development velocity.

### Hiring & Talent Research

Find active open source contributors in specific languages or frameworks. Identify prolific developers by exploring contributor data across popular repositories.

### Investment & Market Research

Spot emerging technologies by tracking rapidly growing repositories. Compare star counts, fork rates, and contributor growth across competing projects.

### Academic Research

Collect structured data on open source software ecosystems for academic studies. Analyze language trends, licensing patterns, and community dynamics.

## Examples

### Scrape specific repositories

```
{
  "repoUrls": [
    "https://github.com/facebook/react",
    "https://github.com/vuejs/vue",
    "https://github.com/angular/angular"
  ]
}
```

### Search for Python machine learning repos with 1000+ stars

```
{
  "searchQuery": "machine learning",
  "language": "python",
  "minStars": 1000,
  "sortBy": "stars",
  "maxResults": 100
}
```

### Find the most-forked JavaScript frameworks

```
{
  "searchQuery": "framework",
  "language": "javascript",
  "sortBy": "forks",
  "maxResults": 50
}
```

### Discover recently updated Rust projects

```
{
  "searchQuery": "async runtime",
  "language": "rust",
  "sortBy": "updated",
  "maxResults": 30
}
```

## Pricing

Pay per result — you only pay for repositories successfully scraped. See the Pricing tab for current rates. Each repository with full metadata counts as one event.

## Rate Limits & Proxies

GitHub allows unauthenticated access but may rate-limit aggressive scraping. For scrapes of 100+ repositories, enabling Apify Proxy is recommended. The actor automatically rotates user agents and adds delays to respect GitHub's servers.

## Limitations

- GitHub search returns a maximum of 1,000 results per query
- Some private or restricted repositories may not be accessible
- Contributor counts on very large repos (10,000+ contributors) may be approximate
- README preview is truncated to the first 500 characters

## Integration — Python

```
from apify_client import ApifyClient

client = ApifyClient("YOUR_API_TOKEN")
run = client.actor("sovereigntaylor/github-repo-scraper").call(run_input={
    "searchTerm": "github repo",
    "maxResults": 50
})

for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print(f"{item.get('title', item.get('name', 'N/A'))}")
```

## Integration — JavaScript

```
import { ApifyClient } from 'apify-client';
const client = new ApifyClient({ token: 'YOUR_API_TOKEN' });

const run = await client.actor('sovereigntaylor/github-repo-scraper').call({
    searchTerm: 'github repo',
    maxResults: 50
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach(item => console.log(item.title || item.name || 'N/A'));
```