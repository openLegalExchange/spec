# OLE Roadmap

**Open Legal Exchange — Founding Architecture v0.1**  
**Published:** May 2026  
**Website:** openlegalexchange.org

This roadmap describes the intended path from the v0.1 public draft toward a stable v1.0 specification. It is not a commitment or a timeline. Progression depends on community engagement, implementation feedback, and available resources.

---

## v0.1 — Founding Architecture (Current)

**Goal:** Publish a coherent founding architecture and first module that can support early implementation experiments and invite community review.

**Status:** Public draft for comment.

### Included in v0.1

- Founding architecture document (six primitives, governance loop, adoption levels)
- Core specification (envelope, identifiers, visibility, consent, audit, extensions)
- CLEAR module specification (referral workflow, status model, resource set)
- Relationship to existing standards (SALI, LEDES, Akoma Ntoso, LegalRuleML, NIEM, W3C VC, FHIR)
- Governance draft
- Starter JSON Schemas (resource envelope, actor, referral, consent record, audit event)
- Draft OpenAPI specification for CLEAR (with request/response schemas)
- Initial example JSON packets
- CLEAR Tier 1 self-declared conformance checklist
- RFC 0001 — Minimum referral workflow
- Open licensing under CC BY 4.0 (text) and Apache 2.0 (schemas and code)

### Not Included in v0.1

- Final normative JSON Schemas
- Conformance certification program
- CLEAR sandbox or test environment
- Fee settlement workflow
- Document exchange protocol
- Conflict-check integration protocol
- Bar association reporting format
- Multi-module implementations beyond CLEAR

---

## v0.2 — Minimum Implementable Specification

**Goal:** Produce a specification stable enough for a production pilot implementation by at least one participating organization, with machine-testable schemas and a sandbox environment.

### Planned for v0.2

| Item | Description |
|------|-------------|
| Normative JSON Schemas | Final schemas for all CLEAR required resources, validated against example packets |
| CLEAR sandbox | Test environment for validating referral packets against schemas |
| Fee arrangement model | Basic fee-sharing structure and consent linkage |
| Document reference model | Structured reference to external documents (not embedding) |
| Conflict-check stub | Minimal `ConflictCheck` resource structure |
| Engagement record | `EngagementRecord` resource for documenting matter opening |
| Webhook event catalog | Full enumeration of CLEAR webhook event types |
| Extended conformance checklist | Updated checklist against normative schemas |
| Actor credential model | Formal structure for bar admission and credential verification |
| Identifier policy finalization | Scoping rules for cross-system identifier disambiguation |
| OpenAPI refinement | Finalized request/response schemas aligned with normative JSON Schemas |
| First external feedback integration | Incorporate community RFC feedback from v0.1 public comment |

### Governance change expected at v0.2

An informal CLEAR advisory working group may be invited during the v0.2 development cycle.

---

## v1.0 — Stable Specification

**Goal:** Publish a stable, machine-testable, implementation-tested Core and CLEAR specification with a formal conformance program and multi-stakeholder review.

### Planned for v1.0

| Item | Description |
|------|-------------|
| Normative Core specification | Final Core v1.0 with all shared conventions locked |
| Normative CLEAR specification | Final CLEAR v1.0 with all required resources and transitions locked |
| Conformance test suite | Automated tests for schema validation and workflow conformance |
| Conformance certification program | Third-party validation path (program design TBD) |
| Multiple reference implementations | At least two independent implementations demonstrating interoperability |
| Final OpenAPI specification | Normative API surface for CLEAR |
| Bar association reporting extension | Draft extension for referral panel reporting |
| Impact aggregation extension | Draft CIVIC module integration for CLEAR-derived impact signals |
| Formal IP policy | Finalized contributor license agreement and trademark policy |
| Governance body | Multi-stakeholder technical steering committee or equivalent |
| Standards body consideration | Evaluation of submission to OASIS, W3C, or similar body |

### Governance change expected at v1.0

v1.0 should be governed by a multi-stakeholder technical steering group, not a single founder.

---

## Future Modules (Post-v1.0)

The following modules are designed in the founding architecture but not being specified in v0.x or v1.0. They are planned for future development if adoption and resources support it:

| Module | Domain |
|--------|--------|
| OLE/ID | Portable professional identity and credential verification |
| OLE/CIVIC | Aggregate legal and civic impact signals |
| OLE/COMPLY | Fee agreements, compliance documentation |
| OLE/TRANSPARENCY | Lobbying, influence, legislative activity |
| OLE/FREIGHT | Regulated logistics and carrier credentialing |
| OLE/HEALTH | Healthcare referrals and credentialing (FHIR interoperability) |
| OLE/BUILD | Construction permitting and contractor licensing |
| OLE/FINANCE | Financial regulation and compliance |
| OLE/GOVERN | Charter-based governance for new institutions |

The timeline for any of these modules is not committed. Each module requires a community of practitioners, technologists, and domain experts willing to participate in the specification process.

---

## Feedback on This Roadmap

If you have feedback on priorities, sequencing, or scope — or if you want to participate in v0.2 development — file a GitHub issue with the `roadmap` label.
