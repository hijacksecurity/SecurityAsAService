# Semgrep Supply Chain (SCA) Security Control

## Overview
Semgrep Supply Chain is a Software Composition Analysis (SCA) tool that detects security vulnerabilities in open source dependencies. It features advanced reachability analysis that reduces false positives by up to 98%.

## Features
- **Vulnerability detection**: Identifies known CVEs in dependencies
- **Reachability analysis**: Shows if vulnerable code is actually used
- **License compliance**: Ensures dependencies meet licensing requirements
- **Malicious package detection**: Identifies compromised packages
- **SBOM generation**: Creates software bill of materials
- **Multi-language support**: 12 languages across 15 package managers

## Supported Languages & Package Managers
- **C#**: NuGet
- **Dart**: Pub
- **Go**: Go modules
- **Java/Kotlin/Scala**: Maven, Gradle
- **JavaScript/TypeScript**: npm, Yarn, pnpm
- **PHP**: Composer
- **Python**: pip, Poetry, Pipenv
- **Ruby**: RubyGems
- **Rust**: Cargo
- **Swift**: SwiftPM

## Best Practices

### 1. Setup
Semgrep Supply Chain requires a Semgrep App Token:
```bash
# Sign up at https://semgrep.dev
# Add SEMGREP_APP_TOKEN to your CI/CD secrets
```

### 2. Basic Scanning
```bash
# Run supply chain analysis
semgrep ci --supply-chain

# Generate SBOM
semgrep ci --supply-chain --output=sbom.json

# Check specific severity levels
semgrep ci --supply-chain --severity=ERROR
```

### 3. CI/CD Integration Options

#### GitHub Actions
Use the template in `templates/sca.yml`

#### GitLab CI
```yaml
semgrep-sca:
  image: docker.io/hijacksecurity/security/semgrep:latest
  script:
    - semgrep ci --supply-chain --gitlab-sast > gl-sast-report.json
  artifacts:
    reports:
      sast: gl-sast-report.json
```

#### Local Scanning
```bash
# Scan current directory
docker run --rm -v $(pwd):/src \
  -e SEMGREP_APP_TOKEN=$SEMGREP_APP_TOKEN \
  docker.io/hijacksecurity/security/semgrep:latest \
  ci --supply-chain
```

### 4. Reachability Analysis
Semgrep's reachability analysis shows:
- **Reachable**: Your code uses the vulnerable function
- **Unreachable**: Vulnerability exists but isn't used
- **Undetermined**: Requires manual review

### 5. Policy Configuration
Configure policies in Semgrep App:
- Block critical vulnerabilities
- Require approval for high severity
- Allow low severity with notification

## Configuration Options

### Environment Variables
- `SEMGREP_APP_TOKEN`: Required for supply chain scanning
- `SEMGREP_TIMEOUT`: Scan timeout (default: 300s)
- `SEMGREP_DISABLE_METRICS`: Disable telemetry

### Command Line Options
```bash
# Output formats
--json                    # JSON output
--gitlab-sast            # GitLab SAST format
--sarif                  # SARIF format

# Filtering
--severity=HIGH,CRITICAL # Filter by severity
--exclude-rule=ID        # Exclude specific rules

# License compliance
--enable-license-scan    # Check licenses
--allowed-licenses=MIT   # Whitelist licenses
```

## Troubleshooting

### Common Issues
1. **No token error**: Set `SEMGREP_APP_TOKEN`
2. **Timeout**: Increase timeout or reduce scope
3. **Missing dependencies**: Ensure lockfiles are committed

### Debug Mode
```bash
semgrep --verbose ci --supply-chain
```

## References
- [Semgrep Supply Chain Docs](https://semgrep.dev/docs/semgrep-supply-chain/)
- [Supported Ecosystems](https://semgrep.dev/docs/supported-languages/#semgrep-supply-chain)
- [Reachability Analysis](https://semgrep.dev/blog/2024/sca-reachability-analysis-methods/)