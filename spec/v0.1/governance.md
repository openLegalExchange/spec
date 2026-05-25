# OLE Governance Draft

**Status:** Draft  
**Published by:** Open Legal Exchange (openlegalexchange.org)  
**Date:** May 2026

---

## Status of This Document

This document describes the current governance structure of OLE and the intended path toward broader multi-stakeholder governance. It is a draft. Governance arrangements will evolve as adoption develops.

---

## Current Governance Status

OLE is currently a founder-stewarded public draft published by Open Legal Exchange and initiated by CyVine LLC.

No formal standards body, consortium, trade association, government agency, bar association, court, or regulatory authority has adopted or endorsed OLE at this stage. This document does not claim otherwise.

The current governance model is intentional for a founding draft: a small, accountable stewardship keeps the architecture coherent while external input is being solicited.

---

## Stewardship Maturity Model

The intended governance path is as follows:

| Stage | Description | Status |
|-------|-------------|--------|
| 1 | **Founder-stewarded draft publication** — Architecture and first module published publicly for comment. | **Current** |
| 2 | **Public issue and RFC process** — Open GitHub issue tracker and RFC process for external proposals. | **Current (open)** |
| 3 | **Advisory working group for CLEAR** — Invite practitioners, technologists, and standards experts to an informal CLEAR working group. | Planned |
| 4 | **Multi-stakeholder technical steering group** — Form a technical steering committee with representation from implementers, legal professionals, and standards community. | Future |
| 5 | **Independent nonprofit or standards-body governance** — If adoption warrants it, transition to independent nonprofit stewardship or submission to a recognized standards body (e.g., OASIS, W3C, or similar). | Long-term |

No timeline is committed for stages 3–5. Progression depends on adoption, community engagement, and available resources.

---

## Decision-Making in v0.1

During Stage 1, the founding editor (Philippe Chaunu, CyVine LLC) retains editorial control over the OLE specifications.

This means:

- RFC proposals are welcome and will be publicly tracked.
- Material changes will be published as RFCs with a public comment period.
- Editorial decisions will be explained and documented.
- Contributors retain attribution for their contributions.

---

## Draft Conformance Caution

Until a formal validation process exists, public materials MUST clearly distinguish between:

| Term | Meaning |
|------|---------|
| `draft` | A proposed packet format or workflow that has not been validated by any external process. |
| `self_declared` | An implementer's own assertion that they have implemented the specification as they understand it. No third-party review. |
| `sandbox_validated` | A system that has been tested against an OLE sandbox environment. (Planned — not yet available.) |
| `third_party_validated` | A system that has been reviewed by an independent party. (Future.) |
| `certified` | A system that has passed a formal certification process. (Not available in v0.1.) |

In v0.1, implementers SHOULD use `draft` or `self_declared` as their `conformanceStatus` value. Any use of `certified` would be unsupported and misleading.

---

## Change Process

Material changes to OLE SHOULD be proposed as OLE Requests for Comment (RFCs).

Each RFC should include:

- **Problem statement** — What problem does this RFC solve?
- **Affected modules** — Which OLE modules or resources are affected?
- **Proposed change** — What is the proposed change, in sufficient detail?
- **Backward compatibility analysis** — Does this break existing implementations?
- **Privacy and security analysis** — Does this affect privacy or security properties?
- **Migration guidance** — How should existing implementations migrate?
- **Reference implementation impact** — How does this affect known implementations?

RFCs are filed as GitHub issues with the `RFC` label and a structured template.

---

## Intellectual Property Policy (Draft)

OLE v0.1 is being published under an open licensing model:

- **Specification text:** Creative Commons Attribution 4.0 International (CC BY 4.0)
- **JSON Schemas, OpenAPI definitions, and reference code:** Apache License 2.0
- **OLE and Open Legal Exchange marks:** Reserved. Use of the marks in conformance claims requires a future trademark policy (to be published).

Contributors who submit pull requests or RFCs are assumed to agree to contribute their submissions under the same open licenses. A formal Contributor License Agreement (CLA) process is planned before any working group is formalized.

This IP policy is a draft. It will be reviewed before any formal governance body is established.

---

## Contact

- **Website:** openlegalexchange.org  
- **Founding steward:** CyVine LLC  
- **GitHub:** Submit issues and RFCs through the OLE public repository.  

Questions about governance, licensing, or participation should be submitted as GitHub issues.
