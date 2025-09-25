EDT Primitives — Functional Roles
1. Connector (Impure – “Fetch truth”)

Role: Bring external truth into the system. APIs, files, sensors.

Function:

Runs an HTTP request, shell command, or sensor read.

Dumps structured JSON into state under outputKey.

May include rules to extract or transform raw responses.

Purity: Impure — depends on external world.

Analogy: IO monad input, or source in streaming systems.

2. Processor (Pure – “Transform truth”)

Role: Apply pure transforms to state.

Function:

Takes inputKey → applies pipes (select, project, join, derive, aggregate) → writes to outputKey.

Always deterministic: same input = same output.

Purity: Pure — no side effects, replayable, hashable.

Analogy: SQL query, Pandas transform, or λ-function in FP.

3. Monitor (Semi-pure – “Watch truth”)

Role: Guardrails. Evaluate boolean expressions or thresholds on state.

Function:

Defines checks (price > 70000, cpu_usage > 0.9).

Emits events (onTrue, onChange, always) when predicate flips.

Purity:

Predicate = pure (deterministic check).

Emit = impure (creates an event signal).

Analogy: watch in Kubernetes, or an observable that emits triggers.

4. Adapter (Impure – “Effect truth”)

Role: Push results into the outside world.

Function:

Side-effect sinks: send webhook, write file, insert DB row, publish to pubsub.

May include retry/circuit-breaker logic for resilience.

Purity: Impure — explicitly changes the world.

Analogy: IO monad output, or “sink” in streaming systems.

5. Aggregator (Borderline – “Summarize truth over time”)

Role: Collapse streams/windows into aggregates.

Function:

Input = stream of state updates.

Applies a window (tumbling/sliding/session).

Reduces with avg, sum, count, or custom reducer.

Output = “rolling truth” (e.g., 1m average BTC price).

Purity:

Pure if replayed with full log.

Impure in live mode because it depends on time progression.

Analogy: Kafka Streams windowing, SQL GROUP BY ... WINDOW.

6. Router (Semi-pure – “Route truth”)

Role: Direct messages to different subtrees/pipelines.

Function:

Matches conditions → sends data to target service(s).

Can shard across multiple targets by weight/condition.

Purity: Pure if routing is deterministic (by condition). Impure if randomized or time-based.

Analogy: Service mesh routing, Nginx rules, functional case statement.

7. Scheduler (Impure – “Trigger truth over time”)

Role: Generate events based on time/intervals.

Function:

Defines triggers: cron jobs, every N seconds, exponential backoff, jitter.

Calls downstream services on schedule.

Purity: Impure — depends on clock.

Analogy: cron, Airflow DAG scheduler, functional delay/interval.

8. Cache (Impure but bounded – “Remember truth temporarily”)

Role: Store results for reuse, avoid redundant fetches.

Function:

TTL-based, size-based, or LRU caching.

May respect HTTP cache headers (ETag, If-None-Match).

Purity: Impure in live mode (stale data vs fresh). Pure in replay mode (cache bypass).

Analogy: Redis cache, memoization wrapper.

9. Vault (Impure – “Hide truth”)

Role: Secret management.

Function:

Fetches secrets (API keys, DB creds) from providers (env vars, Hashicorp, AWS Secrets).

Keeps secrets out of SPC trees.

Purity: Impure — external dependency, but deterministic given provider state.

Analogy: Vault service, KMS, dotenv loader.

10. Ledger (Semi-pure – “Prove truth”)

Role: Append-only log of all inputs/outputs with hashes.

Function:

Stores timestamp, rule_hash, input_hash, output_hash.

Provides replay verification: recompute pipeline, compare hashes.

Purity: Pure in principle (hashes). Impure if replicated/distributed (consensus needed).

Analogy: Git history, blockchain ledger, append-only fact store.

11. Registry (Pure – “Constrain truth”)

Role: Schema/version registry for contracts between services.

Function:

Defines contracts (requires, produces).

Enforces compatibility gates (backward, forward).

Purity: Pure — declarative constraints, no side effects.

Analogy: JSON schema registry, GraphQL SDL, TypeScript interfaces.

12. Quota (Impure but bounded – “Throttle truth”)

Role: Enforce limits on usage.

Function:

Defines max QPS, burst limits, per-tenant quotas.

Enforcement strategies: drop, queue, throttle.

Purity: Impure because it depends on runtime usage.

Analogy: API gateway throttling, rate limiter middleware.

🔑 The Pattern

Pure Core: Processor, Registry → deterministic transforms.

Impure Boundary: Connector, Adapter, Vault, Scheduler → IO effects.

Hybrid / Straddling: Monitor, Aggregator, Router, Ledger, Cache, Quota.

This is basically:
👉 Processors = λ-calculus core.
👉 Everything else = structured monadic effects around it.

That’s why SPC feels like a functional OS for applications.
