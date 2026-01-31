# My Reads

[![Built with Claude](https://img.shields.io/badge/Built%20with-Claude-blueviolet)](https://claude.ai)

A minimalist static website to showcase book recommendations.

## Features

Clean design, customizable shelves, dark mode, responsive layout, book detail overlays.

Management is handled via a separate Electron admin app with Open Library search, drag-and-drop shelves, and built-in preview server.

## Quick Start

```bash
# Build the site
node scripts/build-index.js

# Preview locally
npx serve dist
```

For detailed workflow and deployment instructions, see **[docs/workflow.md](docs/workflow.md)**.

## Project Structure

```
recommended-books/
├── index.html          # Site template
├── app.js              # Book loading and rendering
├── styles-minimalist.css
├── config.json         # Site configuration
├── books/              # Your book data (JSON files)
│   ├── covers/         # Cover images
│   ├── top/            # Top reads
│   ├── good/           # Good reads
│   └── current-and-future-reads/
├── scripts/
│   └── build-index.js  # Build script
├── dist/               # Build output (deploy this)
└── docs/               # Documentation
```

## Book JSON Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | Book title |
| `author` | string | Yes | Author name |
| `category` | string | No | Genre or category |
| `publishDate` | string | No | ISO date (YYYY-MM-DD) |
| `pages` | number | No | Page count |
| `cover` | string | No | URL to cover image |
| `coverLocal` | string | No | Local path (e.g., `covers/my-book.jpg`) |
| `notes` | string | No | Your personal notes |
| `link` | string | No | External URL |
| `clickBehavior` | string | No | `"overlay"` (default) or `"redirect"` |

## Customization

### Site Text

Edit `config.json`:

```json
{
  "siteTitle": "My Reads",
  "siteSubtitle": "Books that shaped my career",
  "footerText": "Books I love and books to explore",
  "shelves": [
    { "id": "top", "label": "Top Reads", "folder": "top" }
  ]
}
```

### Colors

Edit CSS variables in `styles-minimalist.css`:

```css
:root {
    --bg-primary: #FAF5FF;
    --accent: #292524;
    --text-primary: #292524;
}
```

## Deployment

Build and deploy the `dist/` folder to any static hosting (GitHub Pages, Netlify, S3, etc.).

```bash
node scripts/build-index.js
```

Push to main branch to trigger automatic deployment to S3 via GitHub Actions.

See **[docs/workflow.md](docs/workflow.md)** for detailed deployment options.

## License

MIT
