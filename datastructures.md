# 🔢 Beyond Graphs: Expanding the Primitive Set

## 1. **Trees → Graphs → Hypergraphs (Current Path)**

* **Trees**: Executable Declarative Trees (EDTs) — one-to-many causality, simple pipelines.
* **Graphs**: Causal Graphs — DAGs of primitives, composable and auditable.
* **Hypergraphs**: Multi-input/output causality — captures feedback loops, ecosystems, networks.

These are the **core causal structures**, but not the only ones.

---

## 2. **Other Data Structure Primitives**

### **Arrays / Streams**

* *Use case:* Temporal data, ordered events, tick streams.
* *Primitives:* `Window`, `Reduce`, `Map`, `Zip`, `Replay`.
* *Analogy:* Kafka Streams, numpy arrays.
* *Domain fit:* Finance (tick data), physics (time series), biology (gene sequences).

---

### **Stacks / Queues**

* *Use case:* Event buffering, lifo/fifo causality.
* *Primitives:* `Push`, `Pop`, `Enqueue`, `Dequeue`.
* *Analogy:* OS schedulers, messaging systems.
* *Domain fit:* Computer science, logistics, workflow orchestration.

---

### **Tables (Relational Algebra)**

* *Use case:* Set-based causality (joins, filters, projections).
* *Primitives:* `σ` (select), `π` (project), `⨝` (join), `∪`, `∩`, `−`.
* *Analogy:* SQL, Pandas.
* *Domain fit:* Economics (ledger joins), biology (population tables), physics (event logs).

---

### **Heaps / Priority Queues**

* *Use case:* Causality with weighted priority.
* *Primitives:* `Insert`, `ExtractMin/Max`, `Rebalance`.
* *Analogy:* Dijkstra’s algorithm, schedulers.
* *Domain fit:* Network routing, resource allocation, real-time bidding.

---

### **Graphs (Advanced)**

* *Use case:* Pathfinding, influence, network flow.
* *Primitives:* `BFS/DFS`, `ShortestPath`, `Flow`, `Centrality`.
* *Domain fit:* Economics (supply chains), biology (neural nets), physics (particle interactions).

---

### **Topological Spaces**

* *Use case:* Global invariants, robustness, emergent properties.
* *Primitives:* `Homology`, `Cohomology`, `Boundary`, `Loop Detection`.
* *Analogy:* Algebraic topology.
* *Domain fit:* Physics (conservation laws), finance (systemic risk), biology (ecosystem resilience).

---

## 3. **The Principle**

Each domain tends to have a **“native data structure”**:

* Economics → **Ledgers / Graphs** (flows of value)
* Physics → **Fields / Topologies** (flows of energy)
* Biology → **Networks / Trees** (flows of information + lineage)
* CS → **Stacks / Queues / Arrays** (flows of control + events)

Causal OS will eventually **unify them all**, so that:

* A **tree of logic** can invoke a **graph of flows**,
* A **queue of events** can feed into a **topological monitor**,
* And all of it is **hash-verifiable** and **portable across runtimes**.

---

## 4. **Roadmap for Primitives**

1. **Short Term**: Stabilize the causal graph primitives (`connector`, `processor`, `monitor`, `adapter`, `aggregator`).
2. **Medium Term**: Introduce **array/stream** primitives (`map`, `reduce`, `window`).
3. **Next Step**: Expand to **relational algebra tables** (already seeded in JAQL-lite).
4. **Long Term**: Explore **hypergraph + topology primitives** for systemic reasoning.

---

