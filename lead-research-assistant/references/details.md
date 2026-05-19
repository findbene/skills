# Extended Details

## What This Skill Does

1. **Understands Your Business**: Analyzes your product/service, value proposition, and target market → verify: step output matches expected outcome
2. **Identifies Target Companies**: Finds companies that match your ideal customer profile based on: → verify: step output matches expected outcome
   - Industry and sector
   - Company size and location
   - Technology stack and tools they use
   - Growth stage and funding
   - Pain points your product solves
3. **Prioritizes Leads**: Ranks companies based on fit score and relevance → verify: step output matches expected outcome
4. **Provides Contact Strategies**: Suggests how to approach each lead with personalized messaging → verify: step output matches expected outcome
5. **Enriches Data**: Gathers relevant information about decision-makers and company context → verify: step output matches expected outcome

## Instructions

When a user requests lead research:

1. **Understand the Product/Service**
   - If in a code directory, analyze the codebase to understand the product
   - Ask clarifying questions about the value proposition
   - Identify key features and benefits
   - Understand what problems it solves

2. **Define Ideal Customer Profile**
   - Determine target industries and sectors
   - Identify company size ranges
   - Consider geographic preferences
   - Understand relevant pain points
   - Note any technology requirements

3. **Research and Identify Leads**
   - Search for companies matching the criteria
   - Look for signals of need (job postings, tech stack, recent news)
   - Consider growth indicators (funding, expansion, hiring)
   - Identify companies with complementary products/services
   - Check for budget indicators

4. **Prioritize and Score**
   - Create a fit score (1-10) for each lead
   - Consider factors like:
     - Alignment with ICP
     - Signals of immediate need
     - Budget availability
     - Competitive landscape
     - Timing indicators

5. **Provide Actionable Output**
   
   For each lead, provide:
   - **Company Name** and website
   - **Why They're a Good Fit**: Specific reasons based on their business
   - **Priority Score**: 1-10 with explanation
   - **Decision Maker**: Role/title to target (e.g., "VP of Engineering")
   - **Contact Strategy**: Personalized approach suggestions
   - **Value Proposition**: How your product solves their specific problem
   - **Conversation Starters**: Specific points to mention in outreach
   - **LinkedIn URL**: If available, for easy connection

6. **Format the Output**

   Present results in a clear, scannable format:

   ```markdown
   # Lead Research Results
   
   ## Summary
   - Total leads found: [X]
   - High priority (8-10): [X]
   - Medium priority (5-7): [X]
   - Average fit score: [X]
   
   ---
   
   ## Lead 1: [Company Name]
   
   **Website**: [URL]
   **Priority Score**: [X/10]
   **Industry**: [Industry]
   **Size**: [Employee count/revenue range]
   
   **Why They're a Good Fit**:
   [2-3 specific reasons based on their business]
   
   **Target Decision Maker**: [Role/Title]
   **LinkedIn**: [URL if available]
   
   **Value Proposition for Them**:
   [Specific benefit for this company]
   
   **Outreach Strategy**:
   [Personalized approach - mention specific pain points, recent company news, or relevant context]
   
   **Conversation Starters**:
   - [Specific point 1]
   - [Specific point 2]
   
   ---
   
   [Repeat for each lead]
   ```

7. **Offer Next Steps**
   - Suggest saving results to a CSV for CRM import
   - Offer to draft personalized outreach messages
   - Recommend prioritization based on timing
   - Suggest follow-up research for top leads

## Tips for Best Results

- **Be specific** about your product and its unique value
- **Run from your codebase** if applicable for automatic context
- **Provide context** about your ideal customer profile
- **Specify constraints** like industry, location, or company size
- **Request follow-up** research on promising leads for deeper insights

## Related Use Cases

- Drafting personalized outreach emails after identifying leads
- Building a CRM-ready CSV of qualified prospects
- Researching specific companies in detail
- Analyzing competitor customer bases
- Identifying partnership opportunities
