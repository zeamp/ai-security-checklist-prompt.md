# 1. FULL CODEBASE ANALYSIS

First, inspect the codebase systematically before making any modifications.

Do not immediately start fixing individual findings. Build an understanding of:

* Application architecture
* Entry points and exposed interfaces
* Authentication mechanisms
* Authorization and privilege boundaries
* Data flows
* User-controlled input
* Sensitive data flows
* External integrations
* Database/storage access
* File handling
* Network communication
* Background jobs/workers
* Administrative functionality
* Configuration and secrets
* Dependency usage
* Build/deployment configuration
* Client/server boundaries
* Trust boundaries
* Security-sensitive business logic

Search the entire repository, including where applicable:

* Source code
* Configuration files
* Environment/configuration templates
* API definitions
* Routes/controllers/handlers
* Middleware
* Authentication and authorization code
* Database queries and migrations
* Serialization/deserialization logic
* File upload/download functionality
* Templates/views/UI code
* Client-side code
* Scripts and utilities
* Build/deployment files
* Container/VM configuration
* Infrastructure configuration
* Dependency manifests and lockfiles
* Tests
* Documentation when it reveals security-sensitive behavior

Do not assume a file is safe simply because it is not part of the primary application.

---

# 2. SECURITY REVIEW

Analyze the application as an attacker would.

Look for vulnerabilities including, but not limited to:

## Authentication

* Authentication bypasses
* Weak authentication logic
* Broken session handling
* Session fixation
* Session hijacking risks
* Improper credential handling
* Weak password handling
* Missing authentication checks
* Authentication state confusion
* Insecure password-reset/recovery flows
* MFA bypasses where applicable
* Trusting client-controlled authentication state

## Authorization

* Missing authorization checks
* Privilege escalation
* Horizontal privilege escalation
* Vertical privilege escalation
* Insecure direct object references
* Broken access-control boundaries
* Access to another user's resources
* Administrative functionality exposed to unauthorized users
* Client-side-only authorization
* Inconsistent authorization between endpoints

Pay particular attention to cases where an attacker can manipulate:

* IDs
* UUIDs
* usernames
* account identifiers
* resource paths
* API parameters
* request bodies
* cookies
* headers
* tokens
* roles
* permissions

## Input & Injection

Check all externally influenced input for unsafe handling, including:

* SQL injection
* NoSQL injection
* Command injection
* Code injection
* Template injection
* Expression-language injection
* LDAP injection
* XPath injection
* Header injection
* HTTP response splitting
* Cross-site scripting
* Path traversal
* Server-side request forgery
* Unsafe deserialization
* Regular-expression denial of service
* Shell/process execution vulnerabilities

Do not only look for obvious user input. Trace data through the application to determine whether untrusted data can eventually reach a dangerous operation.

## Data Security

Look for:

* Sensitive information exposure
* Credentials stored in source code
* API keys or tokens
* Private keys
* Database credentials
* Hardcoded secrets
* Secrets accidentally committed to the repository
* Excessive API responses
* Sensitive information in logs
* Sensitive information in error messages
* Insecure storage
* Improper encryption
* Weak cryptographic practices
* Sensitive data transmitted insecurely
* Personally identifiable information exposure
* Cross-user data leakage

Never reproduce discovered secrets unnecessarily in your output. If credentials or secrets are found, identify the location and type of secret without unnecessarily exposing the value.

## Web/API Security

Where applicable, inspect:

* API endpoints
* HTTP methods
* Request validation
* Response handling
* CORS
* CSRF protection
* Security headers
* Content types
* Redirect handling
* URL validation
* Rate limiting
* Abuse controls
* API authentication
* API authorization
* Object-level authorization
* Mass assignment
* Parameter pollution
* Excessive data exposure
* Unrestricted resource consumption

## File & Resource Handling

Inspect:

* File uploads
* File downloads
* File paths
* Temporary files
* Archives
* User-controlled filenames
* MIME/type validation
* File permissions
* Symbolic links
* Directory traversal
* Resource exhaustion
* Unsafe file processing
* Image/document/media processing

## Business Logic

Look for vulnerabilities that cannot be detected through simple pattern matching.

Examples include:

* Bypassing required workflow steps
* Replaying operations
* Manipulating prices, quantities, balances, or limits
* Performing actions in an invalid order
* Race conditions
* Trusting client-calculated values
* Circumventing ownership restrictions
* Abuse of legitimate functionality
* Missing transaction boundaries
* Duplicate operations
* Privilege or state confusion

## Configuration & Deployment

Inspect security-sensitive configuration such as:

* Debug/development modes
* Default credentials
* Development backdoors
* Dangerous defaults
* Insecure network exposure
* TLS configuration
* Cookie security settings
* CORS policies
* Permissions
* Container configuration
* Service configuration
* Build scripts
* Deployment scripts
* Environment handling
* Production configuration

## Dependencies & Supply Chain

Where dependency information is available:

* Identify known vulnerable dependencies
* Identify obviously abandoned or dangerous dependencies
* Check for suspicious dependency usage
* Check whether security-sensitive functionality is unnecessarily delegated to dependencies
* Identify unsafe dependency configurations

Do not treat every outdated dependency as a vulnerability. Distinguish between:

* Known exploitable vulnerabilities
* Security-relevant outdated components
* Merely old software with no identified security impact

---

# 3. ATTACK-PATH ANALYSIS

For important findings, reason about the complete attack path rather than identifying isolated patterns.

For example:

UNTRUSTED INPUT
→ ENTRY POINT
→ VALIDATION
→ INTERNAL PROCESSING
→ DANGEROUS OPERATION
→ SECURITY IMPACT

Determine whether the vulnerability is actually reachable and exploitable in the application's architecture.

Where possible, answer:

* Can an unauthenticated attacker reach it?
* Does exploitation require an authenticated account?
* What privileges are required?
* What attacker-controlled data reaches the vulnerable operation?
* What security boundary is crossed?
* What can the attacker ultimately accomplish?
* Are there mitigating controls elsewhere in the application?
* Is exploitation practical or merely theoretical?

Do not report hypothetical vulnerabilities as confirmed vulnerabilities without evidence.

---

# 4. FINDING CLASSIFICATION & PRIORITIZATION

Classify every meaningful finding by severity:

* CRITICAL
* HIGH
* MEDIUM
* LOW
* INFORMATIONAL

Prioritize based on actual security impact and exploitability, considering:

* Remote vs local exploitation
* Authentication requirements
* Required privileges
* User interaction
* Attack complexity
* Scope of compromise
* Confidentiality impact
* Integrity impact
* Availability impact
* Number of affected users/resources
* Whether exploitation can be chained with another weakness

Do not inflate severity simply because a security best practice is not implemented.

Clearly distinguish:

* Confirmed vulnerability
* Likely vulnerability
* Security weakness
* Defense-in-depth recommendation
* Informational observation

---

# 5. DOCUMENT EVERY FINDING

For each confirmed or credible security issue, provide:

### Finding

A concise description of the vulnerability.

### Severity

CRITICAL / HIGH / MEDIUM / LOW / INFORMATIONAL

### Location

Provide the exact:

* File
* Function/class/module where applicable
* Line number or relevant code section

### Security Impact

Explain what an attacker could actually accomplish.

### Attack Scenario

Describe a realistic exploitation path.

### Evidence

Explain exactly what in the code demonstrates the problem.

### Root Cause

Identify the underlying design or implementation mistake.

### Remediation

Describe the smallest appropriate change that properly fixes the vulnerability.

### Security Considerations

Explain what the proposed fix changes from a security perspective.

### Related Risks

Identify other areas that should be reviewed if the same underlying mistake appears elsewhere.

Do not expose credentials, private keys, tokens, passwords, or other secrets in the report.

---

# 6. ANALYSIS MUST COME BEFORE MODIFICATION

Do not modify the code during the initial analysis.

First complete the security assessment and produce a prioritized remediation plan.

The plan should identify:

1. Finding
2. Severity
3. Affected location
4. Root cause
5. Proposed fix
6. Potential side effects
7. Verification method

Only after the analysis and remediation plan are complete should implementation begin.

---

# 7. IMPLEMENTATION

When implementing fixes:

* Make the smallest changes necessary
* Preserve existing functionality
* Preserve the application's intended architecture
* Do not perform unrelated refactoring
* Do not redesign the application unnecessarily
* Do not make cosmetic changes
* Do not optimize unrelated code
* Do not silently change business behavior
* Do not weaken existing security controls to make tests pass

If the same vulnerability exists in multiple locations, fix the underlying pattern consistently rather than patching only the first occurrence.

If a secure fix requires architectural changes, clearly explain why a minimal patch would be insufficient.

---

# 8. VERIFY EVERY FIX

After each security modification, verify that:

* The original vulnerability is actually resolved
* The vulnerable attack path is no longer possible
* Existing legitimate functionality still works
* Authorization boundaries remain intact
* Error handling remains safe
* No sensitive information is newly exposed
* The fix does not introduce another vulnerability
* Related instances of the same vulnerability have been addressed

Where practical, create or update security-focused tests that demonstrate the vulnerability is no longer exploitable.

Tests should verify both:

* The malicious/unauthorized operation is rejected
* The legitimate operation still succeeds

---

# 9. BEFORE/AFTER DOCUMENTATION

For every code change, document:

### Before

What the original code allowed or failed to enforce.

### Vulnerability

Why that behavior created a security risk.

### After

What was changed.

### Protection

Why the new implementation prevents the attack.

### Verification

How the fix was tested or otherwise verified.

Do not include unnecessary full-file dumps. Show only the relevant code or a concise description of the change.

---

# 10. FINAL SECURITY REVIEW

After all approved fixes have been implemented, perform a second security pass over the affected code.

Specifically check for:

* Remaining instances of the same vulnerability
* Bypass paths around the new security control
* Inconsistent enforcement
* New authorization gaps
* New input-handling problems
* Information leakage
* Error-handling problems
* Regression risks
* Security assumptions introduced by the fix

Do not declare the code "secure" or "vulnerability-free." Instead, report what was reviewed, what was fixed, what remains, and what could not be verified.

---

# 11. FINAL REPORT

Finish with a concise security report containing:

## Executive Summary

Summarize the overall security posture discovered during the audit without exaggerating certainty.

## Findings

List each finding with:

* Severity
* Location
* Vulnerability
* Impact
* Status

## Remediation

Summarize the security fixes that were implemented.

## Remaining Risks

List vulnerabilities, weaknesses, assumptions, or areas that still require attention.

## Verification

Describe tests, static analysis, manual review, or other verification performed.

## Recommended Follow-Up

Identify additional security measures that would materially improve the application's security.

---

# IMPORTANT RULES

1. **Security comes first, but do not break legitimate functionality unnecessarily.**
2. **Do not modify code before completing the initial analysis.**
3. **Do not report theoretical issues as confirmed vulnerabilities.**
4. **Do not assume a vulnerability exists solely because a security best practice is absent.**
5. **Trace data and execution paths before reaching conclusions.**
6. **Prioritize exploitable, high-impact vulnerabilities.**
7. **Do not expose discovered secrets in the audit output.**
8. **Do not make unrelated refactors.**
9. **Do not make cosmetic or performance changes unless directly required for security.**
10. **Do not silently alter intended business logic.**
11. **Check for the same vulnerability throughout the entire codebase, not only at the first occurrence.**
12. **Prefer robust security controls at trust boundaries rather than relying solely on client-side protections.**
13. **Defense-in-depth improvements should be clearly distinguished from actual vulnerabilities.**
14. **If you cannot verify something, explicitly say so. Do not guess.**
15. **When security and functionality conflict, explain the trade-off rather than silently choosing one.**
16. **Never claim an audit is complete if significant portions of the codebase were not actually reviewed.**

Treat this as a real adversarial security assessment: think like an attacker, verify like a security engineer, and modify code like a careful maintainer.
