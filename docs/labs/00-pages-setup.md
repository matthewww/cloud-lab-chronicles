# GitHub CI/CD: Markdown to Website
#### 18 Sept 2024

1. [Services covered](#services-covered)
2. [Lab description](#lab-description)
3. [Prerequisites](#prerequisites)
4. [Lab steps](#lab-steps)
5. [Lab files](#lab-files)
6. [Troubleshooting](#troubleshooting)
7. [Acknowledgements](#acknowledgements)

## Services covered
- GitHub Repos
- GitHub Actions
- GitHub Pages

## Lab description
Set up a Continuous Integration / Continuous Deployment pipeline that publishes a website when markdown files are pushed to a repository.

## Prerequisites
- GitHub account
- VS Code
- Python

## Lab steps

### Set Up Your Project
#### Install MkDocs
`pip install mkdocs`

#### Create a New MkDocs Project:
```
mkdocs new my-project
cd my-project
```

#### Configure MkDocs
Edit the mkdocs.yml file to customize your site. For example:
```
site_name: Cloud Lab Chronicles
site_url: https://matthewww.github.io/cloud-lab-chronicles/
docs_dir: ../docs
nav:
  - Home: index.md
  - Labs: labs/00-pages-setup.md

theme:
  name: material
```
#### Add Your Markdown Files
Place your .md files in the docs directory. For example, `docs/index.md` will be your homepage.

#### Serve Locally
You can preview your site locally:
`mkdocs serve`

Open `http://127.0.0.1:8000` in your browser to see your site.

### Push to GitHub
Initialize a Git repository and push your project to GitHub.

### Push to GitHub Pages
One of the best features of MkDocs is its ability to easily deploy to GitHub Pages.

After building your site using mkdocs build, you can deploy it directly to GitHub Pages with:

`mkdocs gh-deploy`

This pushes your generated static site to the gh-pages branch of your GitHub repository.

### Set Up GitHub Actions
Create a .github/workflows/ci.yml file in your repository with the following content:
```
name: Deploy Markdown as Website

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: '3.x'

    - name: Install dependencies
      run: |
        pip install mkdocs mkdocs-material mkdocs-awesome-pages-plugin


    - name: Deploy to GitHub Pages
      run: |
        mkdocs gh-deploy --force
```

Every time you push changes to the main branch, GitHub Actions will automatically build and deploy your site to GitHub Pages. This way, you don’t need to manually run mkdocs gh-deploy each time.

### Add GitHub Pages Extension to VS Code

Rather than going to Actions in GitHub to see workflow runs, you can do this directly in VS Code. Noice!

![GitHub Actions in VS Code](https://github.com/matthewww/cloud-lab-chronicles/blob/main/docs/img/00-pages-setup-1.png)

## Lab files

## Troubleshooting
```
remote: Permission to matthewww/cloud-lab-chronicles.git denied to github-actions[bot].
fatal: unable to access 'https://github.com/matthewww/cloud-lab-chronicles.git/': The requested URL returned error: 403
```
https://stackoverflow.com/questions/72851548/permission-denied-to-github-actionsbot

## Acknowledgements
