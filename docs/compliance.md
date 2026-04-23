# 📋 PCI DSS Compliance

## Scope

Applies when:
- Storing card data
- Processing card data
- Transmitting card data

---

## Key Requirements

### Requirement 3
Protect stored card data

### Requirement 4
Encrypt data in transit

---

## Checklist

- [ ] No CVV storage
- [ ] PAN encrypted
- [ ] TLS enforced
- [ ] Masked display
- [ ] Secure key management
- [ ] Audit logging

---

## Common Mistakes

- Storing CVV
- Logging PAN
- Hardcoding keys
- No network segmentation

---

## Maturity Levels

### Level 1 (Startup)
- Gateway only

### Level 2 (Intermediate)
- Tokenization

### Level 3 (Enterprise)
- Full CDE + HSM