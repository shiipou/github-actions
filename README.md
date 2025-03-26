# GitHub Actions Reusable Workflows

A collection of reusable GitHub Actions workflows designed to streamline your CI/CD pipeline. Chain these workflows together to create powerful and customizable automation for your projects.

## Features

- 📦 Versioning with conventional commits
- 🏗️ Build workflows for various platforms
- 🧪 Testing and code quality checks
- 🐳 Docker image building
- 🏷️ Automated tag generation
- 🚀 Release management

## Available Workflows

### Version Management
- `get-version.yml`: Extracts version from conventional commits
- `generate-tags.yml`: Generates Docker tags from version
- `upsert-release.yml`: Creates/updates GitHub releases
- `increment-variable.yml`: Increments a repository variable
- `release.yml`: Handles release creation with artifacts
- `release-flutter.yml`: Specialized release workflow for Flutter apps

### Build Pipelines
- `build-springboot.yml`: Builds Spring Boot applications
- `build-docker.yml`: Builds and pushes Docker images
- `build-flutter.yml`: Builds Flutter applications for multiple platforms

### Quality Checks
- `check-springboot.yml`: Runs code quality checks for Spring Boot
- `check-flutter.yml`: Analyzes Flutter code quality
- `test-springboot.yml`: Executes tests for Spring Boot apps
- `test-flutter.yml`: Runs Flutter tests
- `coverage-report.yml`: Processes and validates coverage reports

### Deployment
- `deploy-flutter.yml`: Deploys Flutter apps to PlayStore/AppStore

### Status Management
- `run-check-status.yml`: Updates PR check status with custom messages

## Usage Example

```yaml
name: Spring Boot CI/CD

on:
  push:
    branches: 
      - main
      - beta
    paths:
      - 'server/**'
      - '!server/**/*.md'
  pull_request:
    types: [opened, edited, reopened, synchronize, ready_for_review]
    paths:
      - 'mobile/**'
      - '!mobile/**/*.md'
  workflow_dispatch:

permissions: 
  contents: write
  packages: write

jobs:
  get-version:
    name: Get Version
    # Call the reusable workflow for getting the version using convco cli.
    uses: shiipou/github-actions/.github/workflows/get-version.yml@main
    with:
      changelog-include-hidden-sections: true
      
  build:
    name: Build Spring Boot App
    needs: get-version
    uses: shiipou/github-actions/.github/workflows/build-springboot.yml@main
    with:
      artifact-name: server
      envs: |
        VERSION=${{ needs.get-version.outputs.version || '0.0.0' }}
    secrets: 
      envs: | 
        SENTRY_AUTH_TOKEN=${{ secrets.SENTRY_AUTH_TOKEN }}
  check:
    name: Check Spring Boot Code
    uses: shiipou/github-actions/.github/workflows/check-springboot.yml@main

  test:      
    name: Test Spring Boot App
    uses: shiipou/github-actions/.github/workflows/test-springboot.yml@main
    with:
      artifact-name: coverage-report

  generate-tags:
    name: Generate tags from version
    needs: get-version
    if: needs.get-version.outputs.version != ''
    uses: shiipou/github-actions/.github/workflows/generate-tags.yml@main
    with:
      version: ${{ needs.get-version.outputs.version }}
      template: ghcr.io/${{ github.repository }}/server:[:version:]

  build-docker:
    name: Build docker image
    needs: [generate-tags, build, test, check]
    uses: shiipou/github-actions/.github/workflows/build-docker.yml@main
    with:
      tags: ${{ needs.generate-tags.outputs.tags }}
      with-artifact-name: server
      with-artifact-path: build/libs/
```

## Requirements

- GitHub Actions enabled repository
- Appropriate permissions set in workflow
- Required secrets configured in repository settings

## License

This project is licensed under the MIT License - see the LICENSE file for details.
