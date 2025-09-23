# The primitives (periodic table v0.2)

* **connector** — fetch truth (HTTP, WS, file, sensor).
* **processor** — pure transforms/RA (σ, π, ⨝, map/reduce).
* **monitor** — boolean/watch conditions → events.
* **adapter** — side-effects (notify, write, webhook), idempotent by key.
* **aggregator** — fan-in/fan-out combiners (windowed, tumbling, session).
* **router** — content-based + rule-based routing to subtrees/shards.
* **scheduler** — cron/interval/backoff triggers with jitter.
* **cache** — keyed memoization + TTL + ETag/If-None-Match awareness.
* **vault** — secret/material management (no secrets in trees).
* **ledger** — append-only provenance (MNEME facts & hashes).
* **registry** — schema + version constraints (compat gates).
* **quota** — per-tenant rate/volume governors (global + per-service).

# Execution model (deterministic + elastic)

* **Pull loops with leases** (avoid thundering herds), **vectorized fetch** (multi-symbols per call).
* **Windowed compute**: tumbling/sliding windows in `aggregator` for batch efficiency.
* **Sharded trees**: `router` shards by key (e.g., `asset_symbol % N`) → parallel workers.
* **Idempotency everywhere**: `idempotency_key = hash(input_payload, rule_hash, window_id)`.
* **Backpressure**: bounded queues; drop or degrade to coarse windows under load.
* **Retry policy** per edge: exponential backoff + jitter + circuit breaker.

# Data plane

* **Facts only** in ledger (MNEME): inputs, outputs, signatures, rule\_hash, input\_hash, output\_hash.
* **Materialized views** are derived, disposable; recompute from facts.
* **Canonicalization**: sorted keys, stable floats/ints, ISO timestamps.

# Control plane

* **Registry contracts**: `requires.schema`, `produces.schema`, `max_qps`, `auth.scope`.
* **Versioning**: `semver + rule_hash`; router can run canaries by version.
* **Policy**: per-tenant `quota`, `rate_limit`, `allowed_adapters`.

# Reliability & security

* **Graceful degradation**: widen window, fallback source, cache-serve stale with warning.
* **Proofs**: sign fact bundles; adapters attach external receipt (e.g., 2xx body hash).
* **Secrets** via vault pointers; zero plaintext in SPC files.

# Multi-tenant

* Namespaced keys: `tenant/{id}/service/{name}`.
* Per-tenant routers + quotas; shared connector pools (connection reuse).

# Minimal scalable pattern (multi-asset price alerts)

```json
{
  "spc_version": "1.0",
  "meta": { "name": "Multi-Asset Price Guard", "version": "0.2.0" },
  "services": {
    "router": {
      "type": "router",
      "spec": {
        "shard_key": "{{ asset.symbol }}",
        "shards": 16,
        "route": "asset-pipeline"
      }
    },
    "asset-pipeline": {
      "type": "processor",
      "spec": {
        "pipes": [
          { "derive": { "id": "{{ asset.symbol }}::{{ window.id }}" } },
          { "project": ["id","asset.symbol","threshold_below"] }
        ]
      }
    },
    "price-connector": {
      "type": "connector",
      "spec": {
        "url": "https://api.coingecko.com/api/v3/simple/price?ids={{ join(',', uniq(map(assets,'coingecko_id'))) }}&vs_currencies=usd",
        "method": "GET",
        "outputKey": "prices",
        "cache": { "ttl_sec": 10, "etag": true, "key": "prices::*" },
        "schedule": { "every_sec": 10, "jitter_sec": 3 }
      }
    },
    "window-agg": {
      "type": "aggregator",
      "spec": {
        "inputKey": "prices",
        "window": { "type": "tumbling", "size_sec": 30 },
        "reduce": { "emit": "latest" }
      }
    },
    "threshold-monitor": {
      "type": "monitor",
      "spec": {
        "checks": [
          {
            "name": "below_threshold",
            "dataKey": "window-agg",
            "expression": "data[asset.coingecko_id].usd < asset.threshold_below"
          }
        ]
      }
    },
    "notify": {
      "type": "adapter",
      "spec": {
        "kind": "webhook",
        "url": "{{ secrets.alert_webhook }}",
        "method": "POST",
        "body": {
          "symbol": "{{ asset.symbol }}",
          "price": "{{ data[asset.coingecko_id].usd }}",
          "threshold": "{{ asset.threshold_below }}",
          "ts": "{{ now() }}"
        },
        "idempotency_key": "{{ hash(asset.symbol, window.id, rule_hash) }}"
      }
    },
    "quota": { "type": "quota", "spec": { "max_qps": 5, "burst": 10 } },
    "ledger": { "type": "ledger", "spec": { "store": "mneme://prices" } }
  },
  "state": {
    "assets": [
      { "symbol": "ETH", "coingecko_id": "ethereum", "threshold_below": 1500 },
      { "symbol": "BTC", "coingecko_id": "bitcoin", "threshold_below": 52000 }
    ]
  }
}
```

# Deployment profiles

* **Browser/Edge**: read-only dashboards, local monitors, push notifications.
* **Serverless**: connectors + monitors on timers, pay-per-use.
* **Long-running workers**: sharded routers + queues for high QPS.
* **Edge POPs**: cache + aggregator near data sources, ship facts to core.

# Next moves

1. Add `router`, `aggregator`, `scheduler`, `cache`, `quota`, `ledger`, `vault` to your schema.
2. Implement idempotency + windowing helpers in axis\_core.
3. Ship a reference “multi-asset guard” SPC and a perf harness (fake price feed) to measure QPS, latency, and replays.


