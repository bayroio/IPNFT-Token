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

### Diagrama de Secuencia: Tokeniza Paper (NFT)
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

## Tokenización de Propiedad Intelectual
Actores que podrán interactuar con este módulo:
- **Investigadores y Empresas de BCT**, actuando como los creadores del conocimiento
base a partir de uno o más papers, y
- **DTT**, actuando como la dueña de la propiedad intelectual a ser tokenizada, a partir del conocimiento base encapsulado en un NFT.

La **DTT** accederá al **NFT** que representa el paper tokenizado, esto debido al permiso concedido a la **DTT**, por el **investigador** o la **empresa de BCT**. Y deberá realizar lo siguiente:
- Hacer el minting de un nuevo **NFT** al cuál llamaremos **IP-NFT**, a partir del **NFT** que representa el conocimiento base (paper tokenizado por el investigador), y adicionarle nuevos metadatos:
    - Descripción general.
    - El documento legal que representa la patente. De esta manera se relaciona
el conocimiento base con la patente.
    - El documento legal en dónde se otorga o cede el derecho de explotación
comercial del **investigador** o la **Empresa de BCT** a la **DTT**.
    - Documento legal que le otorga al comprador de la IP-NFT, permisos de uso
permanente y no exclusiva.
    - Asignación del precio comercial de la **IP-NFT**
    - Asignación de los porcentajes a repartir cuando
se comercialice la **IP-NFT**:
        - Porcentaje de regalías que se le concede al **investigador** o a la
**Empresa de BCT** por *derecho de explotación del conocimiento base a través de la comercialización de la patente*.
        - El porcentaje de la comisión al protocolo.
        - El porcentaje de aportación a la DAO.
        - El porcentaje de regalías que se le concederán a la **DTT**.
- El proceso de minting del **IP-NFT** deberá ser firmado criptográficamente tanto por la **DTT** y el **investigador** o la **Empresa de BCT**.
    - Esto se debe a que existe una cesión de derechos de explotación comercial.

### Diagrama de Secuencia: Asignación de Permisos sobre NFT (Paper Tokenizado)
```mermaid
sequenceDiagram
Investigador->>UI: Selecciona una NFT representando uno de sus papers tokenizados
Investigador->>UI: Selecciona el UP de la DTT
Investigador->>UI: Asigna permisos de ownership a la DTT sobre el NFT seleccionado previamente
UI->>API Blockchain: Asigna ownership a DTT (address de NFT)
API Blockchain->>Ledger: Asigna nuevo ownership al paper tokenizado en el smart contract del NFT
API Blockchain->>API Blockchain: Genera y envía correo a la DTT
Note right of API Blockchain: Usar servicio de Sendgrid en Azure
API Blockchain-->>UI: Ok
UI-->>Investigador: Permiso asignado
```

### Diagrama de Secuencia: Tokenización Propiedad Intelectual
```mermaid
sequenceDiagram
DTT->>UI: Accede a la página del paper tokenizado (NFT)
Note right of DTT: Desde su email inbox el investigador dió click en la liga del NFT
UI->>UI: Despliega página web del paper tokenizado (NFT)
Note right of DTT: La DTT revisa información del NFT
DTT->>UI: Metadatos y Documentos de soporte
Note right of UI: Nombre, simbolo, precio, Descripción general, patente, derecho de explotación comercial, permisos de uso, distribución de la compensación del token (% regalias del IP-NFGT)
DTT->>UI: Minting IP-NFT
Note right of DTT: Se firma criptográficamente la transacción
UI->>API Blockchain: Miniting nuevo IP-NFT (metadatos, documentos soporte, firma criptográfica de la DTT)
API Blockchain->>Ledger: Se crea un nuevo IP-NFT (propiedad intelectual tokenizada)
Note right of DTT: El dueño de está nueva IP-NFT es la DTT
Ledger-->>API Blockchain: Ethereum address del nuevo IP-NFT + Metadatos
API Blockchaini-->>UI: Ethereum address del nuevo IP-NFT + Metadatos
```

## Marketplace de Propiedad Intelectual Tokenizada
Actores que podrán interactuar con este módulo:
- Cualquier persona que cree su perfil

Este módulo funcional será una aplicación web, en dónde se desplegarán todas las **IP-NFT** con una descripción corta y su nombre, así como el precio.

El actor deberá tener en su wallet suficiente cryptos para pagar la IP-NFT y el gas asociado a la transacción.

La transacción de compra de un IP-NFT deberá ser firmada criptográficamente.

### Diagrama de Secuencia: Busqueda y compra de IP-NFT
```mermaid
sequenceDiagram
Empresa de BCT->>UI Marketplace: Busca IP-NFT
Note right of Empresa de BCT: La búsqueda puede ser visual en la lista de todas IP-NFTs o bien, usando el buscador de la página del Marketplace.
Empresa de BCT->>UI Marketplace: Selecciona el IP-NFT deseado
Note right of UI Marketplace: El Marketplace muestra toda la información del IP-NFT.
Note right of UI Marketplace: La Empresa de BCT revisa toda la información del IP-NFT.
Empresa de BCT->>UI Marketplace: Compra IP-NFT
Note right of Empresa de BCT: Se firma la transacción. Se debe tener los fondos suficientes para pagar la IP-NFT y el gas de la transacción
UI Marketplace->>API Blockchain: Cambiar ownership del IP-NFT
API Blockchain->>Ledger: Cambia ownership en Smart Contract del IP-NFT
Ledger-->>API Blockchain: Ok
API Blockchain-->>UI Marketplace: Ok

```