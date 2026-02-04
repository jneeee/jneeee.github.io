# AGENTS.md - Development Guidelines

This is a Hugo-based static blog site (Grav万有引力). Use these guidelines when working on this repository.

## Build Commands

```bash
# Install Hugo Extended (required for SCSS/SASS processing)
wget -O hugo.deb https://github.com/gohugoio/hugo/releases/download/v0.128.0/hugo_extended_0.128.0_linux-amd64.deb
sudo dpkg -i hugo.deb

# Start local Hugo server with live reload
hugo server

# Build production site (minified)
hugo --minify

# Build with specific environment
HUGO_ENVIRONMENT=production hugo --minify

# Preview drafts
hugo server --buildDrafts

# Generate new content
hugo new content/posts/new-post.md
```

## Theme Development

```bash
# Theme uses npm/webpack for assets
cd themes/minimo_local
npm install
npm run build        # Production webpack build
npm run watch        # Watch mode for development
```

## Testing & Validation

```bash
# Check Hugo configuration
hugo config

# Validate site structure
hugo --validate

# Check for broken links (after building)
hugo --minify && find public -name "*.html" -exec grep -l "http" {} \; | xargs -I {} sh -c 'lynx -dump -listfew {} 2>/dev/null || echo "Install lynx for link checking"'

# Lint Markdown content
# Install markdownlint: npm -g markdownlint-cli
markdownlint content/

# Lint YAML front matter
# Use yamlint or similar tool on content files
```

## Code Style Guidelines

### Hugo Front Matter (YAML)

```yaml
---
date: 2017-09-27T10:00:00+06:00
lastmod: 2018-05-24T02:10:00+06:00
title: Menus Setup Guide
authors: ["Jneeee"]
draft: false
description: Brief description for SEO and previews
slug: menus  # URL slug override
toc: true    # Enable table of contents
menu:
  sidebar:
    name: Menus
    parent: docs
type: page   # or "post"
series: xxx  # content series
tags:
  - menus
categories:
  - features
---
```

**Rules:**
- Always use ISO 8601 timestamps with timezone offset
- Use `lastmod` when updating existing content
- Keep `draft: false` for published content
- Use Chinese categories and tags for Chinese posts
- Use `slug` instead of title in URL for Chinese content

### Markdown Content

- Use Chinese punctuation for Chinese content (`，`, `。`, `！`, `？`)
- Use English punctuation for English content
- Maximum line width: 120 characters
- Use fenced code blocks with language tags
- Include code comments explaining complex logic
- Use relative links for internal content: `[Link Text]({{</* ref "path" */}})`

### Hugo Templates

- Follow Go template syntax: `{{ .Title }}`, `{{ range .Pages }}`
- Use Hugo shortcodes for reusable components
- Templates are in `themes/minimo_local/layouts/`
- Template file extensions: `.html` only
- Template functions: `where`, `first`, `sort`, `groupBy`, etc.

### Naming Conventions

- Content files: kebab-case (e.g., `my-new-post.md`)
- Taxonomies: lowercase plural (`tags`, `categories`, `series`)
- Menu names: sentence case
- Template partials: snake_case (`header.html`, `footer.html`)

### Configuration (config.yml)

- YAML format with 2-space indentation
- Keep baseURL updated for deployment
- Use Chinese content language (`zh`) as default
- Preserve existing parameter structure when modifying

### Error Handling

- Hugo templates: use `with` or `if` to handle nil values
- Check `.IsNode`, `.IsPage`, `.Kind` before accessing properties
- Use `default` function for fallback values: `{{ .Param "author" | default "Jneeee" }}`
- Always validate front matter before committing

## Project Structure

```
/root/code/jneeee.github.io/
├── content/           # All markdown content
│   └── blog/          # Blog posts and pages (2023/, 2022/, page/, etc.)
├── layouts/           # Custom templates override theme
│   ├── index.html     # Homepage (custom, minimal)
│   └── _default/      # List, single, etc. overrides
├── data/              # Data files (widget config overrides)
├── static/            # Static assets (images, files)
├── archetypes/        # Content templates
├── config.yml         # Hugo configuration
├── public/            # Generated site (gitignored)
└── themes/minimo_local/
    ├── layouts/       # Theme templates
    ├── assets/        # Source assets (JS, SCSS)
    └── package.json   # npm scripts for theme assets
```

### Custom Layouts

- `layouts/index.html` - Minimal homepage (title + blog link + beian)
- `layouts/_default/list.html` - Blog list, excludes `content/blog/page/` content

## Cursor Rules

The `.cursor/rules/hugo.mdc` file contains additional guidelines:
- Write code consistent with Hugo best practices
- Avoid over-engineering; keep code concise

## Git Workflow

1. Create feature branch for content changes
2. Preview locally with `hugo server`
3. Commit with descriptive message
4. Push to trigger GitHub Pages deployment via Actions

## Content Language

- Primary language: Chinese (zh)
- English content permitted when appropriate
- Use appropriate language for code comments and documentation
