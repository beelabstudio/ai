---
name: security-review
description: >
  Review code for security vulnerabilities before it ships. Use when adding authentication,
  handling user input, working with secrets, creating API endpoints, or implementing
  payment/sensitive features. Provides a comprehensive checklist and concrete patterns for
  secrets, input validation, SQL injection, XSS, CSRF, authorization, rate limiting, and
  dependency security.
source: https://github.com/affaan-m/ECC
---

# Security Review

Ensure all code follows security best practices and identify potential vulnerabilities before they ship.

## When to Activate

- Implementing authentication or authorization
- Handling user input or file uploads
- Creating new API endpoints
- Working with secrets or credentials
- Implementing payment features
- Storing or transmitting sensitive data
- Integrating third-party APIs

## Security Checklist

### 1. Secrets Management

#### FAIL — never hardcode secrets
```typescript
const apiKey = "sk-proj-xxxxx"   // hardcoded secret
const dbPassword = "password123"  // in source code
```

#### PASS — environment variables
```typescript
const apiKey = process.env.OPENAI_API_KEY
const dbUrl = process.env.DATABASE_URL

if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}
```

- [ ] No hardcoded API keys, tokens, or passwords
- [ ] All secrets in environment variables
- [ ] `.env*` files in `.gitignore`
- [ ] No secrets in git history
- [ ] Production secrets in the hosting platform or secret manager

### 2. Input Validation

```typescript
import { z } from 'zod'

const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  age: z.number().int().min(0).max(150),
})

export async function createUser(input: unknown) {
  try {
    const validated = CreateUserSchema.parse(input)
    return await db.users.create(validated)
  } catch (error) {
    if (error instanceof z.ZodError) {
      return { success: false, errors: error.issues }
    }
    throw error
  }
}
```

- [ ] All user inputs validated with schemas
- [ ] File uploads restricted (size, type, extension)
- [ ] No direct use of user input in queries
- [ ] Whitelist validation (not blacklist)
- [ ] Error messages don't leak sensitive info

### 3. SQL Injection Prevention

```typescript
// FAIL — string concatenation
const query = `SELECT * FROM users WHERE email = '${userEmail}'`

// PASS — parameterized query (value in params, never in the string)
await db.query('SELECT * FROM users WHERE email = ?', [userEmail])
```

- [ ] All queries parameterized (use your driver's placeholder syntax)
- [ ] No string concatenation in SQL
- [ ] ORM/query builder used correctly

### 4. Authentication & Authorization

```typescript
// FAIL — localStorage is XSS-visible
localStorage.setItem('token', token)

// PASS — httpOnly cookies
res.setHeader('Set-Cookie',
  `token=${token}; HttpOnly; Secure; SameSite=Strict; Max-Age=3600`)
```

```typescript
export async function deleteUser(userId: string, requesterId: string) {
  const requester = await db.users.findUnique({ where: { id: requesterId } })
  if (requester.role !== 'admin') {
    return { error: 'Unauthorized', status: 403 }
  }
  await db.users.delete({ where: { id: userId } })
}
```

- [ ] Tokens in httpOnly cookies (not localStorage)
- [ ] Authorization checked before sensitive operations
- [ ] Role-based access control implemented
- [ ] Session management secure

### 5. XSS Prevention

```typescript
import DOMPurify from 'isomorphic-dompurify'

function renderUserContent(html: string) {
  const clean = DOMPurify.sanitize(html, {
    ALLOWED_TAGS: ['b', 'i', 'em', 'strong', 'p'],
    ALLOWED_ATTR: [],
  })
  return <div dangerouslySetInnerHTML={{ __html: clean }} />
}
```

- [ ] User-provided HTML sanitized
- [ ] CSP headers configured (avoid `'unsafe-inline'` / `'unsafe-eval'`)
- [ ] No unvalidated dynamic content rendering

### 6. CSRF Protection

```typescript
export async function POST(request: Request) {
  const token = request.headers.get('X-CSRF-Token')
  if (!csrf.verify(token)) {
    return new Response(JSON.stringify({ error: 'Invalid CSRF token' }), { status: 403 })
  }
  // process request
}
```

- [ ] CSRF tokens on state-changing operations
- [ ] `SameSite=Strict` on session cookies

### 7. Rate Limiting

```typescript
import rateLimit from 'express-rate-limit'

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,
  message: 'Too many requests',
})
app.use('/api/', limiter)
```

- [ ] Rate limiting on all public endpoints
- [ ] Stricter limits on expensive operations (search, generation)

### 8. Sensitive Data Exposure

```typescript
// FAIL — logging sensitive data
console.log('User login:', { email, password })

// PASS — redact
console.log('User login:', { email, userId })
```

```typescript
// FAIL — exposing internals
catch (error) {
  return res.status(500).json({ error: error.message, stack: error.stack })
}

// PASS — generic message, log detail server-side
catch (error) {
  console.error('Internal error:', error)
  return res.status(500).json({ error: 'An error occurred. Please try again.' })
}
```

- [ ] No passwords, tokens, or secrets in logs
- [ ] Generic user-facing error messages
- [ ] No stack traces exposed to users

### 9. Dependency Security

```bash
npm audit          # check for vulnerabilities
npm audit fix      # fix auto-fixable issues
npm outdated       # check for outdated packages
```

```bash
npm ci             # reproducible builds (instead of npm install)
git add package-lock.json   # always commit lock files
```

- [ ] Dependencies up to date, `npm audit` clean
- [ ] Lock files committed
- [ ] Dependabot or equivalent enabled

## Security Testing

```typescript
test('requires authentication', async () => {
  const response = await fetch('/api/protected')
  expect(response.status).toBe(401)
})

test('rejects invalid input', async () => {
  const response = await fetch('/api/users', {
    method: 'POST',
    body: JSON.stringify({ email: 'not-an-email' }),
  })
  expect(response.status).toBe(400)
})
```

## Pre-Deployment Checklist

- [ ] No hardcoded secrets, all in env vars
- [ ] All user inputs validated
- [ ] All queries parameterized
- [ ] User content sanitized (XSS)
- [ ] CSRF protection enabled
- [ ] Proper token handling (authn) and role checks (authz)
- [ ] Rate limiting enabled
- [ ] HTTPS enforced in production
- [ ] Security headers (CSP, X-Frame-Options) configured
- [ ] No sensitive data in errors or logs
- [ ] Dependencies up to date, no known vulnerabilities
- [ ] CORS properly configured
- [ ] File uploads validated (size, type)

## Resources

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Web Security Academy](https://portswigger.net/web-security)

---

**Remember**: Security is not optional. One vulnerability can compromise the entire platform. When in doubt, err on the side of caution.
