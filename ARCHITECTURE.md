# Inferos Cluster Architecture

## Overview

A **Cluster** (CLST) is the fundamental engine powered by inferos technology. It is a self-contained mesh of interconnected modules that collectively act as a virtual private network, providing users with absolute anonymity, privacy, and security without reliance on third-party VPNs or darknet access points.

Each cluster is a closed system by design. Users interact with it through a single controlled entry point, internal modules handle processing and orchestration, and external communication is delegated to isolated agents that have no authority over the cluster itself.

A cluster is composed of **ports** and **modules**:

- **Ports** are the boundary interfaces, the only points where traffic enters or leaves the cluster.
- **Modules** are the processing units that live inside the cluster.

```
          USERS
            │
     ╔══════╧══════╗
     ║  CLST-GATE  ║
     ╚══════╤══════╝
            │ obfuscated requests
 ┌──────────┼──────────────────────────────────────┐
 │          │              CLUSTER (CLST)          │
 │          ▼                                      │
 │  ┌────────────────┐                             │
 │  │ MDL-ST-CITADEL │ ◄─────────────────────┐     │
 │  └───────┬────────┘                       │     │
 │          │                                │     │
 │          ▼                                │     │
 │  ┌───────────────┐      ┌──────────────┐  │     │
 │  │  MDL-ST-?     │◄────►│  MDL-OP-?    │  │     │
 │  │  (stations)   │      │  (operators) │  │     │
 │  └───────┬───────┘      └──────────────┘  │     │
 │          │                                │     │
 │          │         ┌──────────────────┐   │     │
 │          └────────►│   MDL-ST-?  ...  │───┘     │
 │                    └──────────────────┘         │
 │                            │                    │
 │                            ▼                    │
 │                   ╔════════════════╗            │
 │                   ║   CLST-RELAY   ║            │
 │                   ║ ┌────────────┐ ║            │
 │                   ║ │  MDL-RN-?  │ ║            │
 │                   ║ │ (runners)  │ ║            │
 │                   ║ └─────┬──────┘ ║            │
 │                   ╚═══════╪════════╝            │
 └───────────────────────────┼─────────────────────┘
                             │
                             ▼
              ┌──────────────────────────────┐
              │   INTERNET / OTHER RELAYS    │
              └──────────────────────────────┘
```

---

## Cluster Ports

Ports define the cluster's perimeter. There are exactly two types:

### CLST-GATE — The User Entry Point

The Gate is the **only** way a user can interact with the cluster. It accepts incoming connections and presents the cluster's services, creating the illusion that the user is operating from _inside_ the cluster rather than connecting to it from the outside.

- The Gate makes obfuscated requests to the responsible Station.
- No module other than the Gate is directly reachable by users.

### CLST-RELAY — The External Boundary

The Relay is the cluster's outward-facing boundary. It is the **only** point where the cluster touches the public internet or other clusters.

- The Relay provides Runners with a controlled bridge to external networks.
- A Runner from Cluster A can **only** interact with a Relay of Cluster B, never with B's Gate or internal modules.

---

## Module Taxonomy

Modules are the building blocks of a cluster. Each module has a type prefix that defines its role and constraints.

### MDL-ST-? — Station

Stations are **large, stationary** modules that handle high volumes of data or traffic. They are the backbone of a cluster's internal processing.

- Heavy, persistent processes.
- Handle storage, routing, or aggregation of data within the cluster.

### MDL-OP-? — Operator

Operators are **dynamic, task-specific** modules that exist to make Stations work. They are the cluster's workforce.

- Lightweight and purpose-built.
- Operate entirely within the cluster's internal boundary.

### MDL-RN-? — Runner

Runners are **isolated, externally-facing** modules that act on behalf of the cluster but have **zero authority** inside it. They are the cluster's hands in the outside world.

- **Cannot** communicate with the cluster's internal modules directly.
- Receive orders from the cluster (one-way authority).
- A Runner can interact with:
  - The public internet.
  - A **Relay of another cluster**.

---

## Data Flow

### User Request (Inbound)

```
User <--> CLST-GATE <--> MDL-ST-CITADEL <--> (processing)
```

1. User connects to the cluster through the **Gate**.
2. The Gate sends an obfuscated request to the **Station Citadel**.
3. The Station Citadel may delegate work to other **Stations** or **Operators**.
4. The response travels back through the same chain to the user.

### External Action (Outbound)

```
MDL-ST-? --> CLST-RELAY --> MDL-RN-? --> Internet / Other CLST-RELAY
```

1. A Station issues an order that reaches a **Runner** inside the Relay.
2. The Runner executes the action externally (API call, data delivery to another cluster's Relay).
3. The Runner may return results back through the Relay, but it has **no authority** to modify cluster state or interact with internal modules directly.

---

## Design Principles

1. **Isolation by default** — No module has more access than its role requires. Runners cannot touch internals; users cannot bypass the Gate.
2. **No third-party dependence** — The cluster itself provides the anonymity and security layer; no external VPN or proxy is needed.
3. **Modular independence** — Each module is a self-contained unit. Modules can be developed, tested, and deployed independently.
4. **Boundary enforcement** — Only two points (Gate, Relay) bridge the cluster to the outside world. Everything else is internal.
