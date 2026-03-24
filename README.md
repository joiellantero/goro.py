# Guess Repo Owner

Identifies the likely owner of a GitHub repository by finding the top non-bot contributor and mapping their GitHub username to an internal employee identifier (shortname).

## How It Works

1. Fetches the contributor list for the target repository via the GitHub API.
2. Filters out bot accounts.
3. Cross-references each contributor's GitHub username against a company employee list stored as a Markdown table in a separate GitHub repository.
4. Returns the **shortname** (internal employee identifier) of the contributor with the most commits.


## Use Case

Designed for SOC and IR teams who need to quickly identify the responsible engineer for a repository during an incident—for example, when a repo is flagged for exposed secrets or credentials. The script can be integrated into an incident management tool to automate this lookup.

## Prerequisites

- Python 3.x
- A GitHub personal access token with `repo` read scope
- A separate GitHub repository containing an employee directory as a Markdown table (e.g., auto-generated via GitHub Actions) with at least `github_username` and `shortname` columns

## Setup

### 1. Configure environment variables

Create a `.env` file in the project root:

```
OWNER=your_github_org_or_username
REPOSITORY_NAME=repo_to_investigate
ACCESS_TOKEN=your_github_personal_access_token
USERS_REPO=org/repo/path/to/employee_list.md
```

| Variable | Description |
|---|---|
| `OWNER` | GitHub organization or user that owns the target repository |
| `REPOSITORY_NAME` | Name of the repository to find the owner for |
| `ACCESS_TOKEN` | GitHub personal access token for API authentication |
| `USERS_REPO` | Path to the employee Markdown file in the form `org/repo/branch/path/file.md` |

### 2. Set up the Python environment

```shell
virtualenv env
source env/bin/activate
pip install -r requirements.txt
```

### 3. Run the script

```shell
python main.py
```

The script prints the shortname of the most likely repository owner:

```
Possible repo owner: jdoe
```
