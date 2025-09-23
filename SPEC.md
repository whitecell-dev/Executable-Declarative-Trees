# 📜 Executable Declarative Trees (EDT) Specification v1.0

## 1. Introduction

An **Executable Declarative Tree (EDT)** is a portable data structure for representing applications as **deterministic, interpretable trees**.

Unlike imperative code, EDTs describe **what should happen** rather than **how** it should be executed. The runtime is minimal (\~200 LOC), while the tree holds the application logic.

---

## 2. Goals

* **Declarative**: Logic expressed as structured data, not code.
* **Deterministic**: Same input + same state = same output.
* **Portable**: Runs in any compatible runtime (browser, server, CLI).
* **Verifiable**: Content-addressable, hash-stable, auditable.
* **Composable**: Trees can reference, merge, or extend other trees.
* **AI-Native**: Format simple enough for LLMs to generate safely.

---

## 3. Document Structure

An EDT instance is a JSON (or YAML) object with four top-level fields:

```json
{
  "spc_version": "1.0",
  "meta": { ... },
  "services": { ... },
  "state": { ... }
}
```

### 3.1 `spc_version`

* **Type:** String
* **Description:** Version of the EDT specification implemented.
* **Example:** `"1.0"`

---

### 3.2 `meta`

Metadata for provenance and context.

| Field         | Type     | Description                               |
| ------------- | -------- | ----------------------------------------- |
| `name`        | String   | Human-readable name of the application    |
| `author`      | String   | (Optional) Author/creator                 |
| `description` | String   | (Optional) Summary of application purpose |
| `exported_at` | DateTime | UTC timestamp of last export              |

---

### 3.3 `services`

The **core of the tree** — a set of declarative service nodes.

* **Type:** Object keyed by service IDs
* **Each value:** Service object with required fields:

| Field     | Type     | Description                                              |
| --------- | -------- | -------------------------------------------------------- |
| `id`      | String   | Unique identifier for service                            |
| `type`    | Enum     | One of: `connector`, `processor`, `monitor`, `interface` |
| `title`   | String   | Human-readable label                                     |
| `spec`    | Object   | Service-specific configuration (see Section 4)           |
| `status`  | Enum     | `"running"` or `"stopped"`                               |
| `lastRun` | DateTime | Timestamp of last execution (nullable)                   |
| `outputs` | Object   | Most recent service output                               |

---

### 3.4 `state`

* **Type:** Object
* **Description:** Global application state, updated by service outputs and transformations.
* **Properties:** Arbitrary JSON (numbers, strings, arrays, nested objects).
* **Constraint:** Must be serializable and canonicalizable for hashing.

---

## 4. Service Types

Each service type defines its own **`spec`** sub-schema.

---

### 4.1 Connector

Fetches and normalizes external data.

```json
{
  "id": "btc-price",
  "type": "connector",
  "title": "Bitcoin Price Feed",
  "spec": {
    "url": "https://api.example.com",
    "outputKey": "btc_data",
    "rules": {
      "rules": [
        {
          "name": "extract_price",
          "if": "bitcoin",
          "then": {
            "price": "{{ bitcoin.usd }}",
            "change": "{{ bitcoin.usd_24h_change }}"
          }
        }
      ]
    }
  }
}
```

---

### 4.2 Processor

Applies pure transformations to inputs → outputs.

```json
{
  "id": "roi-calc",
  "type": "processor",
  "title": "ROI Calculator",
  "spec": {
    "inputKey": "btc_data",
    "outputKey": "roi_result",
    "transform": [
      {
        "name": "roi",
        "if": "price",
        "then": {
          "roi": "{{ (price - 30000) / 30000 * 100 }}"
        }
      }
    ]
  }
}
```

---

### 4.3 Monitor

Checks thresholds and conditions, emitting alerts/status.

```json
{
  "id": "price-monitor",
  "type": "monitor",
  "title": "Price Alert Monitor",
  "spec": {
    "checks": [
      { "name": "price", "dataKey": "btc_data", "expression": "data.price" }
    ],
    "thresholds": {
      "price": { "above": 80000, "below": 20000 }
    }
  }
}
```

---

### 4.4 Interface

Defines user inputs and presentation logic.

```json
{
  "id": "roi-ui",
  "type": "interface",
  "title": "ROI Sandbox",
  "spec": {
    "inputs": [
      { "name": "initial", "label": "Initial Investment", "type": "number", "default": 30000 },
      { "name": "final", "label": "Final Value", "type": "number", "default": 45000 }
    ],
    "calculation": "((inputs.final - inputs.initial) / inputs.initial) * 100",
    "outputKey": "roi_interface_result"
  }
}
```

---

## 5. Execution Semantics

1. **Initialization**

   * Load `state` into memory.
   * Register `services` as tree nodes.

2. **Evaluation**

   * Connector nodes fetch → update `state[outputKey]`.
   * Processor nodes apply transformations → update outputs in `state`.
   * Monitor nodes evaluate thresholds → update status in `state`.
   * Interface nodes map inputs → calculations → outputs.

3. **Determinism**

   * All evaluation must be **pure** given the same inputs and prior state.
   * External calls (connectors) should be logged and hash-stamped for replayability.

4. **Audit Log**

   * Each execution step produces an append-only log entry:

     * `timestamp`
     * `service.id`
     * `input`
     * `output`
     * `state_hash`

---

## 6. Determinism & Verification

* **Canonicalization**: All JSON must be canonicalized (sorted keys, no NaN/Infinity) before hashing.
* **Hashing**: Use SHA3-256 for state, outputs, and logs.
* **Replay**: A sequence of logged inputs must reproduce the exact same state transitions.

---

## 7. Composability

* Trees may import/merge other trees:

  * **Extend**: Add new services
  * **Override**: Replace service specs by ID
  * **Compose**: Reference other trees by CID (IPFS)

---

## 8. Reserved Keywords

* `state`
* `input`
* `outputKey`
* `when` / `if`
* `then`
* `spec`

These terms are reserved in EDT spec and must not be redefined.

---

## 9. Future Extensions

* **Events** → async triggers across services
* **Modules** → reusable sub-trees published as packages
* **Typed Inputs/Outputs** → stronger validation schema
* **WASM Targeting** → compilation to portable bytecode

---

## 10. License

MIT — free to use, extend, and remix.

---


