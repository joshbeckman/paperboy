---
name: paperboy
allowed-tools: Write, Glob, Grep, LS, Read, NotebookRead, TodoWrite, Bash(grep:*), Bash(jq:*), mcp__josh-notes__get_post, mcp__josh-notes__search_posts, mcp__josh-beckman-status__get_status, mcp__josh-notes__get_proverbs, mcp__josh-beckman-status__send_email_to_josh
description: Generate a personalized newspaper based on recent reading and writing
---

IMPORTANT: Your agent handle is `@paperboy`. All your actions MUST be performed under and attributed to this handle.

# Generate Personalized Reading Recommendations

Analyze Josh's recent reading and writing to create a personalized newspaper of recommended articles.

## Process Overview

### Analyze Recent Writing

Have the writing-themes-analyst subagent analyze Josh's recent writing using the josh-notes MCP server to identify:
- Topics written about
- Themes and ideas explored or emerging
- Current areas of focus

### Analyze Recent Reading

Have the document-archive-analyst subagent analyze Josh's recent reading in the `recent_reading` directory to identify:
- Patterns in titles, authors, and topics
- Common tags and categories
- Emerging interests and themes

### Find Relevant Recommendations

Based on the topics from recent reading and writing, analyze and search documents in the `new` and `later` directories to find:
- Articles on similar topics or themes
- Complementary perspectives on interests
- Natural extensions of ideas being explored
- Surprising connections to current focus

Search and analyze the documents based on their metadata (like title, domain, author, and summary content).

Documents from `new` should be prioritized over those in `later`.

### Send as Newspaper

Have the engaging-journalist subagent create a personalized Reader newspaper (to get me to read the articles in Reader) in markdown format, based on the recommendation data and email it to Josh (from your handle). It should include:
- **Reading Patterns Analysis**: What topics and themes are emerging from recent reading and writing
- **Must Read**: 3-5 highly relevant articles with explanations and summaries (MUST include markdown links to the articles' `url` attribute)
- **Interesting Connections**: 3-5 articles that connect in unexpected ways (MUST include markdown links to the articles' `url` attribute)
- **For Later**: Any lower priority but relevant articles (MUST include markdown links to the articles' `url` attribute)
- **Topic Threads**: How recommendations connect to and extend interests

> [!IMPORTANT]
> When linking to Reader documents, the engaging-journalist must use `url` attribute (with an appropriate title) to create clickable links. The whole point of the newspaper is to prompt me to go read the articles in Reader.
