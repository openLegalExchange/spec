# OLE — The Open Legal Exchange Protocol

## Founding Architecture v0.1

**Status:** v0.1 — Published
**Published by:** Open Legal Exchange (openlegalexchange.org)
**Author:** Philippe Chaunu, CyVine LLC
**Date:** May 2026
**Maturity:** v0.1 founding architecture — production schemas in future versions
**Primary implementation path:** CLEAR — Legal Referral Exchange
**License:** Specification text: CC BY 4.0. Schemas and code: Apache 2.0.

---

## Status of This Document

This document is the founding architecture and v0.1 publication of the Open Legal Exchange Protocol (OLE).

It defines the purpose of OLE, the six governance primitives, the modular standard family, the adoption model, the trust architecture, and the first implementation path through CLEAR, the Legal Referral Exchange module.

v0.1 defines the conceptual architecture, governance primitives, module design, adoption tiers, and core conventions. Final normative JSON Schemas, OpenAPI definitions, production endpoint requirements, conformance tests, certification procedures, and governance voting procedures are scoped to future versions and will be published as the standard matures.

The goal of v0.1 is to establish a stable conceptual foundation, make the architecture public, and support early implementation work.

---

## Abstract

Open Legal Exchange (OLE) is an open protocol for describing, connecting, and exchanging legal and governance data across systems.

OLE begins with a practical problem: referrals between attorneys, firms, bar associations, and legal service providers are unstructured, hard to track, and disconnected from the rules and consent obligations that govern them.

The first OLE module, CLEAR, defines a structured exchange model for legal referrals.

The broader OLE architecture extends beyond referrals. It models governance as a loop connecting six primitives: Rule, Institution, Actor, Action, Impact, and Feedback. These primitives allow systems to connect legal authority, institutional responsibility, professional identity, operational events, aggregated impact, and structured feedback to rulemaking bodies.

OLE is not a single platform. It is a shared language and exchange model that existing platforms can implement incrementally.

---

## Founding Thesis

> Law is the operating system of every government. OLE is the API.

Three foundational observations:

**1. Everything is law.** A building permit is a legal authorization. A medical license is a legal credential. A freight carrier's operating authority is a legal instrument. A condo association's bylaws are legal rules. There is no governed activity that does not derive its authority from law.

**2. The governance loop is universal.** Every governance system in history has the same six components: rules that authorize institutions, institutions that credential actors, actors that take actions, actions that create impact, and impact that should (but rarely does) feed back into rulemaking.

**3. Data flows down but not up.** Statutes get published. Regulations get codified. But the real-world impact of those rules stays trapped in individual case files, intake notes, and informal channels. The people writing the rules almost never hear back from the people living under them.

OLE is the infrastructure for structured, interoperable, privacy-preserving data flow through the entire governance chain — down, across, and up.

---

## Non-Goals of v0.1

OLE v0.1 does not attempt to:

- Replace existing legal, civic, healthcare, identity, billing, or document standards
- Define final production schemas for every module
- Create a centralized legal data repository
- Require participants to use one platform
- Certify vendors or AI systems
- Define binding legal advice, legal ethics rules, or unauthorized-practice-of-law determinations
- Replace attorney judgment, client consent, conflict checks, or professional responsibility obligations
- Publish individual-level civic impact data
- Require blockchain or any single trust anchor

---

## Normative Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

Because this v0.1 document is a founding architecture, most language is intentionally non-final. Future Core and module specifications will define machine-testable normative requirements.

---

## Architectural Principles

1. **Start with the smallest useful exchange.** The first OLE use case is not a national governance platform. It is a legal referral between two professionals. A small referral packet should be useful even if no other OLE module exists.

2. **Build upward without breaking downward.** Fields and workflows introduced for CLEAR should be compatible with the broader OLE primitives. A referral is an Action. A lawyer is an Actor. A law firm is an Institution.

3. **Federate by default.** OLE should not depend on one central database, one vendor, or one public authority. Each participating system hosts its own OLE endpoints.

4. **Preserve professional judgment.** OLE structures exchange. It does not replace attorney judgment, client consent, conflict checks, court rules, ethics rules, or jurisdiction-specific legal obligations.

5. **Minimize data movement.** OLE resources should contain the minimum data needed for the exchange. Sensitive documents should be referenced through controlled links rather than copied unnecessarily.

6. **Make impact visible without exposing people.** Impact data is aggregated, thresholded, and privacy-preserving.

7. **Allow local extension without fragmenting the core.** Implementers may add namespaced extension fields but MUST NOT redefine core meanings.

---

## Part I: The Six Primitives

Every governance system, at any scale, is composed of six universal primitives. These are the atomic building blocks of OLE.

### 1. Rule

An authoritative directive that creates, constrains, or authorizes governance activity.

Rules exist at every scale — constitutional, statutory, regulatory, professional, and contractual. Rules form a hierarchy: a regulation cannot contradict its authorizing statute; a statute cannot contradict the constitution; a bylaw cannot contradict the law.

```json
{
  "resourceType": "Rule",
  "id": "ole_rule_fl_bar_4_1_5",
  "oleVersion": "0.1",
  "ruleType": "professional_rule",
  "jurisdiction": { "country": "US", "state": "FL" },
  "identifier": "Rule 4-1.5",
  "title": "Florida Bar Rule 4-1.5 — Fees and Costs for Legal Services",
  "status": "active",
  "visibility": { "scope": "public" },
  "extensions": {}
}
```

### 2. Institution

A body authorized by one or more rules to perform governance functions.

Institutions include law firms, bar associations, courts, government agencies, and HOA boards. Institutions credential actors, authorize actions, and are accountable for the impact of their operations.

```json
{
  "resourceType": "Institution",
  "id": "ole_inst_fl_bar",
  "oleVersion": "0.1",
  "institutionType": "professional_association",
  "name": "The Florida Bar",
  "jurisdiction": { "country": "US", "state": "FL" },
  "authorizedBy": ["ole_rule_fl_constitution_art5"],
  "visibility": { "scope": "public" },
  "extensions": {}
}
```

### 3. Actor

Any participant — human, organizational, or artificial — operating within a governance system. Actors include attorneys, law firms, clients, and AI agents.

Every Actor should have verifiable credentials where applicable. AI agents are first-class OLE actors with declared capabilities, constraints, and mandatory audit trails.

```json
{
  "resourceType": "Actor",
  "id": "ole_actor_roxana_tejeda",
  "oleVersion": "0.1",
  "actorType": "human_professional",
  "name": "Roxana Tejeda",
  "credentials": [
    { "type": "bar_admission", "jurisdiction": "FL", "number": "FL-123456", "status": "active" }
  ],
  "visibility": { "scope": "institution_visible" },
  "extensions": {}
}
```

### 4. Action

Any event within a governance system: referral, filing, vote, inspection, decision. Every Action links to the actors who performed it, the institution under whose authority it occurred, and the rules that govern it.

```json
{
  "resourceType": "Action",
  "id": "ole_action_referral_001",
  "oleVersion": "0.1",
  "actionType": "referral.sent",
  "module": "clear",
  "actor": "ole_actor_roxana_tejeda",
  "institution": "ole_inst_tejeda_law",
  "timestamp": "2026-05-22T16:45:00-04:00",
  "rulesImplicated": ["ole_rule_fl_bar_4_1_5"],
  "visibility": { "scope": "participant_visible" },
  "extensions": {}
}
```

### 5. Impact

Any real-world effect of governance actions — aggregated, anonymized, and privacy-preserving. Impact data never contains PII. It is always aggregated above a minimum threshold (default: 10 records) to prevent re-identification.

```json
{
  "resourceType": "Impact",
  "id": "ole_impact_hoa_miamidade_2026q1",
  "oleVersion": "0.1",
  "impactType": "referral_volume_change",
  "jurisdiction": { "country": "US", "state": "FL", "county": "Miami-Dade" },
  "timePeriod": { "start": "2026-01-01", "end": "2026-03-31" },
  "signal": { "metric": "referral_count", "currentPeriod": 248, "priorPeriod": 177, "percentChange": 40.1 },
  "dataProtection": { "method": "aggregated_no_pii", "minimumAggregation": 10, "sourceActorCount": 89 },
  "visibility": { "scope": "public" },
  "extensions": {}
}
```

### 6. Feedback

A structured signal flowing from impact data back to rulemaking. Feedback closes the governance loop. It connects ground-truth impact data to the specific rules, institutions, and actors that can change things.

```json
{
  "resourceType": "Feedback",
  "id": "ole_feedback_fs718_2026",
  "oleVersion": "0.1",
  "feedbackType": "rule_effectiveness_signal",
  "targetRule": "ole_rule_fs718_112",
  "targetInstitution": "ole_inst_fl_legislature",
  "basedOnImpact": ["ole_impact_hoa_miamidade_2026q1"],
  "signal": {
    "summary": "F.S. 718.112 notice requirements are implicated in the majority of HOA disputes across South Florida. Referral volume increased 40% YoY in Miami-Dade County.",
    "evidenceStrength": "moderate",
    "dataPoints": 248
  },
  "dataProtection": { "method": "aggregated_no_pii", "minimumAggregation": 10 },
  "visibility": { "scope": "public" },
  "extensions": {}
}
```

---

## Part II: The Governance Loop

The six primitives form a cycle:

```
  RULE ──creates──▶ INSTITUTION ──credentials──▶ ACTOR
    ▲                                               │
    │                                               │ performs
    │ (amended)                                     ▼
  FEEDBACK ◀── IMPACT ◀──────────────────────── ACTION
```

This loop operates at every scale simultaneously — from a single attorney referral (nano), to a bar association's referral patterns (meso), to statewide legislative evidence (macro).

---

## Part III: Adoption Levels

OLE is designed for incremental adoption. Each level builds on the ones below it but is independently useful.

| Level | Name | Description | Minimum Effort |
|-------|------|-------------|----------------|
| Pre-Tier | Packet | Read/write OLE JSON files | Hours |
| Tier 1 | Exchange | Send/receive via RESTful API | 2–4 dev days |
| Tier 2 | Identity | Verified actors and institutions with credential-backed identities | 1–2 weeks |
| Tier 3 | Platform | Vendor conformance declaration and certified interoperability | Weeks–months |
| Tier 4 | Rules | Link actions to authoritative rule sources | 1–2 weeks per jurisdiction |
| Tier 5 | Impact | Aggregate, anonymized impact signals | Significant |
| Tier 6 | Citizen | Individual-facing transparency and participation | Significant |
| Tier 7 | Transparency | Public lobbying, influence, and accountability data | Significant |
| Tier 8 | Feedback | Structured signals from impact back to rulemaking | Requires Tier 4+5 |
| Tier 9 | Automation | AI agent participation with identity, audit, and oversight | 1–2 weeks + ongoing |
| Tier 10 | Interoperability | Cross-jurisdictional and cross-system exchange | Years |

v0.1 defines conformance profiles for **Pre-Tier (Packet)** and **Tier 1 (Exchange)** only. Higher tiers are described to establish the architectural vision. Their conformance requirements will be defined in future specifications.

---

## Part IV: The Module Architecture

OLE is a modular standard family. Each module shares the core primitives, identity layer, audit trail, and trust infrastructure.

| Module | Namespace | Domain | Status |
|--------|-----------|--------|--------|
| Core | `openlegalexchange.org/ns/core` | Shared primitives and conventions | Active |
| CLEAR | `openlegalexchange.org/ns/clear` | Legal referral exchange | Active — first module |
| ID | `openlegalexchange.org/ns/id` | Professional identity and credential verification | Designed |
| CIVIC | `openlegalexchange.org/ns/civic` | Aggregate impact signals for public interest | Designed |
| COMPLY | `openlegalexchange.org/ns/comply` | Compliance documentation, fee agreements | Planned |
| TRANSPARENCY | `openlegalexchange.org/ns/transparency` | Lobbying disclosures, influence, accountability | Future |
| FREIGHT | `openlegalexchange.org/ns/freight` | Carrier credentialing, load exchange, compliance | Future |
| HEALTH | `openlegalexchange.org/ns/health` | Patient referrals, provider credentialing | Future |
| BUILD | `openlegalexchange.org/ns/build` | Permitting, contractor licensing, inspections | Future |
| FINANCE | `openlegalexchange.org/ns/finance` | Public finance, budget, procurement | Future |
| GOVERN | `openlegalexchange.org/ns/govern` | Legislative process, voting records, civic participation | Future |

### CLEAR — Legal Referral Exchange (Active)

CLEAR is the first OLE module and the primary implementation path in v0.1.

**Resources:** `Referral`, `ReferralParticipant`, `Prospect`, `MatterSummary`, `StatusUpdate`, `FeeArrangement`, `DocumentReference`

**Reference implementation:** CertusGate by CyVine LLC

**Free tier:** CLEAR Referral Tracker — free lifetime accounts for verified attorneys

See [clear.md](clear.md) for the full CLEAR specification.

---

## Part V: Core Technical Model

See [core.md](core.md) for the full Core specification. Key conventions:

### Resource Envelope

Every OLE resource MUST use a common envelope providing minimum metadata for identification, routing, audit, validation, and extension. Required fields: `resourceType`, `id`, `oleVersion`, `createdAt`, `visibility`, `extensions`.

### Identifiers

OLE identifiers are strings assigned by the issuing system. Recommended format: `ole_{resourceType}_{YYYY}_{MMDD}_{sequence}`. Identifiers MUST NOT be reassigned. Collision avoidance is the issuing system's responsibility.

### Visibility Scope

Every OLE resource carries a `visibility` declaration. Defined values: `public`, `institution_visible`, `participant_visible`, `team_visible`, `private`, `system_only`.

### Consent

Personal data exchange MUST be accompanied by a `ConsentRecord` with explicit scope, expiry, and revocation support.

### Audit

Every consequential action MUST produce an `AuditEvent`. AI agent actions MUST include structured AI audit fields documenting inputs, outputs, decision rationale, evidence, model version, and policy version.

---

## Part VI: Trust Architecture

### Transport Layer

OLE uses web-native protocols: HTTPS, JSON, REST, OAuth 2.0, and webhooks. No blockchain, no XML, no SOAP, no EDI at the core layer. The implementation bar must be low enough that a solo developer with a web server can participate.

### Trust Anchoring

Critical trust moments — identity verification, credential attestation, document signatures, audit checkpoints — are anchored using tamper-evident mechanisms:

- **W3C Verifiable Credentials** for portable, cryptographically signed attestations
- **Hash chains** for audit event integrity
- **Signed timestamps** from trusted time sources
- **Optional public blockchain anchoring** for highest-assurance attestations (not required for conformance)

### Federated Architecture

OLE is federated, not centralized. Each system hosts its own OLE endpoints. There is no central server, no central database, no single point of failure. Discovery occurs via endpoint advertisement, directory services, or manual configuration.

### Privacy by Default

- Minimum viable data in every exchange
- PII encrypted at rest (AES-256)
- Document URLs are time-limited (signed URLs, 24-hour expiry default)
- Impact data is always aggregated above minimum thresholds
- Consent is explicit, scoped, and tracked via `ConsentRecord`
- Visibility controls on every resource

---

## Part VII: API and Conformance

See [conformance.md](conformance.md) and [security.md](security.md) for full details.

### Conformance Profiles (v0.1)

| Profile ID | Description |
|-----------|-------------|
| `ole-core-packet-draft-v0.1` | Can read and write valid OLE Core JSON resources |
| `ole-clear-packet-draft-v0.1` | Can read and write valid OLE CLEAR JSON resources |
| `ole-clear-exchange-draft-v0.1` | Implements Tier 1 CLEAR API endpoints |
| `ole-clear-identity-draft-v0.1` | Implements Tier 2 credential verification for CLEAR actors |

All v0.1 conformance profiles carry `draft` status. No certification is available in v0.1.

### Discovery Document

OLE systems SHOULD advertise their capabilities via a discovery document at `GET /.well-known/ole`:

```json
{
  "oleVersion": "0.1",
  "institution": { "id": "ole_inst_example", "name": "Example Legal Services" },
  "baseUrl": "https://api.example.com/ole",
  "supportedModules": ["core", "clear"],
  "conformanceProfiles": ["ole-clear-exchange-draft-v0.1"],
  "webhookSigning": true,
  "contact": { "email": "ole@example.com" }
}
```

---

## Part VIII: Governance

See [governance.md](governance.md) for the full governance specification.

**Steward:** Open Legal Exchange (openlegalexchange.org), initiated by CyVine LLC (Philippe Chaunu, Miami FL).

**Intent:** Transition to multi-stakeholder governance as adoption grows.

**Versioning:** Semantic versioning — Major (breaking changes to core primitives), Minor (new optional fields, new modules), Patch (clarifications, examples).

**License:**
- Specification text: Creative Commons Attribution 4.0 International (CC BY 4.0)
- JSON Schemas, OpenAPI definitions, and reference implementation code: Apache License 2.0
- Open Legal Exchange and OLE marks: reserved by the steward for compatibility and anti-confusion purposes

---

## The First Step

Everything in this document — ten tiers, ten modules, six primitives, the full governance loop from a solo attorney's referral to a planetary governance protocol — begins with one thing:

**Roxana gets an HOA call she can't handle. She refers it to Maria. CLEAR makes that referral structured, trackable, and interoperable.**

That is Tier 1 of one module of OLE. It is the smallest possible implementation of the largest possible vision.

The architecture is designed so that every decision made building CLEAR — every field name, every API endpoint, every schema choice — is consistent with the full OLE vision. Nothing built today needs to be torn apart tomorrow.

Build small. Think universal. Ship the referral.

---

*"The law is the operating system of every government. We're writing the API."*

*Open Legal Exchange — openlegalexchange.org*
*Founded by Philippe Chaunu, CyVine LLC, Miami FL*
*May 2026*
