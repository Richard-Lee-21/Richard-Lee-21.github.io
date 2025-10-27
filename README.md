# Richard-Lee-21.github.io

## Scheduled Crawler with GitHub Pages Auto-Update

This repository demonstrates how to set up a scheduled crawler that automatically updates GitHub Pages.

### The Problem (Fixed)

Previously, the crawler workflow would run on schedule but the pages wouldn't update because:
- The crawler modified files but didn't commit them
- Without commits, GitHub Pages never knew to rebuild

### The Solution

The `.github/workflows/crawler.yml` workflow now:
1. ✅ Runs on a schedule (daily at midnight UTC by default)
2. ✅ Executes the crawler to fetch/process data
3. ✅ **Commits the changes** to the repository
4. ✅ **Pushes the changes** back to GitHub
5. ✅ Triggers GitHub Pages to rebuild automatically

### Key Components

- **`permissions: contents: write`** - Allows the workflow to push changes
- **Git commit and push step** - Ensures changes are saved to the repository
- **`workflow_dispatch`** - Allows manual testing of the workflow

### Testing

1. Go to **Actions** tab
2. Select **Scheduled Crawler** workflow
3. Click **Run workflow**
4. After it completes, check if new files appear in the repository
5. GitHub Pages should automatically rebuild with the new content

### Customization

Edit `.github/workflows/crawler.yml` to replace the example crawler with your actual data fetching logic.

See `.github/workflows/README.md` for detailed documentation.