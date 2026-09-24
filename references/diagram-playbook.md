# Diagram Playbook

Use Mermaid by default for Markdown portability.

## Diagram Selection

Use:

- `flowchart LR` for architecture overview.
- `sequenceDiagram` for request/event timing.
- `erDiagram` for data model.
- `stateDiagram-v2` for lifecycle/status transitions.
- `C4-style containers` using Mermaid flowcharts when stakeholders need boundaries.

## Architecture Overview Template

```mermaid
flowchart LR
    actor[Actor / Producer] --> edge[API or Ingestion Gateway]
    edge --> queue[Durable Queue or Stream]
    queue --> worker[Processing Worker]
    worker --> db[(Source of Truth DB)]
    worker --> read[(Read Model / Cache)]
    api[Read API] --> read
    api --> db
    app[Client App] --> api
    api --> realtime[Realtime Channel]
    realtime --> app
```

## Three-Tier Template

```mermaid
flowchart TB
    subgraph P[Presentation Tier]
        app[Mobile App]
        wire[Wireframes / User Flows]
    end

    subgraph A[Application Tier]
        gateway[API Gateway]
        ingest[Ingestion Service]
        queue[Queue / Stream]
        worker[Processing Worker]
        readapi[Read API]
        realtime[Realtime Updates]
    end

    subgraph D[Data Tier]
        raw[(Raw Events)]
        facts[(Computed Facts)]
        cache[(Cache / Read Models)]
    end

    app --> readapi
    readapi --> cache
    readapi --> facts
    gateway --> ingest
    ingest --> queue
    queue --> worker
    worker --> raw
    worker --> facts
    worker --> cache
    realtime --> app
```

## Event Flow Template

```mermaid
sequenceDiagram
    participant Producer
    participant Gateway
    participant Queue
    participant Worker
    participant DB
    participant Cache
    participant Client

    Producer->>Gateway: POST event with idempotency key
    Gateway->>Queue: enqueue durably
    Gateway-->>Producer: 202 Accepted
    Queue->>Worker: deliver event
    Worker->>DB: upsert raw event
    Worker->>DB: compute/update facts
    Worker->>Cache: update read model
    Worker-->>Client: realtime update
```

## Failure Flow Template

```mermaid
flowchart TD
    event[Incoming event] --> validate{Valid schema?}
    validate -- No --> reject[Reject and log]
    validate -- Yes --> enqueue{Queue available?}
    enqueue -- No --> retry[Producer retries with backoff]
    enqueue -- Yes --> dedupe{Duplicate key?}
    dedupe -- Yes --> ack[Return prior result / ignore duplicate]
    dedupe -- No --> process[Process event]
    process --> ok{Success?}
    ok -- Yes --> update[Update DB + read model]
    ok -- No --> attempts{Attempts left?}
    attempts -- Yes --> retrymsg[Retry message]
    attempts -- No --> dlq[Dead-letter queue]
```

## ERD Template

```mermaid
erDiagram
    USER ||--o{ RAW_EVENT : owns
    USER ||--o{ FACT : has
    RAW_EVENT ||--o| FACT : derives
    USER ||--o{ BADGE_AWARD : earns
    BADGE ||--o{ BADGE_AWARD : awarded_as
```

## Diagram Rules

- Keep labels short.
- Put responsibilities in surrounding prose, not giant node text.
- Show source of truth separately from cache/read model.
- Show async boundary explicitly with a queue/stream node.
- Show failure/retry path when the requirement mentions reliability.
- Use subgraphs for tiers, trust boundaries, or ownership boundaries.
