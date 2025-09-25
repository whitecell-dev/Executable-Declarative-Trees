# 🌐 Causal Graphs: Executable Reasoning with EDT Primitives

> **“Correlation is what happens. Causality is why it matters.”**
> Causal Graphs are how Executable Declarative Trees (EDTs) scale into networks of intent, action, and verifiable computation.

---

## 🚀 What Are Causal Graphs?

Causal Graphs are the **composition layer** built on top of **Executable Declarative Trees (EDTs)**.

* **EDT = primitive** (self-contained app logic, portable, verifiable)
* **Causal Graph = system** (a network of EDTs linked by causal dependencies)

Think:

```
HTML → single document  
WWW  → network of documents  

EDT  → single causal tree  
Causal Graph → network of causal trees  
```

Instead of imperative scripts, you **declare intent** as a graph of primitives.

---

## 🧩 The Primitives

Causal Graphs are built from EDT service primitives (schemas live in `causal-graphs/schema/`):

| Primitive      | Role                                        | Purity    | Analogy                        |
| -------------- | ------------------------------------------- | --------- | ------------------------------ |
| **Connector**  | Fetch external truth (APIs, sensors)        | Impure    | IO input                       |
| **Processor**  | Pure transformations (derive, filter, join) | Pure      | λ-function / SQL               |
| **Aggregator** | Rolling averages, windowed state            | Semi-pure | Kafka Streams / Pandas groupby |
| **Monitor**    | Guardrails, thresholds, boolean checks      | Semi-pure | Watcher / Observable           |
| **Adapter**    | Side-effects (webhook, DB write, log)       | Impure    | IO output                      |
| **Ledger**     | Append-only provenance log                  | Semi-pure | Git / blockchain               |
| **Registry**   | Schema & contract constraints               | Pure      | Type system                    |
| **Router**     | Direct flow based on conditions             | Semi-pure | Service mesh / case stmt       |
| **Scheduler**  | Time-based triggers                         | Impure    | Cron / Airflow                 |
| **Cache**      | TTL/layered memory                          | Impure    | Redis / memoization            |
| **Vault**      | Secret/material management                  | Impure    | Vault / dotenv                 |
| **Quota**      | Rate/volume governors                       | Impure    | API gateway                    |
| **Interface**  | UI definitions (inputs/outputs)             | Hybrid    | React / HTML UI bindings       |

Together, they form a **functional operating system for causality**.

---

## 📖 Why Causal Graphs?

Traditional apps are imperative:

```js
fetchPrice()
let avg = rollingAvg(prices)
if (deviation > 0.05) alert()
```

Causal Graphs are declarative:

```yaml
services:
  connector_eth_price:
    type: connector
    spec:
      url: "https://api.coingecko.com/api/v3/simple/price?ids=ethereum&vs_currencies=usd"
      outputKey: eth_raw

  processor_extract_price:
    type: processor
    spec:
      inputKey: eth_raw
      outputKey: eth_price
      pipes:
        - project: ["ethereum"]
        - derive: { price_usd: "ethereum.usd" }

  aggregator_avg:
    type: aggregator
    spec:
      inputKey: eth_price
      outputKey: eth_avg
      window: { type: tumbling, size_sec: 300 }
      reduce: { emit: avg, field: price_usd }

  monitor_volatility:
    type: monitor
    spec:
      checks:
        - name: deviation_check
          dataKey: eth_price
          expression: "abs(price_usd - eth_avg) / eth_avg > 0.05"

  adapter_alert:
    type: adapter
    spec:
      kind: log
      condition: monitor_volatility.passed
      body: "⚠️ ETH deviation >5%! Current: {{eth_price.price_usd}}, Avg: {{eth_avg}}"
```

Instead of code, you describe **cause → effect**.
Execution is deterministic, auditable, and portable.

---

## 🧠 The Trinity Convergence

```
   AI (Correlation)
        ↓ recognizes patterns
 Causal Graphs (EDT/SPC)
        ↓ executes intentionality
 Economics (Value Flows)
```

* **AI = Correlation engines** (what’s happening?)
* **Causal Graphs = Causality bridge** (why it matters?)
* **Economics = Human domain** (what to do?)

This is the **causality compilation loop**: intent → EDT → runtime → audit trail.

---

## 🏗️ Architecture

```
[YAML / JSON Intent]
        ↓
[LLM as Compiler → EDT IR]
        ↓
[Deterministic Runtime (Python/Go/JS)]
        ↓
[Verifiable Outputs + Ledger]
```

* **LLMs compile human intent** → EDT IR (YAML/JSON)
* **Runtimes execute deterministically** (same input → same output)
* **Ledgers hash every step** for audit & reproducibility

Causal Graphs = **LLM as compiler, EDT as IR, runtime as backend.**

---

## 💡 Use Cases

* **Finance** → Volatility monitors, arbitrage detectors
* **Real Estate (House Hack OS)** → Greenlight gates, Monte Carlo sims
* **Education** → Adaptive tutoring with causal guardrails
* **IoT / Edge** → Sensor-driven causal pipelines


Schemas for each primitive are in `causal-graphs/schema/`.

---

## 🧭 Roadmap

* [ ] CLI runner (`edt run`)
* [ ] Visual builder (drag-drop causal primitives)
* [ ] Ledger integration (hash-provenance)
* [ ] Agent marketplace (plug-in causal agents)
* [ ] Codegen (Python, Go, JS runtimes)

---

## 🌍 Philosophy

Causal Graphs aren’t just about software. They’re about **making reasoning itself executable**:

* Transparent (human-readable YAML)
* Deterministic (no stochastic surprises at runtime)
* Portable (runs anywhere, any language)
* Auditable (hash-based provenance)

> **"Reasoning is infrastructure. Causal Graphs are the operating system."**

---

## 📜 License

MIT (core engine)
