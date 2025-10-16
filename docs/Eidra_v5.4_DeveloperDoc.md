# 🧠 Eidra v5.4 Developer Documentation

## Overview
Eidra is a **peer AI guardian system** designed to observe, review, and safeguard the actions of `TxBrain_Engine`.  
It provides dynamic, trust-based decision-making using a hybrid of risk evaluation, emotional decay, and experience-driven learning.

---

## 📂 Folder Structure

```
eidra/
├── core/
│   ├── compat.py          # Python version checks & system compatibility
│   ├── dark_gate.py       # Command safety validator (forbidden commands filter)
│   ├── emotion.py         # Emotional decay engine & mood management
│   ├── experience.py      # Experience-based outcome tracking
│   ├── heartbeat.py       # Heartbeat metrics writer (CPU, memory)
│   ├── ledger.py          # Core event storage & communication hub
│   ├── locks.py           # Resource locking to avoid race conditions
│   ├── memory.py          # Actor trust profile & failure rate tracking
│   ├── reviewer.py        # Decision engine combining trust, risk & emotion
│   ├── risk.py            # Static risk estimator
│   └── simulate.py        # Dry-run validator for proposed actions
│
├── eidra_server.py        # Main runtime loop for decision-making process
├── policy_tiers.json      # Policy configuration & thresholds
└── tools/
    ├── feelings.py        # Command-line mood introspection
    └── policy_report.py   # Policy summary generator
```

---

## 🧩 Core Components

| Module | Description | Key Functions |
|--------|--------------|----------------|
| `ledger.py` | Central database for proposals, verdicts, and heartbeats. | `insert_event`, `insert_vote`, `fetch_new_proposals` |
| `reviewer.py` | Decision engine that integrates trust, emotion, and experience data. | `review_proposal`, `_load_policy` |
| `memory.py` | Maintains trust level and failure rate of each actor. | `update_profile_from_db`, `recent_failure_rate` |
| `experience.py` | Tracks execution outcomes for learning. | `record_outcome`, `risk_adjustment` |
| `emotion.py` | Manages emotional decay and mood influence on risk decisions. | `get_mood`, `update_on_verdict` |
| `dark_gate.py` | Prevents unsafe command execution. | `check_text` |
| `simulate.py` | Ensures proposed commands are safe before execution. | `dry_run`, `is_safe_command` |
| `locks.py` | Prevents concurrent DB writes on same resource. | `with_lock` |
| `heartbeat.py` | Reports Eidra’s health to ledger. | `beat` |

---

## ⚙️ Data Flow: TxBrain ↔ Eidra

```
┌──────────────────┐          proposal(event)         ┌──────────────────┐
│  TxBrain_Engine  │ ───────────────────────────────▶ │      Eidra       │
│  (self-healing)  │                                 │  (risk reviewer) │
└──────────────────┘ ◀────── verdict(decision) ──────┘

   │                                               │
   │ outcome(success/fail)                         │
   ▼                                               ▼
ledger.outcomes table updated           emotion & memory updated
```

---

## 🧠 Decision Formula

Eidra evaluates a proposed action using:
```
score = benefit * confidence - λ_eff * (risk - adj)
```
Where:
- `benefit` = estimated utility of the action
- `confidence` = TxBrain's internal belief in success
- `risk` = estimated harm potential based on resource
- `adj` = experience-based risk adjustment
- `λ_eff` = dynamic lambda (depends on mood, trust, failure rate)

Decision thresholds:
- `score ≥ allow` → ✅ allow
- `revise ≤ score < allow` → ⚠️ revise
- `score < revise` → ❌ deny

---

## 💾 Database Schema

**peer_ledger.db**
- `events(id, ts, actor, type, correlation_id, payload_json, hash_prev, hash_curr)`  
- `verdicts(id, ts, correlation_id, from_actor, decision, score, reasons_json)`  
- `outcomes(id, ts, correlation_id, actor, cmd, resource, ok, meta_json)`  
- `heartbeats(id, ts, actor, status, metrics_json)`  
- `locks(resource, holder, lease_until)`  

**emotion_state (within peer_ledger)**  
Stores mood level & decay over time.

---

## ❤️ Emotional Dynamics

- Mood range: `-1.0` (pessimistic) → `+1.0` (optimistic)  
- Decay half-life: ~8 hours (configurable)  
- Influences decision threshold via `λ_eff`  

Updates:
- Verdict `allow` → mood +0.05  
- Verdict `deny` → mood -0.05  
- Outcome success → mood +0.03  
- Outcome fail → mood -0.03  

---

## 🔄 Lifecycle Overview

1. **TxBrain proposes action** → stored as event
2. **Eidra reviews proposal**
   - Calculates trust, mood, and risk
   - Produces decision (`allow/revise/deny`)
3. **TxBrain executes or skips** based on verdict
4. **TxBrain reports outcome** → affects Eidra’s learning
5. **Eidra adjusts emotion & memory** accordingly

---

## 📊 Phase Development Roadmap

| Phase | Status | Description |
|-------|---------|--------------|
| 1 | ✅ Done | Core modules: ledger, memory, emotion, experience, reviewer |
| 2 | 🟡 Ongoing | TxBrain ↔ Eidra sync cycle with adaptive learning |
| 3 | 🔜 Planned | Emotion-aware auto-policy & contextual review weighting |
| 4 | 🔜 Planned | Multi-agent federation (Eidra cluster coordination) |

---

## 🧪 Testing Commands

**Start Eidra server**
```bash
set RISK_LAMBDA=0.7
python eidra/eidra_server.py --loop --debug
```

**Send proposal manually**
```bash
python - <<'PY'
import time
from eidra.core import ledger
corr = "txb-" + str(int(time.time()*1000))
plan = {
  "commands": [{"cmd":"echo eidra-test"}],
  "verify": {"check_cmd": "echo verify-ok"},
  "rollback": [{"cmd":"echo rollback-noop"}],
  "resource": "generic",
  "meta": {"est_benefit": 0.8, "confidence": 0.9}
}
ledger.insert_event(actor="txbrain", type_="proposal", correlation_id=corr,
    payload={"correlation_id":corr,"actor":"txbrain","type":"proposal","plan":plan})
print("sent:", corr)
PY
```

---

## 🧾 Version Info

**Version:** Eidra v5.4  
**Author:** Jxey (AI Shelter / TxBooster_INT Project)  
**Collaborator:** TxBrain_Engine (peer agent)  
**License:** Internal R&D License (non-commercial)

---
