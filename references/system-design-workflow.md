# System Design Workflow

Use this workflow for architecture design, take-home assignments, ADRs, and interview-style system design answers.

## 1. Clarify Scope

Define:

- Primary actors
- Core user journeys
- In-scope capabilities
- Explicit non-goals
- External systems
- Trust boundaries
- Assumptions forced by missing requirements

Convert vague language into targets:

- "Realtime" -> target update latency, for example p95 under 3 seconds
- "No lost data" -> durable local buffer plus backend idempotency
- "Scalable" -> expected writes/sec, reads/sec, storage growth, hot partitions
- "Available" -> acceptable downtime, failover behavior, degraded mode

## 2. Extract Requirements

Split requirements into:

- Functional: what the system does.
- Non-functional: latency, durability, availability, correctness, security, operability, cost.
- Product constraints: deadline, team size, interview context, existing platform choices.
- Technical constraints: hardware contract, mobile platform, cloud/provider, database, networking.

Always surface missing requirements as assumptions. Do not hide them.

## 3. Estimate Capacity

Use simple math:

- Writes/sec average and burst
- Reads/sec average and burst
- Storage per event and annual growth
- Cacheable vs non-cacheable reads
- Hot keys or hot partitions
- Fan-out size for notifications or realtime updates

Then decide whether the system needs:

- One instance
- Horizontal stateless replicas
- Queue buffering
- Read cache
- Read replicas
- Partitioning/sharding
- Stream processing
- Multi-region active-active

## 4. Start with a Simple Architecture

Build the first diagram from:

- Client or producer
- API gateway or ingestion gateway
- Application service
- Durable queue or stream
- Worker or processor
- Source-of-truth datastore
- Cache or read model
- Realtime/push channel
- Observability

Keep the first diagram small enough to explain in one minute.

## 5. Define Contracts

For APIs:

- Endpoint
- Method
- Request body
- Response body
- Auth model
- Idempotency behavior
- Error cases

For events:

- Event name
- Producer
- Consumer
- Schema
- Required idempotency key
- Ordering key
- Retry/dead-letter behavior
- Versioning

## 6. Design Data

Separate:

- Raw source-of-truth facts
- Derived facts
- Aggregates
- Read models
- Cache keys
- Audit logs

For every key table/entity, define:

- Primary key
- Important indexes
- Partitioning/sharding candidate
- Retention policy
- Recompute path

## 7. Walk the Main Flows

At minimum, trace:

- Happy path
- User read path
- Write retry path
- Duplicate event path
- Out-of-order event path
- Recovery/replay path

Prefer Mermaid `sequenceDiagram` for flows with temporal ordering.

## 8. Make Failure Modes First-Class

For each major component, ask:

- What if it is slow?
- What if it crashes?
- What if it duplicates data?
- What if it loses connection?
- What if it receives stale or out-of-order data?
- What if downstream is unavailable?

Define system behavior under failure, not just component behavior.

## 9. Choose Technologies

Choose boring, well-known technology unless the requirement forces a specialized one.

Good decision format:

- Choice
- Why it fits
- Rejected alternative
- Cost/complexity accepted
- What would make us revisit this choice

## 10. Finish with Evolution Path

Show phases:

- MVP: minimal correct architecture
- Growth: scale the proven bottleneck
- High scale: partition, stream, globalize, automate operations

Call out which pieces are intentionally deferred.
