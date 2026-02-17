## 2. Architecture Diagram (Required)

```mermaid
flowchart LR

  subgraph TRUSTED_USER[Trusted - User Environment]
    U[User]
    UI[Vault Application Frontend]
    KS[Key Store Encrypted Private Keys]
    SIGN[Signing Module]
    ENC[Encryption Module]
    VER[Signature Verification]
    DEC[Decryption Module]
  end

  subgraph TRUSTED_BACKEND[Trusted - Vault Service]
    API[Vault Backend API]
  end

  subgraph UNTRUSTED[Untrusted - Storage and Network]
    ST[(Storage Local or Remote)]
    NET[[Network Transport]]
  end

  PK[Recipients Public Keys]
  C[Encrypted File Container]

  U --> UI
  UI --> KS
  KS --> SIGN
  UI --> SIGN
  SIGN --> ENC
  PK --> ENC
  ENC --> C
  C --> API
  API --> ST
  API --> NET

  NET --> UI
  ST --> API
  API --> UI
  UI --> VER
  VER --> DEC
  KS --> DEC
  DEC --> UI
