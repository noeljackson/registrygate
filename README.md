# Registrygate

A proposed self-contained Rust package registry gateway. Initial adapters are
**Cargo, npm, and Go**, followed by Python. It fetches and verifies immutable
package artifacts, then enforces an explicit decision before releasing bytes.

[Tachikoma](https://github.com/noeljackson/tachikoma) is the proposed independent
approval service for shared policy, vulnerability lookup, AI assessment, human
review and audit. Registrygate connects through a versioned approval API;
[Snitchgate](https://github.com/noeljackson/snitchgate) is another independent
producer for network requests. Registrygate does not require Snitchgate.

The design includes an explicit local policy backend so the gateway can run
without a central approver. Losing a configured remote approver does not select
a permissive fallback.

- [Registrygate architecture](docs/architecture.md)
- [Tachikoma integration](docs/tachikoma-integration.md)
- [Tachikoma queue and approval contracts](https://github.com/noeljackson/tachikoma/blob/main/docs/queue-control-plane.md)

Status: architecture proposal. Registry adapters and the approval API client
are not implemented yet; no client registry settings are changed by this repo.
