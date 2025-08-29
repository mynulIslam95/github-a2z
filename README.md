## Create & Design a GitHub Account
**Definition:** Open a GitHub account and set up your profile.  
**Why:** Needed to host repos, collaborate, and share work.  
**How:** Create on github.com, add name and bio, add SSH key in Settings > SSH and GPG keys.  

---

## Download & Configure Git
**Definition:** Install Git and set your identity.  
**Why:** Git needs your name and email for commit history.  
**Commands:**
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --list

---

## Three Stage Architecture
**Definition:** Working directory, staging area, repository.  
**Why:** Lets you prepare and record snapshots cleanly.  
**Commands:**
git status
git add <file>          # stage
git restore --staged <file>
git commit -m "message" # save snapshot

---

## Clone Repo
**Definition:** Copy a remote repo to your machine.  
**Why:** Start local work on an existing project.  
**Commands:**
git clone <repo_url>
git clone git@github.com:mynulislam95/github-a2z.git   # SSH example

---

## File Status Lifecycle
**Definition:** Untracked → modified → staged → committed.  
**Why:** Know what Git will include in the next commit.  
**Commands:**
git status
git add .              # stage all tracked changes and new files
git commit -m "add notes"

---

## Git diff
**Definition:** Show changes between versions or states.  
**Why:** Review before committing or pushing.  
**Commands:**
git diff                 # working vs index
git diff --staged        # index vs HEAD
git diff main..feature-x # branch vs branch

---

## Rename and Move Files
**Definition:** Change file names or locations with history preserved.  
**Why:** Keep history readable after refactors.  
**Commands:**
git mv old.md docs/new.md
git commit -m "move notes to docs/"

---

## Fetch, Pull, Push
**Definition:** Sync with remotes.  
**Why:** Keep local and remote in sync.  
**Commands:**
git remote -v
git fetch                # download refs only
git pull                 # fetch + merge/rebase
git push                 # upload commits
git push -u origin main  # set upstream

---

## Merge Conflict
**Definition:** Two edits touch the same lines.  
**Why:** Must resolve to combine work.  
**Commands:**
git merge feature-x
# fix files in editor
git add <conflicted_file>
git commit

---

## Collaboration
**Definition:** Work together with branches and pull requests.  
**Why:** Isolate work, review changes, keep main stable.  
**Commands:**
git checkout -b feat/eda-v1
git push -u origin feat/eda-v1
# open Pull Request on GitHub from feat/eda-v1 into main

---

## Hosting Projects in GitHub Pages
**Definition:** Serve a static site from a repo.  
**Why:** Publish docs or demos.  
**Steps:** Push site to main or gh-pages, enable Pages in Settings.  

---

## GitHub Desktop
**Definition:** GUI client for Git and GitHub.  
**Why:** Visual history and easy staging if you prefer not to use CLI.  
**How:** Download from desktop.github.com and sign in.  

---

## CI/CD Pipeline (GitHub Actions)
**Definition:** Automate build, test, and deploy.  
**Why:** Ensure repeatable checks on every push.  
**Example workflow:** `.github/workflows/ci.yml`
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.11"
      - run: python -m pip install -U pip
      - run: pip install -r requirements.txt
      - run: pytest -q || true

---

# Extra essentials for data projects

## Branch Strategy for Experiments
**Definition:** Use branches to isolate experiments.  
**Why:** Compare ideas without breaking main.  
**Commands:**
git checkout -b exp/cnnlstm-01
# work and commit
git push -u origin exp/cnnlstm-01
# after review
git checkout main
git merge --no-ff exp/cnnlstm-01
git branch -d exp/cnnlstm-01

---

## Issues and Project Boards
**Definition:** Track tasks with Issues and plan with Projects.  
**Why:** Replaces scattered to-do lists and keeps context.  
**How:** Create Issues like “clean SMARD PV data”, “add holiday features”, link PRs, manage a Kanban board in Projects.  

---

## Pull Requests and Self Review
**Definition:** Propose changes from a branch to main.  
**Why:** Document intent, run checks, request feedback.  
**How:** Open PR, add summary, screenshots or nbviewer links, assign a reviewer or self-review checklist.  

---

## .gitignore for Data Work
**Definition:** Files Git should not track.  
**Why:** Avoid committing large data or secrets.  
**Example .gitignore:**
data/
*.csv
*.parquet
*.zip
*.h5
__pycache__/
*.pyc
.ipynb_checkpoints/
.env
.DS_Store

---

## Secrets and Environment Files
**Definition:** Keep credentials out of the repo.  
**Why:** Security and compliance.  
**How:** Use .env locally and GitHub Actions Secrets for CI.  
# never commit .env
echo "OPENWEATHER_KEY=xxx" >> .env  
In Actions use ${{ secrets.OPENWEATHER_KEY }}.  

---

## Pre-commit Hooks for Clean Commits
**Definition:** Auto-format or lint before commit.  
**Why:** Consistent style, fewer CI failures.  
**Setup:**
pip install pre-commit black isort flake8 nbstripout
pre-commit sample-config > .pre-commit-config.yaml
pre-commit install
nbstripout --install   # strip notebook outputs on commit

---

## Tags and Releases for Milestones
**Definition:** Mark important points in history.  
**Why:** Easy rollback and citations in the thesis.  
**Commands:**
git tag -a v0.1 -m "EDA complete with features"
git push origin v0.1
# create a Release from this tag on GitHub with notes and assets

---

## Repo Structure for Data Projects
**Definition:** Predictable layout for code and data.  
**Why:** Easier onboarding and automation.  
**Template:**
github-a2z/
  README.md
  data/              # ignored by Git
  notebooks/
  src/
  models/            # binaries ignored or managed
  reports/
  requirements.txt
  .gitignore
  .pre-commit-config.yaml

---

## Git LFS or DVC
**Definition:** Track large files with LFS or version datasets with DVC.  
**Why:** Git is not suited for big binaries.  

**Git LFS:**
git lfs install
git lfs track "*.parquet"
git add .gitattributes  

**DVC basics:**
pip install dvc
dvc init
dvc add data/raw.csv
git add data/raw.csv.dvc .dvc/.gitignore
git commit -m "track raw data with dvc"
# configure remote storage once, for example:
dvc remote add -d storage s3://bucket/path
dvc push

---

## Notebook Best Practices
**Definition:** Keep notebooks lightweight and reproducible.  
**Why:** Smaller diffs and stable runs.  
**Tips:** clear outputs before commit, keep heavy code in src/, use parameters.  

---

## Conventional Commits (optional)
**Definition:** Commit message spec like feat:, fix:, docs:.  
**Why:** Clean history and better changelogs.  
**Examples:**
feat: add holiday feature generation  
fix: handle missing timestamps in SMARD import  
docs: update training instructions  

---

## Basic Contribution Guide (for public repos)
**Definition:** Simple rules for contributors.  
**Why:** Consistent PRs and faster reviews.  
**Add CONTRIBUTING.md:**  
- branch from main  
- write small commits  
- add tests or sample notebook cells  
- open PR with clear summary  
