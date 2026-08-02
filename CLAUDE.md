# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository overview

This repo is a personal GitHub Pages site (repo name `1stastic`, deployed from the `gh-pages` branch) bundled together with a grab-bag of unrelated Jupyter/Colab notebooks. There is no application build, no test suite, and no CI — this is not a conventional software project. Treat changes conservatively and don't introduce tooling (linters, test frameworks, package managers) that isn't already here.

There are two unrelated concerns living side by side in this repo:

1. **The Jekyll site** — the actual GitHub Pages blog/site.
2. **A pile of standalone Colab notebooks** — unrelated experiments, not connected to the site or to each other.

## The Jekyll site

- Built with Jekyll using a remote theme: `remote_theme: 'internet-de-france/swiss'` (Swiss Jekyll Theme, aka "Athena") configured in `_config.yml`. The theme itself is not vendored in this repo — it's pulled in remotely by GitHub Pages/Jekyll at build time.
- `baseurl: '/1stastic'` — the site is served under a `/1stastic` path, so links/assets in content must account for that prefix.
- Content structure follows standard Jekyll conventions:
  - `_posts/` — blog posts, filename convention `YYYY-MM-DD-title.md` with front matter (`layout`, `title`, `date`, `categories`).
  - `_data/nav.yml`, `_data/nav_home.yml` — drive the site navigation (list of `title`/`url` pairs).
  - Top-level `.md` files (`about.md`, `writing.md`, `index.md`) are pages, each with Jekyll front matter (`layout`, `title`, `permalink`). `writing.md` uses `layout: category_index` with `category_name: writing` to group posts by category.
  - `assets/` — static assets (currently empty aside from `.gitkeep`).
- `admin/` is a Stastic CMS integration: `admin/index.html` redirects to the hosted Stastic editor (`editor.stastic.net`) for `gh-pages`-branch editing, and `admin/data.json` is a Liquid template that serializes `site.collections`, `site.pages`, and `site.data` to JSON for that CMS to consume. Both are wired specifically to this repo/branch — don't repurpose them as generic templates.
- There is no local Jekyll build/serve setup checked into the repo (no `Gemfile`). To preview locally you'd need to create one (or install the `jekyll` gem directly) and run `jekyll serve`/`bundle exec jekyll serve` yourself; there's no repo-specific override of that workflow.
- `package.json` / `package-lock.json` at the root are essentially vestigial — the declared dependencies (`node.js`, `wordpress`) are not real usable packages and nothing in the repo requires `npm install`. `.codesandbox/tasks.json` runs `npm install` on CodeSandbox init, but this doesn't reflect a real Node build pipeline.

## Notebooks (unrelated to the site)

These are standalone Colab notebooks, each self-contained, not imported by or connected to each other or to the Jekyll site:

- `Copy_of_text_quickstart.ipynb`, `Welcome_To_Colaboratory.ipynb` — generic Colab quickstart/intro notebooks.
- `Gorilla_hosted.ipynb` — Gorilla LLM hosted-inference notebook.
- `gpt4all_colab_terminal.ipynb` — sets up a GPT4All terminal environment in Colab.
- `onesekode.ipynb` — standalone notebook at repo root.
- `dopamine/colab/agents.ipynb` — Dopamine (RL) agents notebook.
- `site/en/tutorials/chat_quickstart.ipynb` — a chat quickstart tutorial notebook. It lives under the `site/` directory, and since `_config.yml` has no `exclude:` for it, Jekyll copies it into the built site as a static file (unrendered, since it has no front matter) — it's not truly disconnected from the build like the other notebooks are.
- `ngrok-webhook-nodejs-sample/` — currently empty.

When editing a notebook, keep changes scoped to that notebook; there's no shared library code between them.
