# Remediation Playbook

This playbook maps the case-study findings to defensive controls. It is written for platform owners, LMS administrators, and AI product teams.

## Priority 0: Stop Data from Leaving Unexpectedly

Apply these controls first:

- Disable outbound web request tools for student-accessible assistants unless a specific use case requires them.
- Add destination allowlisting for any outbound request capability.
- Block requests to private address ranges, metadata services, internal hostnames, and LMS administrative endpoints.
- Require explicit user confirmation before sending any AI-visible data outside the platform.
- Log tool invocations with user ID, assistant ID, destination, method, timestamp, and redacted payload summary.

Related findings: F-01, F-05, F-06, F-08, F-09.

## Priority 1: Separate User Preferences from Security Policy

User-written assistant instructions should be treated as preferences, not authority.

Recommended controls:

- Remove student access to safety/context bypass controls.
- Enforce platform policy after user instructions, not before.
- Prevent custom instructions from enabling hidden modes, tool escalation, or system prompt disclosure.
- Add role-based access control for assistant configuration panels.
- Reset or review existing custom assistants created before controls are changed.

Related findings: F-02, F-03, F-04, F-14.

## Priority 2: Make Tool Access Default-Deny

Tools should be granted deliberately.

Recommended controls:

- Start new assistants with no external tools enabled.
- Require admins to approve high-risk tools such as web requests, email, file ingestion, memory writes, and API access.
- Separate read-only tools from action-taking tools.
- Add per-tool rate limits and anomaly detection.
- Display tool activity clearly to administrators, not just to the user who triggered it.

Related findings: F-01, F-05, F-07, F-15.

## Priority 3: Lock Down Retrieval and Documents

RAG systems need access control as much as search quality.

Recommended controls:

- Partition knowledge bases by course, role, user, and document sensitivity.
- Remove personal documents from shared collections unless explicitly approved.
- Scan uploads for hidden instructions, sensitive data, and risky embedded content.
- Treat retrieved text as untrusted content, never as system instruction.
- Show users what collection a retrieved document came from.

Related findings: F-11, F-16.

## Priority 4: Treat Memory as Sensitive Infrastructure

Memory changes the lifetime of risk.

Recommended controls:

- Make memory use visible to users.
- Add controls to inspect, delete, and disable memory.
- Isolate memory per user and per assistant unless sharing is explicitly intended.
- Prevent tools from writing raw secrets or policy-modifying instructions into memory.
- Add retention limits.

Related finding: F-10.

## Priority 5: Improve AI Safety for Security Education

Security training assistants need to distinguish practice from production.

Recommended controls:

- Require lab-environment qualifiers before giving administrative or reconnaissance commands.
- Add warnings when commands could affect real infrastructure.
- Prefer sandbox-specific examples over generic live-environment commands.
- Block instructions that encourage credential misuse, unauthorized enumeration, or persistence.
- Provide safer alternatives and defensive explanations.

Related finding: F-12.

## Suggested Fix Order

1. Disable or allowlist outbound requests.
2. Remove student-facing safety bypass controls.
3. Change custom assistant tool defaults to no tools.
4. Audit shared knowledge bases for personal documents.
5. Review email and LMS API tool permissions.
6. Add centralized logs and alerts for tool calls.
7. Review memory isolation and retention.
8. Retest the full chain from a standard student account.

## Retest Checklist

- A student cannot send arbitrary outbound HTTP requests.
- A student cannot disable platform/system context.
- A custom assistant starts with minimal permissions.
- User instructions cannot reveal prompts, tools, schemas, or internal file paths.
- LMS context is minimized and cannot be exfiltrated by tools.
- Personal documents are not visible across unrelated users.
- External documents cannot inject instructions into the assistant.
- Email actions require confirmation and proper authorization.
- Tool logs are visible to administrators.

