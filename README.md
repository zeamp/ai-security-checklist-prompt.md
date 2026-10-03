# AI Security Checklist Prompt

A reusable AI prompt for performing a thorough security review of a software codebase.

The prompt is **language and framework agnostic**, so it can be used with PHP, JavaScript, Python, C++, Java, Go, Rust, .NET, and other languages and stacks.

## What It Covers

The prompt instructs an AI to review a codebase for security issues including:

* Authentication and authorization
* Access-control vulnerabilities
* Input validation and injection
* API security
* Sensitive data exposure
* Hardcoded credentials and secrets
* File handling
* Session security
* Rate limiting
* Business-logic vulnerabilities
* Configuration and deployment issues
* Dependency and supply-chain risks
* Other vulnerabilities that could lead to unauthorized access, data exposure, or system compromise

## How It Works

The prompt uses a simple workflow:

**Analyze → Identify → Plan → Fix → Verify → Report**

The AI is instructed to review the codebase before making changes, explain each vulnerability, prioritize findings by severity, make minimal security-focused changes, and verify that the fixes don't introduce new problems.

It also tells the AI to distinguish between actual vulnerabilities and general security recommendations rather than treating every missing best practice as a critical issue.

## Usage

Give the AI access to the repository and provide:

```text
ai-security-checklist-prompt.md
```

Or paste the contents of the file into your AI agent's context window.

For best results, run it against the entire project rather than individual files. Security problems often involve multiple parts of an application.

## Important

This prompt is intended to **assist** with security reviews. It is not a replacement for professional penetration testing, manual security review, or dedicated security tools.

Always review AI-generated changes before deploying them to production.

## Credits

Prompts written originally by Richard Ward (zeamp) https://www.zpvy.com
