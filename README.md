# 🔬 Secure Digital Document Vault - Laboratorio

## 1. System Overview (Descripción General)

En un entorno de laboratorio, la integridad de los resultados y la confidencialidad de los documentos son críticas.  
Este sistema protege documentos sensibles contra acceso no autorizado y modificaciones maliciosas durante su almacenamiento o envío.

### Funcionalidades principales

- **Cifrado de archivos:** Solo el destinatario puede leer el contenido.
- **Firmas digitales:** Permiten verificar el autor y detectar modificaciones.
- **Gestión de llaves:** Las llaves privadas se protegen con contraseña.
- **Intercambio seguro:** Un archivo puede compartirse con múltiples destinatarios.

### Fuera de alcance

- Seguridad física del equipo
- Recuperación de contraseña olvidada

---

## 2. Architecture Diagram (Diagrama de Arquitectura)

El siguiente diagrama muestra los componentes, límites de confianza y flujo principal del sistema.

```mermaid
flowchart LR

  %% ===== ENTIDADES =====
  U1[Investigador A - Emisor]
  U2[Investigador B - Receptor]

  %% ===== ZONA CONFIABLE =====
  subgraph TRUSTED[Zona Confiable - Vault App]
    UI[Aplicación Vault]
    KM[Key Manager\nProtege llaves con contraseña]
    SIGN[Firma Digital]
    ENC[Cifrado]
    PKG[Contenedor Seguro]
    VER[Verificación]
    DEC[Descifrado]
  end

  %% ===== ZONA NO CONFIABLE =====
  subgraph UNTRUSTED[Zona No Confiable]
    STORE[(Disco / USB / Nube)]
    NET[[Red / Canal de envío]]
  end

  %% ===== FLUJO EMISOR =====
  U1 -->|Selecciona archivo| UI
  UI --> KM
  KM --> SIGN
  SIGN --> ENC
  ENC --> PKG
  PKG --> STORE
  PKG --> NET

  %% ===== FLUJO RECEPTOR =====
  STORE --> UI
  NET --> UI
  UI --> VER
  VER --> DEC
  KM --> DEC
  DEC --> U2
