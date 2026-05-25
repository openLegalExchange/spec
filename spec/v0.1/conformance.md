# OLE Conformance Profiles v0.1

**Status:** v0.1 — Published
**Published by:** Open Legal Exchange (openlegalexchange.org)
**Author:** Philippe Chaunu, CyVine LLC
**Date:** May 2026
**Maturity:** v0.1 — self-declaration only; formal certification in future versions

---

## Status of This Document

This document defines the OLE conformance model, conformance profiles, and conformance status values for v0.1.

No formal certification program exists in v0.1. Systems may self-declare conformance against defined profiles using the `self_declared` status value. Third-party validation and formal certification will be defined in a future version.

---

## Normative Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

---

## 1. Conformance Model

OLE conformance is modular. Systems implement one or more conformance profiles. A profile targets a specific module and adoption tier.

A conforming system:
- Satisfies all **MUST** requirements of the applicable profile
- Declares its conformance via the OLE discovery document
- Uses the correct `conformanceStatus` value (see §3)

No claim of OLE conformance may imply certification, third-party validation, legal ethics compliance, or production readiness unless separately and explicitly documented.

---

## 2. Conformance Profiles (v0.1)

### `ole-core-packet-draft-v0.1`

**Tier:** Pre-Tier (Packet)  
**Module:** Core

A system conforming to this profile can read and write valid OLE Core JSON resources. It MUST:

- Produce JSON documents that include all required envelope fields: `resourceType`, `id`, `oleVersion`, `createdAt`, `visibility`, `extensions`
- Produce JSON that parses successfully against the OLE Core resource envelope JSON Schema
- Use version string `"0.1"` in the `oleVersion` field

No API endpoints are required.

---

### `ole-clear-packet-draft-v0.1`

**Tier:** Pre-Tier (Packet)  
**Module:** CLEAR

A system conforming to this profile can read and write valid OLE CLEAR JSON resources. It MUST:

- Satisfy all requirements of `ole-core-packet-draft-v0.1`
- Produce `Referral` resources that parse against `schemas/v0.1/referral.schema.json`
- Produce `ConsentRecord` resources that parse against `schemas/v0.1/consent-record.schema.json`
- Include a linked `ConsentRecord` in any `Referral` that contains prospect personal data

No API endpoints are required.

---

### `ole-clear-exchange-draft-v0.1`

**Tier:** Tier 1 (Exchange)  
**Module:** CLEAR

A system conforming to this profile sends and receives OLE CLEAR resources via RESTful API. It MUST:

- Satisfy all requirements of `ole-clear-packet-draft-v0.1`
- Implement: `POST /referrals`, `GET /referrals/{id}`, `POST /referrals/{id}/status`, `GET /referrals`
- Serve a valid discovery document at `GET /.well-known/ole-discovery.json`
- Use HTTPS (TLS 1.2+) on all endpoints
- Require bearer token authentication on all endpoints except the discovery document
- Support the complete minimum CLEAR referral workflow (all five stages)
- Support all 14 defined CLEAR referral statuses
- Record `AuditEvent` resources for all state transitions

**Detailed self-assessment checklist:** [conformance/clear-tier-1-checklist.md](../../conformance/clear-tier-1-checklist.md)

---

### `ole-clear-identity-draft-v0.1`

**Tier:** Tier 2 (Identity)  
**Module:** CLEAR

A system conforming to this profile verifies actor identity for CLEAR participants. It MUST:

- Satisfy all requirements of `ole-clear-exchange-draft-v0.1`
- Verify bar admission status for attorney actors before including them as `Referral` participants
- Record credential verification events as `AuditEvent` resources
- Declare the verification method in the `Actor` resource

Conformance requirements for this profile will be expanded in v0.2.

---

## 3. Conformance Status Values

| Value | Description |
|-------|-------------|
| `draft` | Implementation in progress. Not yet ready for exchange. |
| `self_declared` | Operator reviewed the applicable checklist and believes all Required items are satisfied. |
| `sandbox_validated` | Validated against an OLE sandbox (not available in v0.1). |
| `third_party_validated` | Reviewed by a third party against the spec (not available in v0.1). |
| `certified` | Reserved. No certification program exists in v0.1. **MUST NOT be used.** |
| `revoked` | Conformance previously declared or validated but since withdrawn. |

In v0.1, use `draft` or `self_declared` only.

---

## 4. Discovery Document Declaration

```json
{
  "oleVersion": "0.1",
  "institution": {
    "id": "ole_inst_example",
    "name": "Example Legal Services"
  },
  "baseUrl": "https://api.example.com/ole",
  "supportedModules": ["core", "clear"],
  "conformanceProfiles": [
    {
      "profile": "ole-clear-exchange-draft-v0.1",
      "conformanceStatus": "self_declared",
      "declaredAt": "2026-05-22"
    }
  ],
  "webhookSigning": true,
  "contact": {
    "technical": "ole@example.com"
  }
}
```

---

## 5. What Conformance Does Not Mean

A v0.1 conformance declaration does not mean the system has been certified, that its workflows satisfy bar ethics rules, that it is production-ready, or that it provides any legal advice. It means the operator believes their implementation satisfies the technical requirements of the applicable profile.

---

## Related Documents

- [core.md](core.md) — Resource envelope, AuditEvent
- [clear.md](clear.md) — CLEAR module specification
- [security.md](security.md) — Security requirements
- [conformance/clear-tier-1-checklist.md](../../conformance/clear-tier-1-checklist.md) — Detailed self-assessment checklist

---

*Open Legal Exchange — openlegalexchange.org*  
*Published under CC BY 4.0 (specification text)*
