# GitHub Actions Automation Guide

## Overview
This document describes the automated workflows configured for your repositories.

## Available Workflows

### 1. Issue Auto-Management
**File:** `issue-automation.yml`
**Triggers:** When issue is created, reopened, or edited

**Actions:**
- Automatically adds new issues to the Data Gathering project board
- Auto-labels issues based on keywords:
  - `[BUG]` → labels as "bug"
  - `[FEATURE]` → labels as "enhancement"
  - `[DATA]` → labels as "data collection"
- Auto-assigns issues to the issue creator

### 2. Pull Request Workflows
**File:** `pr-automation.yml`
**Triggers:** When PR is opened/reopened or pushed to main/master/develop

**Actions:**
- Links PRs to related issues (references like #123)
- Auto-requests reviewers on new PRs
- Validates PR description has sufficient detail
- Warns if PR doesn't reference an issue

### 3. Project Board Automation
**File:** `project-board-automation.yml`
**Triggers:** Issue/PR state changes and weekly schedule

**Actions:**
- Moves issues to "Done" column when closed
- Moves issues to "Backlog" column when opened
- Archives completed issues older than 30 days
- Closes stale issues (60+ days inactive)

### 4. Data Collection Validation
**File:** `data-collection-validation.yml`
**Triggers:** PRs affecting `/data/` or `sources.md`

**Actions:**
- Validates PR has required data collection metadata:
  - Data Source information
  - Collection Method documentation
  - Proper Citations/Sources
- Verifies sources.md is updated
- Auto-generates data collection report

### 5. Changelog & Release Management
**File:** `changelog-release.yml`
**Triggers:** Pushes to main/master and tag creation

**Actions:**
- Auto-generates CHANGELOG.md from recent commits
- Creates GitHub releases automatically
- Updates repository metrics in documentation

### 6. Documentation Updates
**File:** `docs-update.yml`
**Triggers:** Pushes to main/master, weekly schedule, issue state changes

**Actions:**
- Updates README.md with current metrics
- Updates PROJECT_MANAGEMENT.md with issue breakdowns
- Verifies issue templates exist and are current
- Syncs documentation across branches

## How to Set Up Workflows

### Step 1: Create the Workflow Files
1. Go to your repository
2. Click **Actions** tab
3. Click **New workflow**
4. Create files in `.github/workflows/` with the content below

### Step 2: Copy Workflow Content
Use the workflow templates provided in the WORKFLOW_TEMPLATES.md file

### Step 3: Customize for Your Repo
Replace placeholders:
- `jvzhu` → your username
- `projects/3` → your project number
- Reviewer names and assignments

### Step 4: Enable Workflows
- Workflows are enabled by default once committed
- Check **Actions** tab to see workflow runs

## Manual Workflow Triggers

To manually run a workflow:
1. Go to **Actions** tab
2. Select the workflow name
3. Click **Run workflow**
4. Select the branch
5. Click **Run workflow** button

## Workflow Schedules

### Daily Checks
- Documentation updates (daily at midnight UTC)

### Weekly Checks
- Stale issue detection (Sundays)
- Archive old completed issues
- Documentation sync

### Per-Event Checks
- Issue created → Auto-label, add to board
- PR created → Validate, request reviewers
- PR closed → Move to Done column

## Disabling Workflows

To disable a workflow temporarily:
1. Go to **Actions** tab
2. Click the workflow name
3. Click **...** menu
4. Select **Disable workflow**

To re-enable:
1. Same location
2. Click **...** menu
3. Select **Enable workflow**

## Troubleshooting

### Workflow Failed - Check These:

**Permission Errors:**
- Ensure `GITHUB_TOKEN` has sufficient permissions
- Check repository settings → Actions → General → Workflow permissions

**Issues Not Auto-Labeled:**
- Verify label names match exactly
- Check that the label exists in your repository

**PRs Not Linked to Issues:**
- Ensure PR body includes `Fixes #123` or `Relates to #456`
- Use the proper syntax

**Project Board Not Updating:**
- Verify project exists and is linked to repository
- Check that project number in workflow is correct
- Ensure project is accessible to GitHub Actions

**Release Creation Failed:**
- Verify tag format follows `v*` pattern
- Check that CHANGELOG.md exists

## Monitoring Workflows

### View Workflow Status
1. Go to **Actions** tab
2. Click a workflow run to see details
3. Click a job to see logs

### Workflow Report
Repository → **Insights** → **Actions** shows:
- Workflow execution history
- Pass/fail rates
- Execution times

## Advanced Configuration

### Environment Variables
Add to workflow for dynamic configuration:
```yaml
env:
  PROJECT_NUMBER: 3
  REVIEWER_NAME: jvzhu
```

### Conditional Steps
Run steps only if conditions met:
```yaml
if: github.event.action == 'opened'
```

### Matrix Builds
Test across multiple versions/environments:
```yaml
strategy:
  matrix:
    node-version: [14, 16, 18]
```

## Best Practices

1. **Keep Workflows Simple** - One purpose per workflow
2. **Use Descriptive Names** - Makes logs easier to read
3. **Monitor Performance** - Check if workflows cause slowdowns
4. **Document Changes** - Update this guide when modifying workflows
5. **Regular Reviews** - Periodically check if workflows still serve their purpose

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Available Actions](https://github.com/actions)

---
**Last Updated:** 2026-06-21
**Maintained By:** Research Team
