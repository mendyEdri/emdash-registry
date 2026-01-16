# Security Coding Guidelines

## Input Validation

- Validate all user input on the server side
- Use allowlists over denylists
- Sanitize data based on context (HTML, SQL, shell)
- Validate file uploads: type, size, content

## Authentication

- Use established libraries (don't roll your own crypto)
- Implement proper password hashing (bcrypt, argon2)
- Use secure session management
- Implement rate limiting on auth endpoints
- Support MFA where appropriate

## Authorization

- Check permissions on every request
- Use principle of least privilege
- Validate object-level access (IDOR prevention)
- Log authorization failures

## Data Protection

- Encrypt sensitive data at rest
- Use TLS for data in transit
- Never log sensitive data (passwords, tokens, PII)
- Implement proper key management
- Use parameterized queries (prevent SQL injection)

## OWASP Top 10 Awareness

1. **Injection**: Use parameterized queries, ORMs
2. **Broken Auth**: Secure session handling
3. **Sensitive Data**: Encrypt, don't log secrets
4. **XXE**: Disable external entities in XML parsers
5. **Broken Access Control**: Check permissions everywhere
6. **Misconfig**: Secure defaults, no debug in prod
7. **XSS**: Escape output, use CSP headers
8. **Insecure Deserialization**: Validate before deserializing
9. **Vulnerable Components**: Keep dependencies updated
10. **Logging**: Log security events, protect logs

## Secrets Management

- Never commit secrets to version control
- Use environment variables or secret managers
- Rotate secrets regularly
- Use different secrets per environment
