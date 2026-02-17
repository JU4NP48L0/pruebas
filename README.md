## 2. Diagrama de Arquitectura

```mermaid
flowchart LR

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

  subgraph TRUSTED_BACKEND[Confiable - Servicio Vault]
    direction TB
    API[Backend Vault API]
  end

  subgraph UNTRUSTED[No Confiable - Almacenamiento y Red]
    direction TB
    ST[(Almacenamiento Local o Remoto)]
    NET[[Transporte por Red]]
  end

  PK[Llaves Publicas de Destinatarios]
  C[Contenedor de Archivo Cifrado]

  %% Flujo de creacion
  U -->|Selecciona archivo| UI
  UI -->|Desbloquea llaves con contraseña| KS
  KS -->|Llave privada en memoria| SIGN
  UI -->|Documento en claro| SIGN
  SIGN -->|Documento firmado| ENC
  PK -->|Llaves publicas| ENC
  ENC -->|Contenedor cifrado| C
  C -->|Enviar| API
  API -->|Guardar| ST
  API -->|Transmitir| NET

  %% Flujo de recepcion
  NET -->|Recibir contenedor| UI
  ST -->|Obtener contenedor| API
  API --> UI
  UI -->|Verificar firma| VER
  VER -->|Si es valido| DEC
  KS -->|Llave privada en memoria| DEC
  DEC -->|Documento recuperado| UI
