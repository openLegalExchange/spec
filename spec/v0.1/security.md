# OLE Security Profile v0.1

**Status:** Public Draft for Comment
**Published by:** Open Legal Exchange (openlegalexchange.org)
**Author:** Philippe Chaunu, CyVine LLC
**Date:** May 2026
**Maturity:** Draft — requirements will be finalized before v1.0

---

## Status of This Document

This document defines the security requirements, trust model, and implementation guidance for OLE-conformant systems in v0.1.

v0.1 security requirements are intentionally practical. They establish a workable security baseline without requiring infrastructure that is beyond the reach of small implementation teams. Additional security tiers and formal certification procedures will be defined in future versions.

---

## Normative Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** are to be interpreted as described in RFC 2119 and RFC 8174 when, and only when, they appear in all capitals.

---

## 1. Transport Security

### 1.1 HTTPS Required

All OLE API endpoints MUST be served over HTTPS using TLS 1.2 or higher. Plaintext HTTP MUST NOT be used for any OLE API endpoint in production.

Implementations SHOULD use TLS 1.3 where supported by the deployment environment.

### 1.2 Certificate Validity

Servers MUST present a valid TLS certificate issued by a recognized certificate authority. Self-signed certificates MUST NOT be used in production. Self-signed certificates MAY be used in local development and sandbox environments.

---

## 2. Authentication

### 2.1 Bearer Token Authentication

OLE API endpoints MUST require authentication. Bearer token authentication using JSON Web Tokens (JWT) is the REQUIRED minimum for v0.1.

Tokens MUST be included in the `Authorization` header:

```
Authorization: Bearer <token>
```

Tokens MUST NOT be included in URL query parameters.

### 2.2 OAuth 2.0 for System-to-System

System-to-system OLE exchange SHOULD use OAuth 2.0 with the Client Credentials grant. The OAuth 2.0 authorization server endpoint SHOULD be advertised in the discovery document.

Recommended OAuth 2.0 flows:

| Flow | Use case |
|------|----------|
| Client Credentials | System-to-system API access |
| Authorization Code + PKCE | User-delegated access from client applications |

### 2.3 Token Lifetime

Access tokens MUST have a defined expiry. Access tokens SHOULD NOT have a lifetime exceeding 1 hour for system-to-system flows. Refresh tokens SHOULD be used for long-lived sessions.

### 2.4 Scope Validation

Servers MUST validate that the token's declared scopes authorize the requested operation. Servers MUST return `403 Forbidden` for valid tokens that lack sufficient scope.

---

## 3. Webhook Security

### 3.1 Webhook Signing

Systems that send OLE webhooks MUST sign webhook payloads. The signature is computed as HMAC-SHA256 over the raw request body using a shared secret.

Webhook headers:

| Header | Value |
|--------|-------|
| `OLE-Signature` | `sha256=<hex-encoded-hmac>` |
| `OLE-Timestamp` | Unix timestamp (seconds) at time of dispatch |
| `OLE-Event-Id` | Unique UUID for this event delivery |
| `OLE-Event-Type` | Event type, e.g., `referral.sent` |

### 3.2 Timestamp Validation

Receiving systems MUST validate the `OLE-Timestamp` header. Events with a timestamp older than 300 seconds (5 minutes) MUST be rejected to prevent replay attacks. Receiving systems MUST use a synchronized clock.

### 3.3 Signature Verification

Receiving systems MUST verify the `OLE-Signature` header before processing any webhook payload. Payloads that fail signature verification MUST be rejected with `400 Bad Request`. The shared secret MUST be stored securely and rotated periodically.

### 3.4 Idempotency

Receiving systems MUST handle duplicate webhook deliveries idempotently. The `OLE-Event-Id` header SHOULD be used to detect and deduplicate duplicate deliveries.

---

## 4. Data Protection

### 4.1 Encryption at Rest

Any OLE resource containing personal data (including prospect information, client names, matter details, and contact information) MUST be encrypted at rest. AES-256 is REQUIRED for production deployments.

### 4.2 Time-Limited Document URLs

Document references in OLE resources MUST NOT expose permanent public URLs to private documents. Document access MUST be provided through time-limited signed URLs. The default maximum URL lifetime is 24 hours. Shorter lifetimes are RECOMMENDED for highly sensitive documents.

### 4.3 Minimum Data

OLE implementations MUST apply data minimization. Resources MUST NOT include personal data fields that are not required for the specific exchange. Prospect and client PII fields are OPTIONAL and SHOULD only be populated when necessary.

### 4.4 Data Residency

Systems handling OLE resources containing personal data SHOULD document the geographic region where data is stored. Jurisdiction-specific data residency requirements (e.g., GDPR, CCPA) are the responsibility of the implementing system.

---

## 5. Consent

### 5.1 ConsentRecord Required

Any OLE exchange that includes personal data about a prospect or client MUST be accompanied by a valid `ConsentRecord`. The `ConsentRecord` MUST be created before personal data is included in a `Referral` or related resource.

See [core.md](core.md) for the `ConsentRecord` resource definition.

### 5.2 Consent Scope

The `ConsentRecord` MUST declare the specific scope of data sharing authorized. Implementations MUST NOT share data beyond the declared consent scope.

### 5.3 Consent Revocation

Implementations MUST support consent revocation. When a `ConsentRecord` is revoked, data sharing that depends on that consent MUST cease. Systems SHOULD propagate revocation notifications to all participants who received data under that consent.

---

## 6. Audit Requirements

### 6.1 AuditEvent for Consequential Actions

Every consequential OLE action — referral creation, status update, consent revocation, document access, and authentication failure — MUST produce an `AuditEvent`. See [core.md](core.md) for the `AuditEvent` resource definition.

### 6.2 AI Agent Audit Fields

When an AI agent performs or assists an OLE action, the `AuditEvent` MUST include the `aiAudit` sub-object with the following fields:

| Field | Required | Description |
|-------|----------|-------------|
| `logInputs` | **MUST** | Summary of inputs processed |
| `logOutputs` | **MUST** | Summary of outputs produced |
| `logDecisionRationale` | **MUST** | Explanation of why the decision or action was taken |
| `logEvidenceReferences` | RECOMMENDED | References to evidence or sources used |
| `logModelVersion` | **MUST** | Model or system version |
| `logPolicyVersion` | RECOMMENDED | Policy or constraint set version applied |

### 6.3 Audit Retention

AuditEvent records MUST be retained for the period required by applicable law and professional rules. Where no specific retention period is mandated, a minimum of 7 years is RECOMMENDED for legal sector deployments.

### 6.4 Audit Integrity

AuditEvent records SHOULD NOT be mutable after creation. Implementations SHOULD use hash chaining to ensure tamper-evident audit trails (each event references the hash of the previous event).

---

## 7. API Security

### 7.1 Idempotency Keys

All mutation endpoints (POST, PUT, PATCH) SHOULD support idempotency keys via the `Idempotency-Key` request header. Servers SHOULD store the result of idempotent operations and return the same response for duplicate requests with the same key.

### 7.2 Rate Limiting

OLE API endpoints MUST implement rate limiting. Servers MUST return `429 Too Many Requests` when a client exceeds the allowed request rate. Rate limit headers (`Retry-After`, `X-RateLimit-Limit`, `X-RateLimit-Remaining`) SHOULD be included in responses.

### 7.3 Input Validation

Servers MUST validate all incoming OLE resource payloads against the declared JSON Schema before processing. Servers MUST return `400 Bad Request` or `422 Unprocessable Entity` for payloads that fail schema validation.

### 7.4 Error Response Sanitization

Error responses MUST NOT include internal stack traces, database query details, or file system paths. Error responses SHOULD use the OLE error model defined in [core.md](core.md).

---

## 8. Trust Anchoring (Optional Enhancements)

The following mechanisms are OPTIONAL for v0.1 conformance but RECOMMENDED for high-assurance deployments:

| Mechanism | Purpose | Specification |
|-----------|---------|---------------|
| W3C Verifiable Credentials | Portable, cryptographically signed actor credentials | W3C VC Data Model 2.0 |
| Hash chains | Tamper-evident audit event chains | OLE Core §AuditEvent |
| Signed timestamps | Trusted time anchoring | RFC 3161 |
| Public blockchain anchoring | Highest-assurance attestations | Implementation-defined |

No specific blockchain technology is required or recommended. Implementations that use blockchain for trust anchoring MUST ensure that personal data is never written to a public blockchain.

---

## 9. Security Profile Summary

| Requirement | Level | Applies To |
|-------------|-------|-----------|
| HTTPS / TLS 1.2+ | MUST | All production endpoints |
| JWT bearer token auth | MUST | All API endpoints |
| OAuth 2.0 Client Credentials | SHOULD | System-to-system exchange |
| Webhook HMAC-SHA256 signing | MUST | All webhook senders |
| Timestamp validation (5 min window) | MUST | All webhook receivers |
| AES-256 encryption at rest | MUST | All PII |
| Time-limited document URLs (max 24h) | MUST | All document references |
| ConsentRecord before personal data | MUST | All personal data exchanges |
| AuditEvent for consequential actions | MUST | All implementations |
| Rate limiting | MUST | All API endpoints |
| Input validation against JSON Schema | MUST | All API endpoints |
| Idempotency keys | SHOULD | All mutation endpoints |
| W3C Verifiable Credentials | OPTIONAL | High-assurance identity |
| Hash chain audit integrity | OPTIONAL | High-assurance audit |

---

## Relationship to Other Documents

- [core.md](core.md) — defines `ConsentRecord` and `AuditEvent` resources
- [conformance.md](conformance.md) — conformance profiles and self-declaration requirements
- [governance.md](governance.md) — security profile change process and versioning

---

*Open Legal Exchange — openlegalexchange.org*
*Published under CC BY 4.0 (specification text)*
