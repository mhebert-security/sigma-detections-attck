# Contributing

This document describes the workflow for adding a rule. There are no shortcuts around the quality gate — a rule that hasn't been validated against real telemetry doesn't ship as anything above `experimental`.

---

## Workflow

### 1. Pick a technique

Choose a MITRE ATT&CK technique you want to detect. Check `docs/coverage-matrix.md` to avoid duplication. If the technique is already covered, you can add a second rule for a different detection angle — name it distinctly.

### 2. Write the Sigma rule

Copy `RULE_TEMPLATE.yml` into the correct tactic folder:

```
rules/<tactic>/<descriptive_rule_name>.yml
```

Name the file by what the rule detects, not by technique ID. The ATT&CK ID belongs inside `tags`.

Required fields: `title`, `id` (UUID4), `status`, `description`, `references`, `author`, `date`, `tags`, `logsource`, `detection`, `falsepositives`, `level`.

Set `status: experimental` until validation is complete.

Generate a UUID:
```bash
python3 -c "import uuid; print(uuid.uuid4())"
```

### 3. Translate to Wazuh XML

Create the matching file:
```
wazuh/<tactic>/<same_filename>.xml
```

- Claim the next available Wazuh rule ID from `docs/coverage-matrix.md`
- Verify field names against actual Wazuh-parsed Sysmon events, not assumed names
- Include the technique ID in `<mitre>` and group tags
- Update `docs/coverage-matrix.md` with the new next available ID

### 4. Deploy and test

Deploy the Wazuh XML to the manager:
```bash
cp wazuh/<tactic>/<rule>.xml /var/ossec/etc/rules/
systemctl restart wazuh-manager
```

Run the matching Atomic Red Team test on the Windows agent:
```powershell
Invoke-AtomicTest TXXXX.YYY -TestNumbers N
```

Confirm the Wazuh alert fires with the correct rule ID.

### 5. Document evidence

Fill out the evidence template in:
```
tests/<tactic>/TXXXX.YYY_evidence.md
```

Capture: lab environment, Atomic command, raw Sysmon event fields, Wazuh alert JSON, and any tuning notes.

### 6. Update status and matrix

- Promote rule `status` from `experimental` to `test` (alert fired) or `stable` (FP rate assessed)
- Update `docs/coverage-matrix.md` — technique row and tactic count

---

## Shipping Checklist

Before marking a rule `stable`:

- [ ] Sigma rule lints clean (`sigma check <rule>.yml`)
- [ ] UUID is unique — not copied from the template
- [ ] All required fields present and non-empty
- [ ] `tags` includes both tactic and technique (`attack.<tactic>` and `attack.tXXXX.YYY`)
- [ ] Wazuh XML deployed and rule ID confirmed active
- [ ] Atomic test run and Wazuh alert captured
- [ ] Evidence file complete in `tests/`
- [ ] False positives are specific, not generic
- [ ] `docs/coverage-matrix.md` updated

---

## Field Name Reference (Sysmon → Wazuh)

| Sysmon field | Wazuh parsed field |
|---|---|
| Image | win.eventdata.image |
| CommandLine | win.eventdata.commandLine |
| ParentImage | win.eventdata.parentImage |
| ParentCommandLine | win.eventdata.parentCommandLine |
| TargetFilename | win.eventdata.targetFilename |
| DestinationIp | win.eventdata.destinationIp |
| DestinationPort | win.eventdata.destinationPort |
| User | win.eventdata.user |
| Hashes | win.eventdata.hashes |
| CurrentDirectory | win.eventdata.currentDirectory |
| IntegrityLevel | win.eventdata.integrityLevel |
