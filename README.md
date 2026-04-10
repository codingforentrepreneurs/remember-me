# My Remember Me - Personal Data Pipeline

This project collects personal data from various sources and loads it into a Ghost Postgres database.

## Data Sources


- GitHub
- Google Bookmarks


## Quickstart

### Downloads (read below for more detail)

```bash
# install gh cli
brew install gh

# ghost
curl -fsSL https://install.ghost.build | sh

# psql client
# uncomment to install
# brew install libpq

# workbooks
# so you can run markdown as code
# https://docs.workbooks.dev/cli/installation
curl -fsSL https://get.workbooks.dev | sh

```


### Create Ghost Database
```bash
ghost login
ghost create --name remember-me
```

### Login to GitHub
```bash
gh auth login
```

### Clone the repo
```bash
mkdir -p ~/dev/remember-me
cd ~/dev/remember-me
git clone https://github.com/codingforentrepreneurs/remember-me .
```

### Run the github runbook
```bash
wb run runbooks/1-github.md
```

### Verify the data
```bash 
export DB_ID=$(ghost list --json | jq -r --arg name "remember-me" '.[] | select(.name == $name) | .id')
export PG_HOST=$(ghost connect $DB_ID)
psql $PG_HOST -c "SELECT name, stargazers_count FROM github_top_repos ORDER BY stargazers_count DESC LIMIT 10;"
```