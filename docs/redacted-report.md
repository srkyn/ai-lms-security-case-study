# Redacted AI Security Assessment Report

## Executive Summary

This report summarizes an authorized AI security assessment of an LMS-integrated assistant deployed in a student training environment. Testing was performed from a standard learner account with no administrative credentials.

The assessment found a cluster of AI application risks around tool access, user-controlled assistant configuration, outbound connectivity, data boundary enforcement, and internal configuration disclosure.

The biggest lesson was simple: the assistant behaved less like a locked-down tutor and more like a general-purpose agent with tools, memory, document access, and user-editable behavior.

That combination matters. If an AI assistant can read private context, accept custom high-priority instructions, call external URLs, query platform APIs, and ingest files, then its safety model has to be designed like an application security boundary, not like a chatbot preference panel.

## Scope

The assessment focused on:

- The AI chat interface inside an LMS course environment
- Assistant tool behavior, including outbound web requests and persistent memory reads
- Student-accessible assistant settings
- Knowledge-base and document retrieval behavior
- LMS integration behavior visible from the assistant session

The assessment did not include:

- Direct testing of unrelated LMS vulnerabilities
- Attempts to access other student accounts
- Network infrastructure testing
- Credential theft, destructive testing, or persistence beyond the authorized test configuration

## Methodology

The testing approach followed common AI application security themes from the OWASP Top 10 for LLM Applications:

- Prompt injection and instruction hierarchy testing
- Tool misuse testing
- Data exfiltration path analysis
- Memory and cross-session behavior review
- Retrieval-augmented generation boundary testing
- Sensitive information disclosure checks
- Social engineering and unsafe guidance evaluation

Testing was performed from the perspective of a normal student user. The goal was to understand what could be extracted or influenced without administrative access.

## System Behavior Observed

The assistant demonstrated capabilities beyond basic Q&A:

- It could search and summarize course-connected files.
- It received LMS user context automatically at session start.
- It exposed tool execution details in the user interface.
- It had access to an outbound request tool.
- It checked persistent memory automatically.
- It allowed user-created assistant configurations.
- It supported document ingestion and external document fetching.

Those capabilities are useful in a learning environment. They also become security-relevant when defaults are permissive and user-controlled instructions can influence tool behavior.

## Findings

### F-01: Unrestricted Outbound Requests from AI Tools

**Severity:** Critical

The assistant had an outbound request tool capable of sending data to external web addresses. During testing, an outbound request was successfully triggered from the student session.

The public version of this report omits the exact request payload and listener details. The important security point is that AI-visible context should not be able to leave the trusted environment without policy checks, destination allowlisting, logging, and user-visible confirmation.

**Impact:** AI-visible session data, LMS-provided user context, retrieved documents, or conversation content could potentially be sent off-platform.

### F-02: User-Editable High-Priority Instructions

**Severity:** Critical

A student-accessible assistant configuration panel allowed custom instructions to be inserted at a high level of influence. In testing, those instructions changed how the assistant responded across subsequent interactions.

This is dangerous because user-controlled instructions should not be treated as equivalent to platform policy. A student should be able to customize style and learning goals, not override safety posture or tool governance.

**Impact:** A normal user could weaken expected assistant behavior and create durable behavior changes inside a custom assistant.

### F-03: Safety/Context Bypass Exposed in Assistant Settings

**Severity:** Critical

A visible assistant setting allowed system context behavior to be bypassed or weakened. In the tested configuration, this setting was easy for a student to find and use.

Safety-critical controls should not be exposed as ordinary user preferences. If a bypass exists for development or debugging, it should be limited to trusted roles and heavily logged.

**Impact:** Guardrails could be disabled through standard UI behavior rather than through a technical exploit.

### F-04: Assistant Disclosed Its Own Attack Surface

**Severity:** Critical

After behavior was altered through configuration, the assistant provided a ranked explanation of its own weak points and abuse paths.

The public report does not include the exact prompts or copy-ready strings. The lesson is that an assistant should not help users enumerate ways to defeat the assistant, especially when it has tools and access to sensitive context.

**Impact:** The skill required to exploit the system dropped from "security tester" to "curious user who asks the right question."

### F-05: LMS API Calls Attempted from Assistant Tooling

**Severity:** High

The assistant attempted to reach LMS API endpoints through its outbound request tool. The observed request failed due to missing authentication rather than because the assistant was prevented from trying.

This distinction matters. "The request failed" is not the same as "the system policy blocked the request."

**Impact:** If credentials or tokens were later introduced, intentionally or accidentally, the assistant could become a path to enumerate LMS data.

### F-06: Cloud Command Proxy Behavior Indicated by Tool Errors

**Severity:** High

Tool output indicated the presence of an internal cloud command proxy path. The endpoint was unreachable during testing, but the assistant appeared configured to route cloud-style commands through it.

Internal command proxying is high risk in a student-facing assistant unless strongly separated by role, network boundary, allowlist, and confirmation layer.

**Impact:** If activated or made reachable, a student-accessible assistant could potentially become a cloud reconnaissance interface.

### F-07: Broad Tool Access Enabled by Default

**Severity:** High

Newly created custom assistants appeared to receive broad tool access by default, including tools that can interact with external systems.

Default-deny is the safer model. Educational assistants should receive the minimum tools needed for their purpose.

**Impact:** A student could quickly create an assistant with more capability than the use case required.

### F-08: External Response Content Influenced Chat Context

**Severity:** High

When the assistant fetched external content, response data could enter the conversation context. Some unsafe code-like content was filtered, but plain-language social engineering content was not consistently treated as hostile.

Fetched content should be handled as untrusted input. It should not silently become instruction-bearing context.

**Impact:** External pages could influence assistant responses, mislead users, or steer them toward unsafe actions.

### F-09: User Context Auto-Injected into AI Sessions

**Severity:** Medium

The assistant received LMS-derived user context automatically at session start. This included identity and role-style information.

Automatic context injection is not inherently bad, but it should be transparent, minimized, and protected from tool-based exfiltration.

**Impact:** If outbound requests are available, even ordinary session context becomes privacy-sensitive.

### F-10: Persistent Memory Checked Automatically

**Severity:** Medium

The assistant automatically queried persistent memory at session start.

Memory can improve user experience, but it creates risks around retention, poisoning, isolation, and surprise reuse of past data.

**Impact:** Sensitive data or unsafe instructions could persist longer than the user expects if memory write controls are weak.

### F-11: Uploaded and Fetched Documents Entered AI Context

**Severity:** Medium

The assistant could ingest uploaded or externally fetched documents for summarization and retrieval.

Documents are untrusted inputs. They can contain hidden instructions, malicious content, or sensitive data that should not enter shared retrieval systems.

**Impact:** A document could influence assistant behavior or expose information if ingestion boundaries are weak.

### F-12: Lab-Style Framing Produced Risky Command Guidance

**Severity:** Medium

Requests framed as educational security labs produced command guidance that could be risky if copied into a real environment.

Security education assistants need context-aware warnings and environment scoping. "This is for a lab" should not automatically remove caution.

**Impact:** Students could run reconnaissance or administrative commands against live systems while believing they were following training instructions.

### F-13: Platform and Architecture Details Disclosed

**Severity:** Informational

Tool panels and assistant responses disclosed platform names, tool names, and internal architecture hints.

This was not a standalone compromise, but it made other testing easier.

**Impact:** Architecture details can reduce attacker guesswork and sharpen follow-on abuse attempts.

### F-14: Internal Prompt/Configuration Details Exposed

**Severity:** Critical

The assistant exposed internal prompt and configuration details under altered behavior. The original confidential report included evidence of internal sections, tool schemas, file paths, role data, and platform instructions.

This public version intentionally omits those details.

**Impact:** Prompt and configuration disclosure can give users a map of available tools, internal assumptions, data paths, and policy wording.

### F-15: Email Tool Presented Phishing/Impersonation Risk

**Severity:** High

Tool inventory suggested that an email-sending capability existed in or near the assistant environment.

Any tool that sends messages from a trusted institutional context needs strong confirmation, role checks, abuse detection, and audit logs.

**Impact:** If available to students without separate confirmation, the tool could enable trusted-looking phishing or impersonation.

### F-16: Personal Documents Appeared in Shared Knowledge Base

**Severity:** High

The assistant appeared to list personal documents belonging to people other than the current user inside a shared knowledge base.

The public report removes names, file titles, URLs, and screenshot evidence. The core issue is access control: personal documents should not be discoverable through shared retrieval unless explicitly intended and authorized.

**Impact:** Private resumes, transcripts, or other personal records could be summarized or queried by unauthorized users.

## Attack Chain Summary

The most important risk was the chain:

1. A standard user creates or edits an assistant.
2. The user weakens assistant behavior through exposed settings.
3. The assistant has broad tools by default.
4. The user causes the assistant to reveal internal behavior.
5. The user attempts outbound transfer of AI-visible context.
6. LMS, document, memory, and email capabilities increase the blast radius.

This is why AI application security has to be evaluated as a system. The danger often lives in the combination, not the single checkbox.

## Lessons Learned

- Tool access should be default-deny.
- User customization should never outrank platform policy.
- Outbound requests need allowlists and audit trails.
- Assistant memory needs isolation, transparency, and retention controls.
- Retrieved documents should be treated as untrusted input.
- Internal prompts and tool schemas should not be exposed to end users.
- LMS context should be minimized and clearly disclosed.
- "Educational lab" framing needs extra caution, not less.

## Closing Thought

The assistant was trying to be helpful. That was part of the problem.

In AI security, helpfulness without boundaries becomes capability without consent.

