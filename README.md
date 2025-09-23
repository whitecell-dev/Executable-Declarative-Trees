Got it — here’s a **dedicated README for the primitive itself**, written as the “source-of-truth” manifesto that explains the **what** and **why** before people ever touch Deck.Shell, MicroService OS, or SPC demos.

---

# 🌲 Executable Declarative Trees (EDTs)

**The missing primitive between data and computation.**

> *Objects gave us encapsulation. Git gave us history. IPFS gave us content addressing.*
> **Executable Declarative Trees give us portable, verifiable application logic.**

---

## 🧩 What is an Executable Declarative Tree?

An **Executable Declarative Tree (EDT)** is a universal format for expressing software as **data** instead of imperative code.

Instead of describing **how** to run a program, an EDT describes **what** should happen:

* **Declarative** → logic written as structured data
* **Executable** → runs deterministically in any compatible runtime
* **Portable** → self-contained, framework-agnostic
* **Verifiable** → hash-addressable, reproducible, auditable
* **Composable** → trees can be joined, extended, or remixed

Think of it as **“JSON/YAML as software”** — rules and state as text, interpreted by a minimal runtime.

---

## 🏗️ Example: SPC as a Concrete EDT

SPC (Single Page Computer) is our reference implementation of the EDT primitive.

```json
{
  "state": { "count": 0 },
  "processors": [
    {
      "name": "increment",
      "when": "input.action === 'inc'",
      "then": { "count": "{{state.count + 1}}" }
    }
  ],
  "interfaces": {
    "counter": {
      "components": [
        { "type": "button", "label": "Increment", "action": "inc" },
        { "type": "text", "bind": "count" }
      ]
    }
  ]
}
```

**Key Insight:**
This JSON is not configuration for an app.
**It *is* the app.**

---

## 📜 Historical Lineage

1. **1970s → Objects** (Alan Kay): state + behavior encapsulation
2. **2000s → Git commits** (Torvalds): immutable snapshots of history
3. **2010s → IPFS CIDs** (Benet): content-addressed files
4. **2020s → EDTs**: content-addressed, executable applications

Just as **HTML became the portable primitive for documents**, EDTs become the portable primitive for applications.

---

## 🎯 Why EDTs Matter

### 1. Application Portability

**Before:** Apps tied to frameworks, runtimes, or cloud providers
**After:** Apps run anywhere a tree interpreter exists

### 2. AI-Native Development

**Before:** LLMs generate brittle imperative code
**After:** LLMs generate deterministic, verifiable trees

### 3. Remix Culture

**Before:** Forking requires deep code understanding
**After:** Forking = editing a few JSON properties

### 4. Verifiability

**Before:** Black-box execution, fragile audits
**After:** Transparent tree-walking, hash-verifiable logic + state

---

## 🔬 Technical Foundation

### Tree Structure

```
Application Tree
├── State        (initial conditions)
├── Processors   (pure transformations)
├── Connectors   (side effects / I/O)
└── Interfaces   (UI definition)
```

### Deterministic Runtime (pseudo-JS)

```js
function run(tree, input) {
  let state = tree.state;
  for (let rule of tree.processors) {
    if (evaluate(rule.when, state, input)) {
      state = apply(rule.then, state, input);
    }
  }
  return state;
}
```

The interpreter is \~200 lines of code. The complexity lives in the tree, not the runtime.

---

## 🌍 Real-World Analogies

* **HTML** → portable documents
* **MP3** → portable music
* **Git** → portable history
* **EDT** → portable applications

---

## 🚀 Use Cases

### Immediate

* Personal utilities (trackers, dashboards)
* Business workflows (approvals, invoices)
* Educational apps (calculators, quizzes)

### Near Future

* AI agent orchestration
* Decentralized protocols
* Verifiable business logic

### Long Term

* Smart contracts beyond blockchains
* Interplanetary apps (IPFS-native)
* Post-framework, text-native computing

---

## 🔮 The Vision

EDTs collapse the gap between **data** and **software**.

* From **imperative instructions → declarative trees**
* From **framework lock-in → runtime-agnostic apps**
* From **opaque execution → transparent, verifiable state machines**

We’re building toward a future where **apps are as portable, ownable, and remixable as data.**

---

## 📚 Learn More

* [SPC: Single Page Computer](./deckshell/README.md) – Reference implementation
* [SPEC.md](./SPEC.md) – Formal specification
* [Examples](./deckshell/examples/) – Live SPC demos

---

## 💡 Contributing

This is a new primitive. Contributions welcome in:

* Runtimes (browser, CLI, server, mobile)
* Editors & tooling (validators, debuggers)
* Applications & experiments

**Help us define the next layer of computing.**




