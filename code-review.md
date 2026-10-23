
---
name: secure-code-review
description: Comprehensive evidence-based code review for C#, .NET and Java, covering security vulnerabilities, insecure coding patterns, error handling, business logic, authorization, reliability, dependencies and configuration. Produces strict JSON findings.
---

# Secure Code Review

## 1. Objective

Perform a comprehensive, read-only review of the repository to identify:
- Security vulnerabilities and exploitable weaknesses.
- Insecure coding patterns and unsafe API usage.
- Missing, incorrect or ineffective error and exception handling.
- Business-logic flaws and workflow integrity problems.
- Authentication, authorization, RBAC and tenant-isolation failures.
- Reliability, resource-management and concurrency defects.
- Dependency, secrets-management and configuration risks.
- Code-level defects that can cause data exposure, corruption, service
  disruption or incorrect security decisions.

Review C#, .NET, ASP.NET Core, Entity Framework, Java and applicable
Java frameworks.

Do not modify source code or automatically implement fixes.

## 2. Review methodology

### 2.1 Repository understanding

1. Identify languages, frameworks, application entry points and architecture.
2. Identify controllers, API endpoints, services, repositories, data-access
   layers, middleware, background jobs and external integrations.
3. Locate authentication, authorization, validation and exception-handling
   mechanisms.
4. Inspect project files, dependency manifests, lockfiles and relevant
   deployment configuration.
5. Identify existing security controls before concluding that a control
   is missing.

### 2.2 Security vulnerabilities

Check for applicable issues including:

- SQL, command, LDAP, XPath and other injection vulnerabilities.
- Cross-site scripting, request forgery and server-side request forgery.
- Path traversal, unsafe file operations and unrestricted file uploads.
- Insecure deserialization and unsafe reflection.
- Broken authentication, authorization, RBAC and tenant isolation.
- Insecure direct object references and missing ownership checks.
- Hardcoded credentials, exposed secrets and sensitive-data leakage.
- Weak cryptography, insecure randomness and improper certificate validation.
- Race conditions, time-of-check/time-of-use flaws and privilege escalation.
- Unsafe redirects, excessive data exposure and security misconfiguration.
- Vulnerable dependencies and unsafe dependency or build configurations.

Apply checks according to the technologies and functionality actually present.

### 2.3 Insecure coding patterns

Identify unsafe or error-prone implementations, including:

- Trusting client-supplied roles, identities, ownership or permissions.
- Missing or inconsistent input validation at trust boundaries.
- Unsafe string concatenation in queries, commands or output generation.
- Broad exception suppression or empty catch blocks.
- Catching exceptions without handling, logging or propagating them appropriately.
- Returning stack traces, internal paths, secrets or sensitive data to clients.
- Using insecure defaults or bypassing framework security mechanisms.
- Incorrect null handling, unchecked assumptions and unsafe type conversions.
- Mutable shared state or incorrect synchronization.
- Improper transaction boundaries and partial updates.
- Unbounded loops, expensive operations or uncontrolled resource consumption.
- Dead code or unreachable security checks that create misleading protection.
- Deprecated or dangerous APIs where their use creates a concrete risk.

Do not classify every style violation or deprecated API as a security finding.
Explain the practical consequence of each reported issue.

### 2.4 Error and exception handling

Check for:

- Missing exception handling at appropriate application boundaries.
- Missing centralized error-handling middleware or equivalent mechanisms
  where the application requires them.
- Unhandled exceptions that can terminate requests, background jobs or
  processes unexpectedly.
- Empty catch blocks and swallowed exceptions.
- Exceptions logged without sufficient diagnostic context.
- Sensitive values included in logs or error responses.
- Generic error handling that incorrectly converts failures into success.
- Incorrect HTTP status codes or misleading success responses.
- Failure to roll back transactions or clean up resources after exceptions.
- Lost exception causes or inappropriate rethrowing.
- Inconsistent error responses across API endpoints.
- Incorrect retry handling that duplicates non-idempotent operations.

Do not require a try/catch block around every method.
Report missing handling only when a realistic failure path is not handled
at the appropriate boundary or causes a material reliability or security risk.

### 2.5 Business logic and authorization

Trace sensitive operations end to end.

Check for:

- Missing permission checks at API, service and data-access boundaries.
- Missing object ownership and tenant-isolation checks.
- Privilege escalation and inconsistent role enforcement.
- Invalid workflow or state transitions.
- Missing validation of business invariants.
- Trust in client-side validation alone.
- Replay, duplicate-request and idempotency problems.
- Race conditions involving balances, approvals, inventory or state changes.
- Inconsistent authorization between read, update, delete and export paths.
- Mass assignment and unintended modification of protected fields.
- Incorrect pagination, filtering or data scoping that exposes other users' data.
- Approval bypasses, self-approval or separation-of-duties violations.

Only report issues supported by the implementation and its available context.
If business requirements are unclear, describe the uncertainty rather than
inventing a business rule.

### 2.6 Language-specific checks

For C# and .NET, examine applicable:
- ASP.NET Core authorization policies and middleware.
- Entity Framework and raw SQL usage.
- Async/await exception propagation and cancellation handling.
- Disposal of IDisposable and IAsyncDisposable resources.
- HttpClient usage, certificate validation and outbound requests.
- Serialization, model binding, dependency injection and configuration.
- Cryptographic APIs and secret storage.

For Java, examine applicable:
- Spring Security or equivalent framework configuration.
- JDBC, JPA/Hibernate and raw SQL usage.
- Exception propagation and overly broad catch clauses.
- Resource closure and try-with-resources.
- Thread safety, synchronization and asynchronous execution.
- Deserialization, reflection and process execution.
- Cryptographic APIs, secret storage and logging practices.

Do not report framework features as missing until the relevant code and
configuration have been inspected.

### 2.7 Reliability and maintainability

Identify code-level defects that can cause material operational impact:
- Null dereferences and unchecked failure conditions.
- Resource leaks involving files, sockets, database connections or streams.
- Deadlocks, race conditions and unsafe concurrent access.
- Incorrect cleanup and lifecycle management.
- Failure paths that leave partially updated state.
- Unbounded resource usage and avoidable denial-of-service conditions.
- Incorrect retry, timeout and cancellation behavior.

Include maintainability issues only when there is a concrete correctness,
security, reliability or operational consequence.

## 3. Evidence and verification requirements

For each finding:

1. Identify the exact source location whenever possible.
2. Explain the relevant code behavior.
3. Describe the triggering input, execution condition or failure scenario.
4. Explain the impact and any required attacker privileges.
5. Identify existing controls and why they are insufficient, if applicable.
6. Recommend a specific remediation.
7. Distinguish confirmed vulnerabilities from plausible but unverified concerns.

Do not invent source locations, execution results, CVEs, CWE identifiers
or business requirements.

Use the nearest defensible line and column for findings that span multiple
lines. Use line 0 or column 0 only when the location genuinely cannot
be determined.

Deduplicate the same root cause when it appears in multiple code paths,
unless separate locations represent independently actionable vulnerabilities.

## 4. Severity and classification

Use these severity levels:
- Critical
- High
- Medium
- Low
- Info

The CVSS value must be a numeric value from 0.0 to 10.0.

Use an applicable CVSS scoring methodology and assess exploitability and
impact based on available evidence. Do not assign a precise score when
the evidence is insufficient; use a defensible estimate and document
uncertainty in the description.

Use an applicable CWE identifier such as CWE-79, CWE-89, CWE-476,
CWE-703 or CWE-209. Do not guess identifiers. If no applicable CWE can
be established, use an empty string.

The type field must be an array of applicable classification strings.
Use consistent kebab-case values, including but not limited to:

- security-vulnerability
- insecure-code-pattern
- missing-error-handler
- improper-exception-handling
- input-validation
- authentication
- authorization
- business-logic
- sensitive-data-exposure
- secret-management
- dependency-risk
- security-misconfiguration
- resource-leak
- concurrency
- reliability
- maintainability

A finding may have multiple applicable types.

## 5. Required JSON output

Return ONLY one valid JSON object.
Do not include Markdown fences, introductory prose, trailing commentary,
or additional top-level properties.

Use this exact top-level structure:

{
  "reviewDate": "YYYY-MM-DD",
  "sourceFile": "repository-relative path or repository-wide",
  "findings": [
    {
      "vul_id": "unique-hash",
      "type": [
        "insecure-code-pattern",
        "missing-error-handler"
      ],
      "severity": {
        "cvss": 0.0,
        "level": "Info"
      },
      "cwe": "CWE-####",
      "location": {
        "line": 0,
        "column": 0,
        "method": "function or description"
      },
      "description": "Concrete issue, triggering conditions and potential impact.",
      "recommendation": "Specific remediation guidance.",
      "evidence": "Relevant code evidence with enough context to substantiate the finding."
    }
  ]
}

### JSON field requirements

- reviewDate: Actual review date in ISO 8601 date format.
- sourceFile: Repository-relative file path for a file-scoped review.
  For a repository-wide report, use "repository-wide".
- findings: Array of actionable, evidence-based findings.
- vul_id: Stable, unique hash derived from the finding category, file path,
  location and root cause. Use a deterministic hash where tooling permits.
  Do not generate random identifiers that change on every review.
- type: Array of applicable classifications from the taxonomy above.
- severity.cvss: Numeric CVSS score from 0.0 to 10.0.
- severity.level: Exactly one of Info, Low, Medium, High or Critical.
- cwe: Applicable CWE identifier or an empty string when undetermined.
- location.line: One-based source line number, or 0 when unavailable.
- location.column: One-based source column number, or 0 when unavailable.
- location.method: Method, constructor, endpoint, configuration section,
  or meaningful description of the affected code.
- description: Explain the issue, conditions, and impact without unsupported
  claims.
- recommendation: Describe a concrete corrective action.
- evidence: Include a concise, relevant code excerpt or a precise description
  of the observed implementation. Never include actual secret values.

### Output validation

Before returning the report, verify that:

1. The output parses as valid JSON.
2. Only the three required top-level fields are present.
3. Every finding contains all nine required fields and correct data types.
4. Severity labels use only the allowed values.
5. CVSS values are numeric and within 0.0–10.0.
6. Type values are arrays of strings.
7. Every finding has evidence and a specific remediation.
8. Findings are deduplicated and identifiers are stable.
9. No Markdown or text exists outside the JSON object.

If no findings are identified, return an empty findings array.
Do not manufacture findings to populate the report.

## 6. Safety boundaries

- Perform read-only source inspection.
- Request approval before running commands, builds or scanners.
- Never modify, delete or create application files during the review.
- Do not install packages or execute repository-provided scripts without
  explicit approval.
- Do not disclose credentials, tokens or secret values.
- Clearly disclose uninspected files, unavailable tooling and coverage limits.
- Do not claim that the repository is secure merely because no findings
  were detected.

## 7. Completion criteria

The review is complete only when the inspected scope, tools used, and
limitations are understood well enough to describe the coverage accurately.

Return the required JSON object only.
