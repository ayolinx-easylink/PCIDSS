# 🔄 End-to-End Card Payment Flow (PCI DSS)

## 🎯 Overview
This document illustrates the **complete payment flow** from user to card network and back.

Actors involved:
- End User
- Frontend (FE)
- Backend (BE)
- Acquirer Bank
- Card Network (Visa / Mastercard)

---

## 🧩 Sequence Diagram

```mermaid
sequenceDiagram
    actor User as End User
    participant FE as Frontend (App/Web)
    participant BE as Backend (Ayolinx)
    participant ACQ as Acquirer Bank
    participant NET as Card Network (Visa/Mastercard)

    User->>FE: Enter card details
    FE->>BE: Send payment request (TLS)

    BE->>ACQ: Send authorization request
    ACQ->>NET: Forward to card network
    NET->>NET: Route to issuing bank
    NET-->>ACQ: Authorization response
    ACQ-->>BE: Return result (approved/declined)

    BE-->>FE: Return payment status
    FE-->>User: Display result

    Note over FE,BE: TLS encryption required
    Note over BE,ACQ: No CVV storage allowed
    Note over NET: Handles routing to issuing bank