# SFLOW

**Rust-native workflow engine — W3C Statecharts (xstate v5-compatible).**

> Durable, distributed, embeddable workflow engine for the
> [structured.world](https://structured.world) ecosystem and beyond.

| | |
| --- | --- |
| **Project home** | <https://structured.flow> |
| **Docs** | <https://structured-world.github.io/sflow> |
| **Embedding contract** | [sflow-api](https://github.com/structured-world/sflow-api) (Apache-2.0) |
| **gRPC definitions** | [sflow-proto](https://github.com/structured-world/sflow-proto) (Apache-2.0) |
| **Vue components** | [@structured-world/sflow-vue-ce](https://www.npmjs.com/package/@structured-world/sflow-vue-ce) (Apache-2.0) |

## Editions

| Edition | Quota | Distribution | Source |
|---------|-------|--------------|--------|
| **CE** | Free up to 10 000 state changes per month | binary download — see [Releases](https://github.com/structured-world/sflow/releases) | proprietary, not published |
| **EE** | Prepaid tiers (100K — 10M+ state changes per month, clustering, marketplace, advanced UI) | commercial channel | proprietary, not published |

## Why binary-only

SFLOW core IP (statechart interpreter, actor router, integrity enforcement) lives in a private repository. The single open boundary is the Apache-2.0 [`sflow-api`](https://github.com/structured-world/sflow-api) crate — stable trait contracts that the proprietary engine implements and that consumer products import.

This isolates license worlds cleanly:

```
PROPRIETARY (sflow-engine, distributed as binary)
       │ implements trait
       ▼
  APACHE-2.0   sflow-api   (this crate)
       ▲ consumes trait
       │
CONSUMER (any license — Apache, MIT, AGPL, BSD, proprietary, ...)
```

AGPL applications (CoordiNode CE, future SID CE) can import the API without inheriting AGPL. Proprietary applications can link the engine without inheriting any open-source obligation.

## Installation

### Standalone server (Linux / macOS)

```bash
# CE — free up to 10K state changes/month
curl -L https://github.com/structured-world/sflow/releases/latest/download/sflow-server-linux-x86_64.tar.gz | tar xz
./sflow-server
```

Docker:

```bash
docker run -p 50051:50051 ghcr.io/structured-world/sflow:latest
```

### Embedded in your Rust application

Add the public Apache-2.0 API crate to your `Cargo.toml`:

```toml
[dependencies]
sflow-api = "0.1"
```

Then download the proprietary engine binary from the [Releases](https://github.com/structured-world/sflow/releases) page (or the embedded distribution channel for commercial customers) and link it at build time. See the [embedding guide](https://structured-world.github.io/sflow/embedding/) for the full recipe.

## Architecture in one paragraph

A workflow definition is an [xstate v5](https://stately.ai/docs/xstate) JSON document. The engine interprets it as a hierarchical statechart with parallel regions, history states, delayed transitions, eventless transitions, guards (CEL expressions), and saga compensation. State persists through a pluggable `PersistenceBackend` (MongoDB, PostgreSQL, NATS JetStream, Kafka, or in-process; CoordiNode-embed is the upcoming HA backend with no external DB). Consistency is enforced by a pluggable `ConsensusBackend` (DB-CAS by default, openraft for embedded products). Activities run as in-process actors or external workers via gRPC with capability-based routing.

## Use cases

SFLOW powers workflows across the structured.world product ecosystem:

- Identity processes (SID): account recovery, org onboarding, credential lifecycle
- Commerce (SCOM, SBILL): order fulfillment, return/exchange, dunning, subscription lifecycle
- HR (SHR): employee onboarding/offboarding, leave approval, performance reviews
- Operations (SINF, SNET, SSEC, SPASS): server provisioning, change approval, vulnerability response, credential rotation
- Domain verticals (orentary, SRNT, SEXC, pwr.events, …): booking sagas, dispute resolution, lifecycle entities
- Consumer (qrowner, SMOD, SREV, SMKT): moderation pipelines, dispute resolution, lost-item alerts

## Licensing

- **Engine source code** — proprietary, never published
- **`sflow-api` crate** — MIT OR Apache-2.0, [github.com/structured-world/sflow-api](https://github.com/structured-world/sflow-api)
- **`sflow-proto` definitions** — Apache-2.0, [github.com/structured-world/sflow-proto](https://github.com/structured-world/sflow-proto)
- **`@structured-world/sflow-vue-ce` Vue components** — Apache-2.0, [npm](https://www.npmjs.com/package/@structured-world/sflow-vue-ce)
- **Binary releases (CE)** — free for use up to 10K state changes/month per [CE EULA](LICENSE-CE)
- **Binary releases (EE)** — commercial license, [contact us](https://structured.flow/contact)

## Links

- Documentation: <https://structured-world.github.io/sflow>
- Releases: <https://github.com/structured-world/sflow/releases>
- Status page: <https://status.structured.flow>
- Community discussions: [GitHub Discussions](https://github.com/structured-world/sflow/discussions)
- Security advisories: see [SECURITY.md](SECURITY.md)
