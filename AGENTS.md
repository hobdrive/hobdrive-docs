# hobdrive-docs Agent Guide

This repository is the HobDrive documentation/knowledge-base source.

## Repository Role

- Canonical docs source for user manuals, technical notes, FAQs, layouts,
  ECUXML/dynamic-expression specs, CarPlay instructions, and troubleshooting.
- Rendered independently as a documentation site.
- Also embedded into site/docs during the site build.

## Languages and Structure

- English docs live under `en/`.
- Russian docs live under `ru/`.
- Root indexes:
  - `README.md`
  - `README-ru.md`
- Static assets live beside docs, especially `en/images/`, `ru/images/`, `logo.png`,
  and generated/manual PDFs.

Keep Russian and English pages conceptually aligned when changing shared topics.
It is acceptable to update one language first if the user asks for a localized
change, but note the missing counterpart.

## Link Rules

Use ordinary relative markdown links such as:

```md
[Статистика](ru/statistics.md)
[Core syntax](dynamic-expr-core.md)
```

Do not rewrite links to `.html` inside this repository just to satisfy the parent
site. The parent `hobdrive-ru.site` build owns that compatibility through
`_plugins/docs_relative_links.rb`, which rewrites rendered docs links from `.md`
to `.html`.

Avoid absolute root links like `/docs/...` in docs content unless the link is
intentionally tied to the parent site. Relative links keep the standalone docs
site and embedded parent-site rendering both viable.

## Build and Preview

This repo loads the parent site's `Rakefile` when nested under `hobdrive-ru.site`.
Useful commands from `docs/`:

```sh
bundle exec jekyll s
bundle exec jekyll b
rake build
rake run_dev
```

`rake build` runs `bundle exec jekyll b` and then `relativize_urls`, which rewrites
absolute internal URLs in `_site/**/*.html` to relative URLs for standalone output.

Do not edit `_site/` output directly.

## Generated Inputs

`rake prep` copies changelogs from the sibling `../hobd` repository:

```sh
cp ../hobd/changelog_ru ./ru/changelog_ru.md
cp ../hobd/changelog_en ./en/changelog_en.md
```

Only run this when the user wants changelog refreshes and the sibling repos are
available. Do not silently regenerate content from sibling repos.

## Parent Site Integration

When embedded in `hobdrive-ru.site`:

- GitHub Actions clones this repo into `docs/` before `rake build`.
- Parent `_config.yml` applies `layout: docs` to `docs/**`.
- Parent `_layouts/docs.html` wraps docs pages in the main site chrome.
- Parent `_plugins/docs_relative_links.rb` fixes `.md` cross-links after render.
- Parent `_config.yml` explicitly includes `docs/README.md` so the English root
  index renders as HTML.

If changing the docs structure, check whether the parent site needs matching
updates to docs layout, link rewriting, or feature-page links.

## Content Style

- Prefer practical user-facing explanations over marketing language.
- Preserve technical names exactly: OBD-II, ELM327, ECUXML, PID, KWP/CAN, DashKit,
  CarPlay, Android Auto.
- Keep filenames stable when possible; the parent site and external pages may link
  to them.
- For screenshots and images, use existing `en/images` or `ru/images` conventions.
- Do not add large binaries unless the user explicitly asks.

## Git Hygiene

- This is a separate repo. Check status from inside it:

```sh
git status --short --branch
```

- When working from the parent checkout, use:

```sh
git -C docs status --short --branch
```

- Do not commit parent-site changes from this repo or docs changes from the parent
  repo by accident.
- This checkout may be behind `origin/main`; do not pull/rebase unless the user
  asks or it is necessary for the task.
