# 🔐 PCI DSS Phase in Payment System Lifecycle

```mermaid
sequenceDiagram
    actor Biz as Ayolinx Business Team
    participant Dev as Ayolinx Dev Team
    participant Risk as Ayolinx Risk/Compliance
    participant Bank as Acquirer Bank
    participant Network as Card Network (Visa/Mastercard)
    participant Merchant as Test Merchant / Internal UAT

    Biz->>Bank: Initiate partnership discussion
    Bank-->>Biz: Share onboarding requirements

    Biz->>Risk: Request compliance readiness review
    Risk-->>Biz: Confirm licensing, KYC, fraud policy, PCI readiness

    Biz->>Dev: Start technical integration project
    Dev->>Bank: Request API spec / host-to-host documentation
    Bank-->>Dev: Share sandbox, API spec, test cases

    Dev->>Dev: Build payment API (auth, capture, refund, void)
    Dev->>Dev: Build callback handler and reconciliation module
    Dev->>Risk: Review card data flow and security controls
    Risk-->>Dev: Approve tokenization, logging, access control rules

    Dev->>Bank: Configure sandbox connection
    Bank-->>Dev: Sandbox credentials and endpoint confirmation

    Dev->>Network: Validate card scheme requirements
    Network-->>Dev: Return 3DS / network compliance requirements

    Dev->>Merchant: Prepare UAT environment
    Merchant-->>Dev: Send test transaction scenarios

    Merchant->>Dev: Submit test payment
    Dev->>Bank: Send authorization request (sandbox)
    Bank->>Network: Route test transaction
    Network-->>Bank: Return auth result
    Bank-->>Dev: Return payment response
    Dev-->>Merchant: Show payment result

    Merchant->>Dev: Test refund / void / capture
    Dev->>Bank: Send follow-up transaction requests
    Bank-->>Dev: Return operation results

    Dev->>Risk: Submit UAT report and security evidence
    Risk-->>Biz: Confirm internal readiness for production

    Biz->>Bank: Request production activation
    Bank-->>Biz: Approve go-live checklist

    Dev->>Bank: Switch to production endpoint
    Bank-->>Dev: Production credentials activated

    Merchant->>Dev: Send live transaction
    Dev->>Bank: Production authorization request
    Bank->>Network: Route live transaction
    Network-->>Bank: Return live auth result
    Bank-->>Dev: Return payment response
    Dev-->>Merchant: Display live payment result

    Note over Risk,Dev: PCI scope, tokenization, and no CVV storage must be enforced
    Note over Bank,Dev: Settlement, reconciliation, and dispute handling must be validated before go-live



## 📌 Overview
This document explains where **PCI DSS compliance** fits within the payment system development lifecycle, especially for a payment aggregator like Ayolinx.

---

## 🧭 Payment System Lifecycle

Typical phases:

1. Business & Partnership  
2. Architecture Design  
3. Development  
4. ⚠️ Compliance & Security Readiness (**PCI DSS Phase**)  
5. UAT / Certification  
6. Go-Live  

---

## 🔐 PCI DSS Phase (Compliance & Security Readiness)

This phase ensures the system is secure and compliant **before production**.

---

## ✅ Key Activities

### 1. Define Scope
- Determine:
  - Are you storing PAN?
  - Using tokenization only?
- Scope defines compliance type:
  - SAQ A
  - SAQ A-EP
  - SAQ D

---

### 2. Implement Security Controls
- TLS 1.2+ (data in transit)
- AES-256 (data at rest)
- Tokenization
- RBAC (Role-Based Access Control)
- MFA (Multi-Factor Authentication)
- Logging & monitoring

---

### 3. Build CDE (Card Data Environment) *(if needed)*
- Network segmentation
- Isolated environment
- HSM / Key Management

---

### 4. Documentation
- Security policy
- Incident response plan
- Data retention policy
- Access control policy

---

### 5. Internal Validation
- Self Assessment Questionnaire (SAQ)
- Vulnerability scanning (ASV)
- Penetration testing

---

### 6. External Audit *(if required)*
- QSA (Qualified Security Assessor)
- ROC (Report on Compliance)

---

## 📊 PCI DSS Scope for Ayolinx

### Option 1: Using Gateway (Recommended Early Stage)
- No PAN storage
- Scope: **SAQ A / SAQ A-EP**
- Faster and cheaper

---

### Option 2: Direct Acquirer Integration
- Handle card data internally
- Scope: **SAQ D (Full PCI DSS)**
- Requires:
  - CDE
  - Full audit
  - Higher cost

---

## ⏱️ Estimated Timeline

| Scope Type | Duration |
|------------|---------|
| SAQ A      | 2–4 weeks |
| SAQ D      | 3–9 months |

---

## ⚠️ Common Mistakes

- Defining PCI scope too late  
- Storing PAN unintentionally  
- Logging sensitive card data  
- Hardcoding encryption keys  

---

## 📊 Lifecycle Position

```text
Design → Development → PCI DSS → UAT → Go-Live

---

## 🧩 Sequence Diagram

