# GitHub Actions Workflow Setup Guide

## Quick Start

### Step 1: Access GitHub Actions
1. Go to your repository on GitHub
2. Click the **Actions** tab
3. Click **New workflow**

### Step 2: Set Up a Workflow
1. Click **set up a workflow yourself** (or select a template)
2. You'll see a blank `.yml` file editor
3. Copy the workflow code from WORKFLOW_TEMPLATES.md
4. Paste it into the editor
5. Click **Commit changes**

### Step 3: Verify Workflow
1. Go back to **Actions** tab
2. You should see your workflow listed
3. Workflows automatically run on their triggers

---

## Detailed Setup Instructions

### Issue Auto-Management Workflow

**File to create:** `.github/workflows/issue-automation.yml`

**Steps:**
1. In repository, click **Actions** tab
2. Click **New workflow** → **set up a workflow yourself**
3. Replace default content with code from WORKFLOW_TEMPLATES.md section "Issue Auto-Management Workflow"
4. Modify if needed:
   - Change `projects/3` to your project number
   - Change `jvzhu` to your username
5. Click **Commit changes**
6. Select **Commit directly to the main branch**
7. Click **Commit changes** button

**Expected behavior:**
- When issues are created with `[BUG]`, `[FEATURE]`, or `[DATA]` tags, they're automatically labeled
- Issues are added to your project board

---

### Pull Request Automation Workflow

**File to create:** `.github/workflows/pr-automation.yml`

**Steps:**
1. Click **Actions** → **New workflow** → **set up a workflow yourself**
2. Copy "Pull Request Automation Workflow" code from WORKFLOW_TEMPLATES.md
3. Modify:
   - Change reviewer names: `['jvzhu']` → `['your-username']`
4. Commit directly to main

**Expected behavior:**
- Auto-requests reviewers when PR is opened
- Validates PR has description
- Links PRs to issues

---

### Data Collection Validation Workflow

**File to create:** `.github/workflows/data-collection-validation.yml`

**Steps:**
1. Click **Actions** → **New workflow** → **set up a workflow yourself**
2. Copy "Data Collection Validation Workflow" code from WORKFLOW_TEMPLATES.md
3. No modifications needed (uses generic checks)
4. Commit directly to main

**Expected behavior:**
- When PRs modify `data/` folder or `sources.md`
- Validates that PR description includes:
  - Data Source
  - Collection Method
  - Citation/Sources
- Comments on PR if validation fails

---

### Project Board Automation Workflow

**File to create:** `.github/workflows/project-board-automation.yml`

**Steps:**
1. Click **Actions** → **New workflow** → **set up a workflow yourself**
2. Copy "Project Board Automation Workflow" code from WORKFLOW_TEMPLATES.md
3. No modifications needed
4. Commit directly to main

**Expected behavior:**
- Issues automatically move to Done when closed
- Closes stale issues after 60 days
- Archives completed issues after 30 days

---

### Changelog & Release Workflow

**File to create:** `.github/workflows/changelog-release.yml`

**Steps:**
1. Click **Actions** → **New workflow** → **set up a workflow yourself**
2. Copy "Changelog & Release Workflow" code from WORKFLOW_TEMPLATES.md
3. No modifications needed
4. Commit directly to main

**Expected behavior:**
- Updates CHANGELOG.md on each commit to main
- Creates releases when tags are pushed (format: `v1.0.0`)

---

### Documentation Updates Workflow

**File to create:** `.github/workflows/docs-update.yml`

**Steps:**
1. Click **Actions** → **New workflow** → **set up a workflow yourself**
2. Copy "Documentation Updates Workflow" code from WORKFLOW_TEMPLATES.md
3. No modifications needed
4. Commit directly to main

**Expected behavior:**
- Weekly updates to README metrics
- Verifies issue templates exist
- Keeps documentation current

---

## How to Monitor Workflows

### View Workflow Runs
1. Go to **Actions** tab
2. Click on a workflow name (e.g., "Issue Auto-Management")
3. See all runs with timestamps and status
4. Click a run to see detailed logs

### View Specific Job Logs
1. Click a workflow run
2. Click the job name (e.g., "auto-label")
3. Expand steps to see detailed output
4. Search for errors if workflow failed

### Enable/Disable a Workflow
1. Go to **Actions** tab
2. Click the workflow you want to disable
3. Click **...** menu at top right
4. Select **Disable workflow** or **Enable workflow**

---

## Troubleshooting

### Workflow Not Running

**Check:**
1. Go to **Actions** tab
2. Look for the workflow in the list
3. If not listed, workflow file wasn't created correctly
4. **Fix:** Delete the file and recreate it from WORKFLOW_TEMPLATES.md

**Common issues:**
- Syntax error in YAML (check indentation)
- File in wrong location (must be `.github/workflows/filename.yml`)
- Typo in trigger event names

---

### Issues Not Being Auto-Labeled

**Check:**
1. Go to **Actions** tab
2. Click "Issue Auto-Management" workflow
3. Click the most recent run
4. Click the "auto-label" job
5. Look for error messages

**Common issues:**
- Label doesn't exist in repository
- Wrong trigger syntax in workflow
- Typo in label names

**Fix:**
1. Go to **Issues** tab
2. Click **Labels** 
3. Create any missing labels (bug, enhancement, data collection)
4. Re-run workflow manually

---

### PRs Not Getting Reviewers

**Check:**
1. Open a recent PR
2. Check if reviewers were requested
3. Go to **Actions** → "Pull Request Automation" → most recent run

**Common issues:**
- Username in workflow doesn't exist
- Account permissions insufficient
- PR triggers didn't match

**Fix:**
1. Edit the workflow
2. Verify reviewer usernames are correct and exist
3. Check account has permission to request reviews

---

### Project Board Not Updating

**Check:**
1. Go to project board (https://github.com/users/jvzhu/projects/3)
2. Verify repository is linked
3. Check Actions for "Project Board Automation" errors

**Common issues:**
- Project number incorrect
- Repository not linked to project
- GitHub Actions doesn't have project access

**Fix:**
1. Verify project number in workflow
2. Go to project board settings
3. Confirm all repositories are linked
4. Check Actions → General → Workflow permissions

---

## Manual Workflow Triggers

### Run a Workflow Manually
1. Go to **Actions** tab
2. Click the workflow name
3. Click **Run workflow** button (top right)
4. Select branch to run on
5. Click **Run workflow** (green button)
6. Wait for execution

### When to Use Manual Triggers
- Testing new workflow
- Forcing an update (e.g., generate changelog now)
- Re-running after fixing issues
- Testing data collection validation

---

## Advanced: Editing Workflows

### View/Edit a Workflow
1. Go to **Actions** tab
2. Click workflow name
3. Click the **...** menu
4. Select **Edit workflow**
5. Make changes
6. Click **Commit changes**

### Common Edits
- Change reviewer names
- Update project number
- Modify label names
- Add new conditions

---

## Viewing Workflow Performance

### See Workflow Statistics
1. Go to repository → **Insights** tab
2. Click **Actions** on the left
3. View:
   - Total runs
   - Success/failure rates
   - Execution time trends
   - Most used workflows

---

## Best Practices

1. **Start Simple** - Set up one workflow at a time
2. **Test First** - Use manual triggers to test
3. **Monitor Logs** - Check logs if workflow fails
4. **Document Changes** - Update AUTOMATION.md if you modify workflows
5. **Review Regularly** - Check if workflows still meet your needs

---

## Getting Help

### GitHub Actions Documentation
- https://docs.github.com/en/actions
- https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions

### Workflow Troubleshooting
- Check **Actions** tab logs for specific errors
- Review TROUBLESHOOTING.md for common issues
- See workflow output for detailed execution details

### Team Support
- Review AUTOMATION.md for workflow descriptions
- Check WORKFLOW_TEMPLATES.md for code reference
- Consult PROJECT_MANAGEMENT.md for process questions

---

**Last Updated:** 2026-06-21
**Questions?** Check AUTOMATION.md or TROUBLESHOOTING.md
