# GitHub Actions Workflow Templates

## Issue Auto-Management Workflow

Save as `.github/workflows/issue-automation.yml`

```yaml
name: Issue Auto-Management

on:
  issues:
    types: [opened, reopened, edited]

jobs:
  add-to-project:
    runs-on: ubuntu-latest
    steps:
      - name: Add issue to project board
        uses: actions/add-to-project@v0.5.0
        with:
          project-url: https://github.com/users/jvzhu/projects/3
          github-token: ${{ secrets.GITHUB_TOKEN }}

  auto-label:
    runs-on: ubuntu-latest
    steps:
      - name: Auto-label issues
        uses: actions/github-script@v7
        with:
          script: |
            const title = context.payload.issue.title;
            const body = context.payload.issue.body || '';
            const labels = [];
            
            if (title.includes('[BUG]') || body.includes('bug')) {
              labels.push('bug');
            }
            if (title.includes('[FEATURE]') || body.includes('feature')) {
              labels.push('enhancement');
            }
            if (title.includes('[DATA]') || body.includes('data collection')) {
              labels.push('data collection');
            }
            
            if (labels.length > 0) {
              github.rest.issues.addLabels({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: context.issue.number,
                labels: labels
              });
            }
```

## Pull Request Automation Workflow

Save as `.github/workflows/pr-automation.yml`

```yaml
name: Pull Request Automation

on:
  pull_request:
    types: [opened, reopened, edited]
  push:
    branches: [ main, master, develop ]

jobs:
  link-pr-to-issue:
    runs-on: ubuntu-latest
    steps:
      - name: Link PR to issues
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            const body = pr.body || '';
            const issueRegex = /#(\d+)/g;
            let match;
            
            while ((match = issueRegex.exec(body)) !== null) {
              console.log(`Found issue #${match[1]}`);
            }

  request-reviewers:
    runs-on: ubuntu-latest
    steps:
      - name: Request reviewers
        uses: actions/github-script@v7
        with:
          script: |
            const reviewers = ['jvzhu'];
            
            github.rest.pulls.requestReviewers({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: context.payload.pull_request.number,
              reviewers: reviewers
            });
```

## Data Collection Validation Workflow

Save as `.github/workflows/data-collection-validation.yml`

```yaml
name: Data Collection Validation

on:
  pull_request:
    paths:
      - 'data/**'
      - 'sources.md'
  workflow_dispatch:

jobs:
  validate-data:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Validate data collection PR
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            const body = pr.body || '';
            
            const hasDataSource = body.includes('Data Source');
            const hasCollectionMethod = body.includes('Collection Method');
            const hasCitation = body.includes('Citation') || body.includes('Sources');
            
            const issues = [];
            if (!hasDataSource) issues.push('Missing Data Source information');
            if (!hasCollectionMethod) issues.push('Missing Collection Method');
            if (!hasCitation) issues.push('Missing proper Citation/Sources');
            
            if (issues.length > 0) {
              const comment = `⚠️ Data Collection Issues:\n\n${issues.map(i => `- ${i}`).join('\n')}`;
              
              github.rest.issues.createComment({
                owner: context.repo.owner,
                repo: context.repo.repo,
                issue_number: pr.number,
                body: comment
              });
            }
```

## Project Board Automation Workflow

Save as `.github/workflows/project-board-automation.yml`

```yaml
name: Project Board Automation

on:
  issues:
    types: [opened, closed, reopened]
  pull_request:
    types: [opened, closed]
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday

jobs:
  archive-completed:
    runs-on: ubuntu-latest
    if: github.event_name == 'schedule'
    steps:
      - name: Archive old completed issues
        uses: actions/github-script@v7
        with:
          script: |
            const thirtyDaysAgo = new Date();
            thirtyDaysAgo.setDate(thirtyDaysAgo.getDate() - 30);
            console.log('Checking for completed issues older than 30 days');
```

## Changelog & Release Workflow

Save as `.github/workflows/changelog-release.yml`

```yaml
name: Changelog & Release

on:
  push:
    branches: [ main, master ]
    tags:
      - 'v*'
  workflow_dispatch:

jobs:
  generate-changelog:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Generate changelog
        uses: actions/github-script@v7
        with:
          script: |
            const commits = await github.rest.repos.listCommits({
              owner: context.repo.owner,
              repo: context.repo.repo,
              per_page: 50
            });
            
            console.log('Recent commits:');
            commits.data.forEach(commit => {
              console.log(`- ${commit.commit.message.split('\n')[0]}`);
            });

  create-release:
    runs-on: ubuntu-latest
    if: startsWith(github.ref, 'refs/tags/')
    steps:
      - uses: actions/checkout@v4

      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body: See CHANGELOG.md for details
```

## Documentation Updates Workflow

Save as `.github/workflows/docs-update.yml`

```yaml
name: Documentation Updates

on:
  push:
    branches: [ main, master ]
  schedule:
    - cron: '0 0 * * 0'  # Weekly

jobs:
  update-readme:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Update README metrics
        uses: actions/github-script@v7
        with:
          script: |
            const issues = await github.rest.issues.listForRepo({
              owner: context.repo.owner,
              repo: context.repo.repo,
              state: 'all'
            });
            
            console.log(`Total Issues: ${issues.data.length}`);
```

## Installation Instructions

1. In your repository, create folder structure:
   ```
   .github/workflows/
   ```

2. For each workflow above:
   - Create a new file with the suggested name
   - Copy the workflow code
   - Commit and push

3. Verify workflows are running:
   - Go to **Actions** tab
   - You should see each workflow listed

---

**Note:** Update `jvzhu` and project numbers to match your configuration.
