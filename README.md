# Awesome-Message-Brokers

# Top Message Brokers Ecosystem

**Curated List of SaaS Products & Open-Source GitHub Projects**  
*Focused on AMQP 1.0, KRaft-Only Kafka & Single-Binary Lightweight Brokers*  
**Last updated: March 2026**

This repository tracks notable **SaaS platforms** and **open-source projects** for **Message Brokers**. These tools provide the intermediary layer between producers and consumers — accepting messages, routing them according to rules, buffering during consumer downtime, and guaranteeing delivery semantics — so distributed systems can communicate asynchronously without tight coupling.

**Examples** include RabbitMQ, Apache Kafka, NATS, Apache Pulsar, and the managed broker services (the category leaders).

**Open-source emphasis**: This section is heavily expanded with every major active project for self-hosting, protocol flexibility, and transparent delivery guarantees — especially relevant as **Kafka 4.0 removed ZooKeeper entirely** and **RabbitMQ 4.2 made quorum queues the default** with AMQP 1.0 as a core protocol.

Contributions welcome! Open a PR to add/update entries. Keep descriptions factual and link to official sites.

## Table of Contents
- [SaaS/Hosted Platforms](#saas-hosted-platforms)
- [Open-Source GitHub Projects](#open-source-github-projects)
- [How to Contribute](#how-to-contribute)
- [Disclaimer](#disclaimer)

## SaaS/Hosted Platforms

- **CloudAMQP** — **The largest managed RabbitMQ hosting provider.** **Cheapest dedicated instance (Sassy Squirrel) at ~$50/month**; **Big Bunny (3-node HA cluster) at ~$297/month** with multi-AZ replication and ~1k msg/s sustained . **Dedicated plans are recommended for production** — shared instances have channel limits of 200 and are suitable for testing only . **Best for teams wanting RabbitMQ's flexible routing and quorum queue semantics** without operating the Erlang cluster themselves.

- **Amazon MSK** — **AWS's managed Apache Kafka service.** Handles provisioning, patching, and operations so teams can run Kafka on AWS without managing clusters. **Best for AWS-native teams** wanting Kafka's event streaming model with managed infrastructure.

- **Confluent Cloud** — **The managed Kafka platform from Kafka's original creators.** Adds tooling, connectors, and enterprise features on top of the open-source engine. **Best for organizations wanting Kafka's ecosystem depth** with vendor support and managed operations.

- **StreamNative** — **Managed Apache Pulsar from Pulsar's creators.** Offers multi-tenant messaging and event streaming with Pulsar's separated serving and storage architecture. **Best for teams wanting Pulsar's tiered storage and geo-replication** without operating BookKeeper.

- **Synadia (NATS)** — **Managed NATS with JetStream persistence.** Provides pub/sub, request-reply, and durable streams from the same single-binary footprint. **Best for teams wanting NATS's sub-millisecond latency** with managed infrastructure.

## Open-Source GitHub Projects

- **RabbitMQ** — **The general-purpose message broker built around AMQP.** Mozilla Public License. **Version 4.3 supports classic and quorum queues, plus Streams as a separate append-only data structure for replay** . **Quorum queues are Raft-backed and the production default** for durable work — classic queue mirroring was removed in 4.0, so anything still on 3.13 mirrored queues is past its migration window . **Flexible routing via exchanges, bindings, and routing keys** handles fan-out and selective delivery at the broker . **Streams provide append-only logs supporting replay** — the closest thing to Kafka semantics without a Kafka deployment . **RabbitMQ 4.2 introduced AMQP 1.0 as a core protocol, Khepri as the default metadata store, and quorum queue message priorities** (high and normal, with a 2:1 fairness ratio) . **The trade-off**: **throughput ceilings sit below Kafka's for sustained high-volume streams** — RabbitMQ excels at task distribution and complex routing, not raw event firehosing . **Best for background jobs and task distribution** where routing logic matters more than sustained throughput.

- **Apache Kafka** — **The high-throughput event streaming platform.** Apache 2.0 licensed. **Version 4.0 requires KRaft mode exclusively — ZooKeeper is no longer supported** . Existing clusters on 3.x must first upgrade to 3.9 and migrate to KRaft before moving to 4.x . **The next-generation consumer rebalance protocol (KIP-848) improves rebalance performance** by reducing downtime and latency, especially for large deployments, but consumers must explicitly set `group.protocol=consumer` . **Kafka 4.0 removed previously deprecated APIs** — ensure all clients, Kafka Connect, and MirrorMaker 2 are compatible before upgrading . **Brokers, tools, and Connect now require Java 17** (clients and Streams still support Java 11) . **Best for sustained high throughput, long-horizon replay, and tooling depth** — Kafka Connect, Kafka Streams, and ksqlDB have no equivalent among alternatives .

- **NATS JetStream** — **The lightweight, high-performance messaging system with persistence.** Apache 2.0 licensed. **Single Go binary with no external dependencies** — request-reply and durable streams from one process, keeping the operational surface minimal . **Sub-millisecond latency** for core NATS service-to-service messaging . **Sequence and time-based replay** — consumers can start from a sequence number, timestamp, or latest message . **The trade-off**: **consumers hold persistent connections**, which rules out runtimes that cannot keep one open; **redeliveries can reorder in-flight messages**; **connector and tooling coverage is narrower than Kafka's** . **Best for microservice communication and edge deployments** where small footprint and low latency outrank connector coverage .

- **Apache Pulsar** — **The cloud-native messaging and streaming platform with separated serving and storage.** Apache 2.0 licensed. **Multi-layer architecture separates stateless brokers from Apache BookKeeper storage**, enabling independent scaling and optimization . **Flexible subscription models** (exclusive, shared, failover), **tiered storage for cost-efficient retention**, and **built-in geo-replication** for multi-region deployments . **Pulsar Functions and Pulsar IO** facilitate stream processing and integration . **Pulsar 4.2** improves broker memory management, adds topic-level subscription expiration policies, and introduces customizable Prometheus labels for topic metrics . **The trade-off**: **requires understanding of both message serving and BookKeeper storage layer operations** — a steeper operational learning curve than RabbitMQ or NATS . **Best for multi-tenant messaging, global data distribution, and ETL pipelines** where tiered storage and geo-replication justify the architectural complexity .

### Additional Strong Open-Source Options

- **Apache ActiveMQ** — The veteran JMS-compliant broker. **Best for Java/JMS applications** already standardized on the Jakarta Messaging API.
- **Redpanda** — Kafka-compatible streaming platform written in C++ with no JVM or ZooKeeper. **Best for teams wanting Kafka API compatibility** with simpler operations.
- **EMQX** — Scalable MQTT broker optimized for IoT. **Best for IoT deployments** where MQTT semantics and massive connection counts matter more than AMQP or Kafka compatibility .

**Frameworks for building custom systems**: Combine **RabbitMQ** for flexible routing and task distribution with quorum queue durability, **Apache Kafka** for sustained high-throughput event streaming with long-horizon replay, **NATS JetStream** for lightweight microservice messaging with sub-millisecond latency, and **Apache Pulsar** for multi-tenant geo-replicated streaming with tiered storage — to build a messaging backbone tailored to your throughput requirements, routing complexity, and operational capacity.

## How to Contribute

1. Fork the repo.
2. Add/edit entries in `README.md` (follow existing format).
3. Include: name, link, 1–2 sentence description, and whether it's SaaS or open-source.
4. Submit PR with a short explanation.

Star the repo if you find it useful!

## Disclaimer

- This is a **community-curated** list — not exhaustive and not an endorsement.
- Message brokers differ significantly in throughput ceiling, routing flexibility, and operational complexity. **RabbitMQ excels at complex routing and task distribution** but its throughput sits below Kafka's for sustained high-volume streams . **Kafka 4.0 requires KRaft mode exclusively** — ZooKeeper is gone, and existing 3.x clusters must migrate through 3.9 first . **NATS JetStream's single-binary footprint is operationally minimal** but connector coverage is narrower than Kafka's . **Pulsar's separated architecture enables independent scaling** at the cost of understanding BookKeeper alongside the broker layer . **The most common operational mistake**: running production workloads on CloudAMQP shared instances — dedicated plans are recommended for production, and shared instances have channel limits of 200 .
- Self-hosted open-source solutions require proper cluster configuration (Raft for RabbitMQ quorum queues, KRaft for Kafka), storage provisioning for message persistence, and monitoring of consumer lag and broker memory.

---

**Made for platform engineers and architects choosing messaging backbones that match their throughput, routing, and operational requirements.**
Let's make message brokers more open, workload-appropriate, and operationally transparent.
