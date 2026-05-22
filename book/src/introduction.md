# Introduction

**SFLOW** is a Rust-native workflow engine built on W3C Statecharts (xstate v5-compatible). It is the durable, distributed, embeddable workflow runtime for the [structured.world](https://structured.world) product ecosystem and for external customers who need workflow infrastructure without managing an external database cluster.

## Key properties

- **Statechart formalism.** Workflow definitions are xstate v5 JSON — hierarchical states, parallel regions, history states, delayed transitions, eventless transitions, guards (CEL expressions), saga compensation.
- **Embeddable.** The engine ships as a Rust library that compiles into any product binary (CoordiNode, SID, SCOM, …). No separate workflow service to deploy.
- **Standalone.** The same engine runs as a standalone gRPC server (`sflow-server`) for customers who want a dedicated workflow service.
- **Pluggable storage.** MongoDB, PostgreSQL, NATS JetStream, Kafka, in-process. CoordiNode-embed coming as a single-binary HA backend (no external DB).
- **Multi-language clients.** TypeScript, Python, Rust, with auto-generated stubs from `sflow-proto`.

## Two ways to use SFLOW

### As a standalone product

Download the [`sflow-server`](https://github.com/structured-world/sflow/releases) binary. Define workflows in xstate JSON. Send events via gRPC. Visualize state with the bundled dashboard.

```bash
curl -L https://github.com/structured-world/sflow/releases/latest/download/sflow-server-linux-x86_64.tar.gz | tar xz
./sflow-server
```

CE is free up to 10 000 state changes per month. EE has prepaid tiers and adds clustering, marketplace, and advanced UI widgets.

### Embedded in your application

Add the Apache-2.0 [`sflow-api`](https://github.com/structured-world/sflow-api) crate as a Rust dependency. Link the proprietary engine binary (downloaded from the release page) at build time. Workflows run in-process — no network hops, no separate deployment, no metering on the host product's built-in workflows.

```toml
[dependencies]
sflow-api = "0.1"
```

This is how SFLOW lives inside CoordiNode (cluster operations), SID (account lifecycle), and other structured.world products.

## What this documentation covers

- **Getting Started** — install the standalone server, run your first workflow
- **Concepts** — statechart format, sagas, blocks, persistence
- **Embedding** — integrate the engine into a Rust product
- **gRPC API** — service-by-service reference for clients
- **Clients** — TypeScript, Python, Rust SDKs
- **Operations** — deployment, clustering, monitoring, backup
- **Licensing** — editions, metering, commercial terms

## What is NOT here

- **Engine source code** — proprietary, lives in a private repository, never published
- **Block marketplace runtime** — EE-tier feature, see commercial documentation
- **Integrity enforcement internals** — anti-bypass mechanism, by design opaque

For the public embedding contract, see the [sflow-api](https://github.com/structured-world/sflow-api) repository — that crate contains the full trait surface that the engine implements.

## Related projects

- [coordinode](https://github.com/structured-world/coordinode) — unified graph + vector + spatial + document + time-series database. Embeds SFLOW for cluster lifecycle operations.
- [sflow-proto](https://github.com/structured-world/sflow-proto) — Apache-2.0 gRPC service definitions
- [sflow-api](https://github.com/structured-world/sflow-api) — Apache-2.0 Rust embedding contract
- [@structured-world/sflow-vue-ce](https://www.npmjs.com/package/@structured-world/sflow-vue-ce) — Apache-2.0 Vue 3 components for workflow UIs
