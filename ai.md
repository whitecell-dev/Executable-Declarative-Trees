# 🤖 ai.md

**AI-Native Application Generation with Executable Declarative Trees (EDTs)**

---

## 1. Why AI + EDT Works

Traditional codegen with LLMs is fragile:

* Framework-specific
* Hard to validate
* Brittle, non-deterministic

**EDTs fix this:**

* JSON/YAML format → LLMs can generate reliably
* Schema-constrained → output can be instantly validated
* Deterministic runtime → runs the same everywhere (Deck.Shell, Axis CLI, Kern)

👉 Instead of producing “code,” the AI produces a **valid application tree**.

---

## 2. Workflow Overview

1. **Describe the app you want** in natural language

   ```
   I need a simple expense tracker:
   - Inputs: expense name + amount
   - Stores history
   - Displays total spent
   - Alerts if total > $1000
   ```

2. **Prompt an AI model (Claude, GPT, etc.)** with:

   * `SPEC.md` (what EDTs are, service types)
   * `schemas/*.json` (valid shapes)
   * Optionally `examples/*.json` for reference

3. **AI outputs an `edt.json`** that conforms to `edt.schema.json`.

4. **Validate** the output:

   ```bash
   ajv validate -s schemas/edt.schema.json -d edt.json
   ```

5. **Run in Deck.Shell:**

   * Drop JSON inline into `<script type="application/json">`
   * OR (coming soon) **import the file directly** via UI button

---

## 3. Example Prompt

```
You are an SPC generator.  
Follow `SPEC.md` and output JSON that validates against `schemas/edt.schema.json`.  
App: A Bitcoin ROI dashboard
- Connector: fetch BTC price
- Processor: calculate ROI from $30,000 baseline
- Monitor: alert if price > $80k or < $20k
- Interface: input initial/final, display ROI%
```

**Expected AI Output:**

```json
{
  "spc_version": "1.0",
  "meta": {
    "name": "btc-roi-dashboard",
    "exported_at": "2025-09-23T03:11:00.000Z"
  },
  "services": { ... },
  "state": { ... }
}
```

---

## 4. Tips for Reliable Generation

* **Anchor to the schema.** Always remind the AI:
  *“Validate against `edt.schema.json`.”*
* **Keep prompts declarative.** Describe *what* you want, not *how to code it*.
* **Iterate quickly.** If JSON doesn’t validate, show the AI the error message and retry.
* **Start small.** Simple SPCs (calculators, trackers) → then build up to complex apps.

---

## 5. Roadmap

* ✅ Copy-paste JSON into HTML `<script>` blocks
* 🔜 File import button in Deck.Shell (`edt.json` upload → instant app)
* 🔜 Direct AI integration: “Describe your app → get SPC” inside MicroService OS

---

## 6. Why This is Big

This flips the paradigm:

* **Before:** AI tries to write code → fragile.
* **Now:** AI fills in a structured Sudoku puzzle → valid app.
* **Future:** Everyone can *describe* software, validate it, and run it — without ever touching imperative code.

---

✍️ **Summary**: AI + EDT = a reliable, verifiable pipeline from natural language → running application. 


