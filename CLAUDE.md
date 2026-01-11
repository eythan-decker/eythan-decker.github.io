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
assets/
  icons/           # Custom icon SVG files (Tabler Icons)
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

## Implementation Planning

**All work must use an implementation plan in a markdown file** to track progress, notes, issues, and next steps.

### Purpose
Implementation plans serve as:
- Progress tracking across sessions
- Issue documentation (what broke, how it was fixed)
- Context preservation for future work
- Verification checklist for testing

### Plan File Structure
Create `implementation-plan.md` (or `<feature-name>-plan.md`) in the repo root with:

```markdown
# [Feature/Task Name] - Implementation Plan

## Implementation Status: [NOT STARTED | IN PROGRESS | COMPLETED]

## Implementation Checklist
- [ ] Phase 1: [Name]
  - [ ] Step 1
  - [ ] Step 2
- [ ] Phase 2: [Name]

## Issues Found & Fixed
**Issue: [Description]**
- Cause: [Root cause]
- Fix: [Solution applied]

## Browser Testing Checklist
- [ ] Theme loads correctly
- [ ] No console errors
- [ ] All navigation works

## Current Status (YYYY-MM-DD)
**Status:** [Current state]
**Branch:** [branch-name]
**Commits:** [List key commits]

## Current Issues
[List any blocking or known issues]

## Next Issue to Work On
[Clear description of the next task]

## Notes
- [Important learnings]
- [Configuration gotchas]
- [Future considerations]
```

### When to Create a Plan
- Any feature work (`feature/` branches)
- Complex fixes requiring multiple steps
- Chores involving multiple files or configuration changes
- Any work spanning multiple sessions

Blog posts (`blog/` branches) can use simplified tracking if needed.

### Updating the Plan
- Mark checkboxes as you complete steps
- Document all errors/issues encountered
- Update "Current Status" section regularly
- Always record "Next Issue to Work On" before ending a session

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

### Social Icons Configuration

Social links are configured in `config/_default/menu.toml` under `menu.social`.

**Icon System (Tabler Icons):**
- The Stack theme uses Tabler Icons (MIT-licensed SVG icon set)
- Built-in icons include: `brand-github`, `search`, `home`, `tag`, `link`, etc.
- Custom icons can be added by placing SVG files in `assets/icons/` directory

**Adding Custom Icons:**
1. Download SVG from [Tabler Icons](https://tabler.io/icons)
2. Place in `assets/icons/` directory (e.g., `brand-linkedin.svg`, `mail.svg`)
3. Reference in `menu.toml` using `[social.params]` syntax:

```toml
[[social]]
  identifier = "github"
  name = "GitHub"
  url = "https://github.com/username"
  [social.params]
    icon = "brand-github"
```

**Note:** Icon filename must match the `icon` parameter exactly (e.g., `brand-linkedin` → `brand-linkedin.svg`)

## Common Issues & Gotchas

### Duplicate Navigation Links
**Issue:** Navigation links appear twice in the sidebar

**Cause:** Hugo merges menu entries from multiple sources:
- `config/_default/menu.toml` (main configuration)
- Content page frontmatter (`menu:` blocks)

If the same menu item is defined in both places, Hugo will render it twice.

**Solution:** Choose one source of truth:
- **Recommended:** Define all menus in `menu.toml`, remove `menu:` blocks from page frontmatter
- **Alternative:** Define menus only in page frontmatter (not recommended for non-content items like "Home")

### Generic Social Icons
**Issue:** Social links show generic link icons instead of branded icons

**Cause:** No `icon` parameter specified in `menu.toml`

**Solution:** Add `[social.params]` section with `icon = "icon-name"` parameter. Ensure the corresponding SVG file exists in `assets/icons/` directory.

## Deployment

GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:
1. Builds the Hugo site with `hugo --minify --gc`
2. Deploys to `gh-pages` branch
3. GitHub Pages serves from `gh-pages` branch

Base URL: `https://eythan-decker.github.io/`

## Opening Pull Requests

Use the GitHub MCP tools to open PRs to main branch. Always follow the PR template above for non-blog branches.
