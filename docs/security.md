```mermaid
sequenceDiagram
    actor User
    participant API
    participant CDE
    participant HSM
    participant DB

    User->>API: Submit card data
    API->>CDE: Forward data
    CDE->>HSM: Encrypt PAN
    CDE->>DB: Store encrypted PAN

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
