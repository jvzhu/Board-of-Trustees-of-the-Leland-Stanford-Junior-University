# GitHub Actions Troubleshooting Guide

## Common Issues & Solutions

### 1. Workflow File Not Showing Up in Actions Tab

**Problem:** You created a workflow file but it doesn't appear in the Actions tab.

**Diagnosis:**
1. Check the file exists: Go to your repo → Code tab → `.github/workflows/`
2. Verify file path is correct: Must be exactly `.github/workflows/filename.yml`
3. Check file name: Should end with `.yml` or `.yaml`

**Solutions:**
- **YAML Syntax Error:** Your workflow has invalid syntax
  - Fix: Use a YAML validator (yamllint.com)
  - Check indentation (must be 2 spaces, not tabs)
  - Check quotes around strings with special characters

- **Wrong File Location:** File is not in `.github/workflows/`
  - Fix: Move file to correct directory
  - GitHub only recognizes workflows in this exact path

- **Branch Issue:** File exists on a different branch
  - Fix: Make sure file is on main/master branch
  - Go to Code tab, verify branch dropdown

**How to Fix:**
1. Delete the incorrectly placed file
2. Go to Actions → New workflow → set up a workflow yourself
3. Copy code from WORKFLOW_TEMPLATES.md
4. Paste into editor
5. Commit directly to main

---

### 2. Workflow Runs But All Jobs Fail

**Problem:** Workflow appears in Actions tab but all jobs show red X (failed).

**Diagnosis:**
1. Click the workflow run
2. Click the failed job
3. Expand each step to find the error
4. Look for red text with error message

**Common Error Messages & Fixes:**

**"Resource not found"**
- Cause: GitHub token doesn't have permission
- Fix: Go to Settings → Actions → General → Workflow permissions
- Select "Read and write permissions"

**"jvzhu is not a valid username"**
- Cause: Reviewer username doesn't exist
- Fix: Change `['jvzhu']` to your actual GitHub username

**"Label does not exist"**
- Cause: Trying to add a label that isn't created yet
- Fix: Go to Issues → Labels, create any missing labels

**"Project not found"**
- Cause: Project number is wrong or project is deleted
- Fix: Go to your project board, check the URL for correct number
- Update workflow with correct `projects/3` value

---

### 3. Issues Not Auto-Labeled

**Problem:** Created issue with `[BUG]` or `[FEATURE]` but no labels were added.

**Diagnosis:**
1. Go to Actions tab
2. Click "Issue Auto-Management" workflow
3. Find your issue creation timestamp
4. Click that run
5. Click "auto-label" job
6. Expand all steps and look for errors

**Common Causes:**

**Labels don't exist:**
- Fix: Go to Issues → Labels
- Create labels: "bug", "enhancement", "data collection"

**Issue title doesn't match trigger:**
- Your issue title: "Fix login bug"
- Trigger expects: "[BUG]" or "[FEATURE]" format
- Fix: Edit issue title to include `[BUG]` or use lowercase "bug" in title

**Workflow not committed correctly:**
- File exists but wasn't properly saved
- Fix: Delete workflow file and recreate via GitHub UI

---

### 4. PRs Not Getting Reviewers Assigned

**Problem:** You created a PR but no reviewers were requested.

**Diagnosis:**
1. Go to Actions tab
2. Click "Pull Request Automation" workflow
3. Find the run matching your PR creation time
4. Click that run
5. Click "request-reviewers" job
6. Look at the error output

**Common Causes:**

**Reviewer username doesn't exist:**
- You specified: `reviewers: ['jvzhu']`
- But user "jvzhu" doesn't exist or access is denied
- Fix: Change to your actual username
- Verify the user exists: https://github.com/username

**Repository access:**
- Repository doesn't have Actions enabled
- Fix: Go to Settings → Actions → enable Actions

**PR doesn't trigger workflow:**
- Workflow only runs on `pull_request` events
- Make sure trigger is correct in workflow file
- Verify PR was created, not just drafted

---

### 5. Project Board Not Updating

**Problem:** Issues aren't being added to project board automatically.

**Diagnosis:**
1. Create a test issue
2. Go to Actions tab
3. Click "Issue Auto-Management" workflow
4. Find the run from when you created the issue
5. Click "add-to-project" job
6. Look for error messages

**Common Causes:**

**Project not linked to repository:**
- Go to your project board (https://github.com/users/jvzhu/projects/3)
- Click ... → Settings
- Verify "Linked repositories" includes your repo
- Fix: Add repository if missing

**Wrong project number in workflow:**
- Workflow references: `projects/3`
- Your actual project: `projects/5`
- Fix: Update project number in workflow
- Find correct number from project URL

**GitHub Actions permissions:**
- Go to repository Settings
- Actions → General
- Under "Workflow permissions" select "Read and write permissions"

**Project is archived:**
- Archived projects can't be updated
- Fix: Unarchive project first

---

### 6. Data Collection Validation Not Running

**Problem:** PR validation workflow doesn't run when you modify data files.

**Diagnosis:**
1. Create a PR that modifies `data/` folder or `sources.md`
2. Go to Actions tab
3. Look for "Data Collection Validation" run
4. If no run appears, workflow isn't being triggered

**Common Causes:**

**Wrong file paths in workflow:**
- Workflow watches: `data/**` and `sources.md`
- Your files: `docs/data/file.csv`
- Fix: Update file paths in workflow trigger

**PR not targeting main branch:**
- Workflow only runs PRs to specific branches
- Make sure PR targets `main` or `master`

**Workflow syntax error:**
- Paths section has invalid YAML
- Fix: Copy workflow from WORKFLOW_TEMPLATES.md again

---

### 7. Changelog Not Generating

**Problem:** CHANGELOG.md not being updated automatically.

**Diagnosis:**
1. Make a commit to main branch
2. Go to Actions → "Changelog & Release" workflow
3. Check if run appears after 2-3 minutes
4. Click run and check logs

**Common Causes:**

**CHANGELOG.md doesn't exist:**
- Workflow tries to update it but file doesn't exist
- Fix: Create CHANGELOG.md manually first
- Add: `# Changelog` as content

**Wrong branch trigger:**
- Workflow only runs on `main` or `master`
- You pushed to `feature-branch`
- Fix: Merge to main or update trigger in workflow

**No recent commits:**
- Changelog only generates if commits exist
- Merge a PR or make a commit
- Then check Actions tab for new run

---

### 8. Workflow Runs But Nothing Happens

**Problem:** Workflow completes successfully but no actual changes occur.

**Diagnosis:**
1. Click the workflow run
2. Look at job logs
3. Check if it says "Skipped" for steps
4. Search logs for "echo" statements to see what ran

**Common Causes:**

**Conditional logic preventing execution:**
- Workflow has `if:` conditions that aren't met
- Example: `if: github.event.action == 'opened'`
- But event action was 'edited'
- Fix: Test with manual trigger first

**Insufficient permissions:**
- Workflow can run but can't modify files/labels
- Goes through the motions with no effect
- Fix: Check workflow permissions (see #5)

**Workflow just logging information:**
- Not all workflows make changes
- Some only validate or report
- This is normal behavior

---

### 9. Actions Tab Showing "No Workflows"

**Problem:** Even though you have workflow files, Actions tab is empty.

**Diagnosis:**
1. Go to Code tab
2. Navigate to `.github/workflows/`
3. Do you see your workflow files listed?

**Solutions:**

**If files exist but don't show in Actions:**
- Files have syntax errors
- Copy workflow from WORKFLOW_TEMPLATES.md
- Delete bad workflow file
- Create new one with corrected code

**If files don't exist:**
- Navigate to Actions → New workflow
- Click "set up a workflow yourself"
- Copy code from WORKFLOW_TEMPLATES.md
- Name the file correctly
- Commit

**If Actions tab is disabled:**
- Go to Settings → Actions
- Select "Allow all actions and reusable workflows"
- Click Save

---

### 10. Permission Denied Errors

**Problem:** Workflow fails with "permission denied" message.

**Diagnosis:**
Look at the specific error:
- "permission denied: could not create .github/workflows/..."
- "GITHUB_TOKEN does not have permission..."

**Solutions:**

**Can't create workflow files directly:**
- Use GitHub UI (Actions → New workflow)
- Don't try to create via API or push
- This is expected if you don't have write access

**GITHUB_TOKEN permissions issue:**
- Go to Settings → Actions → General
- Under "Workflow permissions"
- Select "Read and write permissions"
- Click Save

**Repository access issues:**
- Verify you're a collaborator on the repo
- Admin or Maintain role needed for Actions
- Ask repo admin to grant access

---

## How to Get Detailed Error Information

### Step 1: Find the Failed Run
1. Go to **Actions** tab
2. Look for red X or orange circle
3. Click on the run

### Step 2: View Job Details
1. Click the job name (e.g., "auto-label")
2. Each step shows:
   - ✅ Green checkmark = success
   - ❌ Red X = failed
   - ⊘ Gray circle = skipped

### Step 3: Expand Failed Step
1. Click the failed step to expand it
2. Look for error text (usually red)
3. Copy the error message
4. Use to troubleshoot (search TROUBLESHOOTING.md)

### Step 4: Check Workflow File
1. Go to **Actions** tab
2. Click the workflow name
3. Click **...** menu
4. Select **Edit workflow**
5. Review the code for issues

---

## Manual Testing

### Test a Workflow Manually
1. Go to Actions tab
2. Click the workflow name
3. Click **Run workflow** button
4. Select your branch
5. Click **Run workflow** (green button)
6. Check the logs to see what happens

**Use manual testing to:**
- Verify workflow works
- Debug configuration issues
- Test changes before committing

---

## Getting Help

### Check These Resources
1. **AUTOMATION.md** - Workflow descriptions
2. **WORKFLOW_TEMPLATES.md** - Correct code examples
3. **WORKFLOW_SETUP_GUIDE.md** - Step-by-step setup
4. This file - **TROUBLESHOOTING.md** - Common issues

### GitHub Official Resources
- [Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Troubleshooting Workflows](https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-workflow-runs/troubleshooting-workflow-runs)

### Still Having Issues?
1. Check Actions tab logs carefully
2. Search for your error message in this guide
3. Verify workflow code matches WORKFLOW_TEMPLATES.md
4. Try recreating the workflow from scratch

---

**Last Updated:** 2026-06-21
**Need more help?** See AUTOMATION.md, WORKFLOW_SETUP_GUIDE.md, or WORKFLOW_TEMPLATES.md
