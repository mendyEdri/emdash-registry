---
name: code-review
description: Perform systematic code review with focus on security, performance, and maintainability. Use when reviewing pull requests, code changes, or auditing existing code.
user_invocable: true
tools: [grep, glob, read_file]
tags: [review, quality, security]
---

# Code Review

This skill guides systematic code review focusing on security, performance, maintainability, and correctness.

## Review Process

1. **Understand Context**: What is this code trying to accomplish? Read the PR description or commit messages.

2. **Security Analysis**:
   - Input validation and sanitization
   - Authentication and authorization checks
   - SQL injection, XSS, command injection vulnerabilities
   - Secrets/credentials in code
   - OWASP Top 10 considerations

3. **Performance Review**:
   - Algorithm complexity (time and space)
   - Database query efficiency (N+1 problems)
   - Memory leaks and resource cleanup
   - Unnecessary computations in loops
   - Caching opportunities

4. **Maintainability**:
   - Code clarity and readability
   - Appropriate abstractions
   - DRY violations
   - Function/method length
   - Naming conventions

5. **Correctness**:
   - Edge cases handled
   - Error handling
   - Race conditions
   - Null/undefined checks
   - Type safety

## Output Format

Provide feedback in categories:
- **Critical**: Must fix before merge (security, correctness)
- **Important**: Should fix (performance, maintainability)
- **Suggestion**: Nice to have improvements
- **Praise**: What was done well

Be specific with line references and provide concrete suggestions for fixes.
