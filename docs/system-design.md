# Reflex System Design

Group 67 · Week 3 (Reflex, The Readiness Sprint)

The differentiator is doubled scan confirmation. The rider scans at both pickup and delivery,
and each scan traces back to the exact order and rider who made it.

## Entity Relationship Diagram

```mermaid
erDiagram
    RETAILER ||--o{ ORDER : requests
    DISPATCHER ||--o{ ORDER : allocates
    RIDER ||--o{ ORDER : delivers
    ORDER ||--o{ SCAN : "confirmed by"
    RIDER ||--o{ SCAN : performs

    RETAILER {
        string retailer_id PK
        string name
        string request_status "fail | success"
    }
    DISPATCHER {
        string dispatcher_id PK
        string name
        string transit_status "dispatched | pending"
    }
    RIDER {
        string rider_id PK
        string name
        string availability "free | delivering"
    }
    ORDER {
        string order_id PK
        string customer_name
        string customer_phone
        string retailer_id FK
        string items
        string destination
        datetime requested_at
        string dispatcher_id FK "nullable until assigned"
        string rider_id FK "nullable until assigned"
        string status "requested | assigned | picked_up | delivered"
    }
    SCAN {
        string scan_id PK
        string order_id FK
        string rider_id FK
        string checkpoint "pickup | delivery"
    }
```

## System Design Workflow

```mermaid
sequenceDiagram
    participant R as Retailer
    participant S as System
    participant D as Dispatcher
    participant Ri as Rider

    R->>S: Create Order (items, destination, customer info)
    S->>D: New order notification
    D->>S: Assign Rider to Order
    S->>Ri: Order assigned
    S-->>Ri: availability -> delivering

    Ri->>S: Scan at pickup (checkpoint = pickup)
    S->>S: Record Scan (order_id, rider_id, pickup)
    S-->>R: Order status -> picked_up

    Ri->>S: Scan at delivery (checkpoint = delivery)
    S->>S: Record Scan (order_id, rider_id, delivery)
    S-->>R: Order status -> delivered
    S-->>Ri: availability -> free
```

The doubled confirmation shows up as two `Scan` rows per order, each one tied to the order and
the rider who performed it. Not a claim in the pitch deck. Something you can actually query.

## Known Deviations From This Design

- **Scan.scanned_at** — the AI Studio build adds a timestamp per scan, not
  specified above. Kept deliberately: it lets delivery duration be computed
  (delivery scan time minus pickup scan time) and removes any ambiguity about
  which of the two Scan rows happened first. Confirmed acceptable 2026-08-31.
