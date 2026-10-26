# Module-by-module contract ownership and roadmap

**Current:** common money/pagination, public catalog **draft**, AI tool gateway **draft**, independent identity lifecycle **draft**; no published generated SDKs.

| Module | Producer | Consumers | Exposure | Status |
|---|---|---|---|---|
| Common Money, Pagination | Shared wire schema maintainers | Java/TS/Python | Common | v1alpha1 starter |
| Public Catalog GetProduct, ListProducts | Java Commerce | public gateway/Theme SDK | Public-scoped | v1alpha1 draft |
| Authorized AI tool gateway | Java tool authorization | Python AI | **Private** | v1alpha1 draft |
| UserRegistered/OrganizationCreated | Java Identity/Accounts | trusted service subscribers | Internal event | v1alpha1 draft |
| Checkout/cart operations | Java Commerce | public Theme SDK via trusted gateway | Public constrained | Planned, awaiting domain decision |
| Merchant-connected provider capabilities | Java Integrations | trusted Node/Python services | Private | Planned, no credentials exposed |
| Theme marketplace/install/publish | Node Storefront | Portal/creator CLI | Authenticated management | Planned |
| AI voice streaming | Python/Java gateway | internal transport | Private | Planned; browser WebRTC/WS |
| IAM session/OIDC | Java IdP | Auth Web/Portal/Atlas Web | **OAuth/HTTPS, not Protobuf login** | Separate implementation |

## Delivery order

1. Shared contract repository rules, Buf v2 lint/build, breaking baseline, alpha common/messages, GitHub CI.
2. Implement Java public commerce API and compatible browser gateway with host-attested storefront scope, verified two-merchant tests.
3. Implement Java↔Python native gRPC tool service with actual Java authorization and preview/approvals.
4. Publish versioned generated Java/Python/TS packages and test each consumer installed dependency; keep public/internal npm exports separate.
5. Add checkout/creator/provider and event schemas only when producers agree business semantics. Release cross-repo contract changes with compatibility/rollout guidance.

## Policy

Proto DTOs are adapter contracts, **not** DDD domain entities. No compulsory Anchor bank/KYB on registration; optional merchant-connected providers only. Stable contracts require service-owner approvals, language SDK compilation and security tests, not merely valid .proto syntax.
