# Eythan Decker - Portfolio & Blog

![Deploy](https://github.com/eythan-decker/eythan-decker.github.io/actions/workflows/deploy.yml/badge.svg)

Personal portfolio and blog site for platform engineering, developer enablement, and building reliable systems at scale.

**Live Site:** [https://eythan-decker.github.io/](https://eythan-decker.github.io/)

## Tech Stack

- **Static Site Generator:** [Hugo](https://gohugo.io/) (Extended version)
- **Theme:** [Stack](https://github.com/CaiJimmy/hugo-theme-stack) by CaiJimmy
- **Hosting:** GitHub Pages
- **Deployment:** GitHub Actions (automated on merge to `main`)

## Features

- Dark mode by default with light mode toggle
- Built-in search functionality
- Responsive design with sidebar navigation
- Archive and tag cloud widgets
- Syntax highlighting for code blocks
- RSS feed support

## Quick Start

### Prerequisites

- Hugo Extended v0.124.0 or later ([installation guide](https://gohugo.io/installation/))
- Git

### Local Development

```bash
# Clone the repository
git clone https://github.com/eythan-decker/eythan-decker.github.io.git
cd eythan-decker.github.io

# Start the development server
hugo server -D

# Site will be available at http://localhost:1313
```

### Creating Content

**New blog post:**
```bash
hugo new post/<post-slug>/index.md
```

**New page:**
```bash
hugo new page/<page-name>/index.md
```

### Building

```bash
# Build for production
hugo --minify --gc
```

## Project Structure

```
.
├── config/
│   └── _default/          # Site configuration
│       ├── config.toml    # Base Hugo settings
│       ├── menu.toml      # Navigation menus
│       └── params.toml    # Theme parameters
├── content/
│   ├── page/              # Static pages (About, Projects, Links)
│   └── post/              # Blog posts
├── static/
│   └── img/               # Static images (avatar, etc.)
├── assets/
│   └── icons/             # Custom SVG icons
└── .github/
    └── workflows/
        └── deploy.yml     # Automated deployment
```

## Content Guidelines

### Blog Post Frontmatter

```yaml
---
title: "Your Post Title"
description: "Short summary (1-2 sentences)"
date: 2024-01-10
slug: "post-slug"
categories:
  - Platform Engineering
tags:
  - kubernetes
  - gitops
draft: false
image: cover.jpg  # Optional thumbnail
---
```

### Suggested Categories

- Platform Engineering
- DevEx / Enablement
- GitOps / Kubernetes
- CI/CD
- Cloud Cost / FinOps
- AI for Engineering Teams
- Notes

### Adding Images

Place images in the post directory alongside `index.md`:

```
content/post/my-post/
├── index.md
├── cover.jpg      # Homepage thumbnail
└── diagram.png    # Inline images
```

Reference in frontmatter for homepage thumbnail:
```yaml
image: cover.jpg
```

Reference in content:
```markdown
![Diagram](diagram.png)
```

## Branching Strategy

**Main branch is protected.** All changes must go through pull requests.

### Branch Naming

- `blog/<post-slug>` - New blog posts
- `feature/<feature-name>` - New features or site additions
- `fix/<issue-description>` - Bug fixes
- `chore/<task-name>` - Maintenance, updates

### Workflow

```bash
# Create feature branch
git checkout -b blog/my-new-post

# Make changes
# ...

# Commit changes
git add .
git commit -m "blog: add new post about Kubernetes patterns"

# Push to GitHub
git push origin blog/my-new-post

# Open pull request on GitHub
# Merge to main after review
# GitHub Actions deploys automatically
```

## Deployment

The site deploys automatically via GitHub Actions when changes are merged to `main`.

**Workflow:**
1. GitHub Actions builds the site with `hugo --minify --gc`
2. Output is pushed to the `gh-pages` branch
3. GitHub Pages serves the site from `gh-pages`

**Deployment typically takes 2-3 minutes after merge.**

## Configuration

### Customizing the Sidebar

Edit `config/_default/params.toml`:

```toml
[sidebar]
  emoji = "👨‍💻"
  subtitle = "Your subtitle here"

  [sidebar.avatar]
    enabled = true
    local = true
    src = "img/headshot.png"
```

### Navigation Links

Edit `config/_default/menu.toml`:

```toml
[[main]]
  identifier = "about"
  name = "About"
  url = "/about/"
  weight = 10
```

### Social Links

Configure in `menu.toml` using [Tabler Icons](https://tabler.io/icons):

```toml
[[social]]
  identifier = "github"
  name = "GitHub"
  url = "https://github.com/yourusername"
  [social.params]
    icon = "brand-github"
```

Custom icons can be added as SVG files in `assets/icons/`.

## Theme

This site uses the Stack theme via Hugo Modules:

```bash
# Update theme
hugo mod get -u

# Clean module cache
hugo mod tidy
```

**Theme Documentation:** [stack.jimmycai.com](https://stack.jimmycai.com/)

## Development

### Testing Locally

```bash
# Serve with drafts
hugo server -D

# Serve without drafts (production preview)
hugo server

# Build to verify
hugo --minify --gc
```

### Troubleshooting

**Post not showing up?**
- Check `draft: false` in frontmatter
- Verify date is not in the future
- Ensure post is in `content/post/` directory

**Images not loading?**
- Check file path and filename (case-sensitive)
- Restart dev server
- Verify image is in post directory or `static/`

**Build errors?**
- Validate YAML frontmatter syntax
- Run `hugo --verbose` for detailed errors

## Contributing

This is a personal site, but suggestions and bug reports are welcome via issues.

## License

Content is © 2026 Eythan Decker. Theme (Stack) is MIT licensed.

## Resources

- **Hugo Documentation:** [gohugo.io/documentation](https://gohugo.io/documentation/)
- **Stack Theme:** [github.com/CaiJimmy/hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack)
- **Tabler Icons:** [tabler.io/icons](https://tabler.io/icons)
- **Site Development Guide:** See `CLAUDE.md` for detailed development workflows

## Connect

- **GitHub:** [@eythan-decker](https://github.com/eythan-decker)
- **LinkedIn:** [eythandecker](https://www.linkedin.com/in/eythandecker/)
- **Email:** [eythandecker@gmail.com](mailto:eythandecker@gmail.com)
