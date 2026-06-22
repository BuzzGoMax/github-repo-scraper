[Github Repo Scraper](https://apify.com/dash_authority/github-repo-scraper?fpr=data)

# GitHub Repository Scraper

Search and extract GitHub repository data — stars, forks, language, description, topics, license, and owner info. No API key required. Uses GitHub's public REST API under the hood.

## Use Cases

**Tech Market Research:** Find all repos in a niche. How many Python web frameworks have 1,000+ stars? Who's actively maintained? What licenses dominate?

**Developer Prospecting:** Identify repo owners and contributors by language and topic. Useful for recruiting or partnership outreach.

**Open Source Monitoring:** Track stars, forks, and issue counts over time. Spot trending projects before they blow up.

**Competitive Analysis:** Compare repos in the same space. Which CLI tool has the most traction? Which library gets updated most often?

---

## Input

| Field | Type | Description |
| --- | --- | --- |
| query | string | Search keywords (required) |
| language | string | Filter by language (e.g., "Python", "TypeScript") |
| topic | string | Filter by topic (e.g., "machine-learning") |
| sort | string | Sort by: stars, forks, updated, or best-match (default) |
| minStars | integer | Minimum star count filter |
| maxResults | integer | Max repos to return (default: 30) |
| proxyConfiguration | object | Apify proxy settings |

### Search Tips

- GitHub search supports qualifiers directly in `query`: `"web scraper language:Python stars:>500"`
- Use `minStars` to filter out noise. Setting it to 100+ gives you repos people actually use.
- `sort: "updated"` catches recently active projects — good for finding maintained alternatives to abandoned ones.

---

## Output

Each result is a repository profile:

```
{
  "name": "crawl4ai",
  "fullName": "unclecode/crawl4ai",
  "url": "https://github.com/unclecode/crawl4ai",
  "description": "Open-source LLM Friendly Web Crawler & Scraper",
  "language": "Python",
  "stars": 64028,
  "forks": 6562,
  "watchers": 64028,
  "openIssues": 69,
  "topics": ["web-scraping", "llm", "crawler"],
  "license": "Apache-2.0",
  "defaultBranch": "main",
  "createdAt": "2024-05-09T09:48:50Z",
  "updatedAt": "2026-04-15T12:18:34Z",
  "pushedAt": "2026-04-11T09:27:40Z",
  "size": 150467,
  "archived": false,
  "fork": false,
  "owner": {
    "login": "unclecode",
    "type": "User",
    "avatarUrl": "https://avatars.githubusercontent.com/u/12494079?v=4"
  }
}
```

### Key Fields

- `fullName` — owner/repo format, unique identifier
- `stars`, `forks`, `watchers` — traction metrics
- `openIssues` — active issue count (high can mean active development or poor maintenance)
- `topics` — GitHub topic tags for categorization
- `license` — SPDX license identifier
- `pushedAt` — last commit date — the best signal for "is this maintained?"
- `size` — repo size in KB
- `archived` / `fork` — filter out dead or derivative repos

---

## Integrations & API

- **Export formats:** JSON, CSV, Excel, XML, HTML
- **Scheduling:** Track star growth weekly to spot trends.
- **API:** Apify Python or Node.js client for automated analysis pipelines.
- **Combine with:** Other scrapers — scrape a repo's README, then its contributors, then its dependencies.

---

## FAQ

**Does this use GitHub's official API?** Yes — GitHub's public REST API. No authentication token needed for public repos. Rate limits apply (60 requests/hour unauthenticated).

**Can I scrape private repos?** No. This scraper only works with public repositories.

**How many results can I get?** GitHub's search API caps at 1,000 results per query. For larger datasets, break your search into smaller chunks by language or topic.

**What's the difference between `stars` and `watchers`?** They're identical for public repos since GitHub unified them. Both reflect the star count.

**Is `pushedAt` reliable for checking maintenance?** It's the best quick signal, but a repo with a recent `pushedAt` might just have CI config updates. Check the commit history for real activity.

---

## Support

Open an issue in the **Issues** tab for bugs or feature requests.