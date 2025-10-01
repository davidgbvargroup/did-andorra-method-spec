# The `did:andorra` Method Specification

**Status**: Implementer’s Draft / 0.1.1  
**This document defines the `did:andorra` DID Method**, conforming to W3C DID Core and DID Resolution.

## 1. Introduction
The `did:andorra` method identifies subjects in the **Verifiable Data Registry (VDR)** of Andorra. 
It acts as a **DID-compliant wrapper** over the official identifiers assigned by the 
**Andorra National Registry of Legal Entities (NRTAD, Número de Registre Tributari d’Andorra)**, 
which is the authoritative source for company registration in Andorra.

Resolution is **read-only** : DIDs cannot be arbitrarily created or updated by external parties. Only entities that already hold a valid NRTAD identifier may obtain a `did:andorra` . Each DID deterministically retrieves a corresponding DID Document linked to that official identifier.

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
The **Create**, **Update**, and **Deactivate** operations are **not supported** by external parties.

**Resolve**: Given a `did:andorra:NRTAD-<6digits><letter>[_ISS|_SP]`, the resolver queries the Andorra VDR and returns the corresponding **DID Document**.  
The returned DID Document contains:
- `@context` (`https://www.w3.org/ns/did/v1`)  
- `id` (the resolved DID)  
- `verificationMethod` (the subject’s public keys)  
- `assertionMethod` (references to verification methods)  
- `service` (service endpoints)

**Population of the VDR**: The VDR is automatically populated from the Andorra National Registry (NRTAD). Only legal entities with a valid identifier can request a DID.

**Governance**: Issuance of DIDs is authorized by the Government of Andorra through OSCEPA (Oficina de Servicios de Certificación y Protección Andorrana).

## 5. Method-specific Identifiers
The `<unique-identifier>` is assigned by the **Andorra National Registry (NRTAD)**.
Rules:
- **Allowed charset**: digits `[0–9]`, uppercase letters `[A–Z]`, hyphen `-`, underscore `_`.  
- **Length**: 12 characters (`NRTAD-######X`) or up to 16 characters with suffix (`_ISS` / `_SP`).  
- **Suffix meaning**:
    - **(no suffix)** -> default identifier of a legal entity in the NRTAD.
    - **_ISS** -> the entity acts as an Issuer of verifiable credentials.
    - **_SP** -> the entity acts as a Service Provider.
- **Collision management**: uniqueness is guaranteed by the NRTAD at the time of assignment.
- **Examples**:
- `did:andorra:NRTAD-710646J`  
- `did:andorra:NRTAD-059888N_ISS`  
- `did:andorra:NRTAD-059888N_SP`

## 6. DID Resolution
The `did:andorra` method currently supports resolution via a dedicated API:

**Reference endpoint**:  
`GET https://definitiveid.wsg127.com/definitiveid_services/rest/Public/Identity/{did}`

**Example**: 
https://definitiveid.wsg127.com/definitiveid_services/rest/Public/Identity/did%3Aandorra%3ANRTAD-059888N_SP

**Response**: a valid DID Document (conforming to [DID Core](https://www.w3.org/TR/did-core/)).
**Resolution error examples**:
- `400 Bad Request`: Unsupported DID method.
- `404 notFound`: Identity DID not found in registry.  
---
**Note**: A Universal Resolver driver for `did:andorra` is planned; once integrated, resolution through the [Universal Resolver interface](https://github.com/decentralized-identity/universal-resolver) will also be supported.
## 7. DID Document
The returned DID Document complies with DID Core:
- `@context` → always first (`https://www.w3.org/ns/did/v1`)  
- `id` → the root DID being resolved  
- `verificationMethod` → the supported public keys (e.g., `JsonWebKey2020`)
- `assertionMethod` → references to keys valid for assertions  
- `service` → service endpoints (e.g., `DIDResolutionService`)
## 8. Governance
- **Authority** :  Government of Andorra (owner and controller of the NRTAD registry). 
- **Maintainer** : Iberian Unit – Var Group (appointed technology provider for the Andorra Digital Identity and Wallet, operating under mandate of the Government of Andorra).  
- **Process to obtain a DID** : 
    - The legal entity must already have an NRTAD identifier.
    - It requests a DID via OSCEPA.
    - A digital seal certificate is issued to the company.
    - The DID Document published includes the corresponding public key from that certificate.
- **Contact** : david.garciab@vargroupiberia.com
---
**Note**: **Not all entities have a DID** but all DIDs are strictly correlated with an existing national identifier.
## 9. Privacy & Security Considerations
- No personal data beyond organizational identifiers and cryptographic keys is exposed.
- End-to-end TLS for API access.
- Rate limiting and anti-DoS protections.
## 10. Lifecycle 
- Versioning: Semantic Versioning for this spec. Breaking changes require new version and prior notice.  
- Deprecation/Deactivation: If a DID becomes invalid, the backend returns notFound.  
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