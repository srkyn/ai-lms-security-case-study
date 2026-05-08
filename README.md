# AI/LMS Security Assessment Case Study

Public case study from an authorized assessment of an AI assistant embedded in a
learning-management environment.

The confidential report stays private. This repository keeps the reusable parts:
scope, control questions, finding categories, remediation patterns, and redaction
discipline.

![Assessment summary](docs/assets/ai-lms-assessment-summary.svg)

## What This Shows

- How I scoped an AI/LMS assessment
- Which control areas I tested
- How findings were translated into remediation
- How portfolio evidence can be shared without exposing private systems

## Assessment Areas

| Area | Review Question |
|---|---|
| Tool access | Can the assistant call external services, platform APIs, or messaging tools beyond the user's expectation? |
| Instruction hierarchy | Can user-editable instructions weaken system or platform controls? |
| Safety configuration | Are guardrails admin-owned, default-on, and hard to bypass from a normal session? |
| LMS context | What user, role, course, and document data enters the AI session automatically? |
| Retrieval | Are knowledge sources scoped by owner, course, role, and document sensitivity? |
| Memory | Can prior session content or uploaded material cross boundaries? |
| Messaging | Can the assistant send trusted communications without review? |

## Sanitized Findings

| Area | Risk Pattern | Primary Fix |
|---|---|---|
| External tools | Outbound requests could include sensitive session context | Restrict destinations, methods, and data classes; require user approval |
| Instruction control | User-authored instructions could steer behavior beyond intended scope | Keep user instructions below platform and system controls |
| Safety settings | Protective controls were exposed or weakly enforced | Make safety settings admin-owned and regression tested |
| Self-disclosure | Assistant responses could reveal internal behavior and attack paths | Reduce unnecessary introspection and test for disclosure patterns |
| LMS integration | Platform context was broader than needed for the task | Minimize injected context and enforce role-aware scopes |
| Shared knowledge | Retrieval boundaries could expose inappropriate documents | Add document-level ownership and course-section scoping |
| Messaging tools | Trusted-session messages could be abused | Require review before sending from a platform identity |

## Recommended Controls

- Allowlist external tools by domain, method, and approved data type.
- Require visible user approval when requests include session or LMS context.
- Log tool calls with actor, destination, method, time, and sanitized metadata.
- Keep user-authored instructions below system and platform instructions.
- Make safety controls admin-owned, default-on, and covered by regression tests.
- Scope LMS retrieval by owner, course, role, section, and document sensitivity.
- Review generated messages before sending from a trusted identity.

## Public Boundary

Published:

- Assessment workflow
- Control matrix
- Redaction standard
- Sanitized report template
- LinkedIn-ready project copy

Withheld:

- Confidential report and evidence
- Target URLs, tenant IDs, course IDs, and organization names
- Exploit prompts, payloads, screenshots, tokens, headers, and internal endpoints
- Student, staff, document, message, or academic-record data

## Documents

- [Assessment workflow](docs/assessment-workflow.md)
- [Control matrix](docs/control-matrix.md)
- [Public redaction standard](docs/redaction-standard.md)
- [Sanitized report template](docs/report-template.md)
- [LinkedIn project copy](LINKEDIN.md)

The underlying assessment was authorized and reported privately. This version is
for portfolio review and does not provide reproduction steps.
