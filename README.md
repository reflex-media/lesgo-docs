# Lesgo! Documentation

You can find the online version of the Lesgo! documentation at [https://reflex-media.github.io/lesgo-docs](https://reflex-media.github.io/lesgo-docs)

## Contribution Guidelines

This documentation is powered by MkDocs. For full documentation visit [mkdocs.org](https://mkdocs.org).

To encourage active collaboration, we encourage and accept Pull Requests.

## Installation

Refer to the official [MkDocs](https://www.mkdocs.org/#installation) for installation instructions.

## Commands

### Quick Start

Start mkdocs on your local machine with live reloading.

```bash
$ mkdocs serve
```

The local site will be available at http://127.0.0.1:8000.

### Build

Build the static html site, ready for deployment.

```bash
$ mkdocs build
```

The static files will be stored in the `site/` directory.

### Deploy

This command will deploy the static site in `site/` directory to GitHub Pages.

```bash
$ mkdocs gh-deploy
```

## Architecture

```
├── docs          # Markdown files
├── site          # Static site
└── mkdocs.yml    # MkDocs configurations
```
