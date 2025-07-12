# SCA Security Control
Software Composition Analysis & Secrets Detection

## Overview
The SCA security control identifies hardcoded secrets, API keys, passwords, and other sensitive data that may be accidentally committed to your repository. Built for comprehensive secrets detection across all file types and languages with enterprise-grade reporting.

## Features
- **Universal secrets detection**: API keys, passwords, tokens, certificates across all languages
- **Containerized execution**: Pre-built Docker images for consistent environments
- **Multiple report formats**: JUnit, SARIF, JSON, and HTML outputs
- **GitHub integration**: Native Security tab, PR checks, and test reporting
- **Pattern-based scanning**: Advanced regex patterns for comprehensive coverage
- **Enterprise-ready**: Standardized reporting for compliance and audit trails

## Report Formats

### 📊 Standardized Outputs
- **JUnit XML** (`sca-results.xml`) - CI/CD integration, appears as test results
- **SARIF** (`sca-results.sarif`) - GitHub Security tab integration
- **JSON** (`sca-results.json`) - Machine-readable for automation
- **HTML** (`sca-report.html`) - Beautiful visual reports with context

### 🎯 GitHub Integration
- **Security Tab** - Secrets appear in Repository → Security → Code scanning
- **Pull Request Checks** - Secret scans appear as passing/failing checks
- **Test Reports** - Results integrated into GitHub's test reporting
- **Alert Management** - Track and manage secret exposures over time

## Usage

### Basic Implementation
```yaml
# In your .github/workflows/ci.yml
jobs:
  sca:
    uses: hijacksecurity/SecurityAsAService/.github/workflows/sca.yml@main
    secrets: inherit
```

### Required Permissions
```yaml
permissions:
  security-events: write  # For GitHub Security tab
  contents: read          # For code checkout
  checks: write          # For PR checks
  pull-requests: write   # For PR integration
```

## Detection Capabilities

### Secret Types Detected
- **API Keys**: AWS, Azure, GCP, GitHub, Slack, Stripe, etc.
- **Authentication**: Passwords, JWT tokens, OAuth tokens
- **Database**: Connection strings, credentials, DSNs
- **Cryptographic**: Private keys, certificates, symmetric keys
- **Service Tokens**: CI/CD tokens, service account keys
- **Application Secrets**: Session keys, encryption keys, salts

### File Types Scanned
- **Source Code**: All programming languages
- **Configuration**: JSON, YAML, XML, INI, ENV files
- **Documentation**: Markdown, text files, comments
- **Infrastructure**: Dockerfiles, Terraform, CloudFormation
- **Data Files**: CSV, SQL dumps, backup files

## Architecture

### Containerized Execution
- **Pre-built images**: `docker.io/hijacksecurity/semgrep:latest`
- **Consistent environment**: Same detection patterns across all scans
- **Isolated execution**: Secure, reproducible secret detection
- **Version control**: Centrally managed detection rules

### Workflow Process
1. **Checkout** - Code retrieved in secure container
2. **Scan** - Multiple format generation for comprehensive coverage
3. **Upload** - Results sent to GitHub Security tab
4. **Report** - JUnit results appear as security checks
5. **Artifact** - All formats available for download and analysis

## Security Categories

### High-Risk Secrets
- **Cloud Provider Keys** - AWS access keys, Azure service principals
- **Database Credentials** - Production database passwords
- **Private Keys** - RSA, SSH, TLS private keys
- **Service Account Tokens** - GitHub tokens, CI/CD credentials

### Medium-Risk Patterns
- **API Endpoints** - Internal service URLs with embedded credentials
- **Configuration Secrets** - Application-specific tokens
- **Development Keys** - Test environment credentials

### Detection Patterns
- **Entropy Analysis** - High-entropy strings indicating potential secrets
- **Known Patterns** - Industry-standard secret formats
- **Context Awareness** - Considers variable names and surrounding code
- **False Positive Reduction** - Smart filtering to reduce noise

## Best Practices

### Prevention
- Use environment variables for all secrets
- Implement proper secret management (AWS Secrets Manager, Azure Key Vault)
- Never commit secrets to version control
- Use `.gitignore` for sensitive files
- Implement pre-commit hooks for secret detection

### Response
- Immediately rotate any detected secrets
- Review access logs for potential exposure
- Update affected systems and dependencies
- Document incidents for compliance reporting

### CI/CD Integration
- Run on every push and pull request
- Block merges when secrets are detected
- Set up immediate notifications for secret detection
- Integrate with secret management workflows

## Compliance & Reporting

### Standards Supported
- **SOC 2** - Security monitoring and incident response
- **ISO 27001** - Information security management
- **PCI DSS** - Payment card industry compliance
- **GDPR** - Data protection and privacy requirements

### Audit Trail
- Complete scan history with timestamps
- Detailed finding reports with context
- Remediation tracking and verification
- Executive dashboards and metrics

## Troubleshooting

### Common Issues
1. **High false positives**: Review and whitelist known safe patterns
2. **Performance**: Large repositories may benefit from incremental scanning
3. **Missing secrets**: Ensure comprehensive file type coverage

### Configuration
While the SCA control works out-of-the-box, it can be customized for specific needs:

```yaml
# Custom patterns can be added through configuration
env:
  SEMGREP_CONFIG: "r/secrets"  # Use specific secret detection rules
```

### Debug Information
GitHub Actions logs provide detailed information about scan progress and any configuration issues.

## Integration Examples

### Branch Protection
```yaml
# Require passing secret scans
required_status_checks:
  contexts: ["SCA Security Tests"]
```

### Notifications
```yaml
# Alert security team on secret detection
on:
  workflow_run:
    types: [completed]
    workflows: ["SCA Security Tests"]
```

## References
- [Repository](https://github.com/hijacksecurity/SecurityAsAService)
- [Docker Images](https://hub.docker.com/u/hijacksecurity)
- [Secret Management Best Practices](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [GitHub Security Features](https://docs.github.com/en/code-security)