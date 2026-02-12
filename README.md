# Crave Technical Documentation

> **From idea to recipe** — Technical documentation for the AI-powered recipe management iOS app.

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/yourusername/crave-technical)

## Overview

This repository contains the complete technical documentation for **Crave**, an iOS app that transforms recipe discovery and management using AI.

**Live Documentation:** [crave-docs.netlify.app](https://crave-docs.netlify.app) *(update with your URL)*

## Features

- **Elegant Design**: Serif headings (Crimson Pro) with monospace code blocks (IBM Plex Mono)
- **Dark Mode**: Purple onyx midnight theme with wine-colored accents
- **Responsive**: Mobile-friendly documentation
- **Search**: Full-text search across all documentation
- **Diagrams**: Mermaid support for architecture diagrams

## Documentation Structure

```
content/
├── _index.md                  # Homepage
├── docs/
│   ├── _index.md             # Documentation hub
│   ├── overview.md           # What is Crave?
│   ├── tech-stack.md         # Technologies & frameworks
│   ├── architecture.md       # System design & patterns
│   ├── data-flow.md          # How data moves through the system
│   ├── revenuecat.md         # Subscription implementation
│   ├── design-system.md      # UI/UX design tokens
│   ├── backend-api.md        # API endpoints & contracts
│   ├── configuration.md      # Environment setup
│   └── deployment.md         # Deployment guides
```

## Tech Stack

- **Framework**: [Hugo](https://gohugo.io/) (Go-based static site generator)
- **Theme**: [Hextra](https://github.com/imfing/hextra) (modern documentation theme)
- **Fonts**: Crimson Pro (serif), IBM Plex Mono (monospace), Inter (sans-serif)
- **Deployment**: Netlify / GitHub Pages / Vercel

## Local Development

### Prerequisites

- [Hugo](https://gohugo.io/installation/) v0.155.2 or later (extended version)
- [Go](https://golang.org/doc/install) 1.21+
- [Git](https://git-scm.com)

### Quick Start

```bash
# Clone the repository
git clone https://github.com/yourusername/crave-technical.git
cd crave-technical

# Install dependencies
hugo mod tidy

# Start development server
hugo server --buildDrafts --disableFastRender -p 1313
```

Visit [http://localhost:1313](http://localhost:1313) to view the documentation.

## Customization

### Color Theme

The custom dark mode theme is defined in `assets/css/custom.css`:

```css
/* Dark mode - Purple Onyx Midnight */
--color-dark-bg: #1a1625;
--color-dark-surface: #231e31;
--color-dark-card: #2d2640;

/* Wine accent color */
--color-wine: #8b2b47;
```

### Typography

Headings use **Crimson Pro** (elegant serif), while code blocks use **IBM Plex Mono** for technical readability.

### Configuration

Edit `hugo.yaml` to customize:
- Site title and description
- Navigation menu
- Footer content
- Theme settings

## Deployment

### Netlify

[![Deploy to Netlify](https://www.netlify.com/img/deploy/button.svg)](https://app.netlify.com/start/deploy?repository=https://github.com/yourusername/crave-technical)

The `netlify.toml` file is already configured. Just connect your repository.

### GitHub Pages

1. Enable GitHub Pages in repository settings
2. Set deployment source to "GitHub Actions"
3. The workflow in `.github/workflows/pages.yaml` will build and deploy automatically

### Vercel

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2Fyourusername%2Fcrave-technical)

Set `HUGO_VERSION` environment variable to `0.155.2` or later.

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test locally with `hugo server`
5. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) file for details.

## About Crave

Crave is an AI-powered recipe management iOS app that lets you:
- Import recipes from any website or video
- Scan ingredients with your camera
- Manage your pantry intelligently
- Build smart grocery lists
- Discover community recipes

**From idea to recipe** — that's the Crave promise.

---

Built with [Hugo](https://gohugo.io/) and [Hextra](https://github.com/imfing/hextra)
