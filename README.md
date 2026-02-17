## 2. Architecture Diagram (Required)

The following diagram shows the complete system architecture, including trusted and untrusted components, key storage, encryption, signing, and full data flow.

```mermaid
flowchart LR

  %% ================= TRUST BOUNDARIES =================
  subgraph TRUSTED_USER[Trusted - User Environment]
    U[User]
    UI[Vault Application (Frontend/UI)]

    KS[Key Store (Encrypted Private Keys)\nProtected with Password]

    SIGN[Signing Module\n(Digital signature happens here)]
    ENC[Encryption Module\n(Encryption happens here)]

    VER[Signature Verification Module]
    DEC[Decryption Module]
  end

  subgraph TRUSTED_BACKEND[Trusted - Vault Service]
    API[Vault Backend / API\nHandles storage, metadata, sharing workflow]
  end

  subgraph UNTRUSTED[Untrusted Environment]
    ST[(Storage - Local Disk / Cloud)]
    NET[[Network / Transport Channel]]
  end

  PK[Public Keys / Recipients]

  %% ================= SEND / CREATE FLOW =================

  U -->|Select file to protect| UI

  UI -->|Unlock keys with password| KS
  KS -->|Sender private key (in memory)| SIGN

  UI -->|Plain document| SIGN
  SIGN -->|Document + Signature| ENC

  PK -->|Recipients public keys| ENC

  ENC -->|Encrypted File Container| UI
  UI -->|Send container to backend| API

  API -->|Store container| ST
  API -->|Share container| NET

  %% ================= RECEIVE FLOW =================

  NET -->|Receive container| UI
  ST -->|Fetch stored container| API
  API -->|Deliver container| UI

  UI -->|Verify signature first| VER
  VER -->|If valid| DEC

  KS -->|Recipient private key (in memory)| DEC
  DEC -->|Recovered plaintext document| UI

