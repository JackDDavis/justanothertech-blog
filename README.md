# justanothertech-blog — Official source

This repository is the canonical source for "Another Tech Blog" by Jack Davis.
The site is built with Jekyll (using the Minimal Mistakes remote theme) and is published at: https://blog.jackdavis.net

## About

This repository contains the content and site configuration for the blog. Posts are authored as Jekyll markdown files (in `_posts` or `content/`) and rendered by GitHub Pages using the `mmistakes/minimal-mistakes` theme.

> Note: Some content was migrated from a previous platform and converted into Jekyll-friendly Markdown during migration.

## Quick start — preview locally

Prerequisites:
- Ruby (2.7+ recommended)
- Bundler

Steps:

1. Clone the repository:

   git clone https://github.com/JackDDavis/justanothertech-blog.git
   cd justanothertech-blog

2. Install dependencies:

   gem install bundler
   bundle install

3. Serve locally:

   bundle exec jekyll serve --livereload

Visit http://127.0.0.1:4000 to preview the site locally.

## Contributing

- Posts: Add new posts to `_posts/` (or `content/` if you prefer) with front matter including `title`, `date`, `categories`, and `tags`.
- Images and assets: place under `assets/` and reference with absolute paths or site variables.

Pull requests are welcome for content fixes, new posts, and small site improvements. For larger changes, please open an issue first to discuss.

## Deployment

This repository is configured for GitHub Pages. The `CNAME` file points the site to the custom domain `blog.jackdavis.net`. Site builds use the remote theme `mmistakes/minimal-mistakes@4.24.0` defined in `_config.yml`.

If you need CI-based builds or other deployment workflows, consider adding a GitHub Actions workflow to build and validate the site on push.

## License

Content is authored by Jack Davis. Check individual files for licensing details or contact the repository owner for reuse permission.

## Contact

Jack Davis — cloudops@jackdavis.net

GitHub: https://github.com/JackDDavis
