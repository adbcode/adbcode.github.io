# adbcode

Personal blog at <https://adbcode.github.io/>, built with Hugo and deployed to
GitHub Pages by GitHub Actions. The layout and styles are adapted from
[Jekyll Now](https://github.com/barryclark/jekyll-now); its MIT license is retained
in `LICENSE`.

## Local development

Install [Hugo 0.168.0](https://github.com/gohugoio/hugo/releases/tag/v0.168.0) and
[Dart Sass 1.106.1](https://github.com/sass/dart-sass/releases/tag/1.106.1), matching
the pinned CI versions. Put both executables on your `PATH`. For a Dart Sass
release archive, add the extracted `dart-sass` directory and retain its `src`
directory. Hugo's standard edition is sufficient; Ruby, Node, Go, theme
submodules, and Hugo Extended are not required.

```sh
hugo version
sass --version
hugo server --buildDrafts
```

Open <http://localhost:1313/>. To reproduce the production build:

```sh
hugo build --gc --minify --panicOnWarning
```

Generated files go to `public/` and are ignored by Git.

## Writing posts

You can keep writing directly in Markdown, locally or in GitHub's editor.
Create `content/posts/YYYY-MM-DD-slug.md` with the same front matter as before:

```markdown
---
layout: post
title: My new post
---

Write the post here.
```

Commit and push to `master`; the workflow builds and deploys the update. Editing
an existing file works the same way. The homepage shows only the first
paragraph of each post; open the post to see edits further down. The About
page is `content/about.md`; the short header description is separately set in
`hugo.toml` under `[params]`.

Alternatively, create a post with the optional Hugo CLI:

```sh
hugo new content posts/2026-10-24-my-post.md
```

The archetype creates a draft with `layout: post`. Edit the title and Markdown,
then remove `draft: true` (or set it to `false`) when ready to publish. Hugo reads
the publication date and slug from the filename, so this example is published
at `/my-post/`. Dates are interpreted at midnight in **Europe/Paris**, both
locally and in GitHub Actions. Do not rename existing posts unless you also
intend to change their date or URL. Posts marked `draft: true` and future-dated
posts are excluded from production even when the build succeeds. A future post
needs another push or a manual workflow run after its publication date; there
is no scheduled rebuild. `hugo server --buildDrafts --buildFuture` previews
unpublished posts locally.

Put images in `static/images/` and reference them as `/images/filename.png`.
Edit `content/about.md` for the About page, `hugo.toml` for site metadata,
`layouts/` for templates, and `assets/scss/` for styles.

## GitHub Pages setup

1. In [repository Settings → Pages](https://github.com/adbcode/adbcode.github.io/settings/pages),
   set **Build and deployment → Source** to **GitHub Actions**.
2. Commit and push the migration to `master`. The **Hugo GitHub Pages** workflow
   builds and deploys the site. You do not need to create a workflow using the
   GitHub UI; `.github/workflows/hugo.yml` is already included.
3. Check the run in [Actions](https://github.com/adbcode/adbcode.github.io/actions).
   After changing the Pages source, a failed earlier run can be rerun, or choose
   **Hugo GitHub Pages → Run workflow** on `master`.

If Actions is disabled for this repository, enable it. If the `github-pages`
environment has branch restrictions or required reviewers, allow `master` or
approve the deployment as appropriate. No personal access token or extra
repository secrets are needed; the workflow uses GitHub's automatic token.
The [GitHub documentation](https://docs.github.com/en/pages/getting-started-with-github-pages/using-custom-workflows-with-github-pages)
describes this deployment method.

Pull requests targeting `master` build for validation without deploying.
Pushes to `master` and manual runs on `master` deploy the generated Pages
artifact. Tool versions, download checksums, and action commits are pinned in
the workflow; update them together when upgrading. Generated HTML is not
committed to a separate branch.

The repository's old `CNAME` file was blank. For a future custom domain, set it
in **Settings → Pages** and update `baseURL` in `hugo.toml`; production builds
also read the configured Pages URL through `actions/configure-pages`.

## Migration compatibility

All four original post files were moved from `_posts/` to `content/posts/`
without editing their contents or front matter. Images referenced by the posts
are retained unchanged in `static/images/`; unused Jekyll starter-guide graphics
have been removed. Existing post URLs remain `/hello-world/`,
`/tailscale-wsl/`, `/audio-course/`, and `/type-hindi/`.

The original design, first-paragraph excerpts, same-day post ordering,
`/about/`, `/404.html`, `/style.css`, and `/sitemap.xml` are retained. The Atom
feed remains at `/feed.xml` and keeps Jekyll's dated entry IDs for existing
subscribers. The Jekyll build configuration and Liquid layouts have been
replaced with Hugo templates.
