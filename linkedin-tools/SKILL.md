---
name: linkedin-tools
description: "LinkedIn profile, company, job, and post data scraping via browser automation MCP. Use this skill any time LinkedIn data needs to be extracted, profiles need to be scraped, job listings need to be gathered, or company research needs to be done via LinkedIn. Trigger immediately on: \"LinkedIn scrape\", \"LinkedIn profile\", \"scrape LinkedIn\", \"LinkedIn company\", \"LinkedIn job\", \"LinkedIn data\", \"LinkedIn post\", \"LinkedIn MCP\", \"extract LinkedIn\", \"LinkedIn search\", \"company LinkedIn\", \"LinkedIn research\", \"LinkedIn automation\". If someone says \"scrape this LinkedIn profile\" or \"get data from LinkedIn\" this skill MUST trigger."
---

# LinkedIn Tools

Scrape and analyze LinkedIn data via the `linkedin` MCP server (Patchright browser automation).

## Available Tools

| Tool | Purpose | Input |
|------|---------|-------|
| `get_person_profile` | Get detailed profile info | LinkedIn profile URL + section selection |
| `get_company_profile` | Extract company information | Company LinkedIn URL + section selection |
| `get_company_posts` | Get recent posts from a company | Company LinkedIn URL |
| `search_jobs` | Search jobs by keywords and location | Keywords, location, filters |
| `search_people` | Search for people by keywords/location | Keywords, location |
| `get_job_details` | Get full details of a job posting | Job posting URL |
| `close_session` | Close browser and clean up resources | None |

## Common Workflows

### Research a Person
Call `get_person_profile` with the LinkedIn URL and select which sections to retrieve:
- `experience` — work history
- `education` — schools and degrees
- `interests` — followed topics/influencers
- `honors` — certifications and awards
- `languages` — spoken languages
- `contact_info` — email, phone, websites
- `posts` — recent activity

**Example:**
```
get_person_profile({ url: "https://www.linkedin.com/in/johndoe/", sections: ["experience", "education", "contact_info"] })
```

### Research a Company
Call `get_company_profile` with the company URL and optional sections:
- `posts` — recent company posts
- `jobs` — open positions

**Example:**
```
get_company_profile({ url: "https://www.linkedin.com/company/anthropic/", sections: ["posts", "jobs"] })
```

### Job Search
1. `search_jobs({ keywords: "software engineer", location: "Atlanta, GA" })` to find listings
2. `get_job_details({ url: "https://www.linkedin.com/jobs/view/12345" })` for full details

### People Search
```
search_people({ keywords: "wedding photographer", location: "Atlanta" })
```

## Gotchas
- Requires a valid LinkedIn session. Run `uvx linkedin-scraper-mcp --login` to create/refresh the session.
- Sessions expire periodically — re-run `--login` if you get authentication errors.
- This uses browser scraping (Patchright), not an official API. May break if LinkedIn changes their UI.
- Rate limit yourself — too many rapid requests may trigger LinkedIn's anti-bot measures.
- Only one LinkedIn browser session can be active at a time.
- Read-only: cannot post content, send messages, or accept connections.
- Pages with heavy JavaScript may need increased timeout (`--timeout 10000`).
- Company posts and job details may take several seconds to load.
