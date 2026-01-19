# RPP Gap Analysis Report
## Architecture Document vs. Requirements Document

**Documents Analyzed:**
- Requirements: draft-ietf-rpp-requirements-03
- Architecture: draft-ietf-rpp-architecture-01

**Analysis Date:** 19 January 2026

---

## 1. Introduction and Methodology

This document presents a comprehensive gap analysis comparing the RPP (RESTful Provisioning Protocol) Architecture Document against the RPP Requirements Document. Each requirement is evaluated using four status categories:

| Status | Definition |
|--------|------------|
| **Fulfilled** | Architecture explicitly addresses and satisfies the requirement with sufficient detail |
| **Partially Fulfilled** | Architecture addresses the requirement but with incomplete coverage or ambiguity |
| **Not Fulfilled** | Architecture does not address the requirement or contradicts it |
| **Not Applicable** | Requirement is beyond architecture scope (belongs in design/implementation specs) |

**Scope Considerations:** Architecture documents define high-level structural patterns, protocol layers, design principles, and extensibility mechanisms. They typically do not provide detailed data schemas, exact API endpoints, implementation algorithms, or domain-specific protocol extensions.

---

## 2. Detailed Requirements Analysis

### 2.1 General Requirements (R1.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R1.1 | A well-defined architecture MUST be defined for RPP, including layer responsibilities | **Fulfilled** | The architecture provides a comprehensive three-layer design (HTTP Transport, Data Representation, Resource Definition) with clear responsibility definitions for each layer and sub-layers | Section 4.2, 4.2.1, 4.2.2, 4.2.3 |
| R1.2 | *Removed* | **N/A (Removed)** | This requirement has been removed from the requirements document | N/A |
| R1.3 | RPP SHOULD leverage existing best practices and standards for RESTful APIs with clear justification when deviating | **Fulfilled** | Architecture explicitly references RFC9205 (BCP56) for HTTP best practices, REST principles, and mentions OpenAPI for documentation | Section 4, Section 5.1 |
| R1.4 | RPP MUST include support for application-level status codes, MAY reuse EPP status codes | **Fulfilled** | Section 5.1.12 defines a dual-layer approach using both HTTP status codes and RPP-specific status codes transmitted in dedicated HTTP headers | Section 5.1.12 |
| R1.5 | RPP MUST support detailed information about application status codes (e.g., RFC7807/RFC9457) | **Fulfilled** | Architecture explicitly states error responses should contain machine-readable problem details document per RFC9457 | Section 5.1.12 |
| R1.6 | RPP MUST support additional information about successful operations (warnings, deprecation, partial success) | **Fulfilled** | Section 5.1.12 explicitly states successful responses may include RPP-specific headers for warnings, deprecation notices, or partial success details | Section 5.1.12 |
| R1.7 | RPP MUST support both Thin and Thick Registry models with flexibility in data storage/return | **Partially Fulfilled** | Architecture mentions flexibility in representations (Section 5.1.9) and varying response verbosity, but does not explicitly address Thin vs Thick Registry model distinctions | Section 5.1.9, Section 5.2.1 |

### 2.2 HTTP Requirements (R2.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R2.1 | HTTP MUST be used as the transport mechanism for RPP | **Fulfilled** | HTTP is explicitly defined as the foundational transport layer throughout the architecture document | Section 4, Section 4.2.1, Section 5.1 |
| R2.2 | RPP SHOULD use best common practices for HTTP-based applications (BCP56) with justification when deviating | **Fulfilled** | Section 5.1 explicitly states "The RPP architecture uses the best practices described in RFC9205 for the HTTP transport layer" (RFC9205 is BCP56) | Section 5.1 |
| R2.3 | Consistent, predictable, and meaningful URL structures MUST be used for identifying and accessing resources | **Fulfilled** | Section 5.1.2 details resource addressing using hierarchical URL structures (e.g., /domains/{domain-name}), emphasizing human-readable, intuitive, and RESTful URLs | Section 5.1.2 |
| R2.4 | RPP MUST use existing HTTP status codes, define application-level codes mapped to HTTP codes, MUST NOT redefine HTTP semantics | **Fulfilled** | Section 5.1.12 provides detailed guidance on using HTTP status codes appropriately, mapping RPP-specific codes to semantically appropriate HTTP codes | Section 5.1.5, Section 5.1.12 |
| R2.5 | RPP MUST support deployment with intermediary proxies routing to multiple backends based on URL/headers without body parsing | **Fulfilled** | Architecture emphasizes statelessness, URL-based routing, and keeps transaction identifiers in headers for separation of concerns | Section 4.1, Section 5.1.2, Section 5.1.13 |

### 2.3 REST Requirements (R3.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R3.1 | RPP MUST use REST principles; Server MUST conform to at least Richardson Maturity Model Level 2 | **Fulfilled** | Section 4.1 explicitly defines Resource Oriented Architecture with REST principles: uniform interface (HTTP methods), resource identification via URLs, representations | Section 4.1 |
| R3.2 | RPP architecture MUST follow Resource-Oriented Architecture | **Fulfilled** | Section 4.1 is entirely dedicated to Resource Oriented Architecture (ROA), defining resources identified by URLs with uniform interface operations | Section 4.1 |
| R3.3 | RPP MUST minimize round trips; multiple requests for discovery SHOULD be used sparingly | **Fulfilled** | Section 5.1.17 explicitly states URLs shall be deterministic to minimize round trips; service discovery is designed as bootstrap mechanism, not dynamic queries | Section 5.1.17 |
| R3.4 | *Merged with R12.1* | **N/A (Merged)** | This requirement was merged with R12.1 | N/A |
| R3.5 | RPP SHOULD incorporate machine-readable API specs (OpenAPI, RAML) without mandating specific technology | **Fulfilled** | Architecture mentions OpenAPI multiple times as potential tool for documentation and code generation while maintaining flexibility | Section 1, Section 5.1.16, Section 5.2.3 |
| R3.6 | RPP MUST define pattern for resources addressable via multiple identifiers with one canonical and aliases | **Partially Fulfilled** | Section 5.1.2.1 addresses IDN handling with redirects to canonical URLs or "see-also" linking, but does not define a complete general pattern for all resource types | Section 5.1.2.1 |

### 2.4 Data Model Requirements (R4.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R4.1 | Base data model structures MUST be data format agnostic; mappable to JSON, XML, YAML | **Fulfilled** | Section 5.3 defines Resource Definition Layer as independent of media type; Section 5.3.2 defines mapping of data elements to different representations | Section 4.2.3, Section 5.3, Section 5.3.2 |
| R4.2 | Commonly used EPP extensions SHOULD be added to RPP Core data model (e.g., DNSSEC) | **Not Applicable** | This is a protocol design specification detail. Architecture provides extensibility framework but specific extensions belong in protocol mapping specifications | Section 5.4 (framework only) |
| R4.3 | RPP Core MUST only mandate strictly necessary data fields; servers can make optional fields mandatory | **Not Applicable** | This is detailed protocol design specifying field-level optionality. Architecture defines data element concept but field definitions belong in protocol specifications | Section 5.3.1 (concept only) |
| R4.4 | RPP MUST have mechanism for defining/signaling profiles with identifiers describing required components, subsets, versions | **Partially Fulfilled** | Section 5.1.15 defines profiles as identifiers for minimum configuration of protocol version, extensions, and versions, but does not fully detail how profiles describe required data model components or functional subsets | Section 5.1.15 |
| R4.5 | RPP MUST include loose coupling allowing non-coordinated introduction of non-breaking version changes | **Fulfilled** | Section 5.1.14 defines versioning schema allowing independent introduction of new features in non-breaking manner on both client and server sides | Section 5.1.14 |
| R4.6 | RPP MUST enforce strict validation, treating unknown properties/parameters/headers as errors | **Partially Fulfilled** | Section 5.1.10 defines both strict and lenient processing modes via Prefer header, with lenient as **default**. This contradicts the MUST for strict validation, though strict mode is supported | Section 5.1.10, Section 5.2.3 |
| R4.7 | RPP MUST support linking objects with flexible cardinality (1:1, 1:N, N:M) and links with attributes | **Partially Fulfilled** | Section 5.1.2 mentions URL structures for related resources. Section 5.4 supports extension mechanisms. However, link attributes (e.g., contact role) are not explicitly addressed | Section 5.1.2, Section 5.3 |
| R4.8 | RPP MUST allow referencing shared objects sponsored by different clients while sponsor retains control | **Partially Fulfilled** | Section 5.1.1.5 mentions object-level authorization with sponsors/owners having full control, but detailed shared object semantics are not specified | Section 5.1.1.5 |
| R4.9 | RPP MUST support first-class, read-only resources managed by server, possibly with sub-resources | **Partially Fulfilled** | Architecture defines resources and sub-resources but does not explicitly define read-only server-managed resources as a distinct concept | Section 5.1.2, Section 5.1.4 |
| R4.10 | RPP data model MUST support composition with child resources embedded or as sub-resources | **Fulfilled** | Section 5.1.2 defines hierarchical URL structures for sub-resources; Section 5.1.9 mentions dereferenced representations of related objects | Section 5.1.2, Section 5.3, Section 5.1.9 |

### 2.5 Data Representation Requirements (R5.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R5.1 | RPP MUST use JSON as the default data format | **Fulfilled** | Section 5.2.2 explicitly states "The primary format for RPP data representations shall be JSON." Section 5.2.4 defines application/rpp+json as primary media type | Section 5.2.2, Section 5.2.4 |
| R5.2 | It MUST be possible to extend RPP to support other data formats (XML, YAML) | **Fulfilled** | Section 5.2.2 lists JSON, XML, JWT, JWT-SD, and CBOR as potential formats. Layered architecture separates data structure from format | Section 5.2.2, Section 4.2.2 |
| R5.3 | Validation of request/response messages MUST be supported for clients and servers | **Fulfilled** | Section 5.2.3 defines data validation using schema languages (JSON Schema, OpenAPI, CDDL) enabling both client and server validation | Section 5.2.3 |
| R5.4 | RPP MUST define default media type; protocol SHALL be extensible for other media types | **Fulfilled** | Section 5.2.4 defines application/rpp+json as primary media type; Section 5.1.6 supports content negotiation for multiple media types | Section 5.2.4, Section 5.1.6 |
| R5.5 | Clients MUST be able to signal expected request content type and desired response type | **Fulfilled** | Section 5.1.6 explicitly supports content negotiation using HTTP Accept and Content-Type headers | Section 5.1.6 |
| R5.6 | *Removed* | **N/A (Removed)** | This requirement has been removed from the requirements document | N/A |
| R5.7 | RPP SHOULD consider mechanisms for data formats outside core RPP (e.g., Verifiable Credentials) | **Fulfilled** | Section 5.2.1 and 5.2.2 explicitly mention Verifiable Credentials (W3C-VC, SD-JWT) as potential structure adaptations | Section 5.2.1, Section 5.2.2 |
| R5.8 | RPP MUST support partial update of data objects | **Fulfilled** | Section 5.1.3 maps PATCH method for partial modifications, noting this maps to EPP update command | Section 5.1.3 |
| R5.9 | RPP MUST support full update of data objects | **Fulfilled** | Section 5.1.3 maps PUT method for updating resource in its entirety | Section 5.1.3 |
| R5.10 | Response representations with object identifiers MAY include URL reference to object location | **Fulfilled** | Section 5.1.2 states RPP responses will include URLs for related resources, similar to RESTful "links" concept | Section 5.1.2 |
| R5.11 | RPP MUST support representation of collections of resources | **Partially Fulfilled** | Section 5.1.2 mentions URL structures for lists. Section 4.1 mentions collection retrieval. No detailed collection representation specification | Section 5.1.2, Section 4.1 |
| R5.12 | Link representation MUST use server's internal identifiers; SHOULD consider URIs and RFC6570 templates | **Fulfilled** | Section 5.1.17 explicitly mentions URI templates (RFC6570) for resource navigation; Section 5.1.2 defines URL-based resource addressing | Section 5.1.2, Section 5.1.17 |
| R5.13 | RPP MUST define structured data model for DNS objects (Host, Domain) supporting DNS record types with TTL | **Not Applicable** | This is detailed protocol data model specification. Architecture defines data elements conceptually but specific DNS data models belong in protocol mapping documents | Section 5.3.1 (concept only) |
| R5.14 | RPP MUST support client requesting different representation depths: minimal, full, dereferenced | **Fulfilled** | Section 5.1.9 defines client signaling for response verbosity using HTTP Prefer header (return=minimal vs full) and dereferenced representations | Section 5.1.9 |
| R5.15 | RPP MAY return different representations in different contexts (GET, collection, mutation responses) | **Fulfilled** | Section 5.1.9 supports varying response representations based on client preference and operation type | Section 5.1.9 |
| R5.16 | Response MUST contain only object data; transactional info MUST be in HTTP headers | **Fulfilled** | Section 5.1.13 explicitly states transaction identifiers should be outside Data Representation Layer (as HTTP Headers) for clear separation | Section 5.1.13 |

### 2.6 Operations and Request Handling (R6.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R6.1 | *Moved to R5.14* | **N/A (Moved)** | Moved to Section 8 as R5.14 | N/A |
| R6.2 | *Moved to R5.15* | **N/A (Moved)** | Moved to Section 8 as R5.15 | N/A |
| R6.3 | *Moved to R5.16* | **N/A (Moved)** | Moved to Section 8 as R5.16 | N/A |
| R6.4 | RPP SHOULD support search for resource collections with filtering and pagination | **Partially Fulfilled** | Section 5.1.2 mentions URL structures for resource lists. Collection operations acknowledged but search, filtering, pagination details not specified | Section 5.1.2, Section 4.1 |
| R6.5 | Operations modifying state MUST be atomic (complete success or complete failure) | **Not Applicable** | This is protocol implementation/design specification. Architecture defines operation concept but atomicity guarantees belong in protocol specifications | Section 5.3.3 (concept only) |
| R6.6 | RPP MUST provide idempotency services ensuring retried operations execute only once | **Fulfilled** | Section 5.1.13 defines idempotency mechanisms using client-provided identifiers for retry detection without duplicate processing | Section 5.1.13 |
| R6.7 | Protocol MUST define expected server state for requests that time out before response | **Not Applicable** | Detailed protocol design for timeout handling. Architecture mentions idempotency but specific timeout state behavior belongs in protocol specs | Section 5.1.13 (concept) |
| R6.8 | Server MUST generate permanent, unique transaction identifier for every request | **Fulfilled** | Section 5.1.13 explicitly states "Server shall always generate own unique transaction identifier, regardless of nature of the transaction" | Section 5.1.13 |
| R6.9 | RPP MUST support informational/validation functions not tied to persistent objects as read-only resources | **Fulfilled** | Section 5.1.4 defines EPP check equivalent as /availability sub-resource with HEAD for quick response and GET for detailed response | Section 5.1.4 |
| R6.10 | RPP MUST support functional equivalent of EPP Poll command with possible enhancements | **Partially Fulfilled** | Section 5.1.11 mentions asynchronous processing with message queue resources for operation results, which could support polling. Explicit Poll equivalent not specified | Section 5.1.11 |
| R6.11 | RPP MUST include functional equivalent of RFC9038 for service messages with unrecognized extensions | **Not Applicable** | Detailed protocol feature for handling unhandled namespaces. Architecture provides extensibility framework but specific message handling belongs in protocol specs | Section 5.4 (extensibility) |

### 2.7 Discoverability (R7.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R7.1 | RPP MAY include bootstrap mechanism to locate RPP service (IANA bootstrap, DNS TXT) | **Fulfilled** | Section 5.1.17 explicitly mentions "Potential discovery of RPP server location, like IANA bootstrapping document or a special DNS TXT RR" | Section 5.1.17 |
| R7.2 | RPP Server MUST publish service discovery document in well-known directory with machine-readable info | **Fulfilled** | Section 5.1.17 defines well-known URIs (e.g., /.well-known/rpp-capabilities) for service discovery advertising versions, extensions, resource types, auth methods | Section 5.1.17 |
| R7.3 | Supported profiles, languages, extensions MUST be discoverable via discovery document | **Fulfilled** | Section 5.1.17 lists advertising supported protocol versions, extensions, available resource types, and supported features | Section 5.1.17 |
| R7.4 | RPP MUST support versioning of protocol, data objects, representations, operations, profiles, extensions | **Fulfilled** | Section 5.1.14 defines versioning for protocol and elements such as profiles; Section 5.4 mentions versioned extensions in IANA registries | Section 5.1.14, Section 5.4 |
| R7.5 | Versioning schema MUST indicate breaking vs non-breaking changes for compatibility determination | **Fulfilled** | Section 5.1.14 explicitly states versioning should allow clients to determine compatibility, mentioning Semantic Versioning as potential approach | Section 5.1.14 |
| R7.6 | Maintenance notices MAY be included in discovery document (human-readable) | **Partially Fulfilled** | Section 5.1.17 describes discovery document for operational policies but does not explicitly mention maintenance notices | Section 5.1.17 |
| R7.7 | RPP MAY support subset of EPP functionality; supported functionality MUST be discoverable | **Fulfilled** | Section 5.1.17 defines discovery mechanisms for supported features; Section 5.1.15 defines profiles for specifying required protocol elements | Section 5.1.15, Section 5.1.17 |
| R7.8 | *Removed* | **N/A (Removed)** | Removed from requirements document | N/A |
| R7.9 | *Merged with R5.10* | **N/A (Merged)** | Merged with R5.10 | N/A |
| R7.10 | *Removed (included in R7.3)* | **N/A (Removed)** | Removed and incorporated into R7.3 | N/A |

### 2.8 EPP Compatibility (R8.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R8.1 | RPP MUST provide functional equivalents for core EPP functionalities (domain, host, contact, org) | **Partially Fulfilled** | Section 1 and 5.1.3 map EPP commands to HTTP methods. Architecture aims for functional equivalents but detailed mappings are for protocol specs | Section 1, Section 5.1.3, Section 5.1.4 |
| R8.2 | Automatic/mechanical mapping/conversion between EPP and RPP data model MUST be possible | **Partially Fulfilled** | Section 1 states RPP "aims for data model compatibility with EPP core objects to allow automatic and mechanical mapping." Full mapping details are for protocol specs | Section 1, Section 5.2.1 |
| R8.3 | Compatibility definitions for RPP to EPP mapping MAY be defined in profiles | **Fulfilled** | Section 5.1.15 defines profiles; Section 5.4 mentions compatibility profiles for specific use cases such as EPP compatibility | Section 5.1.15, Section 5.4 |
| R8.4 | RPP MUST include extension framework able to define equivalents of common EPP extensions | **Fulfilled** | Section 5.4 comprehensively defines extension mechanisms for new resource types, data elements, operations, and status codes | Section 5.4 |
| R8.5 | EPP AuthInfo MUST be supported; RPP MUST support Secure Authorization for Transfer (RFC9154) | **Partially Fulfilled** | Section 5.1.1.5 mentions "shared secrets for backward compatibility with EPP password-based authorisation" but does not specifically address RFC9154 | Section 5.1.1.5 |
| R8.6 | RPP MUST support client_id/password authentication to match EPP | **Fulfilled** | Section 5.1.1 explicitly states RPP SHOULD support authentication schemes with client identifier and password (HTTP Basic Authentication) for EPP compatibility | Section 5.1.1 |
| R8.7 | RPP MUST support longer auth info, client software metadata, security events (RFC8807 equivalent) | **Not Fulfilled** | Architecture does not specifically address longer authorization information, client software metadata, or security event information equivalent to Login Security Extension | N/A |

### 2.9 Security (R9.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R9.1 | RPP MUST support state-of-the-art authentication and authorization for modern HTTP infrastructure | **Fulfilled** | Section 5.1.1 defines authentication leveraging OAuth 2.0 and related frameworks, with various HTTP authentication schemes | Section 5.1.1 |
| R9.2 | RPP MUST support robust authentication (OAuth 2.0, OpenID Connect) | **Fulfilled** | Section 5.1.1 explicitly mentions OAuth 2.0 as focus area, with Bearer Token usage defined | Section 5.1.1 |
| R9.3 | Simplified transfer with interactive registrant approval MAY be included | **Not Applicable** | Specific transfer workflow design requirement. Architecture provides transfer modeling concept but detailed workflows belong in protocol design | Section 5.1.4 (concept) |
| R9.4 | RPP MUST include authorization model beyond EPP AuthInfo (transfers without AuthInfo, registrant OpenID for DNS) | **Partially Fulfilled** | Section 5.1.1.2 mentions fine-grained authorization beyond auth-code models. Specific use cases like registrant-authorized DNS Operator access not detailed | Section 5.1.1.2, Section 5.1.1.5 |
| R9.5 | All RPP communications MUST use HTTPS (TLS) | **Not Fulfilled** | Architecture does not explicitly mandate HTTPS/TLS. HTTP transport is defined but TLS encryption requirement not stated | N/A |
| R9.6 | Security mechanisms SHOULD be flexible for operators to choose methods and support federation | **Fulfilled** | Section 5.1.1 states "Implementations will be able to choose authentication and authorisation methods appropriate for their security requirements" | Section 5.1.1 |
| R9.7 | RPP MAY include cryptographic verification of request/response messages | **Partially Fulfilled** | Section 5.2.2 mentions JWT for "verifiable data consistency" and JWT-SD for selective disclosure, but not a full message signing framework | Section 5.2.2 |
| R9.8 | RPP MUST allow multiple user accounts per registrar; user management MAY be delegated to admin | **Fulfilled** | Section 5.1.1.3 explicitly states "More than one credential might be authorised to act on behalf of the same RPP client" | Section 5.1.1.3 |
| R9.9 | RPP MUST support granular authorization matrix with permissions per user | **Fulfilled** | Section 5.1.1.1 defines authorization scopes (rpp:read, rpp:write) for granular access control; Section 5.1.1.2 mentions fine-grained authorization | Section 5.1.1.1, Section 5.1.1.2 |
| R9.10 | RPP MUST include considerations for secure credential handling (strong passwords, limited lifetime) | **Partially Fulfilled** | Section 5.1.1.4 states security policies delegated to authentication schemes' best practices, not defined at protocol level. Addresses concern indirectly | Section 5.1.1.4 |
| R9.11 | RPP MUST support Least Privilege Principle | **Fulfilled** | Section 5.1.1.1 states "clients can be granted only the necessary permissions" through granular scopes | Section 5.1.1.1 |
| R9.12 | RPP MUST support secure credentials management (protection against replay/theft, limited lifetimes) | **Partially Fulfilled** | Section 5.1.1.4 delegates to authentication scheme best practices. OAuth 2.0/Bearer tokens have built-in protections but explicit protocol requirements not stated | Section 5.1.1.4 |
| R9.13 | Protocol extensions MUST be subject to same security review as core protocol | **Partially Fulfilled** | Section 5.4 defines extension mechanisms with IANA registration promoting standardization, but explicit security review requirements not stated | Section 5.4 |
| R9.14 | There MUST be mechanisms to revoke/deprecate credentials, tokens, or permissions | **Not Fulfilled** | Architecture does not explicitly define credential revocation mechanisms. OAuth 2.0 supports this but not stated in architecture | N/A |
| R9.15 | RPP MUST support mechanisms to prevent DoS attacks (rate limiting, throttling) | **Partially Fulfilled** | Section 5.1.12 mentions RPP should use standardised HTTP signaling for rate limiting, but comprehensive DoS prevention not detailed | Section 5.1.12 |
| R9.16 | RPP MUST support registry operator impersonation/acting on behalf of any account with elevated access | **Not Fulfilled** | Architecture does not address registry operator impersonation or elevated administrative access capabilities | N/A |

### 2.10 Extensibility (R10.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R10.1 | Protocol MUST be extensible for new functionalities, data elements, resources, operations | **Fulfilled** | Section 5.4 comprehensively defines extension mechanisms through layered design and IANA registries | Section 5.4 |
| R10.2 | RPP MUST support extending data model with new object types and properties to existing types | **Fulfilled** | Section 5.4 states "Existing resources can be extended with additional data elements or operations in a backward-compatible manner" | Section 5.4 |
| R10.3 | RPP MUST define extensibility methods promoting transparency and reusability | **Fulfilled** | Section 5.4 states "RPP shall facilitate standardisation and reuse by requiring that extensions be registered with IANA" | Section 5.4 |
| R10.4 | Extensions for new operations on existing resources MUST be supported | **Fulfilled** | Section 5.4 explicitly mentions extending existing resources with additional operations | Section 5.4 |
| R10.5 | RPP MUST support extensions defining new status codes with IANA registration | **Fulfilled** | Section 5.4 lists "Status codes: New RPP status codes can be defined and registered including their mapping to HTTP status codes" | Section 5.4 |
| R10.6 | RPP MUST support extensions adding new HTTP headers | **Partially Fulfilled** | Architecture does not explicitly address HTTP header extensions. Section 5.4 covers data elements, operations, status codes but not headers specifically | Section 5.4 |
| R10.7 | RPP SHALL have conflict avoidance mechanisms for extensions; MUST support non-coordinated private and coordinated shared extensions | **Fulfilled** | Section 5.4.1 defines Name Management and Collision Avoidance with IANA registry and naming conventions (reverse domain notation) for private extensions | Section 5.4.1 |
| R10.8 | When public Registry for extensions is required, IANA MUST be used | **Fulfilled** | Section 5.4 and 5.4.1 explicitly require IANA registration for standardized extensions | Section 5.4, Section 5.4.1 |
| R10.9 | Extensions MUST include versioning; version MUST be in discovery document | **Fulfilled** | Section 5.1.14 defines versioning for extensions; Section 5.1.17 mentions advertising supported extensions | Section 5.1.14, Section 5.1.17 |
| R10.10 | *Removed* | **N/A (Removed)** | Removed from requirements document | N/A |
| R10.11 | Protocol MUST support extending processes with transient parameters or non-persistent data | **Partially Fulfilled** | Section 5.1.4 models operations as process sub-resources that "may accept additional input data." Architecture enables this but doesn't specify transient parameter mechanisms | Section 5.1.4 |
| R10.12 | Protocol MUST support extending operation results with transient, non-persistent information | **Partially Fulfilled** | Section 5.1.4 mentions processes "may have an outcome or result not being part of the resource state itself." Concept exists but detailed mechanisms not specified | Section 5.1.4 |
| R10.13 | Protocol MUST allow extensions to add information to object statuses (e.g., due date) | **Not Applicable** | Detailed protocol design for status metadata. Architecture supports status codes and extensions but specific status attributes belong in protocol specs | Section 5.1.12, Section 5.4 |
| R10.14 | Data model MUST allow extension for future DNS record types and delegation methods | **Not Applicable** | Detailed DNS data model extensibility. Architecture provides general extensibility but DNS record extensions belong in protocol mapping | Section 5.4 (general) |
| R10.15 | RPP MUST promote cohesive extension patterns by defining preferred "standard way" with guidance | **Fulfilled** | Section 5.4 states RPP "shall facilitate standardisation and reuse" through IANA registration, avoiding "fragmentation from conflicting definitions" | Section 5.4 |
| R10.16 | RPP MUST support extension(s) for clients to update authentication credentials | **Partially Fulfilled** | Section 5.1.1 defines authentication framework but does not specifically address credential update extensions. Extension framework could accommodate | Section 5.1.1, Section 5.4 |

### 2.11 Scalability (R11.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R11.1 | RPP MUST be stateless; server MUST NOT maintain application state for future requests | **Fulfilled** | Section 4.1 explicitly states "Statelessness: Each request to a resource is treated as independent of previous requests. The server does not maintain client state" | Section 4.1 |
| R11.2 | RPP MUST support cacheability; MUST NOT include transaction identifiers in cached content | **Fulfilled** | Section 4.1 lists cacheability; Section 5.1.7 defines caching policies; Section 5.1.13 separates transaction identifiers from representations | Section 4.1, Section 5.1.7, Section 5.1.13 |
| R11.3 | RPP MUST support load balancing at URL level without processing HTTP body | **Fulfilled** | Section 5.1.2 defines URL-based resource addressing; Section 5.1.13 keeps transaction identifiers in headers. Supports proxy-based routing | Section 5.1.2, Section 5.1.13 |
| R11.4 | *Moved to R12.5* | **N/A (Moved)** | Moved to Section 15 as R12.5 | N/A |
| R11.5 | RPP MUST support asynchronous processing for multi-object, resource-intensive, or manual operations | **Fulfilled** | Section 5.1.11 provides comprehensive definition of asynchronous operation processing including status resources, result retrieval, message queues | Section 5.1.11 |

### 2.12 Performance (R12.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R12.1 | RPP SHOULD avoid HTTP body when not necessary (data in URL/headers) to minimize message sizes | **Partially Fulfilled** | Section 5.1.13 separates transaction identifiers into headers; Section 5.1.9 allows minimal responses. Architecture enables this but doesn't mandate body-less operations | Section 5.1.9, Section 5.1.13 |
| R12.2 | RPP SHOULD allow bulk operations, listing, filtering; MUST NOT mandate if impacts performance | **Partially Fulfilled** | Section 4.1 mentions bulk operations through dedicated resources; Section 5.1.2 mentions resource lists. Filtering/pagination details not specified | Section 4.1, Section 5.1.2 |
| R12.3 | *Removed* | **N/A (Removed)** | Removed from requirements document | N/A |
| R12.4 | Protocol MUST be usable in both high volume and low volume environments | **Fulfilled** | Architecture's statelessness, caching, URL-based routing, and async processing support scalability for varied volume environments | Section 4.1, Section 5.1.7, Section 5.1.11 |
| R12.5 | RPP MUST support cacheability; MUST NOT include transaction identifiers | **Fulfilled** | Section 5.1.7 defines caching policies and cache-control headers; Section 5.1.13 explicitly separates transaction identifiers from representations | Section 5.1.7, Section 5.1.13 |

### 2.13 Internationalisation (R13.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R13.1 | RPP MUST support internationalisation for object types and messages in core and extensions | **Fulfilled** | Section 5.1.8 defines language negotiation; Section 5.1.2.1 addresses IDN handling; Section 5.2.1 mentions JSContact for internationalized contacts | Section 5.1.8, Section 5.1.2.1, Section 5.2.1 |
| R13.2 | RPP MUST support human-readable localised response messages | **Fulfilled** | Section 5.1.8 explicitly states "Server implementations MAY support multiple languages for textual content in responses to provide human-readable localised responses" | Section 5.1.8 |

### 2.14 Clients (R14.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| R14.1 | RPP MUST support server applications as clients (primary Registry/registrar use case) | **Fulfilled** | Architecture defines RPP clients as "entity or application that interacts with the RPP server" (Section 2), HTTP-based design supports server-to-server | Section 2, Section 4 |
| R14.2 | RPP MUST support command-line tools and desktop applications (curl, Postman, scripts) | **Fulfilled** | HTTP/REST-based architecture using standard methods and JSON format inherently supports any HTTP-capable client | Section 4, Section 5.1 |
| R14.3 | RPP SHOULD support web browsers as clients (SPA) without proxy backend | **Fulfilled** | Section 1 mentions "direct browser and mobile application integration." Section 5.1.3 notes OPTIONS for CORS pre-flight | Section 1, Section 5.1.3 |
| R14.4 | RPP SHOULD support mobile applications as clients through direct integration | **Fulfilled** | Section 1 explicitly mentions "direct browser and mobile application integration including modern security mechanisms such as OAuth2.0" | Section 1 |

### 2.15 Common Object Requirements (O1.x - O2.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| O1.1 | *Removed* | **N/A (Removed)** | Removed from requirements document | N/A |
| O2.1 | RPP MUST support Pull Transfer (Gaining Client) and Push Transfer (Sponsoring Client) | **Not Applicable** | Detailed transfer operation specification. Architecture models transfers as sub-resources but specific transfer types belong in protocol specs | Section 5.1.4 (concept) |
| O2.2 | Gaining Client MUST provide valid authorization for Pull Transfer | **Not Applicable** | Detailed transfer authorization requirement. Section 5.1.1.5 addresses authorization concept but rules belong in protocol specs | Section 5.1.1.5 (concept) |
| O2.3 | For Pull Transfers, RPP MUST provide operations for Sponsoring Client to approve/reject | **Not Applicable** | Detailed transfer workflow. Section 5.1.4 mentions approve/reject conceptually but operations belong in protocol design | Section 5.1.4 (concept) |
| O2.4 | For Push Transfers, RPP MUST provide operations for Gaining Client to approve/reject | **Not Applicable** | Detailed transfer workflow beyond architecture scope | Section 5.1.4 (concept) |
| O2.5 | RPP MUST provide operation for Initiating Client to cancel pending transfer | **Not Applicable** | Detailed transfer operation. Architecture provides concept but specific operations belong in protocol specs | Section 5.1.4 (concept) |
| O2.6 | RPP MUST provide operation to query transfer status for Sponsoring and Gaining Clients | **Partially Fulfilled** | Section 5.1.4 mentions "GET operation to query the transfer status" in transfer sub-resource pattern | Section 5.1.4 |
| O2.7 | Successful transfer response MUST include object representation and list of transferred associated objects | **Not Applicable** | Detailed transfer response specification. Response concepts exist but specific transfer responses belong in protocol specs | Section 5.1.9 (concept) |

### 2.16 Domain Object Type (D1.x - D4.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| D1.1 | Domain data model MUST include RFC5731 attributes: FQDN, ROID, status, client IDs, timestamps, nameservers, contacts | **Not Applicable** | Detailed domain data model specification. Architecture provides data element concepts but specific attributes belong in protocol mapping | Section 5.3.1 (concept) |
| D1.2 | RPP MUST only accept valid domain names (valid FQDNs) | **Not Applicable** | Data validation rule for domain objects. Architecture supports validation concept but rules belong in protocol specs | Section 5.2.3 (concept) |
| D1.3 | RPP MUST support IDN with A-labels and U-labels (IDNA2008) | **Partially Fulfilled** | Section 5.1.2.1 addresses IDN handling in resource addressing. Detailed data model support is for protocol specs | Section 5.1.2.1 |
| D1.4 | RPP MUST apply Label Equivalence rules (RFC5890) when processing domain names | **Not Applicable** | Detailed domain name processing rule. Architecture mentions IDN but implementation rules belong in protocol specs | Section 5.1.2.1 (concept) |
| D1.5 | Domain MUST allow zero, one, or more DNS configuration objects including nameservers | **Not Applicable** | Detailed domain-nameserver relationship. Architecture supports relationships but cardinality rules belong in protocol mapping | Section 5.1.2 (concept) |
| D1.6 | RPP MUST support domains linked to registrant, admin, tech, billing contacts with extensible types | **Not Applicable** | Detailed domain-contact relationship. Architecture supports relationships and extensibility but specifics belong in protocol specs | Section 5.1.2, Section 5.4 (concepts) |
| D1.7 | *Removed (included in R8.5)* | **N/A (Removed)** | Removed and included in R8.5 | N/A |
| D1.8 | RPP MUST provide functional equivalents for EPP domain status values with HTTP mapping | **Not Applicable** | Detailed domain status mapping. Architecture defines status framework but specific mappings belong in protocol specs | Section 5.1.12 (concept) |
| D1.9 | RPP MUST enforce referential integrity - subordinate host parent MUST NOT be deleted | **Not Applicable** | Detailed referential integrity rule beyond architecture scope | N/A |
| D1.10 | RPP MUST provide mechanism for clients to discover server's IDN policies via discovery | **Partially Fulfilled** | Section 5.1.17 defines service discovery for capabilities/policies. IDN-specific policies could be included but not explicitly specified | Section 5.1.17 |
| D2.1 | RPP MUST provide operations to check, create, read, update, transfer, renew, delete domains | **Partially Fulfilled** | Section 5.1.3 and 5.1.4 map basic CRUD and additional operations to HTTP methods. Detailed specs belong in protocol docs | Section 5.1.3, Section 5.1.4 |
| D2.2 | Creating/renewing domains MUST allow registration period specification with expiration confirmation | **Not Applicable** | Detailed create/renew operation specification. Architecture defines operation concept but parameters belong in protocol specs | Section 5.3.3 (concept) |
| D2.3 | RPP MUST support Domain Registry Grace Period Mapping (RFC3915) | **Not Applicable** | Detailed grace period implementation. Architecture provides extensibility but specific support belongs in protocol specs | Section 5.4 (concept) |
| D2.4 | RPP SHOULD support searching/listing domains filtered by name, Sponsoring Client | **Partially Fulfilled** | Section 5.1.2 mentions URL structures for resource lists but detailed search/filtering not specified | Section 5.1.2 |
| D2.5 | Only Sponsoring Client (or admin) MAY modify/delete domains; servers MUST enforce authorization | **Partially Fulfilled** | Section 5.1.1.5 defines object-level authorization with sponsor/owner control. Enforcement details belong in protocol specs | Section 5.1.1.5 |
| D2.6 | RPP MUST prevent duplicate domain creation; return HTTP 409 on collision | **Not Applicable** | Detailed duplicate prevention. Section 5.1.12 mentions 409 for "object already exists" but domain rules belong in protocol specs | Section 5.1.12 (concept) |
| D2.7 | Domain transfer MUST also transfer subordinate host objects | **Not Applicable** | Detailed transfer cascade requirement. Architecture provides transfer concept but behaviors belong in protocol specs | Section 5.1.4 (concept) |
| D2.8 | Domain transfer MUST allow implicit renewal with new expiration date | **Not Applicable** | Detailed transfer-renewal specification beyond architecture scope | Section 5.1.4 (concept) |
| D2.9 | Domain transfer SHOULD support optional DNSSEC/delegation update with transfer | **Not Applicable** | Detailed transfer operation parameter. Extensibility could accommodate but specifics belong in protocol specs | Section 5.4 (concept) |
| D3.1 | JSON representation MUST include canonical domain name and U-label/A-label for IDN | **Not Applicable** | Detailed JSON representation. Section 5.1.2.1 addresses IDN conceptually but formats belong in protocol mapping | Section 5.1.2.1 (concept) |
| D3.2 | Domain representation MUST include link relations (self, hosts, contacts) | **Partially Fulfilled** | Section 5.1.2 states responses include URLs for related resources. Specific link relations belong in protocol specs | Section 5.1.2 |
| D3.3 | Representation MUST adapt to Registry model (thin: IDs only; thick: full data) | **Not Applicable** | Detailed representation variation for thin/thick models belongs in protocol specs | Section 5.1.9 (concept) |
| D4.1 | RPP Core MUST incorporate functional equivalents of essential EPP extensions: DNSSEC, Grace Period, TTL, Organization | **Not Applicable** | Detailed protocol design for extension incorporation. Architecture provides framework but specifics belong in protocol specs | Section 5.4 (framework) |

### 2.17 Host Object Type (H1.x - H3.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| H1.1 | Host data model MUST include: FQDN, IP addresses, ROID, status, client IDs, timestamps | **Not Applicable** | Detailed host data model specification. Architecture provides concepts but attributes belong in protocol mapping | Section 5.3.1 (concept) |
| H1.2 | RPP MUST map EPP host attribute model to generic JSON DNS model | **Not Applicable** | Detailed EPP-to-RPP mapping. Architecture defines mapping layer but specifics belong in protocol specs | Section 5.3.2 (concept) |
| H1.3 | Host names MUST be valid FQDNs | **Not Applicable** | Data validation rule for host objects belongs in protocol specs | Section 5.2.3 (concept) |
| H1.4 | RPP MUST support IDN for host names with A-labels and U-labels | **Partially Fulfilled** | Section 5.1.2.1 addresses IDN handling. Specific host IDN requirements belong in protocol specs | Section 5.1.2.1 |
| H1.5 | RPP MUST apply Label Equivalence rules (RFC5890) when processing host names | **Not Applicable** | Detailed host name processing rule belongs in protocol specs | Section 5.1.2.1 (concept) |
| H1.6 | RPP MUST support both In-bailiwick and Out-of-bailiwick host objects | **Not Applicable** | Detailed host relationship specification belongs in protocol specs | N/A |
| H1.7 | RPP MUST support zero or more IP addresses (IPv4/IPv6) for hosts | **Not Applicable** | Detailed host IP address specification belongs in protocol specs | Section 5.3.1 (concept) |
| H1.8 | RPP MUST provide functional equivalents for EPP host status values | **Not Applicable** | Detailed host status mapping belongs in protocol specs | Section 5.1.12 (concept) |
| H2.1 | RPP MUST provide operations to check, create, read, update, delete hosts per RFC5732 | **Partially Fulfilled** | Section 5.1.3 maps CRUD to HTTP methods. Specific host operations belong in protocol specs | Section 5.1.3, Section 5.1.4 |
| H2.2 | RPP SHOULD support searching/listing hosts filtered by name, IP, Sponsoring Client | **Partially Fulfilled** | Section 5.1.2 mentions URL structures for lists but detailed search/filtering not specified | Section 5.1.2 |
| H2.3 | Only Sponsoring Client (or admin) MAY modify/delete hosts | **Partially Fulfilled** | Section 5.1.1.5 defines object-level authorization. Enforcement details belong in protocol specs | Section 5.1.1.5 |
| H2.4 | In-bailiwick hosts MUST be managed by same Sponsoring Client as parent domain | **Not Applicable** | Detailed bailiwick management rule belongs in protocol specs | Section 5.1.1.5 (concept) |
| H2.5 | RPP MUST enforce referential integrity - linked hosts MUST NOT be deleted | **Not Applicable** | Detailed referential integrity specification belongs in protocol specs | N/A |
| H2.6 | RPP MUST prevent duplicate host creation; return conflict | **Not Applicable** | Detailed duplicate prevention belongs in protocol specs | Section 5.1.12 (concept) |
| H3.1 | RPP MUST support JSON representation for Host objects and attributes | **Partially Fulfilled** | Section 5.2.2 defines JSON as primary format. Specific host JSON structures belong in protocol mapping | Section 5.2.2, Section 5.3.2 |
| H3.2 | JSON representation MUST include canonical host name and U-label/A-label for IDN | **Not Applicable** | Detailed JSON representation belongs in protocol mapping | Section 5.1.2.1 (concept) |
| H3.3 | Host representation SHOULD include link relations (self, parent domain) | **Partially Fulfilled** | Section 5.1.2 states responses include URLs for related resources. Specific relations belong in protocol specs | Section 5.1.2 |

### 2.18 Contact Object Type (C1.x - C4.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| C1.1 | Contact data model MUST include RFC5733 equivalent attributes | **Not Applicable** | Detailed contact data model specification belongs in protocol mapping | Section 5.3.1 (concept) |
| C1.2 | RPP MUST support server-generated opaque IDs; client-supplied IDs OPTIONAL | **Not Applicable** | Detailed contact ID generation specification belongs in protocol specs | N/A |
| C1.3 | RPP SHOULD support explicit entity type indication (person/organisation) | **Not Applicable** | Detailed contact model attribute belongs in protocol specs | Section 5.3.1 (concept) |
| C1.4 | Thick Registry returns full contact data; thin Registry returns only identifier | **Not Applicable** | Detailed representation variation belongs in protocol specs | Section 5.1.9 (concept) |
| C1.5 | RPP MUST support disclosure and privacy preferences equivalent to EPP "disclose" | **Not Applicable** | Detailed contact privacy specification belongs in protocol specs | N/A |
| C1.6 | RPP MUST support contact referring to external identity provider | **Partially Fulfilled** | Section 5.2.1 mentions Verifiable Credentials integration. Specific contact-identity linking belongs in protocol specs | Section 5.2.1 |
| C1.7 | RPP MUST enforce referential integrity - contacts MUST NOT be deleted when referenced | **Not Applicable** | Detailed referential integrity specification belongs in protocol specs | N/A |
| C1.8 | RPP SHOULD consider renaming contact to "entity" to align with RDAP | **Not Applicable** | Naming/terminology decision belongs in protocol specs | N/A |
| C2.1 | Contact MUST support all RFC5733 operations with partial update | **Partially Fulfilled** | Section 5.1.3 maps CRUD to HTTP methods. Specific operations belong in protocol specs | Section 5.1.3 |
| C2.2 | RPP MAY support contact transfer command | **Not Applicable** | Detailed contact transfer specification belongs in protocol specs | Section 5.1.4 (concept) |
| C2.3 | RPP SHOULD support searching/listing contacts filtered by name, Sponsoring Client | **Partially Fulfilled** | Section 5.1.2 mentions URL structures for lists but search/filtering not detailed | Section 5.1.2 |
| C2.4 | Functional equivalents for EPP contact statuses MUST be supported | **Not Applicable** | Detailed contact status mapping belongs in protocol specs | Section 5.1.12 (concept) |
| C2.5 | RPP MUST prevent duplicate contact creation; return HTTP 409 | **Not Applicable** | Detailed duplicate prevention belongs in protocol specs | Section 5.1.12 (concept) |
| C2.6 | Protocol MUST provide full contact retrieval with authorization ensuring sensitive data only to Sponsoring Client | **Partially Fulfilled** | Section 5.1.1.5 defines object-level authorization. Specific contact authorization belongs in protocol specs | Section 5.1.1.5, Section 5.1.9 |
| C2.7 | Protocol MUST provide appropriate contact representation to non-Sponsoring Clients | **Not Applicable** | Detailed contact access policy belongs in protocol specs | Section 5.1.1.5 (concept) |
| C3.1 | RPP SHOULD consider using JSContact (RFC9553) format for contact representation | **Fulfilled** | Section 5.2.1 explicitly mentions "JSContact Structure Adaptation: Adapting to the existing JSON representation for Contact Information [RFC9553]" | Section 5.2.1 |
| C3.2 | RPP MUST support contact attribute disclosure preferences per field mapped to EPP | **Not Applicable** | Detailed disclosure preference specification belongs in protocol specs | N/A |
| C4.1 | RPP MUST support i18n (character encoding) for contact: name, address, human-readable text | **Partially Fulfilled** | Section 5.1.8 addresses language negotiation; Section 5.2.1 mentions JSContact. Specific encoding belongs in protocol specs | Section 5.1.8, Section 5.2.1 |
| C4.2 | RPP MUST support both localized and internationalized version of EPP postalInfo | **Not Applicable** | Detailed contact postal information specification belongs in protocol specs | Section 5.1.8 (concept) |
| C4.3 | RPP MUST support internationalized email addresses (RFC6530) | **Not Applicable** | Detailed contact email specification belongs in protocol specs | N/A |
| C4.4 | RPP MUST support multiple localized expressions of same data | **Partially Fulfilled** | Section 5.1.8 mentions multi-language representations. Specific support belongs in protocol specs | Section 5.1.8 |
| C4.5 | Future contact extensions MUST handle i18n and localization requirements | **Partially Fulfilled** | Section 5.1.8 establishes i18n framework; Section 5.4 defines extensions. Specific requirements belong in extension specs | Section 5.1.8, Section 5.4 |

### 2.19 Organisation Object Type (G1.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| G1.1 | RPP MUST support data model and operations for Organisations (RFC8543 equivalent) | **Not Applicable** | Detailed organisation object specification. Architecture mentions organizations in Section 1 but specifics belong in protocol specs | Section 1 (mention) |

### 2.20 Privacy Considerations (DP1.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| DP1.1 | Protocol MUST support data privacy principles (GDPR): data minimisation, purpose limitation | **Partially Fulfilled** | Section 5.1.9 supports minimal responses; Section 5.2.1 mentions privacy-preserving formats (SD-JWT). Comprehensive mechanisms not fully defined | Section 5.1.9, Section 5.2.1 |
| DP1.2 | Protocol MUST allow clients to provide/manage only necessary data and request appropriate representations | **Partially Fulfilled** | Section 5.1.9 allows minimal representations; Section 5.1.6 supports content negotiation. Full data minimization mechanisms not explicit | Section 5.1.9, Section 5.1.6 |
| DP1.3 | Operations and data models MUST be flexible for implementing data subject rights (access, rectification, erasure) | **Partially Fulfilled** | Architecture provides CRUD operations enabling access, update, deletion. Specific privacy workflows not defined | Section 5.1.3 |
| DP1.4 | Protocol MUST provide services identifying data collection policies/privacy practices in discovery document | **Partially Fulfilled** | Section 5.1.17 defines discovery for operational policies. Privacy policy discovery not explicitly mentioned but could be accommodated | Section 5.1.17 |
| DP1.5 | Object type specifications MAY define additional privacy requirements | **Not Applicable** | Scoping statement for object-specific privacy requirements. Specifics belong in object type specifications | N/A |

### 2.21 Extension Requirements - Appendix (A1.x - A2.x)

| Req ID | Requirement Summary | Status | Explanation | Architecture Reference(s) |
|--------|---------------------|--------|-------------|---------------------------|
| A1.1 | Extension allowing DNS Operator to update DNSSEC key material without registrar involvement | **Not Applicable** | Specific extension requirement. Architecture provides extension framework but definition belongs in extension specs | Section 5.4 (framework) |
| A1.2 | Extension for clients to update EPP compatible (client_id/password) authentication credentials | **Not Applicable** | Specific extension requirement. Architecture provides framework but definition belongs in extension specs | Section 5.4 (framework) |
| A1.3 | Extensions listed in TigerTeamRecc Section 4.2.3 recommendations | **Not Applicable** | References external extension recommendations. Definitions belong in extension specs | Section 5.4 (framework) |
| A2.1 | Extension for historical overview of an object (events: create, update, etc.) | **Not Applicable** | Optional extension requirement. Architecture provides framework but definition belongs in extension specs | Section 5.4 (framework) |
| A2.2 | Extension for Search API to search objects in Registry database | **Not Applicable** | Optional extension requirement. Architecture mentions search conceptually but definition belongs in extension specs | Section 5.1.2, Section 5.4 |
| A2.3 | Extensions listed in TigerTeamRecc Section 4.2.3 recommendations 3 | **Not Applicable** | References external extension recommendations. Definitions belong in extension specs | Section 5.4 (framework) |

---

## 3. Summary Statistics

### 3.1 Overall Counts

| Status | Count | Percentage |
|--------|-------|------------|
| **Fulfilled** | 70 | 34.8% |
| **Partially Fulfilled** | 50 | 24.9% |
| **Not Fulfilled** | 4 | 2.0% |
| **Not Applicable** | 64 | 31.8% |
| **N/A (Removed/Merged/Moved)** | 13 | 6.5% |
| **Total** | **201** | 100% |

### 3.2 Applicable Requirements Only (Excluding N/A)

| Status | Count | Percentage |
|--------|-------|------------|
| **Fulfilled** | 70 | 56.5% |
| **Partially Fulfilled** | 50 | 40.3% |
| **Not Fulfilled** | 4 | 3.2% |
| **Total Applicable** | **124** | 100% |

---

## 4. Key Findings

### 4.1 Strengths of the Architecture Document

The architecture document provides strong coverage in the following areas:

1. **Core Architecture Definition (R1.1)**: Comprehensive three-layer architecture with clear responsibilities
2. **HTTP/REST Foundations (R2.x, R3.x)**: Excellent adherence to HTTP best practices and REST principles
3. **Extensibility Framework (R10.x)**: Well-defined extension mechanisms with IANA registration
4. **Authentication/Authorization (R9.x)**: Modern OAuth 2.0-based security architecture
5. **Statelessness and Scalability (R11.x)**: Clear separation of concerns enabling scalability
6. **Service Discovery (R7.x)**: Comprehensive discovery mechanisms via well-known URIs

### 4.2 Critical Gaps (Not Fulfilled)

| Req ID | Requirement | Impact |
|--------|-------------|--------|
| **R8.7** | Login Security Extension equivalent (longer auth info, client metadata, security events) | Missing EPP compatibility feature |
| **R9.5** | Mandatory HTTPS/TLS requirement | Critical security gap - transport encryption not mandated |
| **R9.14** | Credential/token revocation mechanisms | Security lifecycle management gap |
| **R9.16** | Registry operator impersonation/elevated access | Administrative operation gap |

### 4.3 Notable Partially Fulfilled Requirements

| Req ID | Issue | Recommendation |
|--------|-------|----------------|
| **R4.4** | Profiles defined but incomplete mechanism for data model requirements | Expand Section 5.1.15 to describe data model requirements |
| **R4.6** | Strict validation supported but lenient is default (contradicts MUST) | Clarify default validation behavior |
| **R6.10** | Poll command equivalent not explicitly specified | Add explicit messaging/notification pattern |
| **DP1.x** | Privacy framework exists but incomplete | Expand privacy mechanisms in architecture |
| **R1.7** | Thin/Thick Registry model support not explicit | Add Registry model considerations |

### 4.4 Appropriately Deferred Requirements

Many requirements (64) are appropriately marked "Not Applicable" because they are:

- **Domain-specific data models** (D1.x, D2.x, D3.x, D4.x): Detailed domain object attributes and operations
- **Host-specific data models** (H1.x, H2.x, H3.x): Detailed host object attributes and operations  
- **Contact-specific data models** (C1.x, C2.x, C3.x, C4.x): Detailed contact object attributes and operations
- **Transfer workflows** (O2.x): Detailed transfer operation specifications
- **Extension definitions** (A1.x, A2.x): Specific extension implementations

These are correctly deferred to protocol mapping specifications.

---

## 5. Recommendations

### 5.1 For Architecture Document Updates

1. **Add explicit HTTPS/TLS requirement** (addresses R9.5)
   - Add to Section 5.1 or create dedicated security transport section
   - State: "All RPP communications MUST use HTTPS (TLS 1.3 or later)"

2. **Address credential revocation** (addresses R9.14)
   - Add subsection to Section 5.1.1 on credential lifecycle management
   - Reference standard OAuth 2.0 token revocation mechanisms

3. **Add registry operator administrative access pattern** (addresses R9.16)
   - Define conceptual model for elevated administrative access
   - Consider impersonation patterns

4. **Expand profiles section** (addresses R4.4)
   - Describe how profiles specify required data model components
   - Define profile signaling mechanisms more completely

5. **Clarify validation default** (addresses R4.6)
   - Either change default to strict, or document rationale for lenient default
   - Ensure consistency with MUST requirement

6. **Add explicit messaging/notification pattern** (addresses R6.10)
   - Define Poll equivalent or alternative messaging architecture
   - Consider async notification patterns

### 5.2 For Protocol Design Documents

The following should be prioritized in protocol mapping specifications:

1. **Object type mappings**: Domain, Host, Contact, Organisation data models and operations
2. **Transfer workflow specifications**: Pull and push transfer operations
3. **EPP extension equivalents**: DNSSEC, Grace Period, TTL, Organization
4. **Privacy mechanisms**: Detailed disclosure and data minimization implementations
5. **Search and filtering**: Collection query specifications

---

## 6. Conclusion

The RPP Architecture Document provides a solid foundation addressing **56.5% of applicable requirements as Fulfilled** and another **40.3% as Partially Fulfilled**. The architecture successfully establishes:

- A clear three-layer design (HTTP Transport, Data Representation, Resource Definition)
- Strong REST/HTTP foundations following BCP56 best practices
- Comprehensive extensibility framework via IANA registries
- Modern authentication architecture based on OAuth 2.0
- Scalability through statelessness, caching, and async operations

The four Not Fulfilled requirements represent critical gaps that should be addressed in architecture revisions, particularly the **HTTPS/TLS mandate** and **credential revocation mechanisms**.

The large number of "Not Applicable" requirements (64) reflects appropriate scoping - detailed data models, object-specific operations, and extension implementations correctly belong in protocol mapping specifications rather than the architecture document.

**Overall Assessment**: The architecture document is well-structured and comprehensive for its intended scope. With the recommended updates addressing the four unfulfilled requirements and enhancing the partially fulfilled areas, it will provide a complete foundation for RPP protocol development.
