# 🚀 CI/CD Workflow: GitHub Actions for Python Projects

This repository demonstrates a complete **CI/CD pipeline** using **GitHub Actions** for a Python-based service. It's structured to ensure code quality, reliability, and maintainability through automatic linting and testing.

---

## 📦 Features

- ✅ **CI Trigger** on `push` and `pull_request` to `main` branch
- 🐍 Runs in a **Python 3.9** container on Ubuntu
- 🧹 **Linting with `flake8`** to enforce coding standards
- 🧪 **Unit testing with `nose`**, plus coverage reports
- 📄 Easily extendable to include deployment steps

---

## 🔧 Workflow File: `.github/workflows/workflow.yml`

```yaml
name: "CI workflow"
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    container:
      image: python:3.9-slim
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Lint with flake8
        run: |
          flake8 service --count --select=E9,F63,F7,F82 --show-source --statistics
          flake8 service --count --max-complexity=10 --max-line-length=127 --statistics

      - name: Run unit tests with nose
        run: |
          nosetests -v --with-spec --spec-color --with-coverage --cover-package=app
