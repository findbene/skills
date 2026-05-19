---
name: "confluence-expert"
description: "Atlassian Confluence expert for creating and managing spaces, knowledge bases, and documentation. Triggers: 'use confluence-expert', 'confluence expert', 'confluence-expert task'."
allowed-tools: Glob, Grep, Read
---

# Atlassian Confluence Expert

Master-level expertise in Confluence space management, documentation architecture, content creation, macros, templates, and collaborative knowledge management.

## Atlassian MCP Integration

**Primary Tool**: Confluence MCP Server

**Key Operations**:

```
// Create a new space
create_space({ key: "TEAM", name: "Engineering Team", description: "Engineering team knowledge base" })

// Create a page under a parent
create_page({ spaceKey: "TEAM", title: "Sprint 42 Notes", parentId: "123456", body: "<p>Meeting notes in storage-format HTML</p>" })

// Update an existing page (version must be incremented)
update_page({ pageId: "789012", version: 4, body: "<p>Updated content</p>" })

// Delete a page
delete_page({ pageId: "789012" })

// Search with CQL
search({ cql: 'space = "TEAM" AND label = "meeting-notes" ORDER BY lastModified DESC' })

// Retrieve child pages for hierarchy inspection
get_children({ pageId: "123456" })

// Apply a label to a page
add_label({ pageId: "789012", label: "archived" })
```

**Integration Points**:
- Create documentation for Senior PM projects
- Support Scrum Master with ceremony templates
- Link to Jira issues for Jira Expert
- Provide templates for Template Creator

> **See also**: `MACROS.md` for macro syntax reference, `TEMPLATES.md` for full template library, `PERMISSIONS.md` for permission scheme details.

## Workflows

### Space Creation
1. Determine space type (Team, Project, Knowledge Base, Personal) → verify: step output matches expected outcome
2. Create space with clear name and description → verify: output file exists + no syntax error
3. Set space homepage with overview → verify: step output matches expected outcome
4. Configure space permissions: → verify: step output matches expected outcome
   - View, Edit, Create, Delete
   - Admin privileges
5. Create initial page tree structure → verify: output file exists + no syntax error
6. Add space shortcuts for navigation → verify: package installed + import succeeds
7. **Verify**: Navigate to the space URL and confirm the homepage loads; check that a non-admin test user sees the correct permission level
8. **HANDOFF TO**: Teams for content population → verify: step output matches expected outcome

### Page Architecture
**Best Practices**:
- Use page hierarchy (parent-child relationships)
- Maximum 3 levels deep for navigation
- Consistent naming conventions
- Date-stamp meeting notes

**Recommended Structure**:
```
Space Home
├── Overview & Getting Started
├── Team Information
│   ├── Team Members & Roles
│   ├── Communication Channels
│   └── Working Agreements
├── Projects
│   ├── Project A
│   │   ├── Overview
│   │   ├── Requirements
│   │   └── Meeting Notes
│   └── Project B
├── Processes & Workflows
├── Meeting Notes (Archive)
└── Resources & References
```

### Template Creation
1. Identify repeatable content pattern → verify: step output matches expected outcome
2. Create page with structure and placeholders → verify: output file exists + no syntax error
3. Add instructions in placeholders → verify: package installed + import succeeds
4. Format with appropriate macros → verify: step output matches expected outcome
5. Save as template → verify: step output matches expected outcome
6. Share with space or make global → verify: step output matches expected outcome
7. **Verify**: Create a test page from the template and confirm all placeholders render correctly before sharing with the team
8. **USE**: References for advanced template patterns → verify: step output matches expected outcome

### Documentation Strategy
1. **Assess** current documentation state → verify: step output matches expected outcome
2. **Define** documentation goals and audience → verify: step output matches expected outcome
3. **Organize** content taxonomy and structure → verify: step output matches expected outcome
4. **Create** templates and guidelines → verify: output file exists + no syntax error
5. **Migrate** existing documentation → verify: step output matches expected outcome
6. **Train** teams on best practices → verify: step output matches expected outcome
7. **Monitor** usage and adoption → verify: step output matches expected outcome
8. **REPORT TO**: Senior PM on documentation health → verify: step output matches expected outcome

### Knowledge Base Management
**Article Types**:
- How-to guides
- Troubleshooting docs
- FAQs
- Reference documentation
- Process documentation

**Quality Standards**:
- Clear title and description
- Structured with headings
- Updated date visible
- Owner identified
- Reviewed quarterly

## Essential Macros

> Full macro reference with all parameters: see `MACROS.md`.

### Content Macros
**Info, Note, Warning, Tip**:
```
{info}
Important information here
{info}
```

**Expand**:
```
{expand:title=Click to expand}
Hidden content here
{expand}
```

**Table of Contents**:
```
{toc:maxLevel=3}
```

**Excerpt & Excerpt Include**:
```
{excerpt}
Reusable content
{excerpt}

{excerpt-include:Page Name}
```

### Dynamic Content
**Jira Issues**:
```
{jira:JQL=project = PROJ AND status = "In Progress"}
```

**Jira Chart**:
```
{jirachart:type=pie|jql=project = PROJ|statType=statuses}
```

**Recently Updated**:
```
{recently-updated:spaces=@all|max=10}
```

**Content by Label**:
```
{contentbylabel:label=meeting-notes|maxResults=20}
```

### Collaboration Macros
**Status**:
```
{status:colour=Green|title=Approved}
```

**Task List**:
```
{tasks}
- [ ] Task 1
- [x] Task 2 completed
{tasks}
```

**User Mention**:
```
@username
```

**Date**:
```
{date:format=dd MMM yyyy}
```

## Page Layouts & Formatting

**Two-Column Layout**:
```
{section}
{column:width=50%}
Left content
{column}
{column:width=50%}
Right content
{column}
{section}
```

**Panel**:
```
{panel:title=Panel Title|borderColor=#ccc}
Panel content
{panel}
```

**Code Block**:
```
{code:javascript}
const example = "code here";
{code}
```

## Templates Library

> Full template library with complete markup: see `TEMPLATES.md`. Key templates summarised below.

| Template | Purpose | Key Sections |
|----------|---------|--------------|
| **Meeting Notes** | Sprint/team meetings | Agenda, Discussion, Decisions, Action Items (tasks macro) |
| **Project Overview** | Project kickoff & status | Quick Facts panel, Objectives, Stakeholders table, Milestones (Jira macro), Risks |
| **Decision Log** | Architectural/strategic decisions | Context, Options Considered, Decision, Consequences, Next Steps |
| **Sprint Retrospective** | Agile ceremony docs | What Went Well (info), What Didn't (warning), Action Items (tasks), Metrics |

## Space Permissions

> Full permission scheme details: see `PERMISSIONS.md`.

### Permission Schemes
**Public Space**:
- All users: View
- Team members: Edit, Create
- Space admins: Admin

**Team Space**:
- Team members: View, Edit, Create
- Team leads: Admin
- Others: No access

**Project Space**:
- Stakeholders: View
- Project team: Edit, Create
- PM: Admin

## Content Governance

**Review Cycles**:
- Critical docs: Monthly
- Standard docs: Quarterly
- Archive docs: Annually

**Archiving Strategy**:
- Move outdated content to Archive space
- Label with "archived" and date
- Maintain for 2 years, then delete
- Keep audit trail

**Content Quality Checklist**:
- [ ] Clear, descriptive title
- [ ] Owner/author identified
- [ ] Last updated date visible
- [ ] Appropriate labels applied
- [ ] Links functional
- [ ] Formatting consistent
- [ ] No sensitive data exposed

## Decision Framework

**When to Escalate to Atlassian Admin**:
- Need org-wide template
- Require cross-space permissions
- Blueprint configuration
- Global automation rules
- Space export/import

**When to Collaborate with Jira Expert**:
- Embed Jira queries and charts
- Link pages to Jira issues
- Create Jira-based reports
- Sync documentation with tickets

**When to Support Scrum Master**:
- Sprint documentation templates
- Retrospective pages
- Team working agreements
- Process documentation

**When to Support Senior PM**:
- Executive report pages
- Portfolio documentation
- Stakeholder communication
- Strategic planning docs

## Handoff Protocols

**FROM Senior PM**:
- Documentation requirements
- Space structure needs
- Template requirements
- Knowledge management strategy

**TO Senior PM**:
- Documentation coverage reports
- Content usage analytics
- Knowledge gaps identified
- Template adoption metrics

**FROM Scrum Master**:
- Sprint ceremony templates
- Team documentation needs
- Meeting notes structure
- Retrospective format

**TO Scrum Master**:
- Configured templates
- Space for team docs
- Training on best practices
- Documentation guidelines

**WITH Jira Expert**:
- Jira-Confluence linking
- Embedded Jira reports
- Issue-to-page connections
- Cross-tool workflow

## Best Practices

**Organization**:
- Consistent naming conventions
- Meaningful labels
- Logical page hierarchy
- Related pages linked
- Clear navigation

**Maintenance**:
- Regular content audits
- Remove duplication
- Update outdated information
- Archive obsolete content
- Monitor page analytics

## Analytics & Metrics

**Usage Metrics**:
- Page views per space
- Most visited pages
- Search queries
- Contributor activity
- Orphaned pages

**Health Indicators**:
- Pages without recent updates
- Pages without owners
- Duplicate content
- Broken links
- Empty spaces

## Related Skills

- **Jira Expert** (`project-management/jira-expert/`) — Jira issue macros and linking complement Confluence docs
- **Atlassian Templates** (`project-management/atlassian-templates/`) — Template patterns for Confluence content creation

## When NOT to use

- Task is unrelated to confluence expert — pick a domain-specific skill instead
- Simple one-line operation that doesn't need this skill's structure
- User explicitly asks for raw output without skill discipline → respect override
- Different toolchain / framework required → search with `find-skills` for alternatives

## Red Flags

| Thought | Reality |
|---------|---------|
| "Output looks right, skip verify" | Eyeball checks miss edge cases — run the verify step |
| "Generic template is good enough" | Confluence Expert needs domain-specific judgment, not boilerplate |
| "I'll inline the context, no need to read references" | Context drift produces stale output; check linked references |
| "One more shortcut won't hurt" | Shortcuts compound — finish the discipline before declaring done |

## Output Contract

Done when:
- Primary deliverable produced matches user's stated goal for confluence expert
- Every verify step in the process passed
- Edge cases addressed or explicitly flagged with assumption
- Output reproducible — no hidden state or one-time setup
- Brief hand-off summary so user can validate without rereading the full flow

## Examples

### Example 1 — golden path
- Input: standard user request involving confluence expert
- Action: follow the documented numbered process with verify clauses at each step
- Output: deliverable matching the Output Contract above

### Example 2 — edge case
- Input: request with partial info, non-standard constraint, or conflicting requirements
- Action: detect the gap, surface a clarifying question OR document the assumption explicitly, then proceed with adapted process
- Output: deliverable + explicit note on the assumption/limitation taken
