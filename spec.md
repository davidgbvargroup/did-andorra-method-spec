# The `did:andorra` Method Specification

**Status**: Implementer’s Draft / 0.1.0  
**This document defines the `did:andorra` DID Method**, conforming to W3C DID Core and DID Resolution.

## 1. Introduction
The `did:andorra` method identifies subjects in the **Verifiable Data Registry (VDR)** of Andorra.  
Resolution is **read-only** (no create/update operations), focusing on deterministic retrieval of DID Documents.

## 2. DID Method Name
The registered method is `andorra`.  
A valid DID starts with:
did:andorra:

## 3. DID Syntax
A `did:andorra` DID follows this grammar:

**Human-readable:** 
```
did:andorra:<unique-identifier>
```

**Regex:**
```
^did:andorra:NRTAD-[0-9]{6}[A-Z](?:_(ISS|SP))?$
```

**ABNF:**

```abnf
did-andorra    = "did:andorra:" nrtad-id
nrtad-id       = "NRTAD-" 6DIGIT UPPER [ suffix ]
suffix         = "_" ( "ISS" / "SP" )
DIGIT          = %x30-39
UPPER          = %x41-5A    
``` 

## 4. Method Operations
This method defines **Read/Resolve** only.  
The **Create**, **Update**, and **Deactivate** operations are **not supported**.

**Resolve**: Given a `did:andorra:NRTAD-<6digits><letter>[_ISS|_SP]`, the resolver queries the Andorra VDR and returns the corresponding **DID Document**.  
The returned DID Document contains:
- `@context` (`https://www.w3.org/ns/did/v1`)  
- `id` (the resolved DID)  
- `verificationMethod` (the subject’s public keys)  
- `assertionMethod` (references to verification methods)  
- `service` (service endpoints)
    
## 5. Method-specific Identifiers
The `<unique-identifier>` is assigned by the **Andorra National Registry (NRTAD)**.
Rules:
- **Allowed charset**: digits `[0–9]`, uppercase letters `[A–Z]`, hyphen `-`, underscore `_`.  
- **Length**: 12 characters (`NRTAD-######X`) or up to 16 characters with suffix (`NRTAD-######X_ISS` / `_SP`).  
- **Normalization**: identifiers are uppercase only; no percent-encoding is used.  
- **Collision management**: uniqueness is guaranteed by the issuing authority (the NRTAD registry) at the time of assignment.  
**Examples**:
- `did:andorra:NRTAD-710646J`  
- `did:andorra:NRTAD-059888N_ISS`  
- `did:andorra:NRTAD-059888N_SP`

## 6. DID Resolution
The `did:andorra` method currently supports resolution via a dedicated HTTP API:

**Reference endpoint**:  
`GET https://definitiveid.wsg127.com/definitiveid_services/rest/Public/Identity/{did}`

**Example**: 
https://definitiveid.wsg127.com/definitiveid_services/rest/Public/Identity/did%3Aandorra%3ANRTAD-059888N_SP

**Response**: a valid DID Document (conforming to [DID Core](https://www.w3.org/TR/did-core/)).
**Resolution error examples**:
- `400 Bad Request`: Método DID no soportado.  
- `404 notFound`: Identity did not found (identity.did.notFound).  
---
**Note**: A Universal Resolver driver for `did:andorra` is planned; once integrated, resolution through the [Universal Resolver interface](https://github.com/decentralized-identity/universal-resolver) will also be supported.
## 7. DID Document
The returned `didDocument` must comply with DID Core:
- `@context` → always first (`https://www.w3.org/ns/did/v1`)  
- `id` → the root DID being resolved  
- `verificationMethod` → the supported public keys (e.g., `JsonWebKey2020`)
- `assertionMethod` → references to keys valid for assertions  
- `service` → service endpoints (e.g., `DIDResolutionService`) 
## 8. Privacy Considerations
- Minimize inclusion of personal data in `service` entries.  
- End-to-end TLS between resolver and backend.
## 9. Security Considerations
- Strict validation of the DID identifier (regex/ABNF).  
- TLS and access control for backend communication.   
- Rate limiting and availability protections (DoS mitigation).  
## 10. Governance & Lifecycle
- Authority/Maintainer: IBERIAN UNIT VAR GROUP david.garciab@vargroupiberia.com.  
- Versioning: Semantic Versioning for this spec. Breaking changes require new version and prior notice.  
- Deprecation/Deactivation: If a DID becomes invalid, the backend return notFound.  
- Change Management: issues/PRs are tracked in this repository.
## 11. W3C Registration
The method is to be published in the **DID Method Registry** (W3C DID Extensions).  
Initial status: **provisional**.
## 12. Test Vectors
- Example 1: did:andorra:NRTAD-059888N_ISS → see examples/andorra-1.json  
- Example 2: did:andorra:NRTAD-710646J → see examples/andorra-2.json
- Example 3: did:andorra:NRTAD-059888N_SP → see examples/andorra-3.json
(Include valid DID Documents in the examples/ folder.)
## 13. References
- W3C DID Core: https://www.w3.org/TR/did-core/  
- W3C DID Resolution: https://www.w3.org/TR/did-resolution/  
- W3C DID Implementation Guide: https://www.w3.org/TR/did-imp-guide/  
- Example methods: did:web, did:key, did:ion  