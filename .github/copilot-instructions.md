# Copilot Instructions for workflows-shared

This repository contains reusable GitHub Actions workflows for Docker builds and Python CI. It does not contain application code that needs to be built, tested, or linted. Instead, it provides workflows that other repositories can call.

## Repository Purpose

This repository provides shared, reusable GitHub Actions workflows that can be called from other repositories. The main goal is to standardize and simplify CI/CD workflows across multiple projects.

## Repository Structure

- `.github/workflows/`: Contains the reusable workflow files
  - `docker-build-push.yml`: Workflow for building and pushing Docker images to GHCR and optionally Docker Hub
  - `python-ci.yml`: Workflow for Python CI with linting (Ruff), type checking (mypy), and testing (pytest)
- `README.md`: Documentation for using the workflows

## Available Workflows

### docker-build-push.yml
Builds Docker images and pushes to GitHub Container Registry (GHCR) and optionally Docker Hub. Features:
- Multi-platform builds (amd64, arm64)
- Smart tagging (latest, semver, SHA, branch)
- Layer caching with GitHub Actions cache
- Push to both GHCR and Docker Hub

### python-ci.yml
Python CI workflow with:
- Ruff linting and formatting checks
- mypy type checking (optional)
- pytest with coverage reporting
- Matrix testing across Python versions
- uv for fast dependency management

## Coding Standards

### GitHub Actions Workflows

1. **Follow GitHub Actions best practices**:
   - Use specific action versions (e.g., `@v4`, not `@main` or `@latest`)
   - Include timeout settings for jobs
   - Set appropriate permissions at the job level
   - Use caching where appropriate

2. **Workflow structure**:
   - Use `workflow_call` trigger for reusable workflows
   - Define clear inputs with descriptions, types, and defaults
   - Document required vs optional inputs
   - Use secrets appropriately and document when they're needed

3. **Input validation**:
   - Provide sensible defaults for optional inputs
   - Use appropriate types (string, boolean, etc.)
   - Include clear descriptions for all inputs

4. **Maintainability**:
   - Use clear, descriptive job and step names
   - Comment complex logic or non-obvious configurations
   - Keep workflows focused on a single purpose
   - Avoid duplication across workflows

5. **Documentation**:
   - Update README.md when adding or modifying workflows
   - Include usage examples for each workflow
   - Document all inputs, secrets, and outputs
   - Provide clear feature descriptions

## Testing and Validation

Since this repository contains reusable workflows rather than executable code:

1. **YAML linting**: Ensure YAML syntax is correct - use `yamllint` or GitHub's workflow validation before pushing
2. **Workflow syntax**: GitHub Actions will validate workflow syntax on push
3. **Test in downstream repositories**: The best way to test workflow changes is to use them in a real repository that calls the workflow

## Common Tasks

### Adding a new workflow
1. Create a new `.yml` file in `.github/workflows/`
2. Start with `on: workflow_call:` trigger
3. Define inputs and secrets clearly
4. Add the workflow documentation to README.md with usage examples
5. Test by calling it from another repository

### Modifying an existing workflow
1. Review the current workflow behavior and inputs
2. Make minimal changes to achieve the goal
3. Update README.md if inputs or behavior changes
4. Consider backward compatibility - avoid breaking changes when possible
5. Test in a downstream repository before merging

### Updating documentation
1. Keep README.md in sync with workflow changes
2. Include practical usage examples
3. Document all inputs, secrets, and features
4. Maintain consistency in formatting and style

## Key Considerations

- **Breaking changes**: Be very careful with changes that affect workflow inputs or behavior, as they can break other repositories
- **Versioning**: Users can pin to specific versions using `@v1`, `@v2`, or `@main` - consider semantic versioning for tags
- **Security**: Review any changes that affect secrets handling, permissions, or external actions
- **Performance**: Consider caching and optimization for frequently run workflows
