# Contributing to OLE

**Open Legal Exchange (OLE) — Founding Architecture v0.1**  
**Website:** openlegalexchange.org  
**Status:** Public Draft — external contributions welcome

---

## Current Status

OLE is a founder-stewarded public draft. The founding editor retains editorial control during this stage. All contributions are welcome and will be publicly tracked.

OLE is not yet governed by a formal standards body or multi-stakeholder committee. The intent is to build toward that if adoption warrants it. See [docs/governance-draft.md](docs/governance-draft.md) for the governance model.

---

## What This Project Needs

### High Priority for v0.1 Feedback

| Area | What Would Help |
|------|-----------------|
| CLEAR referral workflow | Is the 5-stage workflow sufficient? Are statuses missing or ambiguous? |
| Resource envelope | Are the required fields right? Is anything unnecessary? |
| Identifier model | Is the identifier policy implementable? |
| Consent model | Is the `ConsentRecord` model complete enough for legal referral use? |
| Status model | Are there missing statuses for real-world referral lifecycles? |
| Conformance checklist | Is the Tier 1 checklist accurate and achievable? |
| Relationship to existing standards | Are there mapping errors or missing relationships? |
| OpenAPI surface | Is the API surface reasonable for a first implementation? |
| JSON Schemas | Are the schemas usable? Are constraints missing or too loose? |

### Other Useful Contributions

- Identify ambiguity in field names or meanings
- Add or improve example JSON packets
- Propose improvements to privacy, consent, or audit language
- Suggest security requirements
- Identify potential conflicts with attorney ethics obligations
- Propose mappings to additional standards not yet covered
- Identify gaps in the adoption level model
- Propose new RFC topics

---

## How to Contribute

### Filing Issues

File a GitHub issue for:
- Errors, inconsistencies, or ambiguities in any document
- Missing definitions or examples
- Terminology conflicts
- Questions about design intent

Use a clear title that identifies the affected document or resource type.

### Proposing Changes

For small corrections (typos, broken links, minor clarifications):
- Submit a pull request with a brief explanation.

For material changes to data models, workflows, or conformance requirements:
- File an RFC as a GitHub issue with the `RFC` label.
- Follow the RFC template below.

### RFC Template

```
Title: [Short description of proposed change]

Problem: What problem does this RFC solve?

Affected modules: Which OLE modules or resources are affected?

Proposed change: What is the proposed change?

Backward compatibility: Does this break existing implementations?

Privacy and security: Does this affect privacy or security properties?

Migration guidance: How should existing implementations adapt?

Reference implementation impact: How does this affect known implementations?
```

---

## What This Project Does NOT Need

- Contributions that claim OLE certifies AI systems (no certification exists in v0.1)
- Contributions that claim bar approval, court adoption, or government endorsement
- Marketing-style framing or vendor promotion
- Contributions that violate attorney-client privilege or professional responsibility norms
- Personal identifying information about real clients, prospects, or attorneys in examples

---

## Style Guide

### Language

- Use clear, precise, standards-oriented language.
- Avoid marketing language, hype, or claims of adoption.
- Use RFC 2119 / RFC 8174 terms (**MUST**, **SHOULD**, **MAY**) only in all-caps and only where truly normative.
- Do not state that OLE provides legal advice, ethical approval, or compliance certification.

### JSON Examples

- Use fictional but plausible names, identifiers, and jurisdictions.
- Do not use real attorney names, bar numbers, firm names, or client data.
- All examples should be valid JSON. Validate before submitting.
- Include all required envelope fields (`resourceType`, `id`, `oleVersion`, `createdAt`, `visibility`, `extensions`).

### Markdown Documents

- Use `##` for major sections, `###` for subsections.
- Tables are preferred over bullet lists for structured comparisons.
- Include a status line at the top of each document.
- Link to related documents using relative paths.

---

## Attribution

Contributors who submit accepted pull requests or RFCs will be listed in the acknowledgments section of the specification they improved, unless they request otherwise.

By submitting a contribution, you agree that it may be included in OLE specifications under the project's open licensing model:
- Specification text: CC BY 4.0
- Schemas and code: Apache 2.0

A formal Contributor License Agreement process is planned before any working group is formalized.

---

## Contact

- **Website:** openlegalexchange.org
- **Founding steward:** CyVine LLC
- **Governance questions:** File a GitHub issue with the `governance` label.
- **Standards relationship questions:** File a GitHub issue with the `standards` label.
