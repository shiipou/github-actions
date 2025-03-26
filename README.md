# GitHub Actions Reusable Workflows Collection

A comprehensive collection of reusable GitHub Actions workflows designed to streamline your CI/CD pipeline. This project provides modular, composable workflows that can be chained together to create powerful and customizable automation for your projects.

## 🎯 Purpose

This collection aims to:
- Reduce duplication in GitHub Actions workflows across projects
- Provide standardized, tested workflow components
- Enable quick setup of complex CI/CD pipelines
- Support multiple platforms and frameworks (Spring Boot, Flutter, Docker)

## 🚀 Quick Start

To use these workflows in your project, reference them in your GitHub Actions workflow file:

```yaml
name: CI Pipeline
on:
  push:
    branches: [main]

jobs:
  version:
    uses: shiipou/github-actions/.github/workflows/get-version.yml@main
    
  build:
    needs: version
    if: needs.version.outputs.version != ''
    uses: shiipou/github-actions/.github/workflows/build-springboot.yml@main
    with:
      version: ${{ needs.version.outputs.version }}
```

## 📦 Available Workflows

### Version Management
| Workflow | Description | Key Features |
|----------|-------------|--------------|
| `get-version.yml` | Extract version from commits | - Uses conventional commits<br>- Generates changelog<br>- Supports prerelease channels |
| `generate-tags.yml` | Generate Docker image tags | - Semantic versioning support<br>- Customizable tag templates |
| `upsert-release.yml` | Manage GitHub releases | - Creates/updates releases<br>- Handles release assets<br>- Supports prereleases |

### Build Pipelines
| Workflow | Description | Key Features |
|----------|-------------|--------------|
| `build-springboot.yml` | Build Spring Boot apps | - Gradle support<br>- Artifact generation<br>- Environment variables |
| `build-docker.yml` | Build Docker images | - Multi-platform builds<br>- Build cache<br>- Artifact inclusion |
| `build-flutter.yml` | Build Flutter apps | - Multi-platform builds<br>- Versioning support<br>- Signing support |

### Quality & Testing
| Workflow | Description | Key Features |
|----------|-------------|--------------|
| `check-springboot.yml` | Spring Boot code checks | - Code style<br>- Static analysis |
| `check-flutter.yml` | Flutter code analysis | - Static analysis<br>- Code formatting |
| `test-springboot.yml` | Spring Boot tests | - Unit tests<br>- Integration tests<br>- Coverage reports |
| `test-flutter.yml` | Flutter tests | - Widget tests<br>- Integration tests<br>- Coverage reports |
| `coverage-report.yml` | Process coverage data | - Coverage thresholds<br>- Report generation |

### Deployment
| Workflow | Description | Key Features |
|----------|-------------|--------------|
| `deploy-flutter.yml` | Deploy Flutter apps | - App Store deployment<br>- Play Store deployment<br>- Automated releases |

## 📋 Requirements

- GitHub Actions enabled repository
- Required permissions:
  ```yaml
  permissions:
    contents: write    # For releases
    packages: write    # For Docker images
  ```
- Secrets configured as needed per workflow

## 🔧 Configuration

Each workflow accepts specific inputs and secrets. See individual workflow files for detailed configuration options.

### Common Inputs
- `working-directory`: For monorepo support
- `version`: Semantic version string
- `artifact-name`: Name of the artifacts to produce
- `artifact-path`: Path to files to upload as artifact
- `with-artifact-name`: Name of the artifacts to use
- `with-artifact-path`: Path to files in artifact

## 📚 Examples

See the [examples/](./examples) directory for complete workflow examples:
- Spring Boot application pipeline
- Flutter mobile app pipeline
- Docker-based service pipeline

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](./LICENSE) file for details.

## 🤝 Contributing

Contributions are welcome! Please read our [Contributing Guide](./CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.
