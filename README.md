## 2. Architecture Diagram (Required)

```mermaid
flowchart LR

  %% ================= TRUST BOUNDARIES =================
  subgraph TRUSTED_USER[Trusted - User Environment]
    U[User]
    UI[Vault Application (Frontend/UI)]
    KS[Key Store (Encrypted Private Keys)\nProtected with Password]
    SIGN[Signing Module\n(Sign happens here)]
    ENC[Encryption Module\n(Encryption happens here)]
    VER[Signature Verification]
    DEC[Decryption Module]
  end

  %% If your team treats the backend as trusted, keep it here.
  %% If your team treats the backend as untrusted storage-only, move it to UNTRUSTED.
  subgraph TRUSTED_BACKEND[Trusted - Vault Service]
    API[Vault Backend / API\nSharing + storage workflow]
  end

  subgraph UNTRUSTED[Untrusted - Storage / Network]
    ST[(Storage - Local/Remote)]
    NET[[Network / Transport]]
  end

  PK[Public Keys / Recipients]
  C[Encrypted File Container\n(Ciphertext + signature + protected metadata)]

  %% ================= CREATE / SHARE =================
  U -->|Select file + recipients| UI
  UI -->|Unlock keys (password)| KS
  KS -->|Sender private key (in-memory)| SIGN
  UI -->|Plain document| SIGN
  SIGN -->|Document + signature| ENC
  PK -->|Recipients public keys| ENC
  ENC --> C
  C -->|Upload/store| API
  API --> ST
  API --> NET

  %% ================= RECEIVE / VERIFY / DECRYPT =================
  NET -->|Receive container| UI
  ST -->|Fetch container| API
  API -->|Deliver container| UI
  UI -->|Verify first| VER
  VER -->|If valid| DEC
  KS -->|Recipient private key (in-memory)| DEC
  DEC -->|Plain document| UI
