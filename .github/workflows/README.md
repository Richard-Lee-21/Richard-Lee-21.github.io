# Workflows Documentation

## Crawler Workflow (`crawler.yml`)

### Problem Solved
This workflow solves the issue where the crawler runs on schedule but pages don't get updated.

### Key Components

1. **Schedule Trigger**: Runs automatically at scheduled times
   ```yaml
   schedule:
     - cron: '0 0 * * *'  # Daily at midnight UTC
   ```

2. **Workflow Dispatch**: Allows manual triggering for testing
   ```yaml
   workflow_dispatch:
   ```

3. **Write Permissions**: **CRITICAL** - Allows the workflow to push changes
   ```yaml
   permissions:
     contents: write
   ```

4. **Commit and Push Step**: Ensures changes are committed and pushed to trigger page rebuild
   ```bash
   git config --local user.email "github-actions[bot]@users.noreply.github.com"
   git config --local user.name "github-actions[bot]"
   git add -A
   git commit -m "chore: update crawled data [skip ci]"
   git push
   ```

### Why This Works

1. **Before**: Crawler ran and modified files, but changes stayed in the workflow environment
2. **After**: Crawler runs, commits changes, pushes to repository → triggers GitHub Pages rebuild

### Customization

Replace the "Run crawler" step with your actual crawler logic. The example shows:
- A simple Python script that creates/updates a JSON file
- You can replace this with any crawler (Python, Node.js, etc.)

### Testing

1. Trigger manually: Go to Actions → Scheduled Crawler → Run workflow
2. Check if changes are committed to the repository
3. Verify GitHub Pages rebuilds with new content

### Important Notes

- `[skip ci]` in commit message prevents infinite loops if you have CI/CD that triggers on pushes
- The workflow checks if there are changes before committing to avoid empty commits
- Make sure your GitHub Pages source is set to the correct branch (usually `main`)
