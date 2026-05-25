# OLE v0.1 Publication Checklist

**Open Legal Exchange — Founding Architecture v0.1**  
**Status:** Pre-publication review checklist  
**Website:** openlegalexchange.org

Use this checklist before making the v0.1 repository public. Mark each item as complete, deferred, or not applicable.

---

## 1. Document Completeness

| # | Item | Status |
|---|------|--------|
| 1.1 | Founding architecture document includes all six primitives, governance loop, adoption levels, module architecture, and technical model. | ✅ |
| 1.2 | Core specification covers envelope, identifiers, external IDs, extension policy, visibility scope, consent record, audit event, error model, idempotency, and webhook signing. | ✅ |
| 1.3 | CLEAR specification covers purpose, non-goals, workflow, status model, resource set, data minimization, consent requirements, field table, participant roles, referral types, API surface, and security requirements. | ✅ |
| 1.4 | Relationship to existing standards covers SALI LMSS, LEDES, Akoma Ntoso, LegalRuleML, NIEM, W3C Verifiable Credentials, and FHIR with mapping details and examples. | ✅ |
| 1.5 | Governance document covers current status, stewardship maturity model, decision-making, conformance caution, change process, and IP policy. | ✅ |
| 1.6 | README is useful to a first-time visitor: explains what OLE is, what CLEAR solves, what is in v0.1, what is not, repo map, and where to start. | ✅ |
| 1.7 | CONTRIBUTING.md is useful to outside contributors: explains how to file issues, RFC template, style guide, attribution model. | ✅ |
| 1.8 | LICENSE-NOTICE.md clearly states CC BY 4.0 for text, Apache 2.0 for schemas and code, marks policy, no-warranty notice. | ✅ |
| 1.9 | ROADMAP.md covers v0.1 scope, v0.2 goals, v1.0 goals, and future modules. | ✅ |
| 1.10 | RFC 0001 is present and describes the minimum referral workflow. | ✅ |

---

## 2. Consistency Checks

| # | Item | Status |
|---|------|--------|
| 2.1 | All documents use `openlegalexchange.org` (not `ole.pro` or any other domain). | ✅ |
| 2.2 | All documents are titled "Founding Architecture v0.1" or "Public Draft" — none claim "v1.0" or "final specification." | ✅ |
| 2.3 | No document claims official certification, formal adoption, bar approval, court approval, or production readiness. | ✅ |
| 2.4 | No example JSON uses `conformanceStatus: "certified"` — all use `"self_declared"` or `"draft"`. | ✅ |
| 2.5 | No AI actor example uses `logReasoningChain` — all use `logInputs`, `logOutputs`, `logDecisionRationale`, `logEvidenceReferences`, `logModelVersion`, `logPolicyVersion`. | ✅ |
| 2.6 | Adoption levels use "Pre-Tier: Packet" and "Tier 1" through "Tier 10" — no unnamed Tier 0 ambiguity. | ✅ |
| 2.7 | Terminology is consistent: "Founding Architecture," "Public Draft," "CLEAR," "resource," "primitive," "adoption level." | ✅ |

---

## 3. Technical Validation

| # | Item | Status |
|---|------|--------|
| 3.1 | All JSON example files are valid JSON (parseable). | ✅ |
| 3.2 | All example JSON files include required envelope fields: `resourceType`, `id`, `oleVersion`, `createdAt`, `visibility`, `extensions`. | ✅ |
| 3.3 | `consent-record.json` includes `visibility` field. | ✅ |
| 3.4 | `actor-ai-agent.json` uses correct audit field names (`logInputs`, `logOutputs`, etc.). | ✅ |
| 3.5 | OpenAPI YAML is valid YAML and parseable by standard tools. | Verify |
| 3.6 | JSON Schema files use `$schema: "https://json-schema.org/draft/2020-12/schema"`. | ✅ |
| 3.7 | JSON Schema `$id` values use `openlegalexchange.org` domain. | ✅ |
| 3.8 | All internal document links resolve within the repository. | Verify |

---

## 4. Safety and Legal Review

| # | Item | Status |
|---|------|--------|
| 4.1 | No real attorney names, bar numbers, client data, or case information in examples. | ✅ |
| 4.2 | All examples use clearly fictional personas (Roxana Tejeda, Maria Santos) and fictional IDs. | ✅ |
| 4.3 | No document provides legal advice, legal ethics guidance, or represents OLE as a compliance tool. | ✅ |
| 4.4 | No document claims CLEAR-compliant referrals satisfy bar ethics rules. | ✅ |
| 4.5 | No document claims OLE certifies AI systems. | ✅ |
| 4.6 | Fee arrangement examples cite rule basis (e.g., FL Bar Rule 4-1.5) without asserting that OLE validates ethics compliance. | ✅ |
| 4.7 | No PII included in any example Impact or Feedback resource. | ✅ |

---

## 5. Missing Items (Deferred to v0.2)

The following items are known gaps that are intentionally deferred to v0.2. They should be documented as such in the relevant specifications.

| # | Item | Deferred To |
|---|------|-------------|
| 5.1 | Final normative JSON Schemas with full field constraints. | v0.2 |
| 5.2 | CLEAR sandbox environment for schema validation. | v0.2 |
| 5.3 | Fee settlement resource and workflow. | v0.2 |
| 5.4 | Document exchange protocol. | v0.2 |
| 5.5 | Conflict-check integration protocol. | v0.2 |
| 5.6 | Engagement record format. | v0.2 |
| 5.7 | ConsentRecord revocation API. | v0.2 |
| 5.8 | Formal Contributor License Agreement. | Before working group |
| 5.9 | Trademark usage policy for "OLE" and "Open Legal Exchange." | v1.0 |
| 5.10 | Conformance certification program. | v1.0 |
| 5.11 | CLEAR automated test suite. | v1.0 |

---

## 6. Publication Steps

| # | Step | Status |
|---|------|--------|
| 6.1 | Final review of all markdown documents for errors and broken links. | Verify |
| 6.2 | Final review of all JSON examples for validity. | Verify |
| 6.3 | Final review of OpenAPI YAML for validity. | Verify |
| 6.4 | Confirm repository license files are in place. | Verify |
| 6.5 | Confirm GitHub repository is set to public (or planned public). | Pending |
| 6.6 | Publish notice on openlegalexchange.org linking to the repository. | Pending |
| 6.7 | Notify standards communities where appropriate (SALI, LEDES, etc.) of the public draft. | Optional |
| 6.8 | File v0.1 tag or release in GitHub. | Pending |
