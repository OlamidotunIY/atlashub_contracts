# gRPC, Connect, browser transport and security

**Status:** Architectural selection, not implemented network endpoints.

| Caller | Recipient | Protocol | Main rule |
|---|---|---|---|
| Java backend | Python AI | Native gRPC / HTTP/2 | Private mutual authentication, delegated user permissions |
| Java backend | Trusted Node backend | Native gRPC where compatible (or HTTPS) | Service identity and scoped methods |
| Browser Theme SDK | Trusted public storefront gateway | **Connect or gRPC-Web** / HTTPS + Protobuf | Only public catalog/cart/checkout APIs |
| Auth Web, Atlas Web, Storefront Portal | Central Java/IdP | OIDC/OAuth via HTTPS + PKCE/BFF | SSO is not native gRPC |
| Browser voice | Trusted voice gateway | WebRTC or WebSocket | Bidi voice is not ordinary browser gRPC-Web |
| Java events | Internal consumers | Broker wire chosen separately | No unilateral Kafka migration due to .proto files |

A native Java gRPC server does **not automatically** speak Connect/gRPC-Web to a browser. Deploy and test a compatible gateway/adapter or an implementation that supports those protocols. Modern browsers generally cannot support client/bidirectional streaming through fetch as ordinary gRPC; server-streaming is available. Streaming voice uses a dedicated WebRTC/WebSocket path.

## Trusted principal and organization

- Java/IdP owns identity, permission and membership. Organization selection is independent from global User registration.
- Trusted storefront server verifies exact hostname → storefront → organization; browser input org ID cannot grant scope.
- Python AI tool gateway requires mTLS/short-lived delegated identity, scoped tool ID, Java-side schema and approval, idempotency and provider capability checks.
- Theme JavaScript must not receive privileged payment/merchant/HR/ledger tokens, or any internal service client.
- Provider-specific KYB may apply when merchant activates payment collection, not ordinary account registration.

Protobuf enforces message shape, **not** business validation, identity, authorization, data confidentiality or network isolation. Protect endpoints with TLS, audience-scoped tokens, access policy and validated origin/CORS where browsers are involved.

## Error and retry

Use standard gRPC statuses for transport/permission failures and typed workflow status only when semantically meaningful. Bound request sizes, deadlines and resource budgets. Mutations need idempotency keys; unknown payment/payout outcomes must never be fabricated as success.

References:
- https://connectrpc.com/docs/web/getting-started/
- https://grpc.io/docs/platforms/web/basics/
- https://buf.build/docs/bsr/generated-sdks/
