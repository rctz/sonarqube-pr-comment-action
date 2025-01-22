# SonarQube PR Comment Action

## Overview

This GitHub Action integrates SonarQube with your pull request (PR) workflow. It runs a SonarQube scan, analyzes the results, and posts a detailed comment on the PR, helping maintain code quality throughout the development process.

## Inputs

| Input Name          | Description                                | Required |
| ------------------- | ------------------------------------------ | -------- |
| `sonar_token`       | The SonarQube token for authentication.    | Yes      |
| `sonar_host_url`    | The base URL of the SonarQube server.      | Yes      |
| `sonar_project_key` | The key identifying the SonarQube project. | Yes      |
| `github_token`      | GitHub token for API authentication.       | Yes      |

## Outputs

| Output Name       | Description                              |
| ----------------- | ---------------------------------------- |
| `analysis_report` | The formatted SonarQube analysis report. |

## How It Works

1. **Run SonarQube Scan**  
   Executes a SonarQube analysis on the codebase using the `sonarsource/sonarqube-scan-action`.

2. **Query SonarQube API**  
   Retrieves the analysis results from the SonarQube API, focusing on the project's quality gate status.

3. **Parse SonarQube Analysis**  
   Formats the results into a user-friendly comment that highlights:

   - Overall status (e.g., `Passed` or `Failed`).
   - Details of the conditions that make up the quality gate, including thresholds, actual values, and their statuses.

4. **Post Results to PR**  
   Posts the analysis report as a comment on the relevant pull request, providing developers with actionable feedback.

## Example Usage

```yaml
name: SonarQube Analysis
on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  sonar-analysis:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Run SonarQube PR Comment Action
        uses: ./path-to-action
        with:
          sonar_token: ${{ secrets.SONAR_TOKEN }}
          sonar_host_url: ${{ secrets.SONAR_HOST_URL }}
          sonar_project_key: my-sonar-project
          github_token: ${{ secrets.GITHUB_TOKEN }}
```
