# esp-docs-mdbook Theme

[![Example Docs](https://img.shields.io/badge/https%3A%2F%2Fespressif.github.io%2Fesp-docs-mdbook%2F?style=plastic&logoColor=%23e7352c&label=Example&labelColor=e7352c)](https://espressif.github.io/esp-docs-mdbook/)

A custom [mdBook](https://github.com/rust-lang/mdBook) theme based on Espressif’s documentation styling, providing a consistent look and feel for your documentation projects.

## Features

- Clean and modern documentation design
- Styling that aligns with [esp-docs](https://docs.espressif.com/projects/esp-docs/en/latest/) aesthetics
- Built-in blazing fast full-text search functionality

## Example Project

- An example source project using mdBook and this theme can be found in the [src](src) folder.
- The project’s configuration file, [book.toml](book.toml), is located in the root directory of the project.
- A rendered version of the project available on [GitHub Pages](https://espressif.github.io/esp-docs-mdbook/)
- The deployment pipeline for the example project is defined in the [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) file.

## Real-world Example

You can see an example of a real-world documentation project using this theme at [eim](https://github.com/espressif/idf-im-ui).
The project includes:

- The [source folder](https://github.com/espressif/idf-im-ui/tree/master/src)
- The [book.toml file](https://github.com/espressif/idf-im-ui/blob/master/book.toml)
- The [GitHub Actions workflow](https://github.com/espressif/idf-im-ui/blob/master/.github/workflows/build_docs.yaml) for deployment to [https://docs.espressif.com](https://docs.espressif.com)
- The rendered documentation available at [https://docs.espressif.com/projects/idf-im-ui/en/latest/](https://docs.espressif.com/projects/idf-im-ui/en/latest/)

## Installation

You can install the theme in two ways:

### Method 1: Clone the Repository

1. Clone this repository as a submodule in your mdBook project:

```bash
git submodule add https://github.com/yourusername/esp-docs-mdbook themes/esp-docs
```

2. Update your `book.toml` to use the theme:

```toml
[output.html]
theme = "themes/esp-docs"
```

### Method 2: Manual Installation

1. Download the repository as a ZIP file from GitHub
2. Extract the contents
3. Copy the `theme` directory to your mdBook project's root directory

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

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
