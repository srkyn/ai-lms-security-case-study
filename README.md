# AI/LMS Security Assessment Case Study

Sanitized case study of an authorized AI assistant security assessment in a
learning-management environment.

This repository is intentionally public-safe. It does not include the original
confidential report, target URLs, institution names, student data, exploit
strings, screenshots of private systems, or vendor-specific implementation
details.

![Sanitized assessment summary](docs/assets/ai-lms-assessment-summary.svg)

## Summary

The assessment reviewed an AI assistant embedded in an LMS course environment.
Testing focused on how the assistant handled user context, tool access, system
instructions, external requests, uploaded files, and stored knowledge.

The private report was delivered to the authorized stakeholders. This public
case study preserves the security lessons while removing operational details
that could identify or expose the environment.

## What Was Assessed

- AI chat behavior in an authenticated student-style session
- Tool access boundaries for outbound HTTP and platform integrations
- Custom assistant configuration controls
- Prompt and instruction hierarchy behavior
- User context passed from the LMS into the AI session
- File upload and retrieval behavior
- Shared knowledge-base exposure risks

## Methodology

The assessment used OWASP Top 10 for LLM Applications as the primary framework,
with additional checks for:

- Prompt injection and instruction override
- Tool abuse and excessive agency
- External data exfiltration paths
- Sensitive information disclosure
- Insecure output and response handling
- Cross-session memory behavior
- Excessive user context exposure
- Retrieval-augmented generation data boundaries

## Sanitized Finding Categories

| Area | Risk Pattern | Why It Matters |
|---|---|---|
| External tool access | AI could make outbound requests without sufficient restriction | Sensitive session data may leave the platform |
| Instruction control | User-configurable instructions could override intended behavior | Users may alter assistant behavior beyond intended scope |
| Safety configuration | Guardrail controls were exposed or weakly scoped | Protective controls may not apply consistently |
| Self-disclosure | Assistant could reveal internal behavior and attack surface | Attackers gain a map of useful abuse paths |
| LMS integration | Platform APIs and user context were visible to the assistant | AI scope may exceed user expectations |
| Shared knowledge | Documents were retrievable across inappropriate boundaries | Personal or academic data may be exposed |
| Messaging tools | Communication tools could be abused from a trusted session | Phishing and impersonation risk increases |

## Key Lessons

1. **Tool access needs policy, not just availability.**
   AI tools that can send HTTP requests, read memory, or call platform APIs need
   allowlists, user confirmation, logging, and scoped credentials.

2. **Custom instructions must not become system instructions.**
   User-editable prompts should not override platform safety behavior or create
   persistent control paths.

3. **The LMS context boundary matters.**
   Names, roles, cohorts, course IDs, documents, and messages are sensitive when
   automatically injected into AI context.

4. **RAG data needs ownership rules.**
   Knowledge bases should enforce document-level access controls and prevent one
   user from retrieving another user's personal material.

5. **AI security findings need plain-language remediation.**
   The most useful report is not a list of clever prompts. It is a clear map of
   which controls failed, what data was at risk, and how to reduce exposure.

## Recommended Controls

- Restrict outbound HTTP tools to approved domains and methods.
- Require visible user approval for external requests that include session data.
- Log AI tool calls with destination, method, caller, and sanitized payload metadata.
- Separate user-authored instructions from system and developer instructions.
- Disable any user-facing option that bypasses platform safety context.
- Enforce least-privilege API scopes for LMS integrations.
- Remove personal documents from shared retrieval stores unless access control is explicit.
- Add document-level ownership and course-section scoping to AI retrieval.
- Review generated messages before sending from a trusted identity or platform session.
- Add security regression tests for prompt injection, tool misuse, and data boundary failures.

## What Is Not Published

- The original confidential PDF report
- Target URLs, course IDs, tenant IDs, or organization names
- Exploit prompts or copy-paste abuse strings
- Screenshots from private systems
- Student data, documents, names, academic records, or messages
- Vendor credentials, tokens, headers, or internal endpoints

## Portfolio Context

This case study demonstrates AI security assessment and responsible reporting.
It complements the defensive tooling projects in this profile by showing manual
assessment work: defining scope, testing controls, documenting impact, and
translating findings into remediation steps.

## Responsible Disclosure Note

The underlying assessment was performed in an authorized context and shared
privately with the appropriate stakeholders. This public version is intentionally
sanitized and should not be used as an exploit guide.
