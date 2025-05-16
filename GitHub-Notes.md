## 1. GitHub vs Web Hosting

* **GitHub is for code hosting and version control.**
* It **does not host your web app** for live users.
* Use **Render.com** (free tier available) for deploying your Flask app.

---

## 2. Creating a Professional GitHub Project

### Repository Setup

Go to GitHub → Click **New Repository** → Fill in:

* **Repository Name:** `nutritrack-capstone`
* Initialize with a `README.md`
* Add `.gitignore` → Choose `Python`
* Add a License → Choose `MIT License`

---

### What is `.gitignore` (Python)?

Tells Git which files **not to track** or upload to GitHub:

```bash
# Python bytecode
__pycache__/
*.py[cod]
*.egg-info/
dist/
build/

# Virtual Environments
.env
.venv/
venv/
ENV/
env.bak/
venv.bak/

# IDEs
.vscode/
.idea/
```

---

### Why Choose the MIT License?

* A license defines how others can legally use your code.
* MIT is a permissive open-source license — anyone can use, modify, and distribute it.

---

## 3. Inviting Collaborators

1. Go to your GitHub repository.
2. Click **Settings** → **Collaborators**.
3. Add teammates by their GitHub usernames.

---

## 4. Branching Model

| Branch      | Purpose                               |
| ----------- | ------------------------------------- |
| `main`      | Production branch (stable, protected) |
| `dev`       | Active development                    |
| `feature/*` | New features (e.g. `feature/login`)   |
| `bugfix/*`  | Bug fixes                             |
| `docs/*`    | Documentation updates                 |

---

### Feature Workflow

1. Create a `feature/*` branch.
2. Commit code changes.
3. Open a **Pull Request (PR)** into `dev`.
4. **1 approval required**, then merge.

### Bugfix Workflow

* Create a `bugfix/*` branch.
* Open PR into `dev`.
* **2 approvals required**, then merge.

### Development Branch Merges

* PR into `main` from `dev` requires **3 approvals**.

---

## 5. Repository Protection Rules

Enable the following rules under **Settings → Branches → Add Rule** (for `main` and `dev`):

* Require a pull request before merging
* Require approving reviews:

  * 1 approval for features
  * 2 approvals for bugfixes
  * 3 approvals for dev → main
* Block force pushes
* Restrict branch deletions (recommended)
* Require linear history (optional)

---

## 6. Render.com Deployment (Flask App)

### Prerequisites

* App code in GitHub repo
* `requirements.txt` and `gunicorn` installed
* `.env` file (not pushed to GitHub)

### Render Setup

1. Go to [Render Dashboard](https://dashboard.render.com)
2. Sign in with GitHub
3. Click **New → Web Service**
4. Select the repo and branch (test with `dev`)
5. Add environment variables manually in the Render Dashboard under the "Environment" tab

### Creating `requirements.txt`

If not already present, run:

```bash
pip freeze > requirements.txt
```

Example content:

```txt
Flask==2.2.3
gunicorn==20.1.0
Flask-SQLAlchemy==2.5.1
python-dotenv==1.1.0
```

---

## 7. Git Setup & Cloning

### Initial Setup (Local)

```bash
sudo apt update
sudo apt install git

git config --global user.name "YourFullName"
git config --global user.email "your@email.com"
```

### Cloning the Repo

```bash
git clone git@github.com:CapstoneNutritionTeam/nutritrack-capstone.git
cd nutritrack-capstone
```

### Working with Branches

```bash
git fetch
git checkout feature/branch-name
git pull
```

Make sure to untrack local virtual environments:

```bash
git rm -r --cached venv/
git commit -m "Stop tracking venv folder"
```

### Committing Changes

```bash
git add .
git commit -m "Description of changes"
git push origin feature/branch-name
```

To handle merge conflicts or sync cleanly:

```bash
git pull --rebase origin feature/branch-name
```

---

## 8. GitHub Actions (`.github/workflows/test-the-stuff.yml`)

```yaml
name: Deploy Flask App

on:
  push:
    branches: [dev]
  pull_request:
    branches: [dev]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repo code
      uses: actions/checkout@v3

    - name: Set up Python 3.10
      uses: actions/setup-python@v4
      with:
        python-version: '3.10'

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt

    - name: Run Flask tests
      run: |
        python -m unittest discover tests
```

---

## 9. Code Ownership (`.github/CODEOWNERS`)

This file tells GitHub who can approve changes to specific files:

```text
.gitignore @AJprogramming123
.github/CODEOWNERS @AJprogramming123
.env @AJprogramming123
```

---

Let me know if you'd like this broken into multiple files (e.g., `DEPLOYMENT.md`, `WORKFLOW.md`, etc.), or want to generate the `.gitignore`, `requirements.txt`, or `.env` scaffolds too.
