---
name: github-tools
description: 'GitHub repository and workflow management via MCP for issues, PRs, code search, commits, and repository operations. Triggers: "use github-tools", "github tools", "github task".'
allowed-tools: Glob, Grep, Read
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
1. `create_pull_request(owner, repo, title, body, head, base)` → verify: output file exists + no syntax error
2. Wait for CI: `get_pull_request_status(owner, repo, pr_number)` → verify: step output matches expected outcome
3. `merge_pull_request(owner, repo, pr_number)` when checks pass → verify: all tests pass

### Review a PR
1. `get_pull_request(owner, repo, pr_number)` for overview → verify: step output matches expected outcome
2. `get_pull_request_files(owner, repo, pr_number)` to see changes → verify: step output matches expected outcome
3. `get_file_contents` for full file context if needed → verify: step output matches expected outcome
4. `create_pull_request_review(owner, repo, pr_number, body, event)` to submit → verify: output file exists + no syntax error

### Search for Implementation Examples
```
search_code(query="useOptimistic language:typescript")
```
Find real-world usage patterns across public repos.

### Manage Issues
1. `list_issues(owner, repo, state="open", labels="bug")` to find bugs → verify: file readable + content matches expected shape
2. `update_issue(owner, repo, issue_number, assignees=["user"])` to assign → verify: step output matches expected outcome
3. `add_issue_comment` to discuss → verify: package installed + import succeeds

## Tips

- Use `search_code` to find implementation patterns across all of GitHub
- `get_pull_request_status` shows CI check results — wait for green before merging
- `push_files` is more efficient than multiple `create_or_update_file` calls
- For large PRs, review `get_pull_request_files` first to prioritize critical changes

## When NOT to use

- Task is unrelated to github tools — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Github Tools needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for github tools
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving github tools
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
