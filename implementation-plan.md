# Hugo Stack Theme MVP - Implementation Plan

This document tracks the implementation and verification of the Hugo site with Stack theme.

## Implementation Checklist

### Phase 1: Foundation

- [x] Create `go.mod` with Hugo module declaration
- [x] Create `config/_default/config.toml` with base Hugo config
- [x] Create `config/_default/params.toml` with Stack theme parameters
- [x] Create `config/_default/menu.toml` with navigation and social links
- [ ] Run `hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3` to fetch theme
- [ ] Run `hugo mod tidy` to clean up module dependencies

### Phase 2: Static Assets

- [x] Create `static/img/` directory
- [x] Create `static/img/avatar.svg` placeholder avatar

### Phase 3: Content Pages

- [x] Create `content/page/about/index.md`
- [x] Create `content/page/projects/index.md`
- [x] Create `content/page/links/index.md`

### Phase 4: Blog Posts

- [x] Create `content/post/hello-world/index.md`
- [x] Create `content/post/cloud-cost-savings/index.md`

### Phase 5: Deployment Infrastructure

- [x] Create `.github/workflows/deploy.yml`
- [x] Update `.gitignore` to include `spec.md`
- [ ] Stage and commit `.gitignore` and `CLAUDE.md`

### Phase 6: Testing & Verification

- [ ] Initialize Hugo modules (fetch theme)
- [ ] Build site locally (`hugo --minify --gc`)
- [ ] Start development server (`hugo server -D`)
- [ ] Browser testing via Chrome MCP
- [ ] Verify all functionality

### Phase 7: Git Operations

- [ ] Commit all files with descriptive message
- [ ] Push branch to GitHub
- [ ] Configure GitHub Pages settings (source: GitHub Actions)
- [ ] Open PR to `main` branch

## Build Verification Steps

### Local Build Tests

```bash
# Step 1: Fetch Hugo modules
hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3
hugo mod tidy

# Step 2: Build site
hugo --minify --gc

# Step 3: Start local development server
hugo server -D

# Step 4: Access site at http://localhost:1313
```

**Expected Outcomes:**
- [ ] No build errors or warnings
- [ ] `go.sum` file generated
- [ ] `public/` directory created with HTML files
- [ ] Local server runs on `http://localhost:1313`

### File Structure Verification

```bash
# Verify all required files exist
ls -la config/_default/
ls -la content/page/
ls -la content/post/
ls -la static/img/
ls -la .github/workflows/
```

**Expected Structure:**
```
config/
  _default/
    config.toml ✓
    params.toml ✓
    menu.toml ✓
content/
  page/
    about/index.md ✓
    projects/index.md ✓
    links/index.md ✓
  post/
    hello-world/index.md ✓
    cloud-cost-savings/index.md ✓
static/
  img/
    avatar.svg ✓
.github/
  workflows/
    deploy.yml ✓
go.mod ✓
go.sum (generated)
.gitignore ✓
CLAUDE.md ✓
implementation-plan.md ✓
```

## Browser Testing Checklist

**Critical: Use Chrome MCP tools to verify after `hugo server -D` is running**

### Visual & Theme Verification

- [ ] **Homepage loads** with hero section and latest posts
- [ ] **Sidebar visible** with avatar, subtitle, social icons
- [ ] **Dark mode active by default** (black/dark gray background)
- [ ] **Navigation menu** shows all 7 items (Home, About, Blog, Archives, Search, Projects, Links)
- [ ] **Avatar displays** (blue SVG with "ED" initials)

### Asset Loading (check browser Network tab)

- [ ] **CSS loads** - Stack theme styles applied (no FOUC)
- [ ] **JavaScript loads** - No console errors (red text in Console tab)
- [ ] **Fonts load** - Typography renders correctly
- [ ] **Icons render** - Tabler icons visible in menu and sidebar

### Functional Testing

- [ ] **All menu items clickable** - Navigate to each page (About, Projects, Links, Archives, Search)
- [ ] **Search page works** - `/search/` loads with search input field
- [ ] **Search functionality** - Type "cloud" or "platform", returns results
- [ ] **Archives page** - `/archives/` shows timeline
- [ ] **Blog listing** - `/post/` displays 2 posts
- [ ] **Individual posts** - Click post titles, content renders with syntax highlighting
- [ ] **Categories work** - Click "Cloud Cost / FinOps", filters posts
- [ ] **Tags work** - Click tags, filters posts
- [ ] **Social links work** - GitHub/LinkedIn/Email open correct URLs

### Console & Error Checking

- [ ] **No JavaScript errors** in browser console (F12 → Console tab)
- [ ] **No 404 errors** in Network tab (all resources return 200 OK)
- [ ] **No theme warnings** in console

### Responsive Testing

- [ ] **Desktop layout** - Sidebar + content (1200px+ width)
- [ ] **Mobile layout** - Hamburger menu, stacked (< 768px width)

## GitHub Actions Verification

After PR merge and GitHub Pages setup:

### Workflow Checks

- [ ] **Build job runs** - Successful build on push to main
- [ ] **Deploy job runs** - Successful deployment to GitHub Pages
- [ ] **No workflow errors** - Check Actions tab for green checkmarks

### Deployment Verification

- [ ] **Site accessible** - `https://eythan-decker.github.io/` loads
- [ ] **Production assets** - CSS/JS minified, no development artifacts
- [ ] **HTTPS enabled** - Site uses HTTPS (GitHub Pages default)
- [ ] **All pages work** - Navigate to all routes, no 404s

### GitHub Pages Settings

- [ ] **Source: GitHub Actions** - Pages settings configured correctly
- [ ] **Enforce HTTPS** - Enabled (default)

## Troubleshooting Guide

### Common Issues

**Issue: `hugo: command not found`**
- Solution: Install Hugo Extended from https://gohugo.io/installation/

**Issue: Theme not found**
- Solution: Run `hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3`

**Issue: Build errors about missing Go**
- Solution: Install Go 1.21+ from https://go.dev/dl/

**Issue: Dark mode not default**
- Solution: Check `params.toml` has `[colorScheme] default = "dark"`

**Issue: Search not working**
- Solution: Verify `config.toml` has `outputs.home = ["HTML", "RSS", "JSON"]`

**Issue: Avatar not showing**
- Solution: Check `static/img/avatar.svg` exists and `params.toml` has correct path

**Issue: GitHub Actions fails on build**
- Solution: Check workflow has Go setup step before Hugo build

**Issue: Site doesn't deploy after merge**
- Solution: Verify GitHub Pages settings: Source = GitHub Actions

## Success Criteria

The MVP is complete when:

1. **All files created** - Complete checklist above ✓
2. **Local build succeeds** - `hugo --minify --gc` runs without errors
3. **Browser tests pass** - All checklist items verified via Chrome MCP
4. **PR opened** - Pull request created to `main` branch
5. **GitHub Actions configured** - Workflow file committed ✓
6. **Documentation complete** - This plan checked into repo ✓

## TODO: Future Research

Items to investigate after MVP is complete:

- [ ] Detailed comparison: Hugo Modules vs Git Submodules vs Git Clone
- [ ] Benefits and trade-offs of each theme management approach
- [ ] Recommendation for long-term maintenance strategy
- [ ] When to upgrade Hugo versions (strategy)

## Next Steps (Phase 2)

After MVP merge:

- [ ] Add OpenGraph metadata for social sharing
- [ ] Configure custom domain (optional)
- [ ] Add analytics (Plausible/Umami)
- [ ] Write additional blog posts
- [ ] Refine About page with more detail
- [ ] Add project write-ups to Projects page
