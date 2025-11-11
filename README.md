# Hugo — Quick Notes & Workflow

These are concise notes for developing, testing, and building a Hugo site.

## Prerequisites
- **Check version**: `hugo version`
- Install (macOS): `brew install hugo`

## Common Project Structure
```
.
├── archetypes/            # Default front matter templates
├── assets/                # SCSS, images (processed by Hugo Pipes)
├── content/               # Your markdown content
│   └── posts/
├── layouts/               # Custom templates
├── static/                # Copied as-is to / (favicons, images, etc.)
├── themes/
├── config.toml|yaml|json  # Site configuration
└── public/                # Build output (generated)
```

## Create a New Post
Use `hugo new` to scaffold content with front matter (defaults come from `archetypes/default.md`).

- If your `contentDir` is the default (`content/`), either of these typically works:
  ```bash
  hugo new posts/my-first-post.md
  # or (explicit path form used by newer docs)
  hugo new content/posts/my-first-post.md
  ```

- The new file will have `draft: true` in the front matter. To include drafts when testing or building, use `-D` (or `--buildDrafts`).

## Local Development (Preview Server)
Run the built-in server with hot reload:
```bash
hugo server -D
```
Useful flags:
- `-D` / `--buildDrafts` – include draft content
- `-F` / `--buildFuture` – include posts dated in the future
- `-p 1313` – change port (default is 1313)
- `--disableFastRender` – full re-render on changes (safer if layouts are changing)

Then open: <http://localhost:1313>

## Build the Site (Production)
The core build command is simply `hugo`. Many setups also accept `hugo build` as a synonym. If `hugo build` isn’t recognized on your machine, use `hugo`.

Basic builds:
```bash
# Standard production build → outputs to ./public
hugo

# Build including drafts and future-dated posts
hugo -D -F

# Clean up cache, minify assets (nice for CI)
hugo --gc --minify
```

Explicit alias (if supported by your Hugo version):
```bash
# Some installations support this subcommand as an alias
hugo build
```

### Output directory
- Default: `public/`
- Change via `--destination` (e.g., `hugo --destination dist`)

## Configuration Tips
- Set your base URL in `config.toml` (important for production):
  ```toml
  baseURL = "https://your-domain.example/"
  title = "Your Site Title"
  theme = "your-theme"
  ```
- If deploying to a subpath (e.g., GitHub Pages user/organization site vs. project site), adjust `baseURL` accordingly.

## Deployment (Quick Ideas)
- **GitHub Pages (project site)**: Build with `hugo --minify` → push `public/` (or your chosen `--destination`) to the `gh-pages` branch. Consider using a CI workflow to run `hugo` on push and publish automatically.
- **Static hosts** (Netlify, Vercel, Cloudflare Pages): Set build command to `hugo` and publish directory to `public`.

## Troubleshooting
- Nothing shows up locally? Try `hugo server -D --disableFastRender`.
- Post not visible? Check `draft`, `publishDate`, and `expiryDate` in front matter.
- Theme problems? Ensure `theme` is set in config and that the theme is downloaded in `themes/` or added as a module/submodule.

## TL;DR (Cheat Sheet)
```bash
# Create post
hugo new posts/my-first-post.md

# Run local server (include drafts)
hugo server -D

# Build for production
hugo --gc --minify
# (or) if supported: hugo build
```