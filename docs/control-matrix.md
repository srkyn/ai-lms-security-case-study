# Control Matrix

This matrix translates the sanitized findings into controls that an AI/LMS owner
can evaluate.

| Control | Good State | Risk If Missing | Verification Question |
|---|---|---|---|
| Tool allowlisting | External tools are limited to approved domains, methods, and payload classes | Assistant can send sensitive context to untrusted destinations | Can a standard user cause an outbound request to an arbitrary host? |
| Tool confirmation | Risky tool calls require user-visible approval | Users may not know when data leaves the platform | Does the UI clearly show destination, method, and data class before sending? |
| Tool logging | Tool calls are logged with actor, destination, time, and sanitized metadata | Abuse is difficult to investigate after the fact | Can admins reconstruct what the assistant called and why? |
| Scoped credentials | LMS/API credentials are limited to the user's authorized context | AI integrations may exceed user privileges | Does the assistant inherit broad service privileges or user-scoped permissions? |
| Instruction hierarchy | User-authored instructions cannot override system/developer controls | Users can disable or weaken safety behavior | Can custom instructions change core security behavior? |
| Safety configuration | Guardrail settings are admin-controlled and default-on | Regular users can create unsafe assistant modes | Can a standard user disable platform safety context? |
| Context minimization | Only necessary user context is injected into the assistant session | Excess personal or academic data becomes available to prompts/tools | What user fields appear before the user asks anything? |
| Retrieval scoping | Documents are filtered by owner, course, role, and need | One user can retrieve another user's material | Can a user query content outside their expected context? |
| Memory boundaries | Memory is scoped, inspectable, and erasable | Data can persist across sessions unexpectedly | What persists, who can read it, and how is it cleared? |
| Message review | Generated messages require review before sending | Trusted channels can be used for phishing or impersonation | Can the assistant send messages without an explicit final confirmation? |
| Regression testing | Known AI failure modes are tested after config changes | Fixes regress silently | Are prompt injection, tool abuse, and retrieval boundary tests automated? |

## Priority Order

1. Restrict external tools and API scopes.
2. Remove user access to safety-bypass settings.
3. Separate custom instructions from system instructions.
4. Add retrieval ownership checks.
5. Add logging and user-visible tool approvals.
6. Add regression tests for the confirmed failure modes.
