---
name: security-analyzer
description: Automated security analysis and vulnerability detection skill for codebases and applications.
---

# Security Analyzer Skill

## Purpose
Automated security analysis and vulnerability detection skill for codebases and applications.

## Capabilities
1. **Vulnerability Scanning**
   - OWASP Top 10 detection
   - Common vulnerabilities and exposures (CVEs)
   - Dependency vulnerability checking
   - Security header analysis

2. **Code Security Patterns**
   - SQL injection detection
   - XSS vulnerabilities identification
   - CSRF protection verification
   - Authentication/authorization weaknesses
   - Input validation analysis

3. **Secret Detection**
   - Hardcoded API keys
   - Password tokens in code
   - Exposed credentials
   - Sensitive data in logs

4. **Compliance Checks**
   - GDPR compliance verification
   - SOC2 requirements validation
   - PCI DSS standard checks
   - HIPAA compliance (healthcare)

5. **Security Best Practices**
   - Encryption verification
   - Secure storage recommendations
   - API rate limiting analysis
   - Session management review

## Usage

### Security Audit
```
Perform a comprehensive security audit of the authentication system
```

### Vulnerability Scan
```
Scan for OWASP Top 10 vulnerabilities in the web application
```

### Dependency Check
```
Check all npm packages for known security vulnerabilities
```

### Secret Detection
```
Scan codebase for hardcoded secrets or credentials
```

## Security Rules
- Always report security findings with severity levels
- Provide actionable remediation steps
- Prioritize critical and high-severity issues
- Suggest security best practices
- Recommend security testing strategies

## Output
- **Security Report**: Comprehensive vulnerability analysis
- **Risk Assessment**: Severity-based prioritization
- **Remediation Steps**: Actionable recommendations
- **Compliance Status**: Regulatory compliance checklist
- **Secret Scan Results**: Hardcoded credential findings

## Integration
Works with:
- Static analysis tools (SAST)
- Dependency vulnerability databases
- Security scanning platforms
- Git history analysis for exposed secrets
