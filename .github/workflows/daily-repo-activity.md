---
# Trigger - when should this workflow run?
on:
  schedule: daily  # Fuzzy daily schedule (scattered execution time)
  workflow_dispatch:  # Manual trigger

# Permissions - what can this workflow access?
permissions:
  contents: read
  issues: read
  pull-requests: read

# Outputs - what APIs and tools can the AI use?
safe-outputs:
  create-issue:          # Creates issues (default max: 1)
    max: 5               # Optional: specify maximum number
  # create-agent-session:   # Creates GitHub Copilot agent sessions (max: 1)
  # create-pull-request: # Creates exactly one pull request
  # add-comment:   # Adds comments (default max: 1)
  #   max: 2             # Optional: specify maximum number
  # add-labels:

---

# daily-repo-activity

Generate a daily report on recent repository activity and create an issue with the findings.

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

## Notes

- Run `gh aw compile` to generate the GitHub Actions workflow
- See https://github.github.com/gh-aw/ for complete configuration options and tools documentation
