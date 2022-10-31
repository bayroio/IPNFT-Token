# IPNFT-Token: Tokenización de la Propiedad Intelectual
Tokenización de la Propiedad Intelectual

## Perfiles de Investigadores y Empresas de BCT
Actores que podrán interactuar con este módulo:
- Investigadores
- Empresas de BCT, y
- DTT

Este módulo es una aplicación web en dónde los actores podrán crear su perfil y en dónde podrán, a través del proceso de *minting*, podrán tokenizar sus papers para que estén representados por un **NFT**. Durante el proceso del *minting*, al **NFT** se le asignara un nombre, un símbolo que lo represente, una imagen y por supuesto el PDF del paper.

Los papers tokenizados y encapsulados en un **NFT** serán almacenados en una bóveda, es decir en un smart contract que cumple la función de resguardos de activos, y que solo a través del otorgamiento de permisos a ciertas cuentas (Ethereum address) se podrá acceder al activo y a su metadata, es decir al **NFT** que representa al paper tokenizado. Para la prueba de concepto a construir, la Ethereum address que recibirá el permiso de acceso al **NFT** será la que represente a la **DTT**. Toda está funcionalidad es posible gracias al uso de **NFT 2.0** y el estándar **ERC173**.

Los actores podrán incorporar una imagen o foto en su perfil, y hacer una descripción de actividad profesional.

### Diagrama de Secuencia: Crea Universal Profile
```mermaid
sequenceDiagram
Investigador->>UI: Metadatos
Note right of UI: Nombre, descripción, profileImage, backgroundImage, tags, links, email
UI->>UI: Genera Ethereum address
Investigador->>UI: Crea Universal Profile
UI->>API Blockchain: Crea UP (metadatos)
API Blockchain->>Ledger: Crea nuevos Smart Contract para representar una nueva UP
API Blockchain->>API Blockchain: Genera y envía correo para autenticación
Note right of API Blockchain: Usar servicio de Sendgrid en Azure
API Blockchain-->>UI: Ok
UI-->>Investigador: Despliega nueva UP y recibe correo
```

### Diagrama de Secuencia: Login
```mermaid
sequenceDiagram
Investigador->>UI: Accede a su UP
Note right of Investigador: Desde su email inbox el investigador dió click en la liga de su UP
UI->>API Blockchain: Lee UP asociada a la Ethereum address
API Blockchain->>Ledger: Llama al Smart Contract para leer los metadatos
Ledger-->> API Blockchain: Metadatos
API Blockchain-->>UI: Metadatos
```

### Diagrama de Secuencia: Logout
```mermaid
sequenceDiagram
Investigador->>UI: Logut
Note right of UI: Se borra el cache de esa página
```

### Diagrama de Secuencia: Tokeniza Paper (NFGT)
```mermaid
sequenceDiagram
Investigador->>UI: Está en su UP
Investigador->>UI: Crear nuevo Token/Activo
Investigador->>UI: Metadatos del paper a tokenizar
Note right of UI: Nombre, simbolo y descripción del paper, imagen del paper, archivo PDF
Investigador->>UI: Tokeniza Paper (Ok/Submit)
UI->>API Blockchain: Crea nuevo token de paper (metadatos)
API Blockchain->>Ledger: Crea nuevo NFT y le asignar como dueño el UP que lo está creando
Ledger-->>API Blockchain: Ok
API Blockchain-->>UI: Ok
UI->>UI: Despliega UP actualizado con el nuevo paper tokenizado (NFT)
```
