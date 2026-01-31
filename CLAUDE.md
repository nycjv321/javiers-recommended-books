# Claude Code Instructions

A static book recommendation website managed via a separate Electron admin app.

## Project Overview

- **Purpose**: Display personal book recommendations
- **Stack**: Vanilla HTML/CSS/JS (static site)
- **Hosting**: Static files only, no backend
- **Management**: Separate Electron admin app (not included in this repo)

## Project Structure

```
recommended-books/
├── index.html              # Source HTML with {{placeholder}} syntax
├── app.js                  # Book loading, rendering, modal logic
├── styles-minimalist.css   # Active CSS theme
├── favicon.svg             # Book-shaped favicon
├── config.json             # Site configuration (titles, labels, shelves)
├── books/                  # Book data (JSON files)
│   ├── covers/             # Local cover images
│   ├── top/                # Top reads shelf
│   ├── good/               # Good reads shelf
│   └── current-and-future-reads/
├── books-sample/           # Sample data for testing
├── scripts/
│   └── build-index.js      # Build script for CI/CD
├── dist/                   # Build output (gitignored)
├── docs/
│   └── workflow.md         # Deployment documentation
├── .github/
│   └── workflows/
│       └── deploy.yml      # GitHub Actions S3 deployment
├── README.md
├── LICENSE
└── .gitignore
```

## Key Files

| File | Purpose |
|------|---------|
| `index.html` | Source HTML with `{{placeholder}}` syntax |
| `styles-minimalist.css` | Active CSS theme |
| `app.js` | Book loading, rendering, modal logic |
| `config.json` | Site configuration (titles, labels, shelves) |
| `favicon.svg` | Book-shaped favicon |
| `scripts/build-index.js` | Build script for CI/CD |
| `books/` | Book data (source) |
| `books-sample/` | Sample book data for testing |
| `dist/` | Built output (gitignored) |

## Book Data Flow

1. `app.js` fetches `books/index.json` to get list of book files
2. Each book JSON is fetched from its shelf folder
3. Shelf is derived from folder path (e.g., `good/` -> `good` shelf)
4. Books are grouped by shelf and rendered with section headers

## Site Configuration (config.json)

```json
{
    "siteTitle": "My Reads",
    "siteSubtitle": "Books that shaped my career",
    "footerText": "Books I love and books to explore",
    "shelves": [
        { "id": "top", "label": "Top Reads", "folder": "top" },
        { "id": "good", "label": "Good Reads", "folder": "good" },
        { "id": "current-and-future-reads", "label": "Current and Future Reads", "folder": "current-and-future-reads" }
    ]
}
```

Build script replaces `{{placeholders}}` in HTML with config values.
App.js fetches config.json at runtime for shelf labels.

## Common Tasks

### Build Site

```bash
# Build with real data
node scripts/build-index.js

# Build with sample data
node scripts/build-index.js --sample
```

### Test Locally

```bash
node scripts/build-index.js
npx serve dist
```

### Deploy

Push to main branch to trigger GitHub Actions deployment to S3.

## Type Definitions

```typescript
interface Book {
  title: string;
  author: string;
  category: string;
  publishDate: string;
  pages?: number;
  cover?: string;
  coverLocal?: string;        // Relative path: "covers/book.jpg"
  notes?: string;
  link?: string;
  clickBehavior: 'overlay' | 'redirect';
}

interface Shelf {
  id: string;
  label: string;
  folder: string;
}

interface Config {
  siteTitle: string;
  siteSubtitle: string;
  footerText: string;
  shelves: Shelf[];
}
```

## Things to Avoid

- Don't manually edit `dist/` - it gets cleaned on each build
- Don't add `shelf` field to book JSON - it's derived from folder
- Don't serve from site root - always serve from `dist/`

## Dependencies

- No runtime dependencies (vanilla JS)
- Node.js required for build script
