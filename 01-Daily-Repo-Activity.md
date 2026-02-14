# Create an agentic workflow

Create a workflow for GitHub Agentic Workflows using https://raw.githubusercontent.com/github/gh-aw/main/create.md  
The purpose of the workflow is a daily report on recent activity in the repository, delivered as an issue.

## Prompt Use

Create a workflow for GitHub Agentic Workflows using [https://raw.githubusercontent.com/github/gh-aw/main/create.md.](https://raw.githubusercontent.com/github/gh-aw/main/create.md.) The purpose of the workflow is a daily report on recent activity in the repository, delivered as an issue.

## Steps to Create and Execute the Workflow

### Step 1: Install/Verify GitHub Agentic Workflows CLI

Check if `gh aw` is installed:

```
gh aw version
```

If not installed, run the installation script:

```
curl -sL https://raw.githubusercontent.com/github/gh-aw/main/install-gh-aw.sh | bash
```

### Step 2: Create New Workflow

Create a new workflow named `daily-repo-activity`:

```
gh aw new daily-repo-activity
```

This creates:

*   `.github/workflows/daily-repo-activity.md` - Workflow definition file

### Step 3: Configure the Workflow

Edit `.github/workflows/daily-repo-activity.md` with the following configuration:

**Update the trigger section:**

```
on:
  schedule: daily  # Runs daily on fuzzy schedule
  workflow_dispatch:  # Manual trigger option
```

**Set permissions:**

```
permissions:
  contents: read
  issues: read
  pull-requests: read
```

**Enable safe outputs:**

```
safe-outputs:
  create-issue:
    max: 5
```

**Replace instructions section with:**

```
## Instructions

1. Gather recent repository activity from the past 24 hours, including:
   - Recent commits and their commit messages
   - Pull requests opened, closed, or merged
   - Issues opened, closed, or commented on
   - Recent releases or tags

2. Analyze the activity and create a summary report

3. Create a new GitHub issue with:
   - Title: "Daily Repo Activity Report - [Date]"
   - Body: A well-formatted report including:
     - Summary of key activities
     - Recent commits with authors and topics
     - PRs with status (opened/merged/closed)
     - Issues with status updates
     - Any notable patterns or highlights

4. Label the issue with "report" and "automated"
```

### Step 4: Compile the Workflow

Compile the workflow to generate the GitHub Actions lock file:

```
gh aw compile daily-repo-activity
```

This generates:

*   `.github/workflows/daily-repo-activity.lock.yml` - Compiled GitHub Actions workflow

### Step 5: Verify Compilation

Confirm there are no errors or warnings:

```
gh aw compile daily-repo-activity
```

Expected output:

```
✓ .github\workflows\daily-repo-activity.md (50.7 KB)
✓ Compiled 1 workflow(s): 0 error(s), 0 warning(s)
```

### Step 6: Commit and Push Changes

Add and commit the workflow files:

```
git add .gitattributes .github/workflows/daily-repo-activity.md .github/workflows/daily-repo-activity.lock.yml
git commit -m "Add daily repository activity report workflow"
git push
```

### Step 7: Verify Workflow in GitHub

1.  Go to your repository on GitHub
2.  Navigate to **Actions** tab
3.  Look for `daily-repo-activity` workflow
4.  Manually trigger it with the "Run workflow" button to test
5.  Check the workflow runs and verify it creates an issue with the daily activity report

### Expected Output

The workflow will create an issue titled "Daily Repo Activity Report - \[Date\]" with:

*   Summary of commits, PRs, and issues from the past 24 hours
*   Listed authors and changes
*   Status updates on pull requests and issues
*   Labeled with "report" and "automated" tags

### Troubleshooting

**If compilation fails with permission errors:**

*   Ensure `permissions` includes all required access (contents, issues, pull-requests)
*   Use `safe-outputs` for write operations instead of direct permissions

**If workflow doesn't run:**

*   Check the Actions tab for error logs
*   Verify the schedule trigger is properly formatted
*   Manually trigger with `workflow_dispatch` to test

**View workflow logs:**

```
gh aw logs daily-repo-activity
```