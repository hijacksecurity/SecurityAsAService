# Semgrep SAST Security Control

## Overview
Semgrep is a fast, open-source static analysis tool that finds bugs, security vulnerabilities, and anti-patterns in code. It supports 30+ languages and can be customized with organization-specific rules.

## Features
- **Multi-language support**: Python, JavaScript, Go, Java, C, Ruby, PHP, and more
- **Fast scanning**: Designed for CI/CD integration
- **Custom rules**: Write rules in YAML to enforce coding standards
- **SARIF output**: Integrates with GitHub Security tab and other tools
- **Low false positives**: Pattern-based matching reduces noise

## Best Practices

### 1. Rule Selection
```bash
# Use auto configuration for comprehensive coverage
semgrep --config=auto

# Use specific rulesets for focused scanning
semgrep --config=p/security
semgrep --config=p/owasp-top-ten
semgrep --config=p/cwe-top-25
```

### 2. Custom Rules
Create `.semgrep.yml` in your repository:
```yaml
rules:
  - id: hardcoded-secret
    pattern: |
      $KEY = "..."
    pattern-either:
      - metavariable-regex:
          metavariable: $KEY
          regex: '.*(password|secret|token|apikey|api_key).*'
    message: Hardcoded secret detected
    severity: ERROR
```

### 3. CI/CD Integration Options

#### GitHub Actions
Use the template in `templates/semgrep-sast.yml`

#### GitLab CI
```yaml
semgrep-sast:
  image: docker.io/hijacksecurity/security/semgrep:latest
  script:
    - semgrep --config=auto --json --output=semgrep-results.json
  artifacts:
    reports:
      sast: semgrep-results.json
```

#### Jenkins
```groovy
pipeline {
    agent {
        docker {
            image 'docker.io/hijacksecurity/security/semgrep:latest'
        }
    }
    stages {
        stage('SAST Scan') {
            steps {
                sh 'semgrep --config=auto --junit-xml --output=semgrep-results.xml'
                junit 'semgrep-results.xml'
            }
        }
    }
}
```

### 4. Performance Optimization
- **Exclude paths**: Use `.semgrepignore` file
- **Parallel scanning**: Use `--jobs` flag
- **Incremental scanning**: Use `--baseline-commit` for PR scans

### 5. Security Considerations
- Run in isolated container
- Use non-root user
- Limit network access
- Scan results may contain sensitive code snippets

## Configuration Options

### Environment Variables
- `SEMGREP_RULES`: Path to custom rules
- `SEMGREP_TIMEOUT`: Timeout per rule (default: 30s)
- `SEMGREP_MAX_MEMORY`: Memory limit per rule

### Command Line Options
```bash
# Output formats
--json              # JSON output
--sarif             # SARIF format for GitHub
--junit-xml         # JUnit XML for CI tools
--gitlab-sast       # GitLab SAST format

# Filtering
--severity ERROR    # Only high severity findings
--exclude 'test_*'  # Exclude test files
--include '*.py'    # Only scan Python files

# Performance
--jobs 4            # Parallel execution
--max-memory 5000   # Memory limit in MB
--timeout 300       # Timeout in seconds
```

## Troubleshooting

### Common Issues
1. **Out of memory**: Reduce `--jobs` or increase `--max-memory`
2. **Timeouts**: Increase `--timeout` or optimize rules
3. **False positives**: Use `# nosemgrep` comment or update rules

### Debug Mode
```bash
semgrep --verbose --debug
```

## References
- [Semgrep Documentation](https://semgrep.dev/docs/)
- [Rule Registry](https://semgrep.dev/r)
- [Writing Rules](https://semgrep.dev/docs/writing-rules/overview/)