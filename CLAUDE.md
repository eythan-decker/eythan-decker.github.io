# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal portfolio and blog site for Eythan Decker built with Hugo and the Stack theme. The site is hosted on GitHub Pages at `https://eythan-decker.github.io/`.

**Tech Stack:**
- Hugo (Extended version required for Stack theme)
- Hugo Theme: Stack (`CaiJimmy/hugo-theme-stack`)
- Deployment: GitHub Actions → GitHub Pages (`gh-pages` branch)

## Repository Structure

```
config/
  _default/
    config.toml    # Main Hugo configuration
    menu.toml      # Navigation menus
    params.toml    # Theme parameters
content/
  page/            # Static pages (About, Projects, Links)
  post/            # Blog posts
static/
  img/             # Static images (avatar, etc.)
themes/stack/      # Stack theme (git submodule or Hugo modules)
.github/
  workflows/
    deploy.yml     # Build and deploy workflow
```

## Development Commands

### Local Development
```bash
# Serve site locally with drafts
hugo server -D

# Serve site with live reload (default)
hugo server

# Build site locally
hugo --minify --gc
```

### Content Management
```bash
# Create new blog post
hugo new post/<slug>/index.md

# Create new page
hugo new page/<name>/index.md
```

### Testing
```bash
# Build to verify no errors
hugo --minify --gc

# Optional: Markdown linting
markdownlint-cli2 "content/**/*.md"
```

### Browser Testing with Chrome MCP

When implementing or modifying features, use the Chrome MCP tools to verify the Hugo site in the browser:

**Required checks:**
1. **Theme loading** - Verify the Stack theme assets (CSS, fonts, images) load correctly
2. **JavaScript libraries** - Ensure all JS dependencies load without errors
3. **Console errors** - Check browser console for JavaScript errors, failed network requests, or warnings
4. **UI component validation** - Confirm that UI components match the spec.md requirements and feature being implemented
5. **Responsive behavior** - Verify layout works across different viewport sizes if applicable
6. **Dark mode** - Confirm dark mode is applied by default and functioning

**Workflow:**
1. Start Hugo dev server: `hugo server -D`
2. Use Chrome MCP to navigate to `http://localhost:1313`
3. Check browser console for errors
4. Validate visual appearance against spec and requirements
5. Test navigation, search, and interactive components

## Branching Strategy

**Main branch is protected** - never commit directly to `main`. All work must be done in separate branches and merged via pull request.

### Branch Naming Conventions

- `feature/` - New features or website additions
  - Example: `feature/new-theme`, `feature/analytics-integration`

- `fix/` - Bug fixes and corrections
  - Example: `fix/format-error-on-linkedin-link`, `fix/broken-search`

- `chore/` - Tech debt, style updates, dependency updates
  - Example: `chore/library-update`, `chore/hugo-upgrade`

- `blog/` - Publishing new blog post content
  - Example: `blog/cloud-cost-savings`, `blog/gitops-patterns`

### Git Commit Messages

- Keep commits concise: 1-2 sentences maximum
- No "Co-Authored-By: Claude" or similar attribution in commit messages
- Use conventional commit style when appropriate

## Pull Request Process

### PR Title Format
Use conventional commit style:
- `chore:` - Routine tasks, dependency updates, maintenance
- `feat:` - New features or applications
- `fix:` - Bug fixes and corrections
- `docs:` - Documentation updates
- `refactor:` - Code restructuring without functional changes
- `blog:` - New blog post content

**Example:** `chore: upgrade Hugo to v0.120.0`

### PR Template (for non-blog branches)

```markdown
## Summary

[1-2 sentences describing what the PR does and why]

## Changes

- **file/path**: Brief description of what changed
- **file/path**: Brief description of what changed
- **file/path**: Brief description of what changed

## Technical Details

[Optional section for complex changes]

### [Subsection Title if Needed]
[Technical explanation, architecture notes, or breaking changes]

**Before:**
```yaml
[code example if applicable]
```

**After:**
```yaml
[code example if applicable]
```

### References
- [Link to documentation]
- [Link to related issue/PR]
- [Link to upstream changelog]
```

### Blog Branch PRs
Blog branches can use a simpler format focusing on the post title and summary.

## Content Guidelines

### Blog Post Front Matter Template
```yaml
---
title: "Your Post Title"
date: 2024-01-08
draft: false
description: "Short summary of the post"
categories:
  - Platform Engineering
tags:
  - kubernetes
  - gitops
---
```

### Categories (Suggested)
- Platform Engineering
- DevEx / Enablement
- GitOps / Kubernetes
- CI/CD
- Cloud Cost / FinOps
- AI for Engineering Teams
- Notes

### Required Pages
- `/about/` - Professional bio and highlights
- `/projects/` - Project portfolio
- `/links/` - Social links (GitHub, LinkedIn, Email)

## Theme Configuration Notes

The Stack theme uses a sidebar layout with:
- Avatar and social icons
- Dark mode as default
- Built-in local search
- Archive and taxonomy pages (categories/tags)

Social links are configured in `config/_default/menu.toml` under `menu.social`.

## Deployment

GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:
1. Builds the Hugo site with `hugo --minify --gc`
2. Deploys to `gh-pages` branch
3. GitHub Pages serves from `gh-pages` branch

Base URL: `https://eythan-decker.github.io/`

## Opening Pull Requests

Use the GitHub MCP tools to open PRs to main branch. Always follow the PR template above for non-blog branches.
