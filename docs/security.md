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
```

### 2. Masked Card Display (Least Privilege)
```mermaid
sequenceDiagram
    actor Staff
    participant APP as Internal App
    participant SVC as Card Service
    participant DB as Card DB

    Staff->>APP: Request card details
    APP->>SVC: Request display-safe data
    SVC->>DB: Read stored record
    DB-->>SVC: Encrypted PAN / token
    SVC->>SVC: Apply masking policy
    SVC-->>APP: 4111 **** **** 1111
    APP-->>Staff: Display masked PAN
```

### 3. Privileged Access + Audit Logging
```mermaid
sequenceDiagram
    actor Officer
    participant PORTAL as Secure Portal
    participant IAM as IAM / MFA
    participant SVC as Card Service
    participant AUD as Audit Log
    participant DB as Encrypted DB
    participant HSM as HSM

    Officer->>PORTAL: Sign in
    PORTAL->>IAM: Verify + MFA
    IAM-->>PORTAL: Access granted
    Officer->>PORTAL: Request full PAN
    PORTAL->>SVC: Privileged request
    SVC->>AUD: Log access (who/when/why)
    SVC->>DB: Read encrypted PAN
    DB-->>SVC: Encrypted PAN
    SVC->>HSM: Decrypt request
    HSM-->>SVC: Plain PAN
    SVC-->>PORTAL: Approved output
```

---

## Controls by Layer

### Frontend
- No logging card data
- HTTPS only

### Backend
- RBAC + MFA
- No sensitive logs

### Database
- Encrypted PAN
- Audit logs

### Network
- Segmented CDE
- Firewall rules

---

## CVV Rule
- NEVER store CVV
