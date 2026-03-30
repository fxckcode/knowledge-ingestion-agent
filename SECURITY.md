# Security Policy

## Supported Versions

We maintain security support for the following versions:

| Version | Supported          |
| ------- | ------------------ |
| 1.x.x   | :white_check_mark: |

## Reporting a Vulnerability

If you discover a security vulnerability in this agent definition or documentation, please report it by:

1. **Opening a private security advisory** on GitHub:  
   https://github.com/fxckcode/knowledge-ingestion/security/advisories

2. **Or email directly:**  
   Contact the repository owner via the email listed in the GitHub profile

### What to include

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if applicable)

### Response timeline

- **Initial response:** Within 48 hours
- **Status update:** Within 7 days
- **Fix timeline:** Depends on severity and complexity

## Security Scope

This repository contains:
- Agent definition files (Markdown)
- Documentation and reference materials
- No executable code or dependencies

**Security considerations apply to:**
- Agent instructions that could lead to unintended file operations
- Placeholder patterns that might expose sensitive data if not customized properly
- Documentation that could mislead users into insecure practices

## Out of Scope

- Vulnerabilities in Claude Code or Claude models themselves (report to Anthropic)
- Issues with third-party tools referenced in documentation
- User-specific customizations made after copying the agent file
