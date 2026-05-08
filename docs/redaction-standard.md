# Public Redaction Standard

The original assessment was confidential. This standard describes what was
removed before creating the public case study.

## Removed

- institution, vendor, assistant, and course names
- URLs, course IDs, tenant IDs, and internal hostnames
- exploit prompts, trigger strings, payloads, and copy-paste abuse examples
- screenshots from authenticated systems
- student names, messages, documents, academic records, or account details
- request headers, tokens, cookies, IDs, and tool execution payloads
- exact reproduction steps that would enable misuse
- timelines or operational notes that could identify the assessed environment

## Preserved

- assessment scope at a generic level
- control categories tested
- sanitized risk patterns
- remediation themes
- responsible disclosure framing
- lessons that apply to similar AI/LMS systems

## Why This Matters

A portfolio artifact should prove judgment, not publish an attack path. The
public version shows assessment process and remediation thinking while keeping
private evidence private.

## Public Language Rules

- Say "AI assistant embedded in a learning-management environment," not the
  product or institution name.
- Say "external request tool," not the exact tool name.
- Say "shared retrieval store," not the exact document source.
- Say "user-editable instructions," not the exact UI label when it could identify
  the target.
- Describe risk classes, not exploit strings.
