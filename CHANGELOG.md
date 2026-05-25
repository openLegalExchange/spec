# Changelog

All notable changes to the Open Legal Exchange (OLE) specification will be documented in this file.

Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [0.1.0] — May 2026

### Founding Release — Public Draft for Comment

This is the founding v0.1 release of the Open Legal Exchange Protocol. It is a public draft for review and early implementation. No part of v0.1 constitutes a final, normative specification.

#### Added

**Specifications (`spec/v0.1/`)**
- `founding-architecture.md` — Founding architecture document defining the six governance primitives (Rule, Institution, Actor, Action, Impact, Feedback), the governance loop, ten adoption tiers, ten planned modules, and the full OLE technical model
- `core.md` — Core data conventions: resource envelope, identifier policy, visibility scope, consent model, audit model, external identifiers, and extension policy
- `clear.md` — CLEAR module specification: legal referral exchange workflow, status model, resource set, participant roles, API surface, webhook catalog, and security requirements
- `governance.md` — Stewardship model, decision-making process, contribution workflow, IP policy, and versioning scheme
- `security.md` — Security profile: transport security, authentication (JWT/OAuth 2.0), webhook signing (HMAC-SHA256), data protection, consent requirements, audit requirements, and API security
- `conformance.md` — Conformance profiles for Pre-Tier (Packet) and Tier 1 (Exchange), self-declaration checklist, conformance status values
- `relationship-to-existing-standards.md` — OLE's relationship to SALI LMSS, LEDES, Akoma Ntoso, LegalRuleML, NIEM, W3C Verifiable Credentials, and FHIR with code examples and mapping details
- `publication-checklist.md` — Pre-publication review checklist covering document completeness, consistency, technical validation, safety, and publication steps

**Schemas (`schemas/v0.1/`)**
- `resource-envelope.schema.json` — JSON Schema 2020-12 for the OLE common resource envelope
- `actor.schema.json` — JSON Schema for OLE Actor resources
- `consent-record.schema.json` — JSON Schema for ConsentRecord resources
- `audit-event.schema.json` — JSON Schema for AuditEvent resources (includes AI audit sub-object)
- `referral.schema.json` — JSON Schema for CLEAR Referral resources

**Examples (`examples/v0.1/`)**
- `actor-attorney.json` — Human professional Actor (attorney)
- `actor-ai-agent.json` — AI agent Actor with declared capabilities and constraints
- `consent-record.json` — ConsentRecord for legal referral
- `audit-event.json` — AuditEvent for a referral action
- `referral-sent.json` — Complete CLEAR Referral resource
- `status-update-received.json` — CLEAR StatusUpdate resource

**OpenAPI (`openapi/v0.1/`)**
- `ole-clear-v0.1.yaml` — OpenAPI 3.1.0 draft specification for the CLEAR module API

**Root**
- `README.md` — Repository overview, quick-start guide, and navigation map
- `CONTRIBUTING.md` — Contribution guidelines, RFC template, style guide, and feedback areas
- `LICENSE` — CC BY 4.0 (specification text)
- `LICENSE-NOTICE.md` — Dual-track licensing model (CC BY 4.0 text / Apache 2.0 schemas and code)
- `CHANGELOG.md` — This file
- `ROADMAP.md` — v0.1 scope, v0.2 goals, v1.0 goals, and future module catalog

**Website (`site/`)**
- `index.html` — Marketing homepage for openlegalexchange.org
- `spec/index.html` — Specification landing page
- `clear/index.html` — CLEAR module landing page
- `styles.css` — Shared site stylesheet

#### Known Gaps (Deferred to v0.2)

- Final normative JSON Schemas with full field constraints
- CLEAR sandbox environment for schema validation
- Fee settlement resource and workflow
- Document exchange protocol
- Conflict-check integration protocol
- ConsentRecord revocation API
- Formal Contributor License Agreement
- Machine-testable conformance test suite

---

## Versioning

OLE follows semantic versioning:
- **Major** (1.0, 2.0): Breaking changes to core primitives
- **Minor** (0.1, 0.2): New optional fields, new modules
- **Patch** (0.1.1): Clarifications, typo fixes, examples

All v0.1 releases are public drafts. The first non-draft release will be v1.0.
