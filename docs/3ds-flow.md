---

# 📁 docs/3ds-flow.md

```markdown
# 🔐 3D Secure (3DS) Payment Flow

## 🎯 Overview
This document illustrates the **3D Secure authentication flow** in a card payment transaction.

Actors involved:
- End User
- Frontend (FE)
- Backend (BE)
- Acquirer Bank
- Card Network (Visa / Mastercard)
- Issuer ACS (Access Control Server)

---

## 🧩 Sequence Diagram

```mermaid
sequenceDiagram
    actor User as End User
    participant FE as Frontend (App/Web)
    participant BE as Backend (Merchant Backend)
    participant ACQ as Acquirer Bank
    participant NET as Card Network (Visa/Mastercard)
    participant ACS as Issuer ACS / 3DS Server

    User->>FE: Enter card details
    FE->>BE: Send payment request
    BE->>ACQ: Send authorization request with 3DS data
    ACQ->>NET: Forward transaction
    NET->>ACS: Check card enrollment / authentication requirement

    ACS-->>NET: Authentication challenge required
    NET-->>ACQ: Return challenge निर्देश
    ACQ-->>BE: Return 3DS challenge data
    BE-->>FE: Render 3DS challenge

    User->>ACS: Complete OTP / biometric / app challenge
    ACS-->>NET: Return authentication result
    NET-->>ACQ: Forward authentication result
    ACQ-->>BE: Return auth result

    BE-->>FE: Return payment status
    FE-->>User: Display approved / declined result

    Note over FE,BE: TLS required for all communications
    Note over ACS: Performs cardholder authentication
    Note over BE,ACQ: Merchant must not store CVV