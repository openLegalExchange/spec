# Open Legal Exchange (OLE)

**The Open Legal Exchange Protocol — Founding Architecture v0.1, Public Draft for Comment**

> *Law is the operating system of every government. OLE is the API.*

---

## What Is OLE?

OLE is an open protocol for describing, connecting, and exchanging legal and governance data across systems. It provides a shared language for attorneys, institutions, courts, agencies, and AI systems to exchange structured information — without requiring everyone to use the same platform.

The first OLE module, **CLEAR**, defines a structured exchange model for legal referrals.

**openlegalexchange.org**

---

## v0.1 Status

This is a **public draft for comment**. v0.1 establishes the founding architecture, the core data model, and the CLEAR referral module. It is intended for review, early implementation experiments, and community feedback — not for production certification.

No conformance certification is available in v0.1.

---

## Quick Start

| If you want to... | Start here |
|-------------------|-----------|
| Understand the vision | [spec/v0.1/founding-architecture.md](spec/v0.1/founding-architecture.md) |
| Implement referral exchange | [spec/v0.1/clear.md](spec/v0.1/clear.md) |
| Use the data model | [spec/v0.1/core.md](spec/v0.1/core.md) |
| Check security requirements | [spec/v0.1/security.md](spec/v0.1/security.md) |
| See example JSON | [examples/v0.1/](examples/v0.1/) |
| Use the API spec | [openapi/v0.1/ole-clear-v0.1.yaml](openapi/v0.1/ole-clear-v0.1.yaml) |
| Validate your implementation | [spec/v0.1/conformance.md](spec/v0.1/conformance.md) |
| Give feedback | [CONTRIBUTING.md](CONTRIBUTING.md) |

---

## The Six Primitives

OLE models governance as a loop connecting six universal primitives:

| Primitive | Description |
|-----------|-------------|
| **Rule** | An authoritative directive — statute, regulation, professional rule, bylaw, contract |
| **Institution** | A body authorized by rules to perform governance functions |
| **Actor** | Any participant — human, organizational, or artificial |
| **Action** | Any event — referral, filing, vote, inspection, decision |
| **Impact** | Aggregated, anonymized, privacy-preserving signals from actions |
| **Feedback** | Structured signals flowing from impact back to rulemaking |

---

## Repository Structure

```
spec/v0.1/                     Specification documents
  founding-architecture.md     Vision, primitives, modules, trust model
  core.md                      Core data conventions (all modules depend on this)
  clear.md                     CLEAR module — legal referral exchange
  governance.md                Stewardship, contribution, versioning
  security.md                  Security profile — auth, signing, data protection
  conformance.md               Conformance profiles and self-declaration checklist
  relationship-to-existing-standards.md   Relationship to SALI, LEDES, NIEM, VC, FHIR
  publication-checklist.md     Pre-publication review checklist

schemas/v0.1/                  JSON Schema 2020-12 definitions
  resource-envelope.schema.json
  actor.schema.json
  consent-record.schema.json
  audit-event.schema.json
  referral.schema.json

examples/v0.1/                 Valid OLE JSON example documents
  actor-attorney.json
  actor-ai-agent.json
  consent-record.json
  audit-event.json
  referral-sent.json
  status-update-received.json

openapi/v0.1/                  OpenAPI 3.1.0 draft API specification
  ole-clear-v0.1.yaml

site/                          Static website for openlegalexchange.org
  index.html
  spec/index.html
  clear/index.html
  styles.css

old/                           Previous working files (not part of the release)

README.md                      This file
CONTRIBUTING.md                Contribution guidelines and RFC process
LICENSE                        CC BY 4.0 (specification text)
LICENSE-NOTICE.md              Dual-track licensing model
CHANGELOG.md                   Version history
ROADMAP.md                     v0.2 and v1.0 plans
```

---

## What Is and Is Not in v0.1

### In v0.1

- Founding architecture document (primitives, loop, tiers, modules, trust model)
- Core data conventions (envelope, identifiers, visibility, consent, audit, extensions)
- CLEAR module specification (referral workflow, status model, API surface, webhook catalog)
- Draft JSON Schemas for core and CLEAR resources
- Draft OpenAPI 3.1.0 specification for CLEAR endpoints
- Example JSON documents
- Governance, security, conformance, and relationship-to-standards documents

### Not in v0.1

- Final normative schemas
- Certification or conformance testing
- Fee settlement workflow
- Document exchange protocol
- Conflict-check integration
- ConsentRecord revocation API
- CLEAR sandbox environment
- Any claim of bar, court, or government adoption

---

## License

**Specification text:** [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE)

**Schemas and code:** [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)

See [LICENSE-NOTICE.md](LICENSE-NOTICE.md) for the full dual-track licensing model and marks policy.

---

## Stewardship

Open Legal Exchange (openlegalexchange.org) is the initial steward, founded by Philippe Chaunu, CyVine LLC, Miami FL.

CertusGate by CyVine LLC is the first reference implementation.

The CLEAR Referral Tracker provides free lifetime accounts for verified attorneys.

---

*Public Draft for Comment — May 2026*
