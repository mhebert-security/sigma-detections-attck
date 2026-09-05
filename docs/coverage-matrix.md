# ATT&CK Coverage Matrix

Last updated: 2026-09-05
Next available Wazuh rule ID: **100002**

---

## Status Key

| Symbol | Meaning |
|--------|---------|
| 🧪 | `experimental` — written, Atomic test not yet run |
| 🔬 | `test` — Atomic test fired, Wazuh caught it, under tuning |
| ✅ | `stable` — validated, false-positive rate assessed |

---

## Coverage Table

| Tactic | Technique | Sub-technique | Rule File | Wazuh ID | Atomic Test | Status |
|--------|-----------|---------------|-----------|----------|-------------|--------|
| Execution | T1059 | T1059.001 — PowerShell | [proc_create_powershell_encoded_command.yml](../rules/execution/proc_create_powershell_encoded_command.yml) | 100001 | `Invoke-AtomicTest T1059.001 -TestNumbers 1` | 🧪 |

---

## Coverage by Tactic

| Tactic | Rules |
|--------|-------|
| Initial Access | 0 |
| Execution | 1 |
| Persistence | 0 |
| Privilege Escalation | 0 |
| Defense Evasion | 0 |
| Credential Access | 0 |
| Discovery | 0 |
| Lateral Movement | 0 |
| Collection | 0 |
| Command and Control | 0 |
| Exfiltration | 0 |
| **Total** | **1** |
