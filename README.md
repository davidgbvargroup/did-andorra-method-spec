# did:andorra Method Specification

This repository contains the official specification for the `did:andorra` Decentralized Identifier (DID) Method.

- **Specification file:** [spec.md](./spec.md)  
- **Status:** Provisional (Implementer’s Draft)  
- **Registry entry:** Pending submission to the [W3C DID Method Registry](https://github.com/w3c/did-extensions)  

## About

The `did:andorra` method defines identifiers assigned by the **Andorra National Registry (NRTAD)**.  
DIDs follow the pattern:

```
did:andorra:NRTAD-<6digits><letter>[_ISS|_SP]
```

Examples:

- `did:andorra:NRTAD-710646J`  
- `did:andorra:NRTAD-059888N_ISS`  
- `did:andorra:NRTAD-059888N_SP`  

---

## Test Vectors

See [examples/](./examples/) for valid DID Documents:

- [andorra-1.json](./examples/andorra-1.json)  
- [andorra-2.json](./examples/andorra-2.json)  
- [andorra-3.json](./examples/andorra-3.json)  

---

## Governance

- **Maintainer**: IBERIAN UNIT VAR GROUP david.garciab@vargroupiberia.com.  
- **Specification versioning**: [Semantic Versioning](https://semver.org/)  
- **Change management**: Issues and pull requests in this repository  

---

## References

- [W3C DID Core](https://www.w3.org/TR/did-core/)  
- [W3C DID Resolution](https://www.w3.org/TR/did-resolution/)  
- [W3C DID Implementation Guide](https://www.w3.org/TR/did-imp-guide/)  
- Example DID methods: [did:web](https://w3c-ccg.github.io/did-method-web/), [did:key](https://w3c-ccg.github.io/did-method-key/)  