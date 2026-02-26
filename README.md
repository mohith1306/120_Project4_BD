# YAK — Yet Another Kafka

A **distributed event streaming system** built in Python, inspired by Apache Kafka. YAK implements core messaging primitives — producers, consumers, topics, and partitioned logs — with a focus on durability, ordering guarantees, and scalable message throughput.

---

## Overview

YAK simulates a Kafka-like distributed messaging backbone. It supports multiple producers publishing to named topics, partitioned log storage for parallel consumption, offset-based message replay, and consumer group coordination for fault-tolerant load balancing.

---

## Architecture

```
┌─────────────┐        ┌──────────────────────────────────┐
│  Producer   │──────▶ │            Broker                │
└─────────────┘        │                                  │
                       │  ┌────────┐     ┌────────────┐   │
┌─────────────┐        │  │ Topic  │────▶│ Partitioned│   │
│  Producer   │──────▶ │  │Manager │     │    Logs    │   │
└─────────────┘        │  └────────┘     └─────┬──────┘   │
                       │                       │          │
                       └───────────────────────┼──────────┘
                                               │
                        ┌──────────────────────▼──────────┐
                        │         Consumer Group          │
                        │  ┌──────────┐  ┌──────────┐     │
                        │  │Consumer 1│  │Consumer 2│     │
                        │  └──────────┘  └──────────┘     │
                        └─────────────────────────────────┘
```

---

## Features

- **Topics & Partitions** — Messages are organized into named topics, each split into partitions for parallel processing
- **Offset-based Consumption** — Consumers track offsets per partition, enabling message replay and exactly-once semantics
- **Consumer Groups** — Multiple consumers in a group coordinate to load-balance partitions and recover from failures
- **Durability** — Messages are persisted to disk-backed partitioned logs, surviving broker restarts
- **Ordering Guarantees** — Per-partition ordering is preserved across producers and consumers
- **Scalable Throughput** — Parallel partition processing enables horizontal scaling of both producers and consumers

---

## Tech Stack

- **Language:** Python
- **Domain:** Distributed Systems
- **Storage:** File-based partitioned logs (disk persistence)
- **Coordination:** Consumer group protocol with offset tracking

---

## Repository Structure

```
120_Project4_BD/
├── broker/          # Core broker logic — topic and partition management
├── producer/        # Producer client implementation
├── consumer/        # Consumer and consumer group coordination
├── storage/         # Partitioned log storage and offset management
├── utils/           # Shared utilities
└── main.py          # Entry point / demo runner
```

---

## Getting Started

### Prerequisites

- Python 3.10+
- Git

### Installation

```bash
git clone https://github.com/mohith1306/120_Project4_BD.git
cd 120_Project4_BD
pip install -r requirements.txt
```

### Run

```bash
python main.py
```

This starts the broker and runs a demo with sample producers and consumers publishing/consuming messages across topics.

---

## Usage

```python
# Create a topic with 3 partitions
broker.create_topic("orders", num_partitions=3)

# Producer publishes a message
producer.send("orders", key="user_1", value="Order placed")

# Consumer group reads from topic
consumer = Consumer(group_id="order-processors")
consumer.subscribe("orders")
for message in consumer.poll():
    print(message.value)
```

---

## Key Design Decisions

- **Partitioned logs** ensure messages with the same key always route to the same partition, preserving order per key
- **Offset commits** are explicit, giving consumers control over replay and at-least-once delivery guarantees
- **Consumer group rebalancing** redistributes partitions when consumers join or leave the group, ensuring no partition goes unprocessed

---

## Contributing

Contributions are welcome.

- Fork the repo
- Create a feature branch: `git checkout -b feature/my-change`
- Commit changes: `git commit -m "Add my change"`
- Push branch: `git push origin feature/my-change`
- Open a Pull Request

---

## License

All rights reserved.

---

## Author

- GitHub: [mohith1306](https://github.com/mohith1306)
