# Hugo Stack Theme MVP - Implementation Plan

This document tracks the implementation and verification of the Hugo site with Stack theme.

## Implementation Status: ✅ COMPLETED

All phases completed and verified locally with browser testing on 2026-01-10.

## Implementation Checklist

### Phase 1: Foundation

- [x] Create `go.mod` with Hugo module declaration
- [x] Create `config/_default/config.toml` with base Hugo config
- [x] Create `config/_default/params.toml` with Stack theme parameters
- [x] Create `config/_default/menu.toml` with navigation and social links
- [x] Run `hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3` to fetch theme
- [x] Run `hugo mod tidy` to clean up module dependencies

**Issues Found & Fixed:**
- Hugo v0.154 deprecated `paginate` config → Changed to `pagination.pagerSize`
- params.toml format incompatible with Stack v3.33 → Updated to match starter template structure
- menu.toml icon params not supported → Removed icon configuration from menu items

### Phase 2: Static Assets

- [x] Create `static/img/` directory
- [x] Create `static/img/avatar.png` placeholder avatar

**Issues Found & Fixed:**
- Stack theme expects PNG extension, not SVG → Renamed `avatar.svg` to `avatar.png`

### Phase 3: Content Pages

- [x] Create `content/page/about/index.md`
- [x] Create `content/page/projects/index.md`
- [x] Create `content/page/links/index.md`
- [x] Create `content/page/archives/index.md` (required by theme)
- [x] Create `content/page/search/index.md` (required by theme)

**Issues Found & Fixed:**
- Missing Archives page → Added `content/page/archives/index.md` with `layout: "archives"`
- Missing Search page → Added `content/page/search/index.md` with `layout: "search"` and JSON output

### Phase 4: Blog Posts

- [x] Create `content/post/hello-world/index.md`
- [x] Create `content/post/cloud-cost-savings/index.md`

### Phase 5: Deployment Infrastructure

- [x] Create `.github/workflows/deploy.yml`
- [x] Update `.gitignore` to include `spec.md`
- [x] Stage and commit `.gitignore` and `CLAUDE.md`

### Phase 6: Local Environment Setup

- [x] Install Hugo Extended v0.154.3 via Homebrew
- [x] Install Go v1.25.5 via Homebrew
- [x] Initialize Hugo modules (fetch theme)
- [x] Build site locally (`hugo --minify --gc`) - SUCCESS
- [x] Start development server (`hugo server -D`)
- [x] Browser testing via Chrome MCP - ALL TESTS PASSED
- [x] Verify all functionality - VERIFIED

### Phase 7: Git Operations

- [x] Commit all files with descriptive message
- [x] Push branch to GitHub
- [x] Open PR to `main` branch (PR #1)
- [x] Commit configuration fixes
- [x] Push fixes to PR
- [ ] Configure GitHub Pages settings (source: GitHub Actions) - **PENDING POST-MERGE**
- [ ] Merge PR to main - **READY FOR MERGE**

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
- [x] No build errors or warnings ✓
- [x] `go.sum` file generated ✓
- [x] `public/` directory created with HTML files ✓
- [x] Local server runs on `http://localhost:1313` ✓

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

- [x] **Homepage loads** with hero section and latest posts ✓
- [x] **Sidebar visible** with avatar, subtitle, social icons ✓
- [x] **Dark mode active by default** (black/dark gray background) ✓
- [x] **Navigation menu** shows all 7 items (Home, About, Blog, Archives, Search, Projects, Links) ✓
- [x] **Avatar displays** (blue PNG with "ED" initials) ✓

### Asset Loading (check browser Network tab)

- [x] **CSS loads** - Stack theme styles applied (no FOUC) ✓
- [x] **JavaScript loads** - No console errors (red text in Console tab) ✓
- [x] **Fonts load** - Typography renders correctly ✓
- [x] **Icons render** - Tabler icons visible in menu and sidebar ✓

### Functional Testing

- [x] **All menu items clickable** - Navigate to each page (About, Projects, Links, Archives, Search) ✓
- [x] **Search page works** - `/search/` loads with search input field ✓
- [x] **Search functionality** - Type "cloud" returns results in 0.0003 seconds ✓
- [x] **Archives page** - `/archives/` shows timeline with categories ✓
- [x] **Blog listing** - `/post/` displays 2 posts ✓
- [x] **Individual posts** - Click post titles, content renders with proper formatting ✓
- [x] **Categories work** - Click "Cloud Cost / FinOps", filters posts correctly ✓
- [x] **Tags work** - Tag widgets display, tags clickable ✓
- [x] **Social links work** - GitHub/LinkedIn/Email configured in sidebar ✓

### Console & Error Checking

- [x] **No JavaScript errors** in browser console (F12 → Console tab) ✓
- [x] **No 404 errors** in Network tab (all resources return 200 OK) ✓
- [x] **No theme warnings** in console ✓

### Responsive Testing

- [x] **Desktop layout** - Sidebar + content (1200px+ width) ✓
- [x] **Mobile layout** - Not tested (out of scope for MVP)

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

## Current Status (2026-01-10)

**Status:** ✅ Implementation complete, all tests passed, PR ready for merge

**Branch:** `feature/mvp-hugo-setup`
**PR:** #1 - https://github.com/eythan-decker/eythan-decker.github.io/pull/1

**Commits:**
1. Initial Hugo site setup with Stack theme (15 files)
2. Fix Hugo configuration for v0.154 and Stack theme v3.33 compatibility (8 files)
3. Add implementation planning requirement and update plan with completed MVP verification (2 files)

**Environment:**
- Hugo Extended v0.154.3 installed
- Go v1.25.5 installed
- Stack theme v3.33.0 fetched via Hugo Modules

**Verification:**
- All local build tests passed
- All browser tests passed via Chrome MCP
- No console errors
- All functionality working as expected

## Current Issues

**None** - All known issues resolved.

## Next Issue to Work On

**Merge PR and Configure GitHub Pages:**
1. Review and merge PR #1 to main branch
2. Navigate to repo Settings → Pages
3. Set Source to "GitHub Actions"
4. Wait for automatic deployment
5. Verify site at https://eythan-decker.github.io/
6. Test all pages in production environment

## Next Steps (Phase 2)

After MVP merge and production verification:

- [ ] Add OpenGraph metadata for social sharing
- [ ] Configure custom domain (optional)
- [ ] Add analytics (Plausible/Umami)
- [ ] Write additional blog posts
- [ ] Refine About page with more detail
- [ ] Add project write-ups to Projects page

## Notes

- Hugo v0.154 uses new `pagination.pagerSize` config instead of deprecated `paginate`
- Stack theme v3.33 requires specific params.toml structure from starter template
- Archives and Search pages are required with specific layouts
- Avatar must use PNG extension, not SVG
- Menu icons configured via params, not menu.toml
- Search works instantly with Fuse.js local search (no backend needed)
- All browser testing should be done locally before pushing to GitHub
