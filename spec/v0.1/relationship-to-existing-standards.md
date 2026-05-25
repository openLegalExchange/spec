# Relationship to Existing Standards

**Status:** v0.1 — Published  
**Published by:** Open Legal Exchange (openlegalexchange.org)  
**Date:** May 2026

---

## Position Statement

OLE is not intended to replace existing legal, governmental, healthcare, identity, billing, or document standards.

Where strong domain standards already exist, OLE should reuse, reference, or map to them instead of duplicating them. OLE's purpose is to provide the governance loop — the connective tissue between authoritative rules, authorized institutions, credentialed actors, operational actions, aggregated impact, and structured feedback — that existing domain standards do not individually provide.

OLE does not claim to supersede, certify against, or replace any of the standards listed below. OLE implementers are encouraged to treat existing standards as trusted components within the OLE architecture.

---

## Standards Map

### SALI LMSS (Legal Matter Standard Specification)

| Attribute | Detail |
|-----------|--------|
| Maintained by | SALI Alliance |
| Purpose | Standardized classification of legal matters, practice areas, legal services, roles, and documents. |
| OLE relationship | OLE MAY use SALI codes to classify legal matters, practice areas, legal roles, and service types in the `MatterSummary` and `practiceArea` fields. SALI codes SHOULD be represented in `externalIds` using `"system": "sali"`. |
| What OLE does not do | OLE does not replicate the SALI taxonomy or maintain a competing classification system. |
| Reference | https://www.salialliance.com/ |

**Example:**

```json
"externalIds": [
  { "system": "sali", "type": "practice_area_code", "value": "SALI:HOA-LAW" }
]
```

---

### LEDES (Legal Electronic Data Exchange Standards)

| Attribute | Detail |
|-----------|--------|
| Maintained by | LEDES Oversight Board |
| Purpose | Standardized legal billing, invoice, and matter data exchange between law firms and corporate clients. |
| OLE relationship | OLE should not replace legal billing standards. Fee-related OLE resources (e.g., `FeeArrangement`, `FeeSettlement`) MAY reference LEDES-compatible billing data or matter codes where needed. |
| What OLE does not do | OLE does not define billing formats, invoice structures, or timekeeper records. |
| Reference | https://ledes.org/ |

---

### Akoma Ntoso

| Attribute | Detail |
|-----------|--------|
| Maintained by | OASIS Open |
| Purpose | XML markup standard for legislative, judicial, and parliamentary documents. |
| OLE relationship | OLE's `Rule` primitive MAY reference legislative, judicial, and parliamentary documents represented in Akoma Ntoso using a `canonicalUrl` or `externalIds` reference. OLE does not define document markup. |
| What OLE does not do | OLE does not define XML document schemas for legal texts. |
| Reference | https://www.oasis-open.org/committees/akn-ml/ |

---

### LegalRuleML

| Attribute | Detail |
|-----------|--------|
| Maintained by | OASIS Open |
| Purpose | XML-based standard for representing machine-readable legal rules, deontic logic, and normative statements. |
| OLE relationship | OLE's `Rule` primitive MAY reference machine-readable rule expressions encoded in LegalRuleML. OLE does not define deontic rule modeling or normative logic encoding. |
| What OLE does not do | OLE does not replicate the LegalRuleML semantic model. |
| Reference | https://www.oasis-open.org/committees/legalruleml/ |

---

### NIEM (National Information Exchange Model)

| Attribute | Detail |
|-----------|--------|
| Maintained by | NIEM Management Office (U.S.) |
| Purpose | Data model standard for information exchange between U.S. government agencies and justice-sector systems. |
| OLE relationship | OLE MAY map public-sector exchange resources (justice, law enforcement, administrative) to NIEM-compatible models where government information exchange programs require it. OLE's `Institution`, `Actor`, `Action`, and `Rule` primitives are broadly compatible with NIEM domain concepts. |
| What OLE does not do | OLE does not require NIEM compliance. It does not replace NIEM for U.S. government exchanges that already mandate NIEM. |
| Reference | https://www.niem.gov/ |

---

### W3C Verifiable Credentials (VC)

| Attribute | Detail |
|-----------|--------|
| Maintained by | World Wide Web Consortium (W3C) |
| Purpose | Decentralized standard for cryptographically verifiable credentials: educational records, professional licenses, identity attestations. |
| OLE relationship | OLE's `Credential` fields (within `Actor`) MAY be backed by W3C Verifiable Credentials issued by credentialing bodies such as bar associations, licensing boards, or professional certification organizations. OLE's AI agent Actor resources MAY use VCs for portable capability attestations. |
| What OLE does not do | OLE does not define a VC issuance or wallet protocol. It defines how VC references may appear within OLE resources. |
| Reference | https://www.w3.org/TR/vc-data-model/ |

**Example credential with VC reference:**

```json
{
  "credentialType": "bar_admission",
  "issuedBy": "ole_inst_fl_bar",
  "jurisdiction": { "country": "US", "state": "FL" },
  "identifier": "123456",
  "status": "active",
  "verifiedAt": "2026-05-01T00:00:00Z",
  "verificationMethod": "verifiable_credential",
  "verificationSource": "fl_bar_member_database",
  "externalIds": [
    { "system": "w3c_vc", "type": "credential_id", "value": "https://bar.florida.gov/credentials/123456" }
  ]
}
```

---

### HL7 FHIR (Fast Healthcare Interoperability Resources)

| Attribute | Detail |
|-----------|--------|
| Maintained by | HL7 International |
| Purpose | Standard for healthcare data exchange, clinical records, patient data, and provider information. |
| OLE relationship | OLE/HEALTH (a planned future module) should interoperate with FHIR for healthcare data. OLE adds governance-loop context — institutional authority, professional credentialing, impact aggregation, and regulatory feedback — that FHIR does not address. In mixed legal-healthcare contexts (e.g., personal injury referrals, workers' compensation, disability), CLEAR resources MAY include FHIR resource references in `externalIds`. |
| What OLE does not do | OLE does not define clinical data models, patient records, or health data exchange formats. |
| Reference | https://www.hl7.org/fhir/ |

---

## OLE's Distinct Contribution

None of the standards above provides the governance loop:

- SALI classifies matters but does not model the institutional and rule authority behind them.
- LEDES handles billing but not referral workflow, identity, or impact.
- Akoma Ntoso and LegalRuleML represent legal texts and rules but do not model actors, actions, or impact signals.
- NIEM enables government exchange but is sector-specific and not designed for cross-sector governance.
- W3C VC handles credential portability but not the full actor-institution-action model.
- FHIR handles healthcare data exchange but not governance accountability or civic feedback loops.

OLE's contribution is the model that connects all of these domains through shared primitives: a Rule authorizes an Institution, the Institution credentials an Actor, the Actor takes an Action, Actions produce Impact, and Impact generates Feedback that can inform the next Rule.

---

## Feedback on Standards Relationships

If you work on or with any of these standards and see a conflict, opportunity, or improvement in how OLE describes the relationship, please open an issue or RFC. OLE should align with, not compete with, existing standards communities.
