# AWS Cloud Identity Compromise — Detection & Incident Response

> **Status:** 🟡 In progress environment build
> **Last updated:** _2026-10-4_

Staged an IAM credential-compromise attack in a dedicated AWS account, then investigated it end-to-end from CloudTrail and GuardDuty as the responding analyst. Focus is detection engineering and IR process, not console configuration.

**Skills demonstrated:** CloudTrail/Athena threat hunting · GuardDuty triage · MITRE ATT&CK Cloud Matrix mapping · NIST IR lifecycle

---

## Table of Contents
- [Environment](#part-0--environment-build)
- [Attack Scenario](#part-1--attack-scenario)
- [Detection](#part-2--detection)
- [Investigation & Timeline](#part-3--investigation--timeline)
- [Response & Lessons Learned](#part-4--response--lessons-learned)
- [Teardown](#part-5--teardown)
- [Appendix: Query Library](#appendix-a--query-library)
- [Appendix: ATT&CK Mapping](#appendix-b--attck-mapping)
- [Working Log](#working-log)

---

## Part 0 — Environment Build

### Cost guardrails
| Control | Value | Status | Evidence |
|---|---|---|---|
| AWS Budget alarm | $5 | ⬜ | `screenshots/00-budget-alarm.png` |
| Account type | Free plan, dedicated | ⬜ | |
| GuardDuty trial start date | _YYYY-MM-DD_ (30 days) | ⬜ | |
| Region | _e.g. us-east-1_ | ⬜ | |

### Build steps
| # | Component | Config notes | Status | Evidence |
|---|---|---|---|---|
| 1 | AWS account | | ⬜ | |
| 2 | Billing alarm | | ⬜ | |
| 3 | CloudTrail multi-region trail → S3 | Log file validation: on | ⬜ | |
| 4 | GuardDuty | | ⬜ | |
| 5 | Athena table over trail bucket | Partition projection | ⬜ | |
| 6 | CloudGoat / Stratus + Terraform | | ⬜ | |
| 7 | AWS CLI profiles | | ⬜ | |

### Architecture diagram
_What is logging what — attacker path, log flow, query path._

![Architecture](diagrams/architecture.png)

### Athena table definition
```sql
-- paste CREATE EXTERNAL TABLE statement here
```

---

## Part 1 — Attack Scenario

**Tool / scenario:** _CloudGoat `scenario-name` or Stratus techniques_
**Why this one:** _1–2 sentences._

### Attacker narrative

| Step | ATT&CK ID | Technique | API calls | Timestamp (UTC) | Evidence |
|---|---|---|---|---|---|
| 1 | T1078.004 | Valid Accounts: Cloud Accounts | | | |
| 2 | T1087.004 | Account Discovery | `sts:GetCallerIdentity`, `iam:List*` | | |
| 3 | T1580 | Cloud Infra Discovery | `s3:ListBuckets` | | |
| 4 | T1098.003 | Account Manipulation: Add Role | `iam:AttachUserPolicy` | | |
| 5 | T1530 | Data from Cloud Storage | `s3:GetObject` | | |
| 6 | T1562.008 | Impair Defenses: Disable Cloud Logs | `cloudtrail:StopLogging` | | |

### Raw evidence
<details>
<summary>Step 1 — CloudTrail event JSON</summary>

```json

```
</details>

---

## Part 2 — Detection

### GuardDuty findings
| Finding type | Severity | Triggered by | Time to fire | Screenshot |
|---|---|---|---|---|
| | | | | |

**Interpretation:** _What each finding actually tells an analyst, and what it leaves out._

### Custom Athena detections

#### D-01 — _Name_
**Detects:** _attacker step_ | **ATT&CK:** _ID_
```sql

```
**Result:**
| | |
|---|---|

_(repeat per detection)_

### Coverage gap analysis
| Attacker action | GuardDuty | Custom query | Notes |
|---|---|---|---|
| | ⬜ | ⬜ | |

**Takeaway:** _What only custom hunting caught._

---

## Part 3 — Investigation & Timeline

| # | Time (UTC) | Principal | Source IP | User agent | API call | Result | Assessment |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

### Scope
- **Confirmed accessed:**
- **Confirmed attempted / denied:**
- **Not established by logs:** _State what CloudTrail cannot rule out. Do not claim absence of action from absence of a log line._

---

## Part 4 — Response & Lessons Learned

### Containment (NIST SP 800-61)
| Phase | Action | Command / console step | Time |
|---|---|---|---|
| Containment | | | |
| Eradication | | | |
| Recovery | | | |

### Root cause


### Preventive controls
| Control | Addresses | Implemented? |
|---|---|---|
| SCP denying `cloudtrail:StopLogging` | Defense evasion | ⬜ |
| Access key rotation / IAM Identity Center | Initial access | ⬜ |
| Least-privilege policy review | Priv esc | ⬜ |
| CloudTrail log-file validation | Log tampering | ⬜ |

### Detection tuning
_What you'd add to catch this faster._

---

## Part 5 — Teardown

- [ ] `cloudgoat destroy` / `stratus cleanup`
- [ ] Delete CloudTrail trail
- [ ] Disable GuardDuty
- [ ] Empty + delete S3 buckets
- [ ] Delete Athena tables/workgroup
- [ ] Delete IAM users, keys, roles
- [ ] Confirm $0 in Billing
- [ ] All screenshots saved locally ✅

---

## Appendix A — Query Library
_Reusable, parameterized versions of every Athena query above._

```sql

```

---

## Appendix B — ATT&CK Mapping

| Tactic | Technique | ID | Observed as | Detection |
|---|---|---|---|---|
| Initial Access | | | | |
| Discovery | | | | |
| Privilege Escalation | | | | |
| Collection | | | | |
| Exfiltration | | | | |
| Defense Evasion | | | | |

---

## Working Log

Running notes — errors, fixes, dead ends. Keep the failures; they're the interesting part.

| Date | Area | Issue | Fix / outcome |
|---|---|---|---|
| | | | |

---

## Repo Structure
```
.
├── README.md
├── screenshots/
├── diagrams/
├── queries/
├── evidence/          # raw CloudTrail JSON
└── terraform/
```
