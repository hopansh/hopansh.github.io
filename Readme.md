# hopansh.github.io

This repository serves as a centralized hub for showcasing my portfolio and various full-stack projects. Each project is hosted on the same domain (`https://hopansh.github.io`) with its own unique path. The main page (`/`) is dedicated to my portfolio.

---

## 🛠️ Architecture Design

### Repository Structure
- **Main Repository:** [`hopansh.github.io`](https://github.com/hopansh/hopansh.github.io) (this repository)
  - Hosts the main portfolio (`/`) and all sub-projects under their respective paths.
  - Example:
    - Portfolio: `https://hopansh.github.io/`
    - Project 1 (chat2chart): `https://hopansh.github.io/chat2chart`
    - Project 2 (acadmik): `https://hopansh.github.io/acadmik`
    - Project 3 (gigglematrix): `https://hopansh.github.io/gigglematrix`
    - Project 4 (edaddress): `https://hopansh.github.io/edaddress`

- **Individual Project Repositories:**
  - Each project is stored in its own repository and uses CI/CD workflows to deploy to the appropriate path in the main repository.

### Workflow Automation
Every project has a GitHub Actions workflow that automates the deployment process. The key steps include:
1. Building the project using its respective framework (e.g., React).
2. Deploying the build output to a subdirectory in the `hopansh.github.io` repository, corresponding to the project's name.

#### Example Workflow
Below is an example GitHub Actions workflow used in individual projects:

```yaml
name: Deploy Project to GitHub Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      # Set the project name
      - name: Set project name
        run: echo "PROJECT_NAME=chat2chart" >> $GITHUB_ENV

      # Checkout the project repository
      - name: Checkout source repository
        uses: actions/checkout@v2

      # Build the project
      - name: Build project
        run: |
          npm install
          npm run build

      # Checkout the target repository
      - name: Checkout target repository
        uses: actions/checkout@v2
        with:
          repository: hopansh/hopansh.github.io
          token: ${{ secrets.PERSONAL_TOKEN }}
          path: target-repo

      # Deploy the build to the appropriate path
      - name: Deploy build
        run: |
          mkdir -p target-repo/${{ env.PROJECT_NAME }}
          rm -rf target-repo/${{ env.PROJECT_NAME }}/*
          cp -r build/* target-repo/${{ env.PROJECT_NAME }}/
          cd target-repo
          git add .
          if ! git diff-index --quiet HEAD; then
            git commit -m "Deploy build for ${{ env.PROJECT_NAME }}"
            git push origin main
          fi
```

#### Benefits of This Setup
- *Centralized Hosting:* All projects are hosted under one domain, making it easy to showcase multiple projects.
- *Microservices Architecture:* Demonstrates skills in managing and deploying multiple independent projects in a coordinated manner.
- *Automation:* CI/CD ensures that each project is updated seamlessly on deployment.