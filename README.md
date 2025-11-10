# My Blog

A modern blog built with **MkDocs** and the **Material** theme.

## Project Structure

```
blog/
├── docs/               # Main documentation files
│   └── index.md       # Homepage
├── blog/              # Blog posts directory
│   ├── index.md       # Blog main page
│   └── posts/         # Individual blog posts
│       └── first-post.md
├── mkdocs.yml         # MkDocs configuration
├── pyproject.toml     # Poetry configuration
├── .gitignore         # Git ignore file
└── README.md          # This file
```

## Setup & Installation

### Prerequisites
- Python 3.8 or higher
- Poetry (for dependency management)

### Installation

1. Install dependencies using Poetry:
```bash
poetry install
```

2. Activate the virtual environment:
```bash
poetry shell
```

### Running the Blog Locally

To preview your blog locally while developing:

```bash
mkdocs serve
```

The site will be available at `http://localhost:8000`

### Building for Production

To build the static site:

```bash
mkdocs build
```

The compiled site will be in the `site/` directory.

## Creating New Blog Posts

1. Create a new markdown file in `blog/posts/` directory
2. Add frontmatter with metadata:

```markdown
---
date: 2025-11-10
authors:
  - name: Your Name
    description: Author title
    avatar: https://i.pravatar.cc/150?img=1
categories:
  - Category Name
---

# Your Post Title

Your content here...
```

## Customization

### Updating Site Information

Edit `mkdocs.yml` to change:
- Site name, description, and author
- Theme colors and fonts
- Navigation structure

### Theme Configuration

The Material theme is configured in `mkdocs.yml` with:
- Light and dark mode support
- Syntax highlighting for code blocks
- Emoji support
- Responsive design

## Deployment

The compiled `site/` directory can be deployed to:
- GitHub Pages
- Netlify
- Vercel
- Any static hosting service

## Features

✨ **Material Design Theme**
- Modern, clean interface
- Light/dark mode toggle
- Responsive mobile design

🔍 **Search**
- Full-text search functionality
- Instant search results

📝 **Blog Plugin**
- Multi-category support
- Post archives
- Author information

💻 **Developer Friendly**
- Simple Markdown syntax
- Easy to extend
- Git-friendly source files

## Resources

- [MkDocs Documentation](https://www.mkdocs.org/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
- [Poetry Documentation](https://python-poetry.org/)

## License

MIT
