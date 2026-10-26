# Contracts CI and generated-package release rules

**Status:** Basic GitHub Actions Buf lint/build/breaking configured; **generated language SDK publication and cross-repository test gates are not implemented**.

## Current checks

- Every PR/push: Buf lint and Buf build from root `buf.yaml`.
- PR against main: `buf breaking --against '.git#branch=main'` with full Git history.
- Workflow does not publish artifacts or push to a Buf registry.
- Remote generator templates for Java, Python and TS are for development: pin exact versions and validate runtime dependencies before stable release.

## Required future tests

| Owner | Consumer | Validation |
|---|---|---|
| Java Commerce | trusted Storefront gateway | Product schema mapping, tenant and price constraints, not status spoofing |
| Storefront gateway | browser public Theme SDK | Connect or gRPC-Web compatible gateway, CORS, no internal method exports, no cross-tenant reads |
| Java Tool Gateway | Python AI | Native mTLS gRPC, delegated identity, tool schema, approval/idempotency and status |
| Java Identity | internal event subscribers | Separate UserRegistered and OrganizationCreated events; no automatic banking/KYB |
| Node Storefront | Portal/creator CLI | Versioned scoped management contracts after implemented |
| Generated SDK packages | each runtime | Gradle, TypeScript, Python compile, actual package installation and codegen reproduction |

## Release workflow (future)

1. Review and merge schema changes with producer and consumer owners.
2. Pin generator plugin versions and dependency versions; verify Java, Python and TS codegen.
3. Filter public TS packages to allowlisted safe services and messages; prevent internal AI/financial clients from entering browser packages.
4. Tag immutable source and publish versioned Maven/npm/Python artifacts using short-lived CI identities and SBOM/provenance.
5. Consumers update pinned dependencies independently and validate contract+application tests.
6. Run staggered production rollout, retain previous service version during migration and monitor errors.
7. Mark stable packages only after the end-to-end compatibility checks pass.

## Definition of done

New/changed .proto has owner, API exposure, compatibility checks, runtime consumer tests, security review and documented rollout. Buf lint/build alone is insufficient to call gRPC integration implemented.
