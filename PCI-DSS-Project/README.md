# PCI DSS Network Segmentation Review
## FinSecure Pvt. Ltd. — GRC Portfolio Project

**Author:** Pranay Kumar Moluguri — GRC Analyst  
**Standard:** PCI DSS v4.0  
**Date:** May 2026  
**Status:** Complete  

---

## 📋 Project Overview

This project demonstrates a complete PCI DSS Network Segmentation Review for **FinSecure Pvt. Ltd.** — a fictional Hyderabad-based fintech startup processing credit and debit card payments for 100,000 customers on AWS.

The core challenge: FinSecure had a **flat network** where payment servers, customer databases, development servers and employee laptops all shared the same network. A QSA audit was scheduled in 60 days. The CTO wanted to accept the risk due to budget constraints.

This project documents how I approached this as a GRC Analyst — the decisions I made, why I made them, and how I navigated real business constraints.

---

## 🎯 The Real Problem I Solved

Most PCI DSS guides tell you WHAT to implement. This project focuses on the harder questions:

- **Should we accept the risk?** (No — and here's exactly why)
- **Which systems are actually in scope?** (The development server decision was the hardest)
- **How do you implement compliance when budget is limited?** (Phased approach)
- **What happens after segmentation?** (Residual risks remain — documented here)

---

## 📁 Project Files

| File | Description |
|---|---|
| [Gap Analysis Document](./PCI_DSS_Gap_Analysis.docx) | Full assessment of all 12 PCI DSS requirements |
| [Network Diagram](./PCI_DSS_Network_Diagram.md) | Before/after network architecture visualization |
| [This README](./README.md) | Project approach, decisions and learnings |

---

## 🔍 Key Finding — The Flat Network Problem

FinSecure's flat network meant their **entire infrastructure was in PCI DSS scope** — including development servers and employee laptops that had no business reason to access card data.

This created two problems:
1. **Security risk** — any compromised system could laterally move to card data
2. **Compliance burden** — every system needed to meet PCI DSS requirements

**Solution:** Network segmentation to isolate the Cardholder Data Environment (CDE) — reducing scope by 60% and dramatically reducing security risk.

---

## ⚖️ The Hardest Decision — Development Server

The most significant judgment call in this project was the development server.

**Situation:** Developers were using real customer card numbers (PAN data) for testing payment flows.

**Business argument:** "We need real data to test accurately"

**My decision:** This is a Critical PCI DSS violation (Requirement 6) — no exceptions. Real PAN data in development is never acceptable.

**Solution I proposed:** Visa, Mastercard and Amex all provide official test card numbers specifically for development testing. No real PAN data needed. Development server can then be removed from CDE scope entirely — reducing compliance burden.

**Why this matters:** This is the kind of decision that separates a GRC analyst from a compliance checkbox filler. The technically correct answer and the business-friendly answer are the same here — but you need to understand both to communicate it effectively.

---

## 💰 Handling the Budget Constraint

The CTO requested **risk acceptance** for network segmentation due to ₹15 lakh budget constraint.

**Why I declined risk acceptance:**
- PCI DSS requirements for CDE protection are prescriptive — not discretionary
- Unlike ISO 27001 where risk acceptance is permitted for lower severity risks — PCI DSS CDE controls are mandatory
- Non-compliance exposes FinSecure to unlimited liability in a breach

**What I recommended instead:**
A phased 60-day implementation that achieves compliance within budget:
- Phase 1 (0-15 days): AWS Security Group configuration — minimal cost
- Phase 2 (15-30 days): VPC separation — low cost
- Phase 3 (30-45 days): WAF deployment — medium cost
- Phase 4 (45-60 days): Penetration test to verify — medium cost

**Total cost: ₹8-10 lakhs** — reduced from original ₹15 lakh estimate by phasing implementation.

---

## 📊 Results

| Metric | Before | After |
|---|---|---|
| PCI DSS Compliant Requirements | 0 of 12 | 10 of 12 |
| Systems in CDE Scope | 7 | 3 |
| Scope Reduction | — | 60% |
| Critical Gaps | 5 | 0 |
| Real PAN in Development | Yes ❌ | No ✅ |

---

## 🧠 What This Project Demonstrates

1. **Risk judgment** — knowing when risk acceptance is and isn't appropriate
2. **Business communication** — translating compliance requirements into business terms
3. **Scope analysis** — making defensible decisions about what is and isn't in scope
4. **Practical problem solving** — achieving compliance within real budget constraints
5. **Documentation** — producing audit-ready evidence that a QSA would accept

---

## 🔗 Related Portfolio Projects

- [ISO 27001 Gap Analysis](../ISO27001-Project/)
- [NIST CSF 2.0 Assessment](../NIST-Project/)
- [Internal Audit Report](../Audit-Project/)

---

## 📫 Connect

**Pranay Kumar Moluguri**  
GRC Analyst | ISO 27001 | NIST CSF | PCI DSS | SOC 2  
📧 pranaysteyn400@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/pranaykumarmoluguri)  

*This project is part of an independent GRC portfolio demonstrating practical framework knowledge.*
