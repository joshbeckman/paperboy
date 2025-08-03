# Paperboy

A Ruby-based tool that surfaces your [Readwise](https://readwise.io/) [Reader](https://readwise.io/read) documents, creating a personalized newspaper from your reading list.

## Overview

Paperboy fetches [Reader](https://readwise.io/read) documents from a [Readwise](https://readwise.io/) account and exports them to local JSON files, organizing them by location (new, later, recent). It then uses Claude AI with MCP (Model Context Protocol) tools to generate a personalized newspaper and email it to Josh. 

The system is implemented as a [Claude Code slash command](https://docs.anthropic.com/en/docs/claude-code/slash-commands) and uses a [Claude subagent](https://docs.anthropic.com/en/docs/claude-code/sub-agents) to analyze Josh's recent writing through his site's MCP server (see [I Built an MCP Server for My Site](https://www.joshbeckman.org/blog/i-built-an-mcp-server-for-my-site)), ensuring the newspaper content aligns with his current interests and writing themes.

> [!NOTE]
> Others should fork this project and modify the analysis stages and delivery method to suit their needs.

## Features

- Export documents from specific Readwise locations (`new`, `later`, `archive`)
- Fetch recently updated documents (last 2 weeks)
- Organize exports into separate directories
- Generate timestamped, slug-based filenames
- Launch Claude AI to create a personalized newspaper from the new/later reading materials
- Use Claude subagents to analyze recent writing themes and interests
- Email the generated newspaper to Josh using MCP tools

## Prerequisites

- Ruby (version specified in `.ruby-version`)
- Bundler
- A Readwise account with API access
- Claude CLI (for newspaper generation)

## Dependencies

This project uses the [readwise-ruby](https://github.com/joshbeckman/readwise-ruby) gem, a Ruby Readwise API client and highlight parsing library.

## Installation

1. Clone the repository:
```bash
git clone https://github.com/joshbeckman/paperboy.git
cd paperboy
```

2. Install dependencies:
```bash
bundle install
```

3. Set up your Readwise API token:
```bash
export READWISE_TOKEN="your-readwise-api-token"
```

## Usage

### Export all documents and generate newspaper (default task)
```bash
rake
```

This will:
1. Export documents from your "new" and "later" Readwise locations
2. Export documents updated in the last 2 weeks to `recent_reading/`
3. Launch Claude to generate a personalized newspaper
4. Email the newspaper to Josh using MCP tools

### Using as a Claude Code Slash Command

Paperboy is configured as a Claude Code slash command. Simply run:
```bash
/paperboy
```

### Export only inbox documents
```bash
rake readwise:export_inbox
```

Exports documents from "new" and "later" locations to their respective directories.

### Export only recent documents
```bash
rake readwise:export_recent
```

Exports documents updated in the last 2 weeks to the `recent_reading/` directory.

## Directory Structure

After running the export tasks, your directory will contain:

```
paperboy/
├── new/              # Documents from "new" location
├── later/            # Documents from "later" location
├── recent_reading/   # Recently updated documents
└── *.json           # Exported document files
```

## File Naming Convention

Exported files use the following naming pattern:
```
YYYYMMDDHHMMSS-slugified-title.json
```

Example: `20240328222951-showing-mastodon-reactions-on-a-statamic-website.json`

## Development

The project includes development tools configured via:
- `.rubocop.yml` - Ruby style guide enforcement
- `Gemfile` - Ruby dependencies

To run the linter:
```bash
bundle exec rubocop -a
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Customization

### Changing the Delivery Method

By default, paperboy emails the generated newspaper to Josh using MCP tools. To customize this for your own use:

1. Fork the repository
2. Modify the `paperboy` Claude command to use your preferred delivery method
3. Update the MCP configuration or implement your own delivery mechanism

### Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Author

[Josh Beckman](https://www.joshbeckman.org/) ([@joshbeckman](https://github.com/joshbeckman))
