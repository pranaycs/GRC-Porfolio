# ISO 27001:2022 Statement of Applicability
## FinSecure Pvt. Ltd. — GRC Portfolio Project

**Author:** Pranay Kumar Moluguri — GRC Analyst  
**Standard:** ISO/IEC 27001:2022  
**Date:** May 2026  
**Status:** Complete  

---

## 📋 Project Overview

This project documents a complete **ISO 27001:2022 Statement of Applicability (SoA)** for **FinSecure Pvt. Ltd.** — a fictional Hyderabad-based fintech startup processing payments for 100,000 customers on AWS.

The SoA is one of the most important documents in an ISO 27001 ISMS. It answers three questions:
1. Which of the 93 Annex A controls apply to this organization — and WHY?
2. Which controls are excluded — and can that be defended under audit?
3. What is the current implementation status of each control?

---

## 🎯 The Real Challenge

Most people treat the SoA as a checkbox exercise — mark everything "applicable" and move on.

**That's wrong — and auditors know it.**

The real challenge is making **defensible decisions** about applicability. Every exclusion will be questioned by the auditor. Every inclusion must be justified by an identified risk.

For FinSecure, the hardest decisions were:

---

## ⚖️ Key Decisions I Made

### Decision 1 — Excluding Physical Controls (A.7.3 to A.7.8)

**The situation:** ISO 27001 has several controls around physical security of server rooms and equipment.

**My decision:** Exclude them — with documented justification.

**Why:** FinSecure is a cloud-native company. All infrastructure runs on AWS. There are no on-premises servers or data centres. Physical security is handled by AWS under their ISO 27001 certification.

**How I defended it:** FinSecure must maintain evidence that AWS provides equivalent controls — specifically AWS SOC 2 reports and AWS ISO 27001 certificate. These must be kept on file as justification for the exclusion.

**What an auditor would ask:** "How do you know AWS is physically secure?" — Answer: "We review AWS compliance reports annually as part of our vendor risk assessment process."

---

### Decision 2 — Segregation of Duties (A.5.3) as Highest Priority

**The situation:** A single administrator manages all AWS production access.

**My decision:** Mark as Not Implemented — Critical priority.

**Why this matters:** This is not just an ISO 27001 issue. It's a fundamental control failure. One compromised or malicious admin = complete access to all 100,000 customer payment records.

**The business pushback:** "We only have 4 people — how do we segregate duties?"

**My response:** Segregation doesn't require more people. It requires role separation:
- One person approves access changes
- A different person implements them
- A third person reviews audit logs
- No single person does all three

---

### Decision 3 — A.5.14 Information Transfer (Critical Finding)

**The situation:** Internal APIs between payment server and customer database use unencrypted HTTP.

**My decision:** Applicable — Not Implemented — Critical.

**Why:** Card data transmitted in plaintext across internal network. An attacker already inside the network can capture PAN data without ever touching the database directly.

**Business pushback:** "It's internal traffic — surely it's safe?"

**My response:** The "internal is safe" assumption is one of the most dangerous in security. Once a threat actor is inside your network — through phishing, a compromised employee device or a vendor breach — all your internal traffic is visible. TLS costs nothing to implement. The risk of not implementing it is enormous.

---

## 📊 Controls Visual Overview

```
ISO 27001:2022 — 93 Total Annex A Controls
├── Assessed in this SoA: 38 controls
└── Excluded (cloud-only justification): 5 controls

APPLICABILITY BREAKDOWN:
────────────────────────────────────────
Applicable          ████████████████████ 35 controls (92%)
Partially Applicable ██                   2 controls  (5%)
Not Applicable      █                    1 control   (3%)

IMPLEMENTATION STATUS:
────────────────────────────────────────
Implemented         ███                  3 controls  (8%)
Planned             ████████             8 controls  (21%)
Not Implemented     █████████████████████27 controls  (71%)

KEY MESSAGE: 71% not implemented = significant work ahead
             This is NORMAL for a startup beginning ISO 27001 journey
```

---

## 🗂️ Control Themes Breakdown

```
┌─────────────────────────────────────────────────────────┐
│              ISO 27001:2022 ANNEX A THEMES              │
├─────────────────┬───────────────┬───────────────────────┤
│   ORGANIZATIONAL│    PEOPLE     │      PHYSICAL         │
│     (A.5.x)     │   (A.6.x)     │      (A.7.x)           │
│                 │               │                       │
│  19 controls    │  5 controls   │  5 controls           │
│  assessed       │  assessed     │  assessed             │
│                 │               │                       │
│  Highest risk   │  Training &   │  3 excluded           │
│  area for       │  HR security  │  (cloud company)      │
│  FinSecure      │  focus        │  2 applicable         │
├─────────────────┴───────────────┴───────────────────────┤
│                   TECHNOLOGICAL (A.8.x)                 │
│                                                         │
│              9 controls assessed                        │
│                                                         │
│   A.8.2  Privileged Access  → NOT IMPLEMENTED CRITICAL  │
│   A.8.7  Malware Protection → PARTIAL                   │
│   A.8.8  Vuln Management   → NOT IMPLEMENTED            │
│   A.8.9  Config Management → NOT IMPLEMENTED            │
│   A.8.13 Backup            → PLANNED                    │
│   A.8.15 Logging           → PLANNED                    │
│   A.8.24 Cryptography      → PARTIAL (HTTP gap)         │
│   A.8.28 Secure Coding     → NOT IMPLEMENTED            │
└─────────────────────────────────────────────────────────┘
```

---

## 🚨 Top 5 Critical Gaps

```
┌─────┬──────────┬────────────────────────────┬──────────────────────────┐
│ #   │ Control  │ Gap                        │ Business Risk            │
├─────┼──────────┼────────────────────────────┼──────────────────────────┤
│ 1   │ A.5.3    │ Single admin — no          │ One account compromise = │
│     │          │ segregation of duties      │ full CDE access          │
├─────┼──────────┼────────────────────────────┼──────────────────────────┤
│ 2   │ A.5.14   │ Internal APIs use HTTP     │ Card data visible in     │
│     │          │ — no encryption            │ plaintext on network     │
├─────┼──────────┼────────────────────────────┼──────────────────────────┤
│ 3   │ A.5.15   │ All staff have CDE         │ Excessive access =       │
│     │          │ access — no RBAC           │ insider threat risk      │
├─────┼──────────┼────────────────────────────┼──────────────────────────┤
│ 4   │ A.8.2    │ No PAM solution —          │ Privileged sessions      │
│     │          │ admin unmonitored          │ not recorded or audited  │
├─────┼──────────┼────────────────────────────┼──────────────────────────┤
│ 5   │ A.5.24   │ No Incident Response       │ Breach response will be  │
│     │          │ Plan exists                │ chaotic and slow         │
└─────┴──────────┴────────────────────────────┴──────────────────────────┘
```

---

## 📅 Implementation Roadmap

```
MONTH 1 — CRITICAL (Weeks 1-4)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Week 1-2:  A.5.3  Segregation of duties
           A.5.15 Access control — RBAC
           A.5.17 Password policy + MFA
           A.8.2  PAM solution deployment
           A.5.14 Force TLS on all APIs

Week 3-4:  A.5.1  Finalize security policy
           A.5.24 Develop Incident Response Plan
           A.6.3  Launch awareness training

MONTH 2 — HIGH PRIORITY (Weeks 5-8)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Week 5-6:  A.8.8  Vulnerability scanning program
           A.8.9  CSPM tool + AWS hardening
           A.5.19 Vendor risk assessments

Week 7-8:  A.6.5  Automate offboarding
           A.5.18 Quarterly access reviews
           A.5.12 Data classification scheme

MONTH 3-4 — PLANNED (Weeks 9-16)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
           A.8.13 Backup restoration testing
           A.8.15 Complete SIEM onboarding
           A.7.9  MDM deployment
           A.5.29 BCP testing
           A.8.28 Secure coding standards
```

---

## 💡 What I Learned

**1. The SoA is about judgment, not compliance**
The hardest part isn't knowing the controls — it's making defensible decisions about which ones apply and which don't. Every exclusion is a risk decision.

**2. Cloud changes everything**
A cloud-native company like FinSecure has a fundamentally different control landscape than a traditional on-premises company. The SoA must reflect this — not blindly apply all 93 controls.

**3. Implementation status is a journey, not a destination**
71% not implemented sounds alarming. But for a startup beginning its ISO 27001 journey — this is normal. The SoA tracks progress over time. The certification audit evaluates the journey, not just the destination.

**4. Auditors look for honesty, not perfection**
An auditor would rather see honest "Not Implemented" status with a clear remediation plan than inflated "Implemented" claims that fall apart under questioning.

---

## 📁 Project Files

| File | Description |
|---|---|
| [Statement of Applicability](./ISO27001_Statement_of_Applicability_FinSecure.docx) | Complete SoA document — all 38 controls |
| [This README](./README.md) | Project approach, decisions and visual overview |

---

## 🔗 Related Projects

- [PCI DSS Network Segmentation Review](../PCI-DSS-Project/)
- [NIST CSF 2.0 Assessment](../NIST-Project/)
- [Internal Audit Report](../Audit-Project/)

---

## 📫 Connect

**Pranay Kumar Moluguri** — GRC Analyst  
📧 pranaysteyn400@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/pranaykumarmoluguri)  

*Part of an independent GRC portfolio demonstrating practical ISO 27001 knowledge.*
