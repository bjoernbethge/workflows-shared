# Shared GitHub Actions Workflows

Reusable GitHub Actions workflows for Docker builds, Python CI, and security scanning.

## Available Workflows

### 🐳 docker-build-push.yml

Builds Docker images and pushes to GitHub Container Registry (GHCR) and optionally Docker Hub.

**Features:**
- Multi-platform builds (amd64, arm64)
- Smart tagging (latest, semver, SHA, branch)
- Layer caching with GitHub Actions cache
- Pushes to both GHCR and Docker Hub

**Usage:**

```yaml
# .github/workflows/docker.yml
name: Docker Build

on:
  push:
    branches: [main]
    tags: ['v*']
  pull_request:

jobs:
  docker:
    uses: bjoernbethge/workflows-shared/.github/workflows/docker-build-push.yml@main
    with:
      dockerfile: Dockerfile
      platforms: linux/amd64,linux/arm64
      push-dockerhub: true
      dockerhub-username: your-username
    secrets:
      DOCKERHUB_TOKEN: ${{ secrets.DOCKERHUB_TOKEN }}
```

**Inputs:**
- `dockerfile`: Path to Dockerfile (default: `Dockerfile`)
- `context`: Build context directory (default: `.`)
- `platforms`: Target platforms (default: `linux/amd64`)
- `push-dockerhub`: Push to Docker Hub (default: `false`)
- `dockerhub-username`: Docker Hub username
- `image-name`: Override image name (default: repo name)

**Secrets:**
- `DOCKERHUB_TOKEN`: Required if `push-dockerhub: true`

---

### 🐍 python-ci.yml

Python CI with linting (Ruff), type checking (mypy), and testing (pytest).

**Features:**
- Ruff linting and formatting checks
- mypy type checking (optional)
- pytest with coverage reporting
- Matrix testing across Python versions
- uv for fast dependency management

**Usage:**

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:

jobs:
  python-ci:
    uses: bjoernbethge/workflows-shared/.github/workflows/python-ci.yml@main
    with:
      python-versions: '["3.11", "3.12"]'
      src-path: 'src/'
      test-path: 'tests/'
      run-mypy: true
      coverage-report: true
```

**Inputs:**
- `python-versions`: JSON array of Python versions (default: `["3.11", "3.12"]`)
- `src-path`: Source code path (default: `src/`)
- `test-path`: Test path (default: `tests/`)
- `test-command`: Test command (default: `uv run pytest`)
- `run-mypy`: Run type checking (default: `true`)
- `coverage-report`: Generate coverage (default: `true`)

---

## Setup

### Docker Hub Token

To push to Docker Hub:

1. Create access token: https://hub.docker.com/settings/security
2. Add as repository secret:
   ```bash
   gh secret set DOCKERHUB_TOKEN --repo owner/repo
   ```

### GHCR (GitHub Container Registry)

No setup needed! Uses `GITHUB_TOKEN` automatically.

---

## Examples

### Minimal Docker Build (GHCR only)

```yaml
jobs:
  docker:
    uses: bjoernbethge/workflows-shared/.github/workflows/docker-build-push.yml@main
```

### Multi-Platform Docker Build

```yaml
jobs:
  docker:
    uses: bjoernbethge/workflows-shared/.github/workflows/docker-build-push.yml@main
    with:
      platforms: linux/amd64,linux/arm64
```

### Python CI with Custom Paths

```yaml
jobs:
  ci:
    uses: bjoernbethge/workflows-shared/.github/workflows/python-ci.yml@main
    with:
      src-path: 'my_package/'
      test-path: 'test/'
      test-command: 'uv run pytest -v'
```

---

## License

MIT
