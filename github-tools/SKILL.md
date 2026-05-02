---
name: github-tools
description: "GitHub repository and workflow management via MCP for issues, PRs, code search, commits, and repository operations. Use this skill any time GitHub repos need to be managed programmatically, issues need to be created or searched, PRs need to be worked with via API, or code search on GitHub is needed. Trigger immediately on: \"GitHub\", \"create issue\", \"GitHub PR\", \"GitHub API\", \"repository\", \"GitHub search\", \"code search\", \"create PR\", \"GitHub issue\", \"repo management\", \"fork\", \"GitHub MCP\", \"GitHub workflow\", \"push files\". If someone says \"create a GitHub issue\" or \"search GitHub for X\" this skill MUST trigger."
---

# GitHub Tools

Manage repositories, issues, pull requests, and code search via the GitHub API.

## Available Tools

### Pull Requests
| Tool | Purpose |
|------|---------|
| `create_pull_request` | Create a new PR |
| `get_pull_request` | Get PR details |
| `list_pull_requests` | List PRs with filters |
| `get_pull_request_files` | See changed files in a PR |
| `get_pull_request_comments` | Read PR comments |
| `get_pull_request_reviews` | Get review status |
| `get_pull_request_status` | Check CI/status checks |
| `create_pull_request_review` | Submit a review |
| `merge_pull_request` | Merge a PR |
| `update_pull_request_branch` | Update PR branch with base |

### Issues
| Tool | Purpose |
|------|---------|
| `create_issue` | Create a new issue |
| `get_issue` | Get issue details |
| `list_issues` | List issues with filters |
| `update_issue` | Edit issue title/body/labels/state |
| `add_issue_comment` | Comment on an issue |

### Repository
| Tool | Purpose |
|------|---------|
| `create_repository` | Create a new repo |
| `fork_repository` | Fork a repo |
| `create_branch` | Create a branch remotely |
| `get_file_contents` | Read a file from a repo |
| `create_or_update_file` | Create or edit a file via API |
| `push_files` | Push multiple files at once |
| `list_commits` | View commit history |

### Search
| Tool | Purpose |
|------|---------|
| `search_code` | Search code across all of GitHub |
| `search_issues` | Search issues/PRs across repos |
| `search_repositories` | Find repos by topic/language |
| `search_users` | Find GitHub users |

## Common Workflows

### Create and Merge a PR
1. `create_pull_request(owner, repo, title, body, head, base)`
2. Wait for CI: `get_pull_request_status(owner, repo, pr_number)`
3. `merge_pull_request(owner, repo, pr_number)` when checks pass

### Review a PR
1. `get_pull_request(owner, repo, pr_number)` for overview
2. `get_pull_request_files(owner, repo, pr_number)` to see changes
3. `get_file_contents` for full file context if needed
4. `create_pull_request_review(owner, repo, pr_number, body, event)` to submit

### Search for Implementation Examples
```
search_code(query="useOptimistic language:typescript")
```
Find real-world usage patterns across public repos.

### Manage Issues
1. `list_issues(owner, repo, state="open", labels="bug")` to find bugs
2. `update_issue(owner, repo, issue_number, assignees=["user"])` to assign
3. `add_issue_comment` to discuss

## Tips

- Use `search_code` to find implementation patterns across all of GitHub
- `get_pull_request_status` shows CI check results — wait for green before merging
- `push_files` is more efficient than multiple `create_or_update_file` calls
- For large PRs, review `get_pull_request_files` first to prioritize critical changes
