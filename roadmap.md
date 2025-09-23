# 🛣️ roadmap.md

**Scaling Executable Declarative Trees (EDTs) from toy SPCs to Tree OS**

---

## 🎯 Phase 0 — Proof of Primitive (Now)

* ✅ **Hello World SPCs**

  * ETH price alert, tip tracker, ROI calculator.
  * Single-file apps running in Deck.Shell + GitHub Pages.

* ✅ **Schema Foundation**

  * `edt.schema.json` + core services (`connector`, `processor`, `monitor`, `interface`).
  * Validation pipeline (AJV, CI tests).

* ✅ **AI Generation Loop**

  * LLMs produce valid SPCs from natural language.
  * Deterministic runtime guarantees they just work.

---

## 🚀 Phase 1 — WordPress for Apps

* **Deck.Shell**: Import/export `edt.json` directly, no copy-paste.
* **MicroService OS**: App launcher + gallery (SPC equivalents of WordPress themes).
* **AI Integration**: “Describe → generate → run” inside UI.
* **Distribution**: GitHub Pages + IPFS pinning for free hosting.

👉 Goal: make SPC the easiest way to spin up a working app.

---

## 🌐 Phase 2 — Periodic Table v0.2

Extend primitives to cover scale and multi-tenant use cases:

* `router` — content-based routing, sharded execution.
* `aggregator` — windowed/sliding/tumbling reduce.
* `scheduler` — cron/interval/backoff with jitter.
* `cache` — TTL + ETag/If-None-Match.
* `adapter` — safe side-effects (webhooks, writes, notify).
* `quota` — rate/volume governors.
* `vault` — secret/material references (no plaintext secrets).
* `ledger` — MNEME append-only fact store.
* `registry` — schema + version gates.

👉 Goal: SPCs scale from “toy dashboards” → “multi-asset price guards.”

---

## ⚙️ Phase 3 — Execution Model (Deterministic + Elastic)

* Pull loops with leases (avoid thundering herds).
* Vectorized fetch (multi-symbols per call).
* Windowed compute (tumbling/sliding/session).
* Idempotency baked in: `hash(input, rule_hash, window_id)`.
* Backpressure + bounded queues → graceful degradation.
* Retry policies (exponential backoff + circuit breaker).

👉 Goal: show SPCs can handle **real-world load** while preserving determinism.

---

## 📊 Phase 4 — Data & Control Plane

**Data Plane (MNEME)**

* Facts-only ledger: inputs, outputs, hashes, proofs.
* Materialized views = disposable, recomputed from facts.
* Canonicalization: sorted keys, stable floats, ISO timestamps.

**Control Plane**

* Registry contracts (`requires.schema`, `produces.schema`).
* Versioning (`semver + rule_hash`).
* Policy: per-tenant quotas, auth scopes, adapter allowlists.

👉 Goal: multi-tenant SPCs with auditability + governance.

---

## 🏗️ Phase 5 — Deployment Profiles

* **Browser/Edge**: SPC dashboards, offline monitors.
* **Serverless**: connectors + monitors as timed functions.
* **Workers**: sharded trees, long-running pipelines.
* **Edge POPs**: cache + aggregation near data, facts shipped inward.

👉 Goal: Tree OS = portable logic layer across infra tiers.

---

## 🌍 Phase 6 — Planet-Scale Tree OS

* AI-native app ecosystem: SPCs as shareable logic units.
* Multi-tenant MNEME ledgers for provenance + proofs.
* Sharded SPC meshes running across browsers, servers, and edge.
* Interplanetary distribution via IPFS → **logic is as portable as data**.

👉 Goal: EDTs recognized as the **next software primitive**.

---

## ✨ North Star

* **History.md** proves it fits the lineage.
* **Spec.md** defines the rules.
* **Math.md** proves it’s irreducible.
* **AI.md** shows it’s AI-native.
* **Roadmap.md** charts the scale.

Together → SPC/EDTs evolve from **toy apps** → **WordPress for apps** → **Tree OS for the planet.**



