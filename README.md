# Helm Chart Bump Action

[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/Scalified/helm-chart-bump-action/blob/master/LICENSE)
[![Release](https://img.shields.io/github/v/release/Scalified/helm-chart-bump-action?style=flat-square)](https://github.com/Scalified/helm-chart-bump-action/releases/latest)

# Overview

GitHub Action that bumps a Helm chart to the latest version matching Docker image tag from Docker Hub

The action:
- Fetches image tags from Docker Hub for a namespace/repository
- Filters tags using a configurable regex pattern
- Selects the latest matching tag
- Updates `version` and `appVersion` in `Chart.yaml`
- Commits and pushes the change when an update is needed

# Usage

```yaml
name: Bump Helm chart

on:
  schedule:
    - cron: "0 2 * * *"

jobs:
  bump-chart:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    steps:
      - name: Bump chart
        uses: Scalified/helm-chart-bump-action@v1
        with:
          token: ${{ secrets.TOKEN }}
          dockerhub-namespace: your-org
          dockerhub-repository: your-image
          docker-image-tag-pattern: '^v?[0-9]+\.[0-9]+\.[0-9]+$'
          chart-file-path: charts/your-chart/Chart.yaml
```

## Inputs

| Input                      | Description                                                   | Required | Default                |
|----------------------------|---------------------------------------------------------------|----------|------------------------|
| `token`                    | GitHub token with permissions to push changes                 | No       | `${GITHUB_TOKEN}`      |
| `dockerhub-namespace`      | Docker Hub namespace (organization or user)                   | No       | `library`              |
| `dockerhub-repository`     | Docker Hub repository name                                    | Yes      |                        |
| `docker-image-tag-pattern` | Regex pattern used to match tags                              | No       | SemVer version pattern |
| `chart-file-path`          | Path to Helm chart `Chart.yaml` (relative to repository root) | Yes      |                        |

## Outputs

| Output           | Description                                                     |
|------------------|-----------------------------------------------------------------|
| `latest-app-version` | The latest Docker image tag matching `docker-image-tag-pattern` |

---

**Made with ❤️ by [Scalified](http://www.scalified.com)**
