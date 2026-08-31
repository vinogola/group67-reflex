#  Trade-Off Log

### Trade-Off 1: Rider Dual-Scanning vs. Single-Action Friction

* **What it is:** We require the rider to perform two distinct physical scans per order (scanning the retailer's parcel label at pickup and scanning the receipt/invoice at the drop-off location) instead of a single completion tap.
* **Why we accepted it:** A single scan leaves half the chain of custody blind. Under informal WhatsApp and phone coordination, retailers suffered frequent disputes where riders claimed pickups that had not occurred or deliveries that customers denied. Two independent scans permanently close both dispute windows with zero additional hardware overhead.
* **What we would improve with more time:** We would automate drop-off verification using device geofencing (GPS proximity to the delivery address) to convert the second scan into a one-tap confirmation whenever the rider is within 30 meters of the destination.

---

### Trade-Off 2: Short-Polling vs. WebSocket Infrastructure

* **What it is:** The dispatcher dashboard and retailer interface fetch order status updates via HTTP polling every 8 to 10 seconds rather than maintaining persistent, full-duplex WebSocket connections.
* **Why we accepted it:** Short-polling eliminates complex WebSocket connection state handling and reconnect storms over unstable 3G/4G cellular networks. An 8-to-10-second latency is operationally imperceptible to a shopkeeper monitoring whether a package has been dispatched.
* **What we would improve with more time:** We would introduce Server-Sent Events (SSE) or WebSockets backed by a Redis Pub/Sub backplane once active platform concurrency exceeds 500 simultaneous delivery requests.

---

### Trade-Off 3: Synchronous Database Locking vs. Distributed Queues

* **What it is:** We enforce assignment locking directly in PostgreSQL using row-level transactional locks (SELECT ... FOR UPDATE) instead of implementing an external distributed message queue such as RabbitMQ or Kafka.
* **Why we accepted it:** For our target workload across regional retail clusters, ACID-compliant database locking prevents race conditions between concurrent dispatchers assigning the same order, introducing zero extra infrastructure dependencies or points of failure.
* **What we would improve with more time:** We would introduce an asynchronous job queue (such as BullMQ with Redis) to support high-volume bulk dispatching, automated rider batching, and cross-regional routing at scale.
