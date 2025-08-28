# Post Merged PRs to Mattermost

This GitHub Action collects all pull requests merged in the **last 24 hours** for the current repository and posts a formatted list to a Mattermost channel via an incoming webhook.

## Features

* Uses the GitHub CLI (`gh`) to query merged PRs.
* Automatically detects the current repository (`$GITHUB_REPOSITORY`).
* Outputs a nicely formatted list including PR number, title, URL, and author.
* Sends the result to Mattermost using a webhook.
* If no PRs were merged in the last 24h, it posts a fallback message.

## Inputs

| Name                     | Required | Description                                                                        |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- |
| `mattermost_webhook_url` | Yes      | Incoming webhook URL for your Mattermost channel.                                  |
| `gh_token`               | Yes      | GitHub token (usually `secrets.GITHUB_TOKEN`) to authenticate with the GitHub CLI. |

## Example Usage

```yaml
name: post-merged-prs
on:
  schedule:
    - cron: "0 0 * * *" # Runs daily at 00:00 UTC
  workflow_dispatch:

jobs:
  post-prs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: your-org/your-repo@v1
        with:
          mattermost_webhook_url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
          gh_token: ${{ secrets.GITHUB_TOKEN }}
```

## Output Example

If PRs were merged in the last 24h, the Mattermost message will look like:

<img width="484" height="161" alt="image" src="https://github.com/user-attachments/assets/ebeb0b42-336a-4a08-af9a-a6c889d8e22c" />


If no PRs were merged:

<img width="458" height="71" alt="image" src="https://github.com/user-attachments/assets/3e87676e-6896-4aba-b05d-440b2687a05a" />


## Requirements

* The [GitHub CLI](https://cli.github.com/) is preinstalled on GitHub-hosted runners.
* This action must have permission to read pull requests. By default, `secrets.GITHUB_TOKEN` is sufficient.
