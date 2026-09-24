# Scaling and Reliability Rubric

Use this rubric when the user asks for high scale, Netflix-scale, high availability, fault tolerance, or low latency.

## Capacity Questions

Answer before designing scale:

- How many active users/producers?
- How many writes/sec average?
- How many writes/sec peak?
- How many reads/sec average?
- Which reads are hot?
- How much data per event?
- What is daily/monthly storage growth?
- Is ordering required globally or per entity?
- What is acceptable data freshness?
- What is acceptable data loss? Usually zero for business events.

## Latency Budget

Break end-to-end latency into:

- Producer/network time
- Gateway validation
- Queue delay
- Worker processing
- Database write
- Cache/read model update
- Realtime delivery
- Client rendering

If p95 is too high, optimize the biggest budget item first.

## Reliability Targets

Define:

- RPO: how much data can be lost
- RTO: how quickly service must recover
- Availability target
- Degraded mode behavior
- Replay/reconciliation procedure

For event ingestion, RPO should usually be zero after producer has accepted the scan/event locally.

## Failure Mode Checklist

Producer:

- Offline
- Clock skew
- Duplicate send
- Local storage full
- Bad payload

Gateway:

- Rate limited
- Partial outage
- Validation bug
- Idempotency store unavailable

Queue:

- Backlog
- Poison message
- Consumer lag
- Duplicate delivery
- Partition hot spot

Worker:

- Crash mid-processing
- Slow processing
- Out-of-order event
- Non-idempotent side effect

Database:

- Primary unavailable
- Slow writes
- Lock contention
- Index bloat
- Migration failure

Cache/read model:

- Stale data
- Cache miss storm
- Lost invalidation
- Rebuild required

Client:

- Offline
- Reconnect duplicates updates
- Stale local cache
- Slow network

## Scale Evolution

### Stage 1: Correct MVP

- Single API service
- Durable DB
- Local buffering at producer if needed
- Basic queue
- Idempotent worker
- Simple cache/read model
- Logs and basic metrics

### Stage 2: Production Growth

- Stateless horizontal API replicas
- Managed queue/stream
- Worker autoscaling
- Redis or managed cache
- Read replicas for history
- Better alerting and dashboards
- Dead-letter replay tooling

### Stage 3: High Scale

- Partitioned stream by entity key
- Dedicated ingestion service
- Dedicated read API
- Materialized views
- Database partitioning/sharding
- Regional failover
- Load testing and chaos testing
- Automated reconciliation jobs

### Stage 4: Netflix-Like Scale

Only justify this if user scale requires it.

- Multi-region active-active or active-passive
- Regional queues and regional write ownership
- Global traffic manager
- Cell-based architecture
- Service mesh or strong platform operations
- Automated canary deploys
- Circuit breakers and bulkheads
- Multi-layer caching
- Centralized observability and tracing

## Correctness Over Scale

Do not scale a system by sacrificing:

- No lost events
- Correct idempotency
- Correct ordering scope
- Recomputability
- Privacy/security boundaries
- Operational visibility

Scale problems are easier to fix than corrupted facts.
