# Generated SDK package distribution

**Status:** Proposal. No Maven, npm or Python package has been published or registry connected.

The Contracts repo is the single source of .proto schemas. Publish generated code as **normal build-time dependencies** rather than Git submodules. Dependency injection/ports-and-adapters then wires generated clients into local infrastructure.

| Runtime | Planned artifact | Scope |
|---|---|---|
| Java | `com.atlashub:contracts-java` | Protobuf messages and native gRPC stubs |
| Python | `atlashub-contracts` | Generated messages and grpcio stubs |
| Node trusted | `@atlashub/contracts-internal` | Internal approved TS service types |
| Browser Theme SDK | `@atlashub/contracts-public` | **Only** public catalog/cart/checkout types |
| Portal and CLI | `@atlashub/contracts-management` | Authenticated management/creator API types |

**Names are illustrative.** Buf Schema Registry generated SDK coordinates differ from custom npm/Maven/PyPI aliases. Select actual registry, publication scope, visibility, runtime and package versions first.

## Development generators

```bash
buf lint
buf build
buf generate --template buf.gen.java.yaml
buf generate --template buf.gen.python.yaml
buf generate --template buf.gen.ts.yaml
```

The three generation templates currently reference unpinned remote plugins. **Pin tested plugin versions before any tagged or production SDK release**. Generated outputs go to ignored `gen/` and are not automatically published. Java Gradle, Python package and TypeScript workspace packaging still require their own build/test steps.

Consumer examples (after packages exist):

```groovy
dependencies {
    implementation "com.atlashub:contracts-java:1.0.0"
}
```

```bash
pip install atlashub-contracts==1.0.0
npm install @atlashub/contracts-public @connectrpc/connect @connectrpc/connect-web
```

These are **examples, not functioning install commands today**. Browser SDK must use a Connect/gRPC-Web-capable gateway and expose only allowlisted public services. Java/Python map generated messages at RPC boundaries, never in core business domain.

## Release and migration

1. Producer and all consumers review .proto change, exposure tier and semantics.
2. Buf lint, build and breaking check; generate Java/Python/TypeScript and compile contract clients/servers.
3. Tag source commit and publish exact-version language packages with provenance, dependencies and compatibility notes.
4. Consumer repos pin versions and migrate via PRs; keep backwards-compatible service implementations during staggered rollouts.
5. For breaking contracts, publish a new versioned package, run old/new concurrently and reserve removed field names/numbers.

See https://buf.build/docs/bsr/generated-sdks/ and https://buf.build/docs/generate/remote-plugins/ .
