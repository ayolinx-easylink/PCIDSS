# 💳 PCI DSS – Full Card Data Flow (CDE Architecture)

## 📌 Overview
Diagram ini menggambarkan alur pemrosesan kartu kredit pada arsitektur **Full PCI DSS** menggunakan **Card Data Environment (CDE)**.

- Data kartu diproses di environment terisolasi (CDE)
- PAN disimpan dalam bentuk terenkripsi
- Key dikelola oleh HSM (bukan di aplikasi)
- Semua komunikasi menggunakan TLS

---

## 🧩 Sequence Diagram

```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend / App
    participant API as API Gateway
    participant CDE as Card Data Environment Service
    participant HSM as HSM / Key Management
    participant DB as Encrypted Card Database
    participant PRC as Processor / Acquirer

    User->>FE: Enter card details
    FE->>API: Submit card data over TLS
    API->>CDE: Forward card data

    CDE->>HSM: Request encryption operation
    HSM-->>CDE: Return encrypted PAN

    CDE->>DB: Store encrypted PAN + metadata

    CDE->>PRC: Authorize / charge request
    PRC-->>CDE: Return authorization result

    CDE-->>API: Return payment result
    API-->>FE: Return payment status
    FE-->>User: Display result

    Note over FE,API: TLS 1.2+ is required
    Note over CDE,DB: PAN stored in encrypted form
    Note over HSM: Keys stored securely (not in app code)