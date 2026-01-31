# Workflow and Deployment

This document explains how to work with and deploy your book collection site.

## Site Structure

```
recommended-books/
├── index.html              # Template with {{placeholder}} syntax
├── app.js                  # Book loading and rendering
├── styles-minimalist.css   # CSS theme
├── favicon.svg             # Site icon
├── config.json             # Site configuration
├── books/                  # Your book data
│   ├── top/                # Top reads shelf
│   │   └── book.json
│   ├── good/               # Good reads shelf
│   │   └── another-book.json
│   ├── current-and-future-reads/
│   └── covers/
│       └── book-cover.jpg
├── books-sample/           # Sample data (optional)
├── scripts/
│   └── build-index.js      # Build script
└── dist/                   # Build output (gitignored)
```

## Management

Books can be managed via:

1. **Separate Electron admin app** - Desktop app with Open Library search, drag-and-drop shelves, and preview server
2. **Direct JSON editing** - Edit `books/*.json` and `config.json` in any text editor

## Typical Workflow

### 1. Edit Your Collection

Add or edit book JSON files in the appropriate shelf folder:

```json
{
  "title": "Domain-Driven Design",
  "author": "Eric Evans",
  "category": "Software Architecture",
  "publishDate": "2003-08-30",
  "coverLocal": "covers/ddd.jpg",
  "notes": "Essential reading for software architects",
  "clickBehavior": "overlay"
}
```

### 2. Build the Site

```bash
node scripts/build-index.js
```

Or with sample data for testing:

```bash
node scripts/build-index.js --sample
```

### 3. Preview Locally

```bash
npx serve dist
# or
cd dist && python -m http.server 8080
```

### 4. Commit and Deploy

```bash
git add books/ config.json
git commit -m "Add new books"
git push
```

Pushing to main triggers automatic deployment via GitHub Actions.

## Build Process

When you run `node scripts/build-index.js`:

1. Read `config.json` for shelves and site metadata
2. Clean the `dist/` directory
3. Copy static files (CSS, JS, favicon)
4. Process `index.html`, replacing `{{siteTitle}}`, `{{siteSubtitle}}`, `{{footerText}}`
5. Copy `config.json` to `dist/`
6. Copy each shelf folder with all book JSON files
7. Generate `books/index.json` (list of all book files)
8. Copy `books/covers/` directory

## Deployment Options

### GitHub Pages

1. Build the site: `node scripts/build-index.js`
2. Push `dist/` to a `gh-pages` branch, or configure GitHub Pages to serve from `dist/`

### Netlify / Vercel

1. Connect your repository
2. Set build command: `node scripts/build-index.js`
3. Set publish directory: `dist`

### Amazon S3 (Automatic)

This repo includes a GitHub Actions workflow that deploys to S3 on push to main.

Required GitHub secrets:
- `AWS_ROLE_ARN` - IAM role ARN for OIDC authentication
- `AWS_REGION` - AWS region
- `S3_BUCKET_NAME` - S3 bucket name

### Amazon S3 (Manual)

```bash
node scripts/build-index.js
aws s3 sync dist/ s3://your-bucket --delete
```

## FAQ

### Can I edit JSON files directly instead of using the admin app?

Yes. The admin app is a convenience—you can edit book JSON files and `config.json` directly in any text editor. Just follow the [book schema](../README.md#book-json-schema).

### What happens if I delete the dist/ folder?

Nothing bad—just rebuild with `node scripts/build-index.js`. The `dist/` folder is generated output and is gitignored.

### Can multiple people edit the collection?

Yes, through git. Each person clones the repo, makes changes, commits, and pushes. Merge conflicts in JSON files are easy to resolve.
