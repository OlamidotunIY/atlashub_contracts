## Contract scope and owner

- [ ] Producer/API owner identified and impacted Java, Python, Node and browser consumers listed.
- [ ] Schema package/version and public-vs-internal-vs-event classification documented.
- [ ] No sensitive AI tool, payment or authentication service unintentionally exported to browser.
- [ ] Protobuf fields/numbers/enum values/presence/money semantics reviewed.

## Backward compatibility

- [ ] Buf lint and build pass.
- [ ] Buf breaking result reviewed, old field numbers/names reserved if removed.
- [ ] Java, Python and TS generated types compile where affected.
- [ ] Producer-first deployment and consumer upgrade timeline documented.

## DDD/Security

- [ ] Generated DTOs remain in adapter boundary, not used as domain aggregates.
- [ ] Tenant/auth/gateway trust boundary and cross-org data separation reviewed.
- [ ] Provider state/KYB is capability-specific, not mandatory user signup.
- [ ] No secret/token/biometric/private customer data added to public contracts.

## Delivery

- [ ] Tests, changelog, package version and documentation updated.
- [ ] No assertion of implementation/published artifact unless verified.
