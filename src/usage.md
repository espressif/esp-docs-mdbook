# Usage

## Local Development

To get started with local development:

1a. Install mdBook if you haven't already:

```bash
cargo install mdbook
```

1b. Download prebuild mbbook binary for your platform from https://github.com/rust-lang/mdBook/releases and add it to your PATH

2. Create a new mdBook project (skip if you already have one):

```bash
mdbook init my-documentation
```

3. Apply the theme as described in the Installation section

4. Run the development server:

```bash
mdbook serve --open
```

Your documentation will be available at `http://localhost:3000` by default.

## GitHub Workflows Integration

To automatically build and deploy your documentation using GitHub Actions, create a `.github/workflows/deploy.yml` file with the following content:

```yaml
name: Deploy mdBook site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: true

      - name: Setup mdBook
        uses: peaceiris/actions-mdbook@v2
        with:
          mdbook-version: "latest"

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Build with mdBook
        run: mdbook build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: "book"

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Make sure to:

1. Enable GitHub Pages in your repository settings
2. Set the source to "GitHub Actions"
3. Configure appropriate permissions for the GitHub Actions workflow

## Customization

To customize the theme, you can modify the following files:

- `theme/css/general.css` - General styling
- `theme/book.js` - JavaScript functionality
- `theme/index.hbs` - HTML layout
