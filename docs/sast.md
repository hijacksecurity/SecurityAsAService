# SAST Security Control
Static Application Security Testing

## Overview
The SAST security control provides comprehensive static analysis to identify security vulnerabilities, bugs, and anti-patterns in your code. Built on industry-leading tools and delivered through containerized workflows for consistent, reliable scanning across all languages and frameworks.

## Features
- **Universal language support**: Python, JavaScript, TypeScript, Go, Java, C#, Ruby, PHP, Scala, and more
- **Containerized execution**: Pre-built Docker images for consistent environments
- **Multiple report formats**: JUnit, SARIF, JSON, and HTML outputs
- **GitHub integration**: Native Security tab, PR checks, and test reporting
- **Auto-configuration**: Automatically detects languages and applies appropriate rules
- **Enterprise-ready**: Standardized reporting for compliance and dashboards

## Report Formats

### 📊 Standardized Outputs
- **JUnit XML** (`sast-results.xml`) - CI/CD integration, appears as test results
- **SARIF** (`sast-results.sarif`) - GitHub Security tab integration
- **JSON** (`sast-results.json`) - Machine-readable for automation
- **HTML** (`sast-report.html`) - Beautiful visual reports with syntax highlighting

### 🎯 GitHub Integration
- **Security Tab** - Vulnerabilities appear in Repository → Security → Code scanning
- **Pull Request Checks** - Security scans appear as passing/failing checks
- **Test Reports** - Results integrated into GitHub's test reporting
- **Trend Tracking** - Historical analysis and baseline comparisons

## Usage

### Basic Implementation
```yaml
# In your .github/workflows/ci.yml
jobs:
  sast:
    uses: hijacksecurity/SecurityAsAService/.github/workflows/sast.yml@main
```

### Required Permissions
```yaml
permissions:
  security-events: write  # For GitHub Security tab
  contents: read          # For code checkout
  checks: write          # For PR checks
  pull-requests: write   # For PR integration
```

### Configuration Options
The SAST control uses automatic configuration by default, but can be customized:

```yaml
# Set environment variable for custom config
env:
  SEMGREP_CONFIG: "p/security"  # Use specific ruleset
```

## Language Support

### Automatically Detected Languages
- **Web**: JavaScript, TypeScript, HTML, CSS
- **Backend**: Python, Java, Go, C#, PHP, Ruby, Scala
- **Systems**: C, C++, Rust
- **Mobile**: Swift, Kotlin, Objective-C
- **Data**: SQL, YAML, JSON, XML
- **Infrastructure**: Dockerfile, Terraform, CloudFormation

### Security Rule Categories
- **Injection Vulnerabilities** - SQL injection, XSS, command injection
- **Authentication Issues** - Weak passwords, insecure sessions
- **Authorization Flaws** - Access control bypasses
- **Cryptographic Issues** - Weak encryption, insecure hashing
- **Business Logic** - Race conditions, logic flaws
- **Best Practices** - Code quality, security patterns

## Architecture

### Containerized Execution
- **Pre-built images**: `docker.io/hijacksecurity/semgrep:latest`
- **Consistent environment**: Same tool versions across all scans
- **Isolated execution**: Clean, reproducible results
- **Version control**: Centrally managed security tool versions

### Workflow Process
1. **Checkout** - Code retrieved in container
2. **Scan** - Multiple format generation in parallel
3. **Upload** - Results sent to GitHub Security tab
4. **Report** - JUnit results appear as checks
5. **Artifact** - All formats available for download

## Best Practices

### CI/CD Integration
- Run on every push and pull request
- Use branch protection rules to require passing security checks
- Set up notifications for new security findings
- Integrate with code review workflows

### Performance Optimization
- Scans run in parallel with other CI jobs
- Pre-built containers eliminate installation time
- Automatic language detection reduces configuration overhead
- Incremental scanning for large repositories

### Security Considerations
- Containerized execution provides isolation
- No sensitive data exposure in logs
- Standardized reporting for audit trails
- Compliance-ready outputs

## Troubleshooting

### Common Issues
1. **Permission denied**: Ensure proper GitHub permissions are set
2. **No findings**: Verify supported languages are present
3. **Timeout**: Large repositories may need workflow timeout adjustments

### Debug Information
Check the GitHub Actions logs for detailed scan information and any configuration issues.

## References
- [Repository](https://github.com/hijacksecurity/SecurityAsAService)
- [Docker Images](https://hub.docker.com/u/hijacksecurity)
- [GitHub Security Integration](https://docs.github.com/en/code-security/code-scanning)