# Security as a Service

A collection of containerized security controls for CI/CD pipelines, enabling teams to easily integrate security scanning into their development workflows.

## Available Security Controls

### SAST (Static Application Security Testing)
- **Default**: Semgrep - Multi-language static analysis for security vulnerabilities
- **Configurable**: Use any SAST tool by specifying a different Docker image

### SCA (Software Composition Analysis)
- **Default**: Semgrep OSS - Dependency vulnerability scanning
- **Configurable**: Use any SCA tool by specifying a different Docker image

## Quick Start

### Use Individual Security Controls
```yaml
name: Security Scan
on: [push, pull_request]

jobs:
  sast:
    uses: your-org/SecurityAsAService/.github/workflows/sast.yml@main
    
  sca:
    uses: your-org/SecurityAsAService/.github/workflows/sca.yml@main
```

### Use Custom Images
```yaml
name: Security Scan
on: [push, pull_request]

jobs:
  sast:
    uses: your-org/SecurityAsAService/.github/workflows/sast.yml@main
    with:
      image: 'docker.io/your-org/custom-sast:latest'
      config: '--config=r/security-audit'
      
  sca:
    uses: your-org/SecurityAsAService/.github/workflows/sca.yml@main
    with:
      image: 'docker.io/your-org/custom-sca:latest'
```

## Repository Structure

```
├── docs/           # Documentation for each security control
├── images/         # Dockerfiles for security tools
│   └── semgrep/    # Semgrep SAST/SCA container
└── .github/
    └── workflows/
        ├── sast.yml         # Generic SAST control
        ├── sca.yml          # Generic SCA control
        └── build-images.yml # Build and publish images
```

## Workflow Details

### SAST Control (`sast.yml`)
- **Purpose**: Static Application Security Testing
- **Inputs**:
  - `image`: Docker image for SAST tool (default: semgrep)
  - `config`: Configuration for the tool
- **Outputs**: SARIF, JSON, JUnit XML formats
- **GitHub Integration**: Results appear in Security tab

### SCA Control (`sca.yml`)
- **Purpose**: Software Composition Analysis
- **Inputs**:
  - `image`: Docker image for SCA tool (default: semgrep)
- **Outputs**: SARIF, JSON, JUnit XML formats
- **GitHub Integration**: Results appear in Security tab


## Setup

### GitHub Repository Configuration

#### Required Repository Variables
Configure these in Settings → Secrets and variables → Actions → Variables:

| Variable | Value | Description |
|----------|-------|-------------|
| `DOCKER_REGISTRY` | `docker.io` | Docker registry URL |
| `DOCKER_REPOSITORY` | `your-org/security` | Your Docker repository path |

#### Required Repository Secrets
Configure these in Settings → Secrets and variables → Actions → Secrets:

| Secret | Description |
|--------|-------------|
| `DOCKERHUB_USERNAME` | Your Docker Hub username |
| `DOCKERHUB_TOKEN` | Docker Hub access token |

### Docker Hub Setup

1. Create a Docker Hub account at [hub.docker.com](https://hub.docker.com)
2. Create a repository named `security` under your namespace
3. Generate an access token in Account Settings → Security

### Using Different Registries

Update the variables for other registries:
- GitHub Container Registry: `ghcr.io`
- AWS ECR: `123456789.dkr.ecr.region.amazonaws.com`

## Adding New Security Tools

### 1. Create a New Docker Image
```bash
mkdir images/your-tool
# Create Dockerfile that follows the same interface as semgrep
```

### 2. Update Build Matrix
Add your tool to `.github/workflows/build-images.yml`:
```yaml
matrix:
  include:
    - name: semgrep
      context: ./images/semgrep
    - name: your-tool
      context: ./images/your-tool
```

### 3. Use in Workflows
```yaml
jobs:
  sast:
    uses: ./.github/workflows/sast.yml
    with:
      image: 'docker.io/your-org/security/your-tool:latest'
```

## Tool Interface Requirements

For tools to work with our workflows, they should:

1. **Accept standard output format flags**:
   - `--sarif --output=filename.sarif`
   - `--json --output=filename.json`
   - `--junit-xml --output=filename.xml`

2. **Run as non-root user** (UID 1001)

3. **Work with volume mounts** (`/src` working directory)

4. **Exit gracefully** (non-zero exit codes are handled)

## Contributing

1. Add new security control in `images/` directory
2. Update build matrix in `.github/workflows/build-images.yml`
3. Document usage in this README
4. Test with the workflow templates

## License

MIT