# gh-review

A command-line tool for fetching and managing GitHub Pull Request review comments efficiently.

## Overview

`gh-review` simplifies the PR review workflow by providing a streamlined interface to view unresolved review comments and reply to review threads directly from the command line. It leverages the GitHub CLI (`gh`) to interact with GitHub's GraphQL and REST APIs.

## Features

- **Fetch unresolved comments**: View all unresolved review threads for a PR with detailed statistics
- **Review approvals**: See review decision status and who approved, requested changes, or commented
- **PR status summary**: Check mergeability, merge state, and CI/CD check status at a glance
- **Detailed check breakdown**: See which checks passed, failed, or are still pending
- **Smart PR detection**: Automatically detects PR number from current branch or accepts explicit PR number
- **Reply to threads**: Respond to review comments directly from the command line
- **Pagination support**: Handles large PRs with many review threads
- **Status tracking**: Shows which threads are outdated, responded to, or still need attention
- **Direct links**: Provides clickable URLs to each comment and check for easy navigation

## Requirements

- Python 3.10 or higher
- [GitHub CLI (`gh`)](https://cli.github.com/) installed and authenticated
- Git repository with a configured GitHub remote

## Installation

1. Clone this repository or download the `gh-review` script
2. Make the script executable:
   ```bash
   chmod +x gh-review
   ```
3. (Optional) Add it to your PATH or create a symlink:
   ```bash
   ln -s /path/to/gh-review ~/.local/bin/gh-review
   ```

## Usage

### Fetch unresolved comments

Get unresolved review comments for the current branch's PR:
```bash
gh-review get
```

Get unresolved comments for a specific PR number:
```bash
gh-review get 123
```

### Reply to a review thread

Reply to a thread using content from a file:
```bash
gh-review reply 1234567890 response.md
```

Reply to a thread using stdin:
```bash
echo "Thanks for the review! Fixed in the latest commit." | gh-review reply 1234567890 -
```

### Get help

```bash
gh-review help
```

## Output Format

The `get` command displays:

### Review Threads
- Thread ID and file location (path:line)
- Whether the thread is outdated
- All comments in the thread with author information
- Direct links to each comment
- Summary statistics showing:
  - Total addressed threads (responded + outdated)
  - Threads with responses
  - Outdated threads

### PR Status
- **Reviews**: Overall review decision (APPROVED, CHANGES_REQUESTED, etc.)
  - Individual reviewer statuses with usernames
  - Shows who approved, requested changes, or commented
- **Mergeability**: Whether the PR can be merged (conflicts, etc.)
- **Merge State**: Current state (CLEAN, BLOCKED, BEHIND, etc.)
- **CI/CD Checks**: Overall status and detailed breakdown
  - Failed checks with links (shown first for quick attention)
  - Pending/in-progress checks
  - Passed checks (summarized count)

## Examples

```bash
# Check current PR for unresolved comments
gh-review get

# Check specific PR
gh-review get 456

# Reply to a comment
echo "LGTM, thanks!" | gh-review reply 1234567890 -

# Reply with a longer response from a file
gh-review reply 1234567890 my-response.txt
```

## How It Works

`gh-review` uses:
- **GitHub GraphQL API** to fetch review threads with pagination support
- **GitHub REST API** to post replies to review threads
- **GitHub CLI (`gh`)** as the authentication and API transport layer

The tool identifies review threads by their `discussion_r` ID, which can be found in the comment URL or in the output of the `get` command.

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

## Credits

Based on the approach described in [this Stack Overflow answer](https://stackoverflow.com/a/66072198/844449).

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.
