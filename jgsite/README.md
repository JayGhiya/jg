# JAY GHIYA Personal Website

A personal website built with [Hugo](https://gohugo.io/) using the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

## Prerequisites

Before you begin, ensure you have the following installed:

- **Hugo** v0.151.0 or later (Extended version)
- **Go** 1.24.5 or later (required for Hugo modules)

### Installing Hugo

#### macOS

**Using Homebrew:**
```bash
brew install hugo
```

**Using MacPorts:**
```bash
sudo port install hugo
```

#### Linux

**Debian/Ubuntu:**
```bash
sudo apt install hugo
```

**Fedora/CentOS/RHEL:**
```bash
sudo dnf install hugo
```

**Arch Linux:**
```bash
sudo pacman -S hugo
```

#### From Source

If you have Go installed:
```bash
CGO_ENABLED=1 go install -tags extended github.com/gohugoio/hugo@latest
```

#### Verify Installation

```bash
hugo version
```

You should see output similar to:
```
hugo v0.151.0+extended+withdeploy darwin/arm64
```

## Quick Start

### 1. Clone the Repository

```bash
git clone <repository-url>
cd jgsite
```

### 2. Install Dependencies

This project uses Hugo modules for theme management. Install all dependencies:

```bash
hugo mod get
```

### 3. Run Development Server

```bash
hugo server -D
```

The site will be available at `http://localhost:1313/`

## Development Guide

### Starting the Development Server

**Basic server (published content only):**
```bash
hugo server
```

**Include drafts:**
```bash
hugo server -D
# or
hugo server --buildDrafts
```

**Custom port:**
```bash
hugo server --port 8080
```

**Navigate to changed content automatically:**
```bash
hugo server --navigateToChanged
```

**Bind to specific interface (for network access):**
```bash
hugo server --bind 0.0.0.0
```

### Creating New Content

**Create a new blog post:**
```bash
hugo new content posts/my-first-post.md
```

This will create a new markdown file in `content/posts/` using the default archetype template.

**Edit the front matter:**
```yaml
---
date: '2025-11-03'
draft: true  # Set to false when ready to publish
title: 'My First Post'
---
```

### Project Structure

```
jgsite/
├── archetypes/          # Content templates for `hugo new`
│   └── default.md       # Default template
├── assets/              # Files to be processed (SCSS, JS, images)
├── content/             # Markdown content files
│   ├── _index.md        # Homepage content
│   └── posts/           # Blog posts
├── data/                # Data files (JSON, YAML, TOML)
├── i18n/                # Translation files
├── layouts/             # Custom templates (override theme)
│   ├── _default/        # Default templates
│   ├── partials/        # Reusable components
│   └── shortcodes/      # Custom shortcodes
├── public/              # Generated site (gitignored)
├── static/              # Static files (copied as-is)
│   └── images/          # Static images
│       └── profile-photo.jpeg
├── themes/              # Empty (using modules instead)
├── go.mod               # Go module dependencies
├── go.sum               # Module checksums
└── hugo.yaml            # Main configuration file
```

**Key Directories:**

- **`content/`**: All your Markdown content files
- **`layouts/`**: Custom templates that override theme defaults
- **`static/`**: Files copied directly to `public/` without processing
- **`assets/`**: Files processed by Hugo Pipes (SCSS, JS, images)
- **`public/`**: Generated static site (excluded from version control)

## Building for Production

### Standard Build

```bash
hugo
```

### Recommended Production Build

```bash
hugo --gc --minify
```

**Flags explained:**
- `--gc`: Removes unused cache files
- `--minify`: Minifies HTML, CSS, JS, JSON, XML, and SVG

### Additional Build Options

**Clean destination before building:**
```bash
hugo --cleanDestinationDir
```

**Build for specific environment:**
```bash
hugo --environment production
```

**Custom base URL:**
```bash
hugo --baseURL "https://example.com/"
```

The generated site will be in the `public/` directory.

## Deployment

### Cloudflare Pages (Current Setup)

This project is configured for Cloudflare Pages deployment.

**Build Configuration:**
- **Build command:** `hugo --gc --minify`
- **Build output directory:** `public`
- **Root directory:** `/`

**Environment Variables:**
```
HUGO_VERSION=0.151.0
GO_VERSION=1.24.5
NODE_VERSION=22.18.0
```

### GitHub Pages

Create `.github/workflows/hugo.yaml`:

```yaml
name: Build and deploy
on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.151.0
    steps:
      - name: Checkout
        uses: actions/checkout@v5
        with:
          submodules: recursive
          fetch-depth: 0

      - name: Setup Hugo
        run: |
          curl -sLJO "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          mkdir "$HOME/.local/hugo"
          tar -C "$HOME/.local/hugo" -xf "hugo_extended_${HUGO_VERSION}_linux-amd64.tar.gz"
          echo "$HOME/.local/hugo" >> "${GITHUB_PATH}"

      - name: Build
        run: hugo --gc --minify

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy
        uses: actions/deploy-pages@v4
```

### Netlify

Create `netlify.toml`:

```toml
[build.environment]
HUGO_VERSION = "0.151.0"
GO_VERSION = "1.24.5"
NODE_VERSION = "22.18.0"

[build]
publish = "public"
command = "hugo --gc --minify --baseURL ${URL}"
```

### Vercel

Create `vercel.json`:

```json
{
  "buildCommand": "hugo --gc --minify",
  "outputDirectory": "public"
}
```

### GitLab Pages

Create `.gitlab-ci.yml`:

```yaml
pages:
  stage: deploy
  image: golang:1.24.5
  script:
    - apt-get update && apt-get install -y curl
    - curl -sLJO "https://github.com/gohugoio/hugo/releases/download/v0.151.0/hugo_extended_0.151.0_linux-amd64.tar.gz"
    - tar -xf hugo_extended_0.151.0_linux-amd64.tar.gz
    - ./hugo --gc --minify
  artifacts:
    paths:
      - public
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
```

## Hugo Modules Management

This project uses Hugo modules for theme management instead of Git submodules.

### Viewing Current Modules

```bash
hugo mod graph
```

### Updating Dependencies

**Update all modules:**
```bash
hugo mod get -u
```

**Update all modules recursively:**
```bash
hugo mod get -u ./...
```

**Update specific module:**
```bash
hugo mod get github.com/adityatelange/hugo-PaperMod@latest
```

### Other Module Commands

**Download/sync dependencies:**
```bash
hugo mod get
```

**Clean module cache:**
```bash
hugo mod clean
```

**Vendor dependencies (optional):**
```bash
hugo mod vendor
```

**Tidy go.mod:**
```bash
hugo mod tidy
```

## Configuration

### Main Configuration File: `hugo.yaml`

```yaml
baseURL: 'https://<project>.pages.dev/'
languageCode: 'en-us'
title: 'JAY GHIYA'

module:
  imports:
    - path: github.com/adityatelange/hugo-PaperMod
    - path: github.com/hugomods/giscus

params:
  profileMode:
    enabled: true
    title: "Jay Ghiya"
    subtitle: "Software Engineer"
    imageUrl: "/images/profile-photo.jpeg"
  socialIcons:
    - name: github
      url: "https://github.com/JayGhiya"
    - name: linkedin
      url: "https://linkedin.com/in/jayghiya"
```

### Environment-Specific Configuration

Hugo supports environment-specific configs. Create:

```
config/
├── _default/
│   └── hugo.yaml      # Default config
├── production/
│   └── hugo.yaml      # Production overrides
└── development/
    └── hugo.yaml      # Development overrides
```

Build with specific environment:
```bash
hugo --environment production
```

### Theme Configuration

This project uses **PaperMod** theme via Hugo modules. See the [PaperMod documentation](https://github.com/adityatelange/hugo-PaperMod) for available configuration options.

## Content Management

### Content Organization

```
content/
├── _index.md           # Homepage
├── about/
│   └── index.md        # About page
└── posts/
    ├── _index.md       # Posts list page
    └── post-1.md       # Individual post
```

### Front Matter

Each content file starts with front matter:

```yaml
---
title: "My Post Title"
date: 2025-11-03
draft: false
tags: ["technology", "hugo"]
categories: ["blog"]
description: "Brief description"
---
```

### Archetypes

Archetypes are templates for new content. The default archetype (`archetypes/default.md`):

```yaml
---
date: '{{ .Date }}'
draft: true
title: '{{ replace .File.ContentBaseName "-" " " | title }}'
---
```

Create custom archetypes for different content types:

```bash
# Create content with custom archetype
hugo new content --kind post posts/article.md
```

## Troubleshooting

### Module Cache Issues

If you encounter module-related errors:

```bash
# Clean module cache
hugo mod clean

# Re-download dependencies
hugo mod get
```

### Build Fails

**Check Hugo version:**
```bash
hugo version
```

Ensure you have the **extended** version installed.

**Clear cache and rebuild:**
```bash
hugo --gc --cleanDestinationDir
```

### Template Issues

**View template metrics:**
```bash
hugo --templateMetrics
```

This helps identify slow templates.

### Configuration Issues

**Print current configuration:**
```bash
hugo config
```

**Print as JSON:**
```bash
hugo config --format json
```

**Print mount configuration:**
```bash
hugo config mounts
```

## Useful Commands

### View Hugo Configuration

```bash
# Print configuration
hugo config

# Print as JSON
hugo config --format json

# Show mount points
hugo config mounts
```

### List Content

```bash
# List all content
hugo list all

# List drafts
hugo list drafts

# List future posts
hugo list future

# List expired posts
hugo list expired
```

### Performance Analysis

```bash
# Build with template metrics
hugo --templateMetrics

# Build with build metrics
hugo --templateMetrics --templateMetricsHints
```

## Resources

- **Hugo Documentation**: https://gohugo.io/documentation/
- **PaperMod Theme**: https://github.com/adityatelange/hugo-PaperMod
- **Hugo Modules**: https://gohugo.io/hugo-modules/
- **Hugo Community**: https://discourse.gohugo.io/

## License

[Add your license information here]
