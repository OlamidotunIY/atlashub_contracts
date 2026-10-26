# Shared Contracts: DDD, Clean and Hexagonal Architecture

**Status:** Contract governance and transport DTOs only. This is not a deployable service or a domain-model library.

## Responsibility and dependency direction

- Each service owns its **private business domain**. Java owns User, Organization, commerce, financial and provider rules. Python owns conversations, AI plans and risk signals. Node owns theme catalog, installations, assets and publications.
- This repository owns cross-repository **wire schema**, semantic meaning, service ownership, API version, exposure tier and compatibility. No Spring aggregates, React hooks, Python ML implementations, database entities or authorization policies belong here.
- Generate Protobuf message classes and service descriptors, then map them to/from *local application DTOs* in **inbound/outbound adapters**. Never treat generated Protobuf objects as Java JPA entities or domain aggregates.
- Domain and application layers import inward abstractions, not generated transport code. Network protocols are infrastructure/presentation edges, not business logic.

## Contract families

| Contract | Producer/owner | Expected consumer | Exposure |
|---|---|---|---|
| Common Money and Pagination | Contract maintainers | Java, TypeScript, Python | Shared |
| Public Commerce | Java Commerce | Trusted storefront gateway + public Theme SDK | Public-scoped |
| Identity lifecycle | Java accounts/IAM | Internal authenticated consumers | Event/internal |
| AI authorized tool gateway | Java tools/IAM | Python Intelligence | **Private internal** |
| Future integrations eligibility | Java integrations | Node/Python trusted services | Private/internal |
| Future themes/installations | Node Storefront | Authenticated Portal/CLI | Authenticated management |

A User can register with **zero Organizations**. `UserRegisteredEvent` and `OrganizationCreatedEvent` must remain independent. Neither requires an Anchor bank account or Atlas-owned KYB. Provider eligibility remains capability-specific.

## Hexagonal / CQRS guidance

Schema does not bypass application commands and queries. Implement gRPC interfaces as inbound adapters, invoke application use cases, map results back to messages. Tool calls and payment commands require Java IAM, tenant verification, idempotency and truthful results; merely possessing a Protobuf client gives no permission.

Use CQRS intentionally for distinct state-mutating operations and read projections. Avoid exposing one gRPC method for every private aggregate method or distributing Java domain class definitions.

## Browser security

Generated JS can represent a private service even when it is not callable: **never publish internal tool/payment/credential service descriptors in browser public packages**. The Theme SDK only imports an reviewed public contract subset. Browser session authentication remains OIDC/HTTPS, not gRPC auth.
