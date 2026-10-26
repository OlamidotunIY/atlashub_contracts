# Contracts repository layout

**Status:** Actual files listed here exist; future folders are proposals. No Turborepo/Nx or Git submodules.

```text
atlashub_contracts/
├── proto/atlashub/
│   ├── common/v1alpha1/
│   │   ├── money.proto
│   │   └── pagination.proto
│   ├── commerce/v1alpha1/public_catalog.proto
│   ├── identity/v1alpha1/lifecycle_events.proto
│   ├── intelligence/v1alpha1/tool_gateway.proto
│   ├── integrations/v1alpha1/       # FUTURE
│   └── storefront/v1alpha1/         # FUTURE
├── docs/
│   ├── architecture/
│   ├── guidelines/
│   └── modules/
├── .github/workflows/buf-checks.yml
├── .github/pull_request_template.md
├── buf.yaml
├── buf.gen.java.yaml
├── buf.gen.python.yaml
├── buf.gen.ts.yaml
└── README.md
```

## Naming and versioning

A package such as `atlashub.commerce.v1alpha1` matches directory `atlashub/commerce/v1alpha1/`. Fields use snake_case, messages PascalCase, RPC service names end in Service. Publish stable `v1` only after producer/consumer agreement; initial `v1alpha1` contracts are not implemented APIs and can change through review.

Set Java-specific `java_package` and `java_multiple_files` in proto3 files. Generated TS/Python/Java output goes under ignored `gen/` during experimentation, not as duplicate checked-in application source. When stable, publish generated language SDK packages.

## Public package filtering

The default TypeScript generation template processes all files for **trusted development**. It does not automatically create a safe browser package. Packaging must explicitly include only reviewed public services, their messages and transitive imports; internal AI tooling and identity event contracts must be excluded from the public Theme SDK artifacts.
