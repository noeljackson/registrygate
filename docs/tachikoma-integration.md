# Registrygate integration with Tachikoma

Registrygate owns package protocols, local facts, quarantine, byte integrity,
storage and delivery. Tachikoma owns durable review queues, external advisory
lookups, risk policy, AI assessment, human transports and the decision audit.
Snitchgate is an independent network producer using the same approver API.

Use the [Tachikoma queue control plane proposal](https://github.com/noeljackson/tachikoma/blob/main/docs/queue-control-plane.md)
as the contract source. API schemas and a lightweight client SDK belong there;
Registrygate consumes a compatible version without importing Snitchgate types.

Initial queue mapping is `packages.cargo`, `packages.npm` and `packages.go`,
all using the artifact subject contract. Python follows with its own adapter
and queue. The queues can share an assessment policy while retaining distinct
limits, evidence configuration and human routing.

For each requested artifact, Registrygate resolves immutable coordinates,
fetches into bounded quarantine, computes its digest and checks available
integrity evidence. It submits the subject and a sanitized facts snapshot to
Tachikoma with a producer request ID, idempotency key and deadline. It watches
for a scoped decision and validates it against the still-pending transfer.
Only then does it serve the already-verified immutable bytes.

Neither a coordinate-only approval nor a response for another queue, producer,
digest, deadline or use scope can release the artifact. Cancellation and HTTP
timeouts revoke that transfer's eligibility. A cache hit still needs current
authorization. A reported delivery receipt is not proof of installation.

The remote backend is optional configuration. An explicit local-rules backend
can operate the gateway independently; observation is visibly non-enforcing.
Manual mode needs a human-capable backend. A failed remote backend holds or
rejects downloads under the original deadline, with no silent local allow.

The first implementation should use mock approval services and isolated stock
client tests before connecting the two real services. Package and network
queues need separate admission and assessment budgets so a large dependency
installation does not exhaust Snitchgate's short request deadline.
