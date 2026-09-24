# Pattern Library

Use these as architecture building blocks. Pick only the patterns justified by requirements.

## API Gateway

Use when clients or hardware producers need a stable entrypoint.

Responsibilities:

- Routing
- Authentication/authorization
- Rate limiting
- Request validation
- Idempotency enforcement
- Correlation ID generation
- Versioned public contract

Avoid putting business logic in the gateway. Keep it thin.

## Load Balancer

Use to distribute traffic across stateless application instances.

Key choices:

- L4 vs L7
- Health checks
- Sticky sessions only if unavoidable
- Autoscaling based on CPU, request rate, latency, or queue lag

Prefer stateless services so load balancing stays simple.

## Durable Queue or Stream

Use when writes cannot be lost, producers can retry, or processing can be asynchronous.

Examples:

- SQS, Pub/Sub, RabbitMQ: work queue
- Kafka, Redpanda, Pulsar: ordered durable event stream
- Redis Streams: simpler stream for moderate workloads

Design points:

- Idempotency key
- Ordering key
- Retry policy
- Dead-letter queue
- Consumer group
- Backpressure behavior
- Replay strategy

Rule: acknowledge producer only after the event is durably accepted.

## Event Sourcing Lite

Use when raw events are valuable for audit, recomputation, or correcting out-of-order arrivals.

Pattern:

- Store immutable raw event
- Process into derived facts
- Recompute derived facts if late data changes history

Do not use full event sourcing ceremony unless the domain needs it.

## CQRS Lite

Use when write shape and read shape differ.

Pattern:

- Write model: normalized, auditable, correctness-first
- Read model: denormalized, query-fast, rebuildable

This is common for mobile dashboards and realtime summaries.

## Cache-Aside

Use for hot read paths.

Flow:

1. Read from cache.
2. On miss, read DB.
3. Populate cache.
4. Expire or invalidate on write.

Risks:

- Stale cache
- Cache stampede
- Big keys
- Inconsistent TTL choices

Mitigations:

- Short TTL for volatile summaries
- Single-flight or lock for expensive recompute
- Explicit invalidation after important writes

## Materialized View / Read Model

Use for dashboards, stats, history summaries, leaderboards, and feeds.

Good for:

- Latest state per user
- Weekly summaries
- Personal bests
- Badge progress
- Ranking snapshots

Store enough metadata to rebuild it.

## Realtime Delivery

Options:

- WebSocket: bidirectional, richer connection lifecycle
- SSE: one-way server-to-client stream, simpler for updates
- Push notification: good for background alerts, not reliable for instant in-app state
- Polling: simplest fallback

For mobile dashboards, WebSocket or SSE plus REST refresh is usually enough.

## Idempotency

Use for every retried write.

Common keys:

- Client-generated request ID
- Natural unique tuple, for example `(device_id, timestamp, sequence_number)`
- Event UUID generated at producer

Backend should return the same result for repeated idempotency keys.

## Ordering

Ordering is expensive. Scope it narrowly.

Good ordering keys:

- User ID
- Device ID
- Wristband ID
- Account ID

Avoid global ordering unless explicitly required.

## Dead Letter Queue

Use when messages fail repeatedly.

Include:

- Original payload
- Error reason
- Attempt count
- Last failed timestamp
- Correlation ID

Build a replay path or manual repair process.

## Database Read Replicas

Use when DB reads exceed primary capacity or analytics/history reads interfere with writes.

Risks:

- Replication lag
- Read-your-writes inconsistency

Route critical fresh reads to cache/read model or primary when necessary.

## Sharding / Partitioning

Use when one database cannot handle data volume, write rate, or hot indexes.

Shard keys should match access patterns:

- user_id for user-centric apps
- venue_id for multi-venue systems
- time bucket for time-series data
- composite keys when one dimension gets hot

Do not shard early. It is operationally expensive.

## Multi-Region

Use only when requirements demand regional disaster recovery, global latency, or strict availability.

Patterns:

- Active-passive: simpler failover
- Active-active: lower latency, much harder consistency
- Regional writes with global reads: common middle path

State conflict resolution before choosing active-active.
