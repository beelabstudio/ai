# Security Review

## Description

Automatically review code for security vulnerabilities and best practices.

## Activation

This skill is auto-invoked when:
- Files in `auth/`, `security/`, `middleware/` are modified
- Dependencies are updated
- Environment variables or secrets handling is changed
- Authentication/authorization code is touched

## Security Checklist

### Authentication
- [ ] Passwords hashed with bcrypt/Argon2 (not MD5/SHA1)
- [ ] JWT secrets are strong and rotated
- [ ] Session tokens have expiration
- [ ] Rate limiting on login endpoints

### Authorization
- [ ] Check permissions before actions
- [ ] No direct object reference without validation
- [ ] Principle of least privilege

### Input Validation
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (output encoding)
- [ ] CSRF tokens for state-changing operations
- [ ] File upload validation (type, size, content)

### Secrets Management
- [ ] No hardcoded secrets in code
- [ ] .env files in .gitignore
- [ ] Secrets loaded from environment variables
- [ ] No secrets in logs or error messages

### Dependencies
- [ ] Check for known vulnerabilities (`npm audit`)
- [ ] Keep dependencies updated
- [ ] Remove unused dependencies

## Output

Report any findings with:
- Severity: Critical / High / Medium / Low
- Location: file and line number
- Issue: description of the vulnerability
- Recommendation: how to fix it
