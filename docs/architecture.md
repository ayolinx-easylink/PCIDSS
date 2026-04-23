# 🏗️ PCI DSS Architecture

## Overview
This document explains system architecture for handling card data securely.

---

## Architecture Types

### 1. Low Scope (Startup)
- Use payment gateway
- Store token only

### 2. Full PCI DSS (Enterprise)
- Isolated Card Data Environment (CDE)
- Encrypted database
- HSM integration

---

## Sequence Diagram – Gateway Flow
```mermaid
sequenceDiagram
    actor User
    participant Frontend
    participant Gateway
    participant Backend
    participant Database

    User->>Frontend: Enter card details
    Frontend->>Gateway: Send card data
    Gateway-->>Frontend: Return token
    Frontend->>Backend: Send token
    Backend->>Database: Store token