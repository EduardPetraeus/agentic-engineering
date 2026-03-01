# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this repository or any repository in the Agentic Engineering ecosystem, please report it responsibly.

### How to Report

**Do NOT open a public issue for security vulnerabilities.**

Instead, send an email to the repository maintainer via GitHub's private reporting:

1. Go to the relevant repository on GitHub
2. Click the **Security** tab
3. Click **Report a vulnerability**
4. Provide a detailed description of the issue

Alternatively, contact the maintainer directly through GitHub.

### What to Include

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if you have one)

### Response Timeline

- **Acknowledgment:** Within 48 hours of receiving the report
- **Initial assessment:** Within 7 days
- **Fix or mitigation:** Within 30 days for critical issues

### Scope

This security policy covers:

| Repo | Scope |
|---|---|
| agentic-engineering | Documentation content (broken links to sensitive resources, leaked information in docs) |
| ai-governance-framework | Governance bypass vulnerabilities, secret scanning gaps, CI/CD security |
| ai-project-management | Task schema injection, command injection via agent commands |
| ai-engineering-standards | Linter configuration that disables security checks |
| ai-project-templates | Scaffolder generating insecure defaults, template injection |

### Out of Scope

- AI agent behavior that follows the governance rules as designed (this is not a vulnerability)
- Typos or formatting issues (use regular issues for these)
- Feature requests (use regular issues)

## Security in the Framework

The Agentic Engineering ecosystem includes built-in security mechanisms:

- **Secret scanning** in the governance gate CI workflow
- **`.gitignore` validation** ensuring `.env` files are excluded
- **Security protocol section** in every CLAUDE.md template
- **Confidence ceilings** preventing auto-approval of high-risk agent actions
- **Blast radius controls** limiting what files agents can modify

## Supported Versions

| Version | Supported |
|---|---|
| Latest release | Yes |
| Previous minor | Best effort |
| Older versions | No |

We recommend always using the latest version of all ecosystem repos.
