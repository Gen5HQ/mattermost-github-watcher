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
| `hours`                  | No       | How many hours back to search for merged PRs. (default: 24)                         |

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
      - uses: Gen5HQ/mattermost-github-watcher@[version]
        with:
          mattermost_webhook_url: ${{ secrets.MATTERMOST_WEBHOOK_URL }}
          gh_token: ${{ secrets.GITHUB_TOKEN }}
```

## How to get MATTERMOST_WEBHOOK_URL

<img width="378" height="385" alt="image" src="https://github.com/user-attachments/assets/2f667a6c-8b19-4c76-a713-2fae790d3d38" />
<img width="1607" height="857" alt="image" src="https://github.com/user-attachments/assets/48649d01-d801-4dec-9ed4-d30a3876bfe5" />
<img width="1311" height="268" alt="image" src="https://github.com/user-attachments/assets/331e6291-0a0a-476b-8373-b846e9ffd668" />


## Output Example

If PRs were merged in the last 24h, the Mattermost message will look like:

<img width="484" height="161" alt="image" src="https://github.com/user-attachments/assets/ebeb0b42-336a-4a08-af9a-a6c889d8e22c" />


If no PRs were merged:

<img width="458" height="71" alt="image" src="https://github.com/user-attachments/assets/3e87676e-6896-4aba-b05d-440b2687a05a" />


## Requirements

* The [GitHub CLI](https://cli.github.com/) is preinstalled on GitHub-hosted runners.
* This action must have permission to read pull requests. By default, `secrets.GITHUB_TOKEN` is sufficient.
