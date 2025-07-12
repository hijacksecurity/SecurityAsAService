# Security as a Service

A collection of containerized security controls for CI/CD pipelines, enabling teams to easily integrate security scanning into their development workflows.

## Available Security Controls

### SAST (Static Application Security Testing)
- **Semgrep** - Multi-language static analysis for security vulnerabilities

### SCA (Software Composition Analysis)
- **Semgrep Supply Chain** - Dependency vulnerability scanning with reachability analysis

### Coming Soon
- **OWASP ZAP** - Dynamic application security testing (DAST)
- **Gitleaks** - Secret scanning
- **Container Scanning** - Image vulnerability scanning

## Quick Start

### Using in GitHub Actions

```yaml
name: Security Scan
on: [push, pull_request]

jobs:
  sast:
    uses: ./.github/workflows/security-controls.yml
    with:
      control: semgrep
```

### Using Docker Images

```bash
# Run Semgrep locally
docker run --rm -v $(pwd):/src \
  docker.io/hijacksecurity/security/semgrep:latest \
  scan --config=auto
```

## Repository Structure

```
├── docs/           # Documentation for each security control
├── images/         # Dockerfiles for security tools
├── templates/      # Reusable CI/CD templates
└── .github/        # GitHub Actions workflows
```

## Setup

### GitHub Repository Configuration

#### Required Repository Variables
Configure these in Settings → Secrets and variables → Actions → Variables:

| Variable | Value | Description |
|----------|-------|-------------|
| `DOCKER_REGISTRY` | `docker.io` | Docker registry URL |
| `DOCKER_REPOSITORY` | `hijacksecurity/security` | Your Docker Hub namespace and repository path |

#### Required Repository Secrets
Configure these in Settings → Secrets and variables → Actions → Secrets:

| Secret | Description | How to Get |
|--------|-------------|------------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username | Your Docker Hub account username |
| `DOCKERHUB_TOKEN` | Docker Hub access token | 1. Log in to Docker Hub<br>2. Go to Account Settings → Security<br>3. Click "New Access Token"<br>4. Give it a descriptive name<br>5. Copy the generated token |
| `SEMGREP_APP_TOKEN` | Semgrep platform token (for SCA) | 1. Sign up at [semgrep.dev](https://semgrep.dev)<br>2. Go to Settings → Tokens<br>3. Create a new token<br>4. Copy the token value |

### Docker Hub Setup

1. Create a Docker Hub account at [hub.docker.com](https://hub.docker.com)
2. Create a namespace (e.g., `hijacksecurity`)
3. Create a repository named `security` under your namespace
4. Generate an access token as described above

### Using Different Registries

If you want to use a different registry (e.g., GitHub Container Registry, AWS ECR):

1. Update the `DOCKER_REGISTRY` variable (e.g., `ghcr.io`, `123456789.dkr.ecr.us-east-1.amazonaws.com`)
2. Update the `DOCKER_REPOSITORY` variable accordingly
3. Update the login action in `.github/workflows/build-images.yml` to match your registry

#### Example for GitHub Container Registry:
```yaml
- name: Log in to GitHub Container Registry
  uses: docker/login-action@v3
  with:
    registry: ghcr.io
    username: ${{ github.actor }}
    password: ${{ secrets.GITHUB_TOKEN }}
```

### First Run

1. Push your changes to trigger the workflow
2. Check the Actions tab to monitor the build
3. Once successful, your images will be available at:
   - `docker.io/hijacksecurity/security/semgrep:latest`

### Consuming the Security Controls

Other projects can now use your security controls by:

1. Copying the relevant template from `templates/`
2. Adding the required secrets (e.g., `SEMGREP_APP_TOKEN` for SCA)
3. Configuring the same repository variables or hardcoding the image path

## Contributing

1. Add new security control in `images/` directory
2. Create corresponding template in `templates/`
3. Document usage in `docs/`
4. Update build matrix in `.github/workflows/build-images.yml`

## License

MIT
