# PCI DSS Risk Assessment — FinSecure Pvt. Ltd.

**Assessor:** Pranay Kumar Moluguri — GRC Analyst  
**Date:** May 2026  
**Standard:** PCI DSS v4.0  
**Methodology:** Likelihood × Impact (Scale 1-5, Max Score 25)

---

## Risk Scoring Matrix

| Score | Level | Action Required | Timeframe |
|---|---|---|---|
| 20-25 | 🔴 CRITICAL | Immediate remediation | 7 days |
| 12-19 | 🟠 HIGH | Priority remediation | 30 days |
| 6-11 | 🟡 MEDIUM | Planned remediation | 90 days |
| 1-5 | 🟢 LOW | Monitor and review | Annual |

---

## Risk Register

| Risk ID | Risk | FinSecure Context | Likelihood | Impact | Score | Level | PCI DSS Req | Recommended Action |
|---|---|---|---|---|---|---|---|---|
| R-001 | Weak Password Policy | No password policy exists. Simple passwords likely used across all systems including CDE access. No enforcement mechanism in place. | 4 | 5 | **20** | 🔴 CRITICAL | Req 8.3.6 | Enforce 12 character minimum with complexity rules. Implement 90-day rotation for all CDE accounts. Deploy password manager. |
| R-002 | No MFA for Admin Accounts | Admin accounts have single factor authentication only. Stolen or guessed password gives immediate full CDE access. | 5 | 5 | **25** | 🔴 CRITICAL | Req 8.4.2 | Implement MFA immediately for ALL CDE access without exception. No single factor authentication permitted for admin accounts. |
| R-003 | Unpatched Systems | No formal patch management process. Systems not regularly updated. Known vulnerabilities likely present in payment processing and database servers. | 4 | 4 | **16** | 🔴 CRITICAL | Req 6.3.3 | Establish monthly patch management cycle. Critical patches within 7 days. High severity within 30 days. Automate patching where possible. |
| R-004 | No Log Monitoring | Splunk deployed but CDE systems not fully onboarded. No alerting configured for suspicious CDE activity. No defined log retention period. | 4 | 4 | **16** | 🔴 CRITICAL | Req 10.4 | Onboard all CDE systems to Splunk immediately. Configure alerts for failed logins, privilege escalation and unusual access. Set 12-month log retention. |
| R-005 | Shared Administrative Accounts | Development team shares admin credentials for convenience. No individual accountability. Impossible to determine who took what action in CDE. | 3 | 4 | **12** | 🟠 HIGH | Req 8.2.1 | Eliminate all shared accounts. Assign unique IDs to every user. Implement PAM solution for privileged account management. |
| R-006 | Insecure Transmission of Cardholder Data | Internal APIs between payment server and customer database use unencrypted HTTP. Card data transmitted in plaintext across internal network. | 4 | 5 | **20** | 🔴 CRITICAL | Req 4.2.1 | Force TLS 1.2 minimum on ALL internal and external communications involving card data. No HTTP permitted in CDE. Immediate action required. |
| R-007 | Excessive User Privileges | All developers have direct read access to customer database. Access not restricted to business need. Violates principle of least privilege. | 4 | 4 | **16** | 🔴 CRITICAL | Req 7.2.1 | Implement RBAC immediately. Revoke all unnecessary CDE access. Document and formally approve all remaining access with business justification. |
| R-008 | No Periodic Access Review | No quarterly access reviews conducted. Former contractors and employees may retain active credentials. No process to identify orphaned accounts. | 3 | 4 | **12** | 🟠 HIGH | Req 7.2.4 | Conduct immediate access audit — identify and remove all unnecessary accounts. Implement quarterly access review process. Automate account deprovisioning on termination. |

---

## Risk Summary

| Level | Count | Percentage |
|---|---|---|
| 🔴 CRITICAL | 6 | 75% |
| 🟠 HIGH | 2 | 25% |
| 🟡 MEDIUM | 0 | 0% |
| 🟢 LOW | 0 | 0% |
| **Total** | **8** | **100%** |

---

## GRC Analyst Observations

The concentration of Critical risks at FinSecure reflects a common pattern in fintech startups — security is deprioritized during rapid growth phases. The most urgent finding is the combination of **no MFA (R-002)** and **excessive privileges (R-007)** — together these mean any compromised credential gives immediate unrestricted access to all customer card data.

The **unencrypted internal API (R-006)** is particularly concerning as it exposes card data to interception even within the internal network — meaning a threat actor who has already breached any internal system can capture card data in transit.

**Priority order for immediate remediation:**
1. R-002 — MFA (fastest to implement, highest impact)
2. R-006 — Encrypt internal APIs (critical data exposure)
3. R-007 — Remove excessive privileges (scope reduction)
4. R-001 — Password policy (foundational control)
5. R-004 — Log monitoring (detection capability)
6. R-003 — Patch management (vulnerability reduction)
7. R-005 — Eliminate shared accounts (accountability)
8. R-008 — Access reviews (ongoing governance)

---

*PCI DSS Risk Assessment | FinSecure Pvt. Ltd. | Pranay Kumar Moluguri — GRC Analyst | May 2026*
