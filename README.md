## 2. Diagrama de Arquitectura (Requerido)

```mermaid
flowchart LR

  %% ===== ENTORNO CONFIABLE USUARIO =====
  subgraph TRUSTED_USER[Confiable - Entorno del Usuario]
    direction TB
    U[Usuario]
    UI[Aplicacion Vault Frontend]
    KS[Key Store Llaves Privadas Cifradas]
    SIGN[Modulo de Firma Digital]
    ENC[Modulo de Cifrado]
    VER[Verificacion de Firma]
    DEC[Modulo de Descifrado]
  end

  %% ===== ENTORNO CONFIABLE BACKEND =====
  subgraph TRUSTED_BACKEND[Confiable - Servicio Vault]
    direction TB
    API[Backend Vault API]
  end

  %% ===== ENTORNO NO CONFIABLE =====
  subgraph UNTRUSTED[No Confiable - Almacenamiento y Red]
    direction TB
    SP1[ ]:::invisible
    SP2[ ]:::invisible
    ST[(Almacenamiento Local o Remoto)]
    NET[[Transporte por Red]]
  end

  classDef invisible fill=transparent,stroke=transparent,color=transparent;

  PK[Llaves Publicas de Destinatarios]
  C[Contenedor de Archivo Cifrado]

  %% ===== FLUJO DE CREACION =====
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

  %% ===== FLUJO DE RECEPCION =====
  NET --> UI
  ST --> API
  API --> UI
  UI --> VER
  VER --> DEC
  KS --> DEC
  DEC --> UI
