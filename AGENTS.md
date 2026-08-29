# AGENTS.md

Guidance for Codex when working in this repository.

## Project

This is Eythan Decker's personal portfolio and blog, built with Hugo Extended and the Stack theme. Content lives under `content/`, site configuration under `config/_default/`, and custom assets under `assets/` or `static/`.

## Working practices

- Inspect the working tree before making changes and preserve unrelated user work.
- Never commit directly to `main`. Create work on an appropriately named branch:
  - `blog/<post-slug>` for blog posts
  - `feature/<feature-name>` for features or site additions
  - `fix/<issue-description>` for fixes
  - `chore/<task-name>` for maintenance
- Use an implementation-plan Markdown file for substantial feature work, complex multi-step fixes, or work spanning sessions. Blog branches may use simplified tracking when useful.
- Do not commit, push, open a pull request, merge, publish, or deploy unless the user explicitly asks for that action.

## Blog posts

- Create posts as Hugo leaf bundles at `content/post/<slug>/index.md`; keep post-specific images in the same directory.
- Follow the front matter and writing conventions established by existing posts. Include at least `title`, `date`, `draft`, `description`, `categories`, and `tags`; add fields such as `slug` and `image` when appropriate.
- Use an established category where possible, such as `Platform Engineering`, `DevEx / Enablement`, `GitOps / Kubernetes`, `CI/CD`, `Cloud Cost / FinOps`, `AI for Engineering Teams`, or `Notes`.
- Preserve the author's voice. Make editorial changes only when warranted by clarity, correctness, consistency, or the user's instructions.

## Validation

- Run `hugo --minify --gc` before handing off changes.
- Run `git diff --check` and inspect `git status`.
- Use `markdownlint-cli2 "content/**/*.md"` when available and relevant.
- For visual or interactive changes, run `hugo server` and verify the affected pages in a browser. Check asset loading, console errors, responsive behavior, and dark mode when applicable.
- Report validation warnings and distinguish pre-existing site issues from regressions introduced by the change.

## Commits and pull requests

- Keep commit messages concise (one or two sentences) and use conventional prefixes when appropriate, including `blog:`, `feat:`, `fix:`, `chore:`, `docs:`, and `refactor:`.
- Do not add `Co-Authored-By: Claude`, Codex attribution, or similar AI attribution to commit messages.
- Target pull requests to `main`. Use a conventional prefix in the PR title.
- Blog pull requests may use a short body focused on the post title, purpose, key files, and validation. Non-blog pull requests should summarize the change, list important files, explain relevant technical details, and include references when useful.

## Deployment

- Pull requests to `main` run the Hugo build but do not deploy.
- A push or merge to `main` triggers the GitHub Pages deployment workflow in `.github/workflows/deploy.yml`.
- Do not edit generated `public/` output or deploy manually unless the user explicitly requests it.
