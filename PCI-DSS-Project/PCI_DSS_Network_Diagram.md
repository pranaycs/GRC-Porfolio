# PCI DSS Network Segmentation — Network Diagram

**FinSecure Pvt. Ltd. | GRC Analyst: Pranay Kumar Moluguri | May 2026**

---

## ⚠️ BEFORE — Flat Network (NON-COMPLIANT)

```
┌─────────────────────────────────────────────────────────────────┐
│                    FLAT NETWORK (ENTIRE SCOPE)                  │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐            │
│  │  💳 Payment  │  │  🗄️ Customer │  │  💻 Dev      │            │
│  │  Processing │  │  Database   │  │  Server     │            │
│  │  Server     │  │  (PAN Data) │  │  (Real PAN!)│            │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘            │
│         │                │                │                    │
│  ───────┴────────────────┴────────────────┴──────────────────  │
│                    SINGLE FLAT NETWORK                          │
│  ───────┬────────────────┬────────────────┬──────────────────  │
│         │                │                │                    │
│  ┌──────┴──────┐  ┌──────┴──────┐  ┌──────┴──────┐            │
│  │  👤 Employee │  │  📁 HR       │  │  ☁️ AWS S3   │            │
│  │  Laptops    │  │  System     │  │  Buckets    │            │
│  └─────────────┘  └─────────────┘  └─────────────┘            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

RISK: Any compromised system = direct access to card data
SCOPE: ENTIRE network is in PCI DSS scope
FINDING: 7 Non-Compliant, 5 Partial across 12 requirements
```

---

## ✅ AFTER — Segmented Network (COMPLIANT)

```
                        🌐 INTERNET
                             │
                    ┌────────┴────────┐
                    │   🛡️ WAF / DDoS  │
                    │   Protection    │
                    └────────┬────────┘
                             │
              ┌──────────────┴──────────────┐
              │           DMZ               │
              │   ┌─────────────────────┐   │
              │   │  🔌 Payment API     │   │
              │   │  Gateway (Public)   │   │
              │   └──────────┬──────────┘   │
              └──────────────┼──────────────┘
                             │ (Firewall — strict rules)
              ┌──────────────┴──────────────┐
              │    🔒 CDE NETWORK (PCI SCOPE)│
              │                             │
              │  ┌──────────────────────┐   │
              │  │  💳 Payment Server   │   │
              │  │  (Processes PAN)     │   │
              │  └──────────────────────┘   │
              │                             │
              │  ┌──────────────────────┐   │
              │  │  🗄️ Customer Database │   │
              │  │  (Stores PAN - Enc.) │   │
              │  └──────────────────────┘   │
              │                             │
              │  ┌──────────────────────┐   │
              │  │  ☁️ S3 (Card Logs)   │   │
              │  │  (Encrypted)         │   │
              │  └──────────────────────┘   │
              │                             │
              └─────────────────────────────┘
                    (No direct access from below)
                             ║ FIREWALL ║
              ┌──────────────╨──────────────┐
              │   💻 DEVELOPMENT NETWORK     │
              │                             │
              │  ┌──────────────────────┐   │
              │  │  Dev Server          │   │
              │  │  (Test Cards Only ✅) │   │
              │  └──────────────────────┘   │
              └─────────────────────────────┘
                             ║ FIREWALL ║
              ┌──────────────╨──────────────┐
              │   🏢 CORPORATE NETWORK       │
              │                             │
              │  ┌──────────┐ ┌──────────┐  │
              │  │ Employee │ │    HR    │  │
              │  │ Laptops  │ │  System  │  │
              │  └──────────┘ └──────────┘  │
              └─────────────────────────────┘

RESULT: CDE isolated — breach in Corporate/Dev cannot reach card data
SCOPE: Only CDE network in PCI DSS scope — 60% scope reduction
```

---

## 🔥 Key Changes Made

| Area | Before | After |
|---|---|---|
| Network Architecture | Flat — all systems connected | Segmented — 3 isolated zones |
| CDE Scope | Entire network | CDE network only |
| Dev Environment | Real PAN data used | Test card numbers only |
| Firewall | None between internal systems | Strict rules between each zone |
| PCI DSS Scope | All 7 systems | Only 3 CDE systems |
| Attack Surface | High — lateral movement possible | Low — CDE fully isolated |
| Monitoring | Partial | All CDE traffic monitored |

---

## 📊 Compliance Impact

```
BEFORE segmentation:
Non-Compliant ████████████████████ 7 requirements
Partial       ████████████████     5 requirements
Compliant                          0 requirements

AFTER segmentation:
Non-Compliant                      0 requirements  
Partial       ████                 2 requirements (minor)
Compliant     ████████████████████ 10 requirements
```

---

## 🛡️ Firewall Rules — CDE Protection

### Rule Set 1 — Internet to DMZ
```
ALLOW: HTTPS (443) from Internet to Payment API
DENY:  All other traffic
```

### Rule Set 2 — DMZ to CDE
```
ALLOW: Payment API to Payment Server (Port 8443)
ALLOW: Payment Server to Customer Database (Port 5432 — encrypted)
DENY:  All other traffic
```

### Rule Set 3 — Corporate to CDE
```
DENY:  All direct access from Corporate network to CDE
ALLOW: Specific admin access via PAM solution only (with MFA)
LOG:   All access attempts
```

### Rule Set 4 — Development to CDE
```
DENY:  ALL traffic from Development to CDE
ALLOW: None — complete isolation
LOG:   Alert on any attempt
```

---

*Network Diagram — FinSecure PCI DSS Project | Pranay Kumar Moluguri | May 2026*
