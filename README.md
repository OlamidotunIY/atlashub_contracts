# Atlas Hub Shared Contracts

**Status: documentation and `v1alpha1` Protobuf starter schemas.** This repository is the canonical source for **wire contracts**, not a domain model library or a deployed API. Java, Python and TypeScript applications must keep their own DDD/Clean/Hexagonal business models and map generated Protobuf messages at their inbound/outbound adapters.

## What belongs here

- Versioned `.proto` messages and service definitions used **across** repository boundaries.
- Public/storefront and private/internal service boundaries **clearly separated**, plus approved event envelopes.
- API naming/versioning, ownership, compatibility and deprecation rules.
- Buf configuration for lint/build/breaking-change checks and future language SDK generation.
- Contract release/consumer compatibility documentation.

**Never** place passwords, OAuth session implementation, secret keys, Spring entities, Prisma schemas, React components, business decision logic, or arbitrary internal administrator RPCs into public browser packages.

## Architecture

| Producer / consumer | Wire transport | Generated code |
|---|---|---|
| Java ↔ Python Atlas Intelligence | **Native gRPC** over authenticated private HTTP/2 | Java classes/stubs + Python message/stubs |
| Java ↔ Node Storefront Backend (trusted) | **Native gRPC** where deployment/gateway supports it | Java classes/stubs + Node TypeScript client |
| Browser Theme SDK ↔ public Storefront gateway | **Connect or gRPC-Web** over HTTPS, backed by an explicitly compatible server/gateway | Browser-safe TypeScript Protobuf/Connect client |
| Browser login and SSO | **OIDC/OAuth HTTPS** (not a theme RPC), Java/IdP authority | Separate authentication client |
| Browser real-time voice | WebRTC/WebSocket to trusted gateway, internal gRPC streaming | No direct browser native bidi gRPC |
| Asynchronous events | Versioned event schema; transport determined separately | Serialized Protobuf only if adopted by producer/consumers |

**Protobuf is the schema; gRPC is one possible transport.** Using Protobuf does not magically expose native gRPC to JavaScript browsers, nor does it replace OAuth/OIDC. Theme SDK code gets only the approved *public* catalog/cart/checkout API, never Java's internal tool, ledger, payout or merchant credentials.

## Contracts now present

- [Shared currency-safe money and pagination messages](proto/atlashub/common/v1alpha1/money.proto)
- [Public commerce catalog API draft](proto/atlashub/commerce/v1alpha1/public_catalog.proto)
- [Private Java ↔ AI tool gateway draft](proto/atlashub/intelligence/v1alpha1/tool_gateway.proto)
- [Independent identity and business lifecycle event draft](proto/atlashub/identity/v1alpha1/lifecycle_events.proto)

These are **alpha designs, not deployed/signed-off business API contracts**. Do not build production integrations until the owning Java/Storefront teams approve, implement and test the exact schema.

## Key docs

- [DDD/Clean/Hexagonal boundaries](docs/architecture/clean-hexagonal.md)
- [Folder structure and package boundaries](docs/architecture/folder-structure.md)
- [Transport and trust design](docs/architecture/transports-security.md)
- [Package distribution to Java, Python and TS](docs/architecture/generated-packages.md)
- [Compatibility and naming rules](docs/guidelines/compatibility.md)
- [Testing/CI/release plan](docs/guidelines/testing-release.md)
- [Module ownership and roadmap](docs/modules/index.md)

## CLI (after installing Buf)

```bash
buf lint
buf build
# Once a base branch/tag exists and has stable proto content:
buf breaking --against '.git#branch=main'
```

`buf.yaml` uses current Buf v2 format, with package-level versioning and strict `STANDARD` lint / `FILE` breaking checks. Generated Maven/npm/PyPI SDK packages are **planned, not published**. Pin generator plugin versions before the first release; do not claim an SDK can be installed yet.

## Repo separation / boundaries

[Java backend](https://github.com/OlamidotunIY/atlashub_backend) · [Python Intelligence](https://github.com/OlamidotunIY/atlashub_intelligence) · [Storefront management/hosting](https://github.com/OlamidotunIY/atlashub_storefront) · [Theme SDK](https://github.com/OlamidotunIY/atlashub_theme_sdk) · [Storefront Portal](https://github.com/OlamidotunIY/atlashub_storefront_portal) · [Auth Web](https://github.com/OlamidotunIY/atlashub_auth_web) · [Atlas Web](https://github.com/OlamidotunIY/atlashub_web).

This is an independent contracts repository. **No Turborepo/Nx or Git submodule** is required. Applications consume tagged/generated versioned dependencies, not copy/paste schemas.

Reference standards: [Buf configuration](https://buf.build/docs/configuration/v2/buf-yaml/), [Connect browser transport](https://connectrpc.com/docs/web/getting-started/), [Protobuf best practices](https://protobuf.dev/programming-guides/dos-donts/).
