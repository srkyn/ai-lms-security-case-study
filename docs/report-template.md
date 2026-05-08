# Sanitized Report Template

This template is safe to publish because it contains no target-specific
information. It shows the structure used for private reporting.

## Executive Summary

Briefly describe the assessed system, authorization context, assessment window,
and highest-risk control failures.

## Scope

In scope:

- AI assistant behavior from a standard-user session
- assistant tools and platform integrations
- retrieval and file handling behavior
- user context exposure

Out of scope:

- unrelated LMS functionality
- other users' accounts
- network infrastructure
- publication of private evidence

## Methodology

Reference OWASP Top 10 for LLM Applications and list the specific control areas
tested.

## Findings

| ID | Title | Severity | Affected Control | Status |
|---|---|---|---|---|
| F-01 | External tool access not sufficiently restricted | Critical | Tool access | Private report only |
| F-02 | User-editable instructions affect high-priority behavior | Critical | Instruction hierarchy | Private report only |
| F-03 | Safety configuration exposed to standard users | Critical | Safety controls | Private report only |
| F-04 | Excessive user context exposed to AI session | Medium | Context minimization | Private report only |
| F-05 | Retrieval boundaries need stronger ownership checks | High | RAG access control | Private report only |

## Finding Format

Each private finding should include:

- severity and rationale
- affected component
- plain-language description
- evidence summary
- impact
- reproduction context
- recommended remediation
- validation steps

## Remediation Summary

Group recommendations by owner:

- platform configuration
- AI assistant configuration
- LMS/API integration
- logging and monitoring
- regression testing

## Validation Plan

List how fixes should be retested without including exploit payloads in the
public version.
