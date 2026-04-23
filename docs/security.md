
---

# 📁 docs/security.md

```markdown
# 🔐 PCI DSS Security Controls

## Core Principles

### Data Minimization
- Store only required data

### Encryption
- AES-256 (storage)
- TLS 1.2+ (transport)

### Tokenization
- Replace PAN with token

### Key Management
- Use HSM / KMS
- Rotate keys

---

## 🔄 Security Flow Diagrams

### 1. CVV Handling (Must Not Be Stored)
```mermaid
sequenceDiagram
    actor User
    participant FE as Frontend
    participant API as Backend API
    participant MEM as Temp Memory
    participant PRC as Processor
    participant DB as Database

    User->>FE: Enter PAN + Expiry + CVV
    FE->>API: Send over TLS
    API->>MEM: Use CVV for auth only
    MEM->>PRC: Authorize request
    PRC-->>MEM: Result
    MEM->>MEM: Clear CVV from memory
    MEM->>DB: Store token / encrypted PAN (no CVV)
    DB-->>API: OK
    API-->>FE: Status
