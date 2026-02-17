## 2. Diagrama de Arquitectura

```mermaid
flowchart LR

  subgraph TRUSTED_USER[Confiable - Entorno del Usuario]
    U[Usuario]
    UI[Aplicacion Vault Frontend]
    KS[Key Store Llaves Privadas Cifradas]
    SIGN[Modulo de Firma Digital]
    ENC[Modulo de Cifrado]
    VER[Verificacion de Firma]
    DEC[Modulo de Descifrado]
  end

  subgraph TRUSTED_BACKEND[Confiable - Servicio Vault]
    API[Backend Vault API]
  end

  subgraph UNTRUSTED[No Confiable - Almacenamiento y Red]
    ST[(Almacenamiento Local o Remoto)]
    NET[[Transporte por Red]]
  end

  PK[Llaves Publicas de Destinatarios]
  C[Contenedor de Archivo Cifrado]

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
