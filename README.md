# NJ Civic Information Consortium

The website for the **New Jersey Civic Information Consortium** — a first-of-its-kind state initiative dedicated to strengthening local journalism and civic information across New Jersey.

**Live site**: [https://madi-mccool.github.io/njcivicinfo/](https://madi-mccool.github.io/njcivicinfo/)

## Architecture

- **Static site generator**: [Jekyll](https://jekyllrb.com/) 4.x
- **Hosting**: [GitHub Pages](https://pages.github.com/)
- **Source directory**: `docs/` (configured as the GitHub Pages source)
- **Layouts**: `docs/_layouts/` — `default.html` and `page.html`
- **Includes**: `docs/_includes/` — reusable components (impact stats, etc.)
- **Data files**: `docs/_data/` — staff, board members, and testimonials in YAML
- **Styles**: `docs/assets/css/style.css` — custom CSS using Mulish font
- **Plugins**: jekyll-feed, jekyll-seo-tag, jekyll-sitemap

## Local Development

```bash
cd docs
bundle install
bundle exec jekyll serve
```

Then visit `http://localhost:4000/njcivicinfo/` in your browser.

### Prerequisites

- Ruby 2.7+ and Bundler
- Jekyll 4.x (installed via `bundle install`)

## Project Structure

```
docs/
├── _config.yml          # Jekyll configuration
├── _data/               # YAML data files (staff, board, testimonials)
├── _includes/           # Reusable HTML components
├── _layouts/            # Page templates
├── assets/
│   ├── css/style.css    # Main stylesheet
│   └── images/          # Site images
├── index.html           # Homepage
├── what-we-do.html      # What We Do page
├── grantmaking.html     # Grantmaking page
├── aboutus.md           # About Us page
├── news-resources.html  # News & Resources page
├── press-forward-nj.html
├── faqs.html
├── internship.html
├── 404.html             # Custom 404 page
└── robots.txt           # Search engine crawling directives
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request. For questions, email [info@njcivicinfo.org](mailto:info@njcivicinfo.org).

## License

This project is licensed under the [MIT License](LICENSE).
