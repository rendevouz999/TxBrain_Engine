# 🧩 System Overview — TxBrain Engine

## 1. Architecture Summary
TxBrain Engine is a modular AI-driven self-repair framework.  
It consists of **three primary layers**:

### 🧠 Layer 1 — Brain Core
- Signature detection & hashing (`sha1`)
- Log anomaly scanning (`scan_logs`)
- Heuristic rule mapping (`heuristic_validate`)
- AI/LLM query fallback (`call_gemini_for_solution`)

### 🔧 Layer 2 — Execution Framework
- Safe command execution (`safe_exec`)
- Dry-run validation (`execute_solution`)
- Automatic rollback
- Sandbox testing mode (isolated virtual env)

### 🌐 Layer 3 — Knowledge Synchronization
- Bidirectional sync with **Shelter Server**
- Local SQLite persistence:
  - `knowledge.db`
  - `resolution_memory`
- Remote knowledge sharing via `/api/v1/submit_knowledge`

---

## 2. Workflow Overview
```plaintext
[Error Detected] → [Signature Generated] 
 → [Search Local Knowledge] 
   → (Found?) → [Execute Solution] 
   → (Not Found?) → [Ask Gemini AI] 
       → [Sandbox Test] 
         → [Store to DB if Safe]
```

---

## 3. Integration Points
- **Shelter Server**: RESTful API knowledge hub.
- **Gemini AI**: Large Language Model interface for reasoning.
- **Sandbox Layer**: Isolated environment for safe auto-testing.

---

## 4. Design Goals
- Self-improving AI for autonomous maintenance
- Modular integration for any infrastructure (desktop, server, IoT)
- Minimal dependencies — pure Python core
- Privacy-first local learning

---

© 2025 **Jxey**  
📧 `joefreccejunior50@gmail.com`  
R&D Project — *AI Shelter / TxBooster*
