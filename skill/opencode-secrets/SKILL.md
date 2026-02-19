---
name: opencode-secrets
description: Detect secrets in code and help prevent credential leakage
---

# OpenCode Secrets Detection Skill

## What I Do
- Scan codebases for hardcoded secrets
- Detect API keys, tokens, and passwords
- Identify sensitive configuration values
- Search for leaked credentials in Git history
- Provide remediation guidance
- Generate security alerts

## When to Use
Use when:
- User asks to scan for secrets
- Security audit is requested
- Code review needs secret detection
- Repository cleanup is needed
- Pre-commit checks are required
- Security vulnerability assessment

## Secret Types Detected

### 1. API Keys
- **AWS**: `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`
- **Google**: `GOOGLE_API_KEY`, `GCP_SERVICE_ACCOUNT`
- **Azure**: `AZURE_CLIENT_SECRET`, `AZURE_SUBSCRIPTION_KEY`
- **OpenAI**: `OPENAI_API_KEY`, `sk-...`
- **GitHub**: `GITHUB_TOKEN`, `ghp_...`, `gho_...`
- **Stripe**: `sk_live_...`, `sk_test_...`
- **Twilio**: `AC...`, `SK...`
- **SendGrid**: `SG....`

### 2. Database Credentials
- **MySQL**: `mysql://user:password@...`
- **PostgreSQL**: `postgresql://user:password@...`
- **MongoDB**: `mongodb://user:password@...`
- **Redis**: `redis://:password@...`
- **SQLite**: Database paths with sensitive names

### 3. JWT Tokens
- `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`
- `Bearer` tokens in headers
- JWT secrets in configuration

### 4. Private Keys
- **RSA**: `-----BEGIN RSA PRIVATE KEY-----`
- **ECDSA**: `-----BEGIN EC PRIVATE KEY-----`
- **OpenSSH**: `-----BEGIN OPENSSH PRIVATE KEY-----`
- **PGP**: `-----BEGIN PGP PRIVATE KEY-----`

### 5. Certificates
- SSL certificates
- TLS keys
- Certificate signing requests

### 6. Passwords and Auth
- Plain text passwords: `"password": "..."`, `"pwd": "..."`
- Basic auth strings: `Authorization: Basic ...`
- Session tokens: `session_id`, `csrf_token`

### 7. Cloud Service Secrets
- **AWS**: `AKIA...` (access key)
- **Google Cloud**: `AIza...` (API key)
- **Firebase**: `AAAA...` (Firebase token)
- **Heroku**: `e...` (API key)
- **Vercel**: `vc_...`

### 8. Third-Party Services
- **Slack**: `xoxb-...`, `xoxp-...`
- **Discord**: `M...` (bot token)
- **Telegram**: `...:AAE...`
- **Twitter**: `...` (bearer token)
- **NPM**: `npm_...`
- **Docker Hub**: `dckr_pat_...`

## Detection Methods

### 1. Pattern Matching
```javascript
// AWS Access Key
/AKIA[0-9A-Z]{16}/

// GitHub Token
/ghp_[a-zA-Z0-9]{36}/

// Stripe Key
/sk_live_[0-9a-zA-Z]{24,}/

// Generic API Key
/api[_-]?key["']?\s*[:=]\s*["']?[a-zA-Z0-9_\-]{20,}/
```

### 2. Entropy Analysis
- High entropy strings (random-looking)
- Base64 encoded secrets
- Hex encoded values

### 3. Contextual Analysis
- Variable names: `apiKey`, `secret`, `password`, `token`
- File types: `.env`, `.pem`, `.key`, `.crt`
- Config sections: `credentials`, `auth`, `api`

### 4. Git History Scan
```bash
# Search for secrets in git history
git log --all --full-history -S "API_KEY"
git log --all --full-history -S "password"

# BFG Repo-Cleaner for bulk removal
bfg --replace-text passwords.txt
```

## Scanning Workflow

### 1. File Selection
```
1. Scan all source files
2. Exclude common non-secret files
3. Check git-ignored files for staging
4. Review configuration files
5. Check environment templates
```

### 2. Pattern Detection
```
1. Apply secret patterns
2. Calculate entropy scores
3. Analyze variable names
4. Check file context
5. Prioritize by confidence
```

### 3. Reporting
```
1. Group by secret type
2. Provide file locations
3. Show severity levels
4. Suggest remediation
5. Generate security report
```

## Remediation Strategies

### 1. Environment Variables
```bash
# ❌ Before - Hardcoded
const apiKey = "sk_live_1234567890";

# ✅ After - Environment variable
const apiKey = process.env.API_KEY;

# .env file (gitignored)
API_KEY=sk_live_1234567890
```

### 2. Secret Management Services
- **AWS Secrets Manager**: Store and retrieve secrets
- **HashiCorp Vault**: Centralized secret management
- **Google Secret Manager**: GCP secret storage
- **Azure Key Vault**: Azure secret storage
- **1Password**: Developer API
- **Doppler**: Secret sync for dev environments

### 3. Git History Cleanup
```bash
# Using git-filter-repo
git filter-repo --invert-paths --path secrets.json

# Using BFG
bfg --delete-files secrets.txt
bfg --strip-blobs-bigger-than 50M

# Force push (BE CAREFUL!)
git push origin --force --all
```

### 4. Rotate Exposed Secrets
1. Identify all exposed credentials
2. Revoke/reissue each secret
3. Update applications with new values
3. Remove old secrets from code
4. Verify no remaining references

## Prevention Measures

### 1. Pre-commit Hooks
```bash
# .git/hooks/pre-commit
#!/bin/bash

# Run secret scanner
if npx git-secrets --scan; then
  echo "No secrets detected. Proceeding with commit."
else
  echo "⚠️  Secrets detected! Commit aborted."
  exit 1
fi
```

### 2. CI/CD Integration
```yaml
# GitHub Actions
- name: Scan for secrets
  run: |
    npx trufflehog --regex --entropy --fail
```

### 3. Automated Tools
- **TruffleHog**: Git history scanner
- **GitLeaks**: Secret detection
- **gitleaks**: Fast secret scanner
- **Detect Secrets**: Python-based scanner
- **git-secrets**: AWS-sponsored tool

### 4. Code Review Process
- Require approval for config changes
- Automated secret scanning in PRs
- Manual review of sensitive files
- Regular security audits

## Configuration Files to Check

### High-Risk Files
- `.env` and `.env.*`
- `config/credentials.json`
- `secrets.json`
- `api_keys.yml`
- `docker-compose.yml` (for environment vars)
- `terraform.tfvars`
- `*.key`, `*.pem`, `*.crt`
- `id_rsa`, `id_dsa`
- `.aws/credentials`
- `.kube/config`

### Medium-Risk Files
- `config/settings.py`
- `application.properties`
- `web.config`
- `appsettings.json`
- `firebase-adminsdk.json`

## False Positive Handling

### Common False Positives
- UUIDs and GUIDs
- Hash values (MD5, SHA)
- Random test data
- Placeholder values (e.g., "YOUR_API_KEY")
- Example code documentation

### Mitigation
```javascript
// ✅ Mark as placeholder
const API_KEY = "YOUR_API_KEY_HERE"; // TODO: Replace with env var

// ✅ Use comment to indicate non-secret
// This is a test key, not a real credential
const TEST_KEY = "test_1234567890";

// ✅ Use documentation keys
const EXAMPLE_KEY = "sk_test_example_placeholder";
```

## Security Report Format

```markdown
# Secret Detection Report

## Summary
- **Files Scanned**: 1,234
- **Secrets Found**: 15
- **Critical**: 3
- **High**: 5
- **Medium**: 7

## Critical Issues 🔴

### 1. AWS Access Key (Critical)
- **File**: `src/config/aws.js`
- **Line**: 42
- **Secret**: `AKIAIOSFODNN7EXAMPLE`
- **Type**: AWS Access Key ID
- **Action Required**: Rotate immediately, use AWS Secrets Manager

### 2. Stripe Live Key (Critical)
- **File**: `src/payment/stripe.js`
- **Line**: 18
- **Secret**: `sk_live_51M...`
- **Type**: Stripe API Key
- **Action Required**: Revoke in Stripe dashboard, move to env var

## High Severity 🟠

### 1. GitHub Token (High)
- **File**: `.github/workflows/deploy.yml`
- **Line**: 23
- **Secret**: `ghp_1234567890abcdef`
- **Type**: GitHub Personal Access Token
- **Action Required**: Use GitHub Secrets

### 2. Database Password (High)
- **File**: `config/database.js`
- **Line**: 12
- **Secret**: `postgresql://user:SuperSecret123@db.example.com`
- **Type**: Database Connection String
- **Action Required**: Move to environment variable

## Medium Severity 🟡

[... additional issues ...]

## Remediation Plan

1. **Immediate** (Today):
   - [ ] Rotate all critical secrets
   - [ ] Remove from codebase
   - [ ] Update environment variables

2. **Short-term** (This week):
   - [ ] Implement secret scanning in CI/CD
   - [ ] Set up pre-commit hooks
   - [ ] Configure secret management service

3. **Long-term** (This month):
   - [ ] Regular security audits
   - [ ] Team training on secret management
   - [ ] Implement secret rotation policy
```

## Best Practices

1. **Never commit secrets** - Use environment variables or secret managers
2. **Scan regularly** - Automated scanning in CI/CD
3. **Rotate frequently** - Implement secret rotation policies
4. **Limit access** - Principle of least privilege
5. **Audit access** - Track who accesses secrets
6. **Document procedures** - Clear incident response plan
7. **Educate team** - Security awareness training

## Safety Pattern

1. **Read-only scanning** - Don't modify code
2. **Report first** - Alert before taking action
3. **False positive checks** - Verify before flagging
4. **Secure handling** - Don't log actual secrets
5. **Provide guidance** - Clear remediation steps
6. **Privacy respect** - Don't share secrets in reports

## Example Prompts

```
"Scan for secrets in this codebase"
"Check for API keys in src/"
"Search git history for leaked credentials"
"Generate security report"
"Find hardcoded passwords"
"Scan for private keys"
```

## Integration with Tools

### Git Hooks
```bash
# Install git-secrets
git secrets --install
git secrets --register-aws
git secrets --add 'password\s*=\s*.+'
```

### CI/CD
```yaml
# GitHub Actions
- name: TruffleHog Scan
  uses: trufflesecurity/trufflehog@main
  with:
    path: ./
    base: ${{ github.event.repository.default_branch }}
    head: HEAD
```

### IDE Extensions
- VS Code: Secret Scanner extension
- JetBrains: Built-in password detection
- Vim: git-secrets integration
