---
hide:

  - navigation
  - toc

---

<div style="text-align: center;">
  <div class="centered-logo-text-group">
    <h1>Agent Directory Service</h1>
  </div>
</div>

## What is the Agent Directory Service?

The **Agent Directory Service (ADS)** is the discovery layer of agents, an open source
project under the [Linux Foundation](https://www.linuxfoundation.org/press/linux-foundation-welcomes-the-agntcy-project-to-standardize-open-multi-agent-system-infrastructure-and-break-down-ai-agent-silos)
building the Internet of Agents.
It gives agent builders a place to publish structured metadata about their agents and
lets others find them by capability, trust signals, and federation policy—not by
vendor or framework.

Directory records use the
[OASF](https://docs.agntcy.org/oasf/open-agentic-schema-framework/) schema, discovery
follows a hierarchical skill taxonomy, and independent directory nodes interconnect
through content routing and DHT-based federation.

## Why use the Agent Directory Service

<div class="grid cards" markdown>

- :material-magnify:{ .lg .middle } **Capability-Based Discovery**

    Publish and find agents by structured skills and attributes using OASF taxonomies
    and content routing across a distributed network of directory servers.

- :material-lan:{ .lg .middle } **Federated Architecture**

    Interconnect directory instances through DHT-based content routing, enabling
    decentralized discovery without a single central registry.

- :material-shield-check:{ .lg .middle } **Verifiable Claims**

    Cryptographic integrity and provenance for directory records help users make
    informed decisions about agent selection and trust.

</div>

## Get started with ADS

<div class="grid cards" markdown>

- :material-rocket-launch:{ .lg .middle } **Quickstart**

    Run a local Directory instance in minutes.

    [:octicons-arrow-right-24: Quickstart](dir/dir-quickstart.md)

- :material-book-open:{ .lg .middle } **Read the Introduction**

    Understand core concepts, architecture, and features.

    [:octicons-arrow-right-24: Overview](dir/dir-overview.md)

    [:octicons-arrow-right-24: Architecture](dir/dir-architecture.md)

- :material-file-document-outline:{ .lg .middle } **Dive into the Specification**

    Explore the ADS Internet Draft and protocol definition.

    [:octicons-arrow-right-24: ADS Specification](https://spec.dir.agntcy.org/draft-mp-agntcy-ads.html)

- :material-code-braces:{ .lg .middle } **SDKs and Tools**

    Client libraries, CLI, and API references.

    [:octicons-arrow-right-24: SDK Overview](dir/dir-sdk.md)

    [:octicons-arrow-right-24: CLI Reference](dir/dir-cli-reference.md)

- :material-cloud-upload:{ .lg .middle } **Deploy**

    Local, Kubernetes, and production deployment guides.

    [:octicons-arrow-right-24: Local Deployment](dir/dir-deployment-local.md)

    [:octicons-arrow-right-24: Production Deployment](dir/dir-prod-deployment.md)

- :fontawesome-brands-github:{ .lg .middle } **Source Code**

    Reference implementation and related repositories.

    [:octicons-arrow-right-24: github.com/agntcy/dir](https://github.com/agntcy/dir)

- :material-lan-connect:{ .lg .middle } **Join the Federation Testbed**

    We invite organizations, researchers, and developers to join the Agent
    Directory Testbed—a decentralized, open staging environment for next-generation
    AI agent discovery and secure registry federation.

    [:octicons-arrow-right-24: Call for federation partners](https://github.com/agntcy/dir/discussions/455)

    [:octicons-arrow-right-24: Federated Directory setup](dir/dir-federation-setup.md)

- :material-newspaper-variant-outline:{ .lg .middle } **Linux Foundation Press Release**

    Read how the Linux Foundation welcomed the AGNTCY project to standardize open
    multi-agent system infrastructure and break down AI agent silos.

    [:octicons-arrow-right-24: LF press release](https://www.linuxfoundation.org/press/linux-foundation-welcomes-the-agntcy-project-to-standardize-open-multi-agent-system-infrastructure-and-break-down-ai-agent-silos)

</div>

---

## Announcement and lookup across directory nodes

Independent directory servers store agent records locally and announce skill
mappings into a shared routing layer. When a client searches by capability,
the network returns record identifiers and the servers that host them—the
sequence below walks through publication and discovery end to end.

<div class="centered-diagram" markdown="1">

```mermaid
sequenceDiagram
    participant User
    participant DHT
    participant ServerA
    participant ServerB
    participant ServerC

    Note over ServerA,ServerC: Publication Phase
    ServerA->>ServerA: Generate record CID
    ServerA->>ServerA: Extract skills from record
    ServerA->>ServerA: Store record locally
    ServerA->>DHT: Announce CID + skills
    ServerB->>ServerB: Generate record CID
    ServerB->>ServerB: Extract skills from record
    ServerB->>ServerB: Store record locally
    ServerB->>DHT: Announce CID + skills
    DHT->>DHT: Update routing tables<br/>(skills→CIDs→servers)

    Note over User,ServerC: Discovery Phase
    User->>DHT: Query by skills
    DHT->>DHT: Search routing tables
    DHT->>User: Return matching CIDs<br/>+ server addresses
    User->>User: Select records
    User->>ServerA: Download record 1
    User->>ServerB: Download record 2
```

</div>
