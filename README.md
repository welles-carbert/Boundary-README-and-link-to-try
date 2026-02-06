
                                    Boundary

**Boundary is a control plane for agent behavior.**  
It sits between your agent and the world and enforces execution rules **without changing how your agent thinks**.

Think of it as **middleware for agent safety, cost control, and execution law**.

---

## What Boundary Does (v1)

Boundary lets you:

- Log agent actions (HTTP calls, tools, primitives)
- Evaluate them against deterministic policies
- **Block or allow actions at execution time**
- Audit *what would have happened* vs *what actually happened*
- Run in **shadow mode** or **enforce mode**

All with a single API call.

---

## Live API

Boundary is live and running here: https://boundary-9iyg.onrender.com

Health check: curl https://boundary-9iyg.onrender.com/health

## Core Concept

Boundary separates decision from consequence.
	•	Your agent still reasons however it wants
	•	Boundary evaluates the action
	•	Enforcement is controlled centrally

This makes agent behavior:
	•	Predictable
	•	Auditable
	•	Reversible
	•	Cheap to reason about

⸻

Quick Start (5 minutes)

1 Get a project API key

Project keys are issued by an admin (me for now).
Each key maps to a single project.

Once you have a key, you can immediately send events.

⸻

2 Send an event

curl -sS -X POST "https://boundary-9iyg.onrender.com/v1/events" \-H "X-API-Key: bf7c0de3aa25416bb39e0f5f310ab80e" \-H "Content-Type: application/json" \-d '{"project_id":"demo","run_id":"example-run-1","type":"value","amount":1.0,"meta":{"primitive":{"kind":"HTTP_REQUEST","method":"GET","url":"https://example.com"}}}'

Successful response:
  {
  "ok": true,
  "event_id": "...",
  "deduped": false
}

## Shadow vs Enforce Mode

Boundary supports two execution modes:

🟡 Shadow Mode
	•	Actions are allowed
	•	Violations are recorded
	•	Nothing breaks
	•	Perfect for testing and rollout

🔴 Enforce Mode
	•	Violations return HTTP 403
	•	Action is blocked at execution
	•	Still fully audited

Enforcement is a deployment setting, not a code change.

⸻

## Audit & Replay

See what Boundary decided:
curl "https://boundary-9iyg.onrender.com/v1/audit/decisions?run_id=example-run-1" \-H "X-API-Key: bf7c0de3aa25416bb39e0f5f310ab80e"


you get:
	•	decision
	•	action (allow / block)
	•	rule_id
	•	severity
	•	enforce flag

This lets you answer:

“What would my agent have done if enforcement was on?”

⸻

## Why Boundary Exists

Agent failures are not reasoning failures — they are execution failures.

Boundary exists to:
	•	Prevent runaway behavior
	•	Control cost and tool usage
	•	Create provable guardrails
	•	Enable safe autonomy

It’s designed to be invisible when correct and decisive when wrong.

⸻

## Current Status
	•	✅ Live API
	•	✅ Shadow + Enforce modes
	•	✅ Project-level keys
	•	✅ Audit trail
	•	🚧 SDKs coming later
	•	🚧 Policy authoring UI later

Boundary is intentionally API-first.


The demo API key is scoped to the demo project.
Using it with any other project_id will be rejected. 

⸻
Demo Key Limits

The public demo API key is rate-limited to prevent abuse.

Limits (demo project only):
	•	30 events per minute
	•	HTTP 429 returned when exceeded

This limit does not apply to real project keys.

## Contact / Access

If you want a project key or want to integrate Boundary:

Open an issue or reach out directly. 
