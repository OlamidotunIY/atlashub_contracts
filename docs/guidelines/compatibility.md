# Protobuf compatibility and governance

**State:** Team policy for draft and future stable schemas.

## Naming and evolution

- Protobuf packages and directories are versioned, for example `atlashub.commerce.v1alpha1` in `atlashub/commerce/v1alpha1`. Protos become stable `v1` only after producer/consumer tests and API signoff.
- Fields use lower_snake_case; messages PascalCase; services end in Service. Always assign unique numeric tags. Removed fields and enum numbers must be `reserved`, never reused.
- Prefer additive changes and clear defaults. Changing field meaning, required business semantics, wire type or authorization scope may break API even if the wire format technically accepts it.
- Standard Proto3 scalars may not distinguish absence from zero; use `optional` or `oneof` when absence matters. Document validity constraints separately.
- Money uses signed int64 minor units and ISO currency string. Test bigint mapping in TypeScript, precision in Java/Python, ISO currency minor-unit differences and serialization.
- A user lifecycle event does not include an implicit Organization. A later OrganizationCreated event must not imply provider banking, mandatory Anchor KYC/KYB or provider activation.

## Classify every service

**Public browser:** read-only published catalog plus explicitly trusted cart/checkout initiation. Host-attested tenant, no org spoofing.

**Authenticated management:** creators/merchants using a separate Portal or CLI with scoped server authorization; not for public theme browser package.

**Internal:** Java↔Python authorized tools, provider capabilities, payment/ledger, approvals, sensitive identity; never exported from public browser package.

**Events:** typed immutable past facts with event IDs, deduplication, timestamps and independent producer/consumer rollout. Existing Kafka event payloads need a migration decision, not silent replacement.

## Breaking checks

`buf.yaml` enables STANDARD lint and FILE breaking detection. Use `buf breaking --against '.git#branch=main'` on PRs and stable-version baselines; full Git history needed. This is a guard, not a substitute for security, domain semantics, live provider legality or consumer approval.

## Approval checklist

- Identified owner and all consumers.
- Public/internal/event classification.
- Auth, tenant and PII effects and allowed package exports reviewed.
- Compatibility/breaking review, field numbering, currency and optional presence checked.
- Java/TS/Python generated code and relevant real services/transport tests passed before stable package release.
- Deprecation, rollback and producer-first deployment order documented.

References: https://buf.build/docs/lint/usage/ and https://protobuf.dev/programming-guides/dos-donts/ .
