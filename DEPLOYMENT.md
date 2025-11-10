# GitHub Pages Deployment Guide

This guide walks you through deploying your MkDocs blog to GitHub Pages.

## Prerequisites

- A GitHub account
- Git installed locally
- Your blog repository initialized with git

## Step 1: Initialize Git Repository (if not already done)

```bash
cd /path/to/blog
git init
git add .
git commit -m "Initial blog commit"
```

## Step 2: Create GitHub Repository

1. Go to [GitHub.com](https://github.com/new)
2. Create a new repository named `blog` (or your preferred name)
3. Choose public or private based on your preference
4. Do NOT initialize with README, .gitignore, or license (we already have these)

## Step 3: Add Remote and Push

```bash
git remote add origin https://github.com/YOUR_USERNAME/blog.git
git branch -M main
git push -u origin main
```

Replace `YOUR_USERNAME` with your actual GitHub username.

## Step 4: Enable GitHub Pages

1. Go to your repository settings
2. Scroll down to "Pages" section
3. Under "Source", select:
   - Branch: `gh-pages`
   - Folder: `/ (root)`
4. Save

The `gh-pages` branch will be created automatically by the GitHub Actions workflow on first push.

## Step 5: Configure Custom Domain (Optional)

If you have a custom domain:

1. Go to your domain registrar's DNS settings
2. Add a CNAME record pointing to `YOUR_USERNAME.github.io`
3. In the deployment workflow (`.github/workflows/deploy.yml`), update:
   ```yaml
   cname: yourdomain.com
   ```

For GitHub Pages subdomain (free):
- Your blog will be available at: `https://YOUR_USERNAME.github.io/blog`

## Step 6: Update Repository Settings (Important!)

After the first deployment:

1. Go to repository Settings → Pages
2. Verify that:
   - Source is set to `Deploy from a branch`
   - Branch is set to `gh-pages`
   - Folder is set to `/ (root)`

## Automatic Deployment

Once set up, your blog will automatically deploy whenever you:

1. Push to the `main` branch
2. Create a pull request (builds but doesn't deploy)

The GitHub Actions workflow (`.github/workflows/deploy.yml`) will:
- Build the site with `mkdocs build`
- Deploy to GitHub Pages automatically
- Make your blog live at your GitHub Pages URL

## Manual Deployment (Alternative)

If you prefer to deploy manually without GitHub Actions:

```bash
# Build the site
mkdocs build

# Deploy using ghp-import
pip install ghp-import
ghp-import -p site
```

## Troubleshooting

### Site not appearing after push
- Wait 2-3 minutes for GitHub Actions to complete
- Check the "Actions" tab in your repository for any failed workflows
- Verify the `gh-pages` branch exists in your repository

### 404 errors on pages
- Check that `site_url` in `mkdocs.yml` matches your actual domain
- For GitHub Pages: `https://YOUR_USERNAME.github.io/blog`

### Custom domain not working
- Verify CNAME file is created (GitHub creates this automatically)
- Wait up to 48 hours for DNS propagation
- Check your domain registrar's DNS settings

## Monitoring Deployments

1. Go to your repository
2. Click the "Actions" tab
3. View the deployment workflow runs
4. Check logs for any errors

## Updating Your Blog

After deployment setup, simply:

1. Edit your blog posts in `docs/blog/posts/`
2. Commit changes: `git add . && git commit -m "Update posts"`
3. Push to GitHub: `git push`
4. GitHub Actions will automatically build and deploy

Your changes will be live within a few minutes!

## Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [MkDocs Deployment Guide](https://www.mkdocs.org/user-guide/deploying-your-docs/)
- [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)
