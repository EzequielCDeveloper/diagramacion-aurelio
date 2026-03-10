# Diagramas UML - Sistema de Venta de Boletos

Sistema tipo Eventbrite donde organizadores crean eventos y usuarios compran boletos.

---

## 1. Diagrama de Casos de Uso

```mermaid
graph TB
    subgraph Actores
        U((Usuario))
        O((Organizador))
        A((Administrador))
        SP((Sistema de Pagos))
    end

    subgraph "Casos de Uso"
        CU1[Registrarse]
        CU2[Iniciar sesión]
        CU3[Crear evento]
        CU4[Comprar boleto]
        CU5[Descargar boleto]
        CU6[Administrar eventos]
        CU7[Procesar pago]
        CU8[Generar reportes]
    end

    U --> CU1
    U --> CU2
    U --> CU4
    U --> CU5
    
    O --> CU2
    O --> CU3
    O --> CU6
    O --> CU8
    
    A --> CU2
    A --> CU6
    A --> CU8
    
    SP --> CU7
    CU4 --> CU7
```

---

## 2. Diagrama de Clases

```mermaid
classDiagram
    class Usuario {
        +int id
        +string nombre
        +string email
        +string password
        +registrarse()
        +iniciarSesion()
        +comprarBoleto()
    }

    class Evento {
        +int id
        +string titulo
        +date fecha
        +string ubicacion
        +int capacidad
        +crearEvento()
        +actualizarEvento()
    }

    class Boleto {
        +int id
        +string codigoQR
        +string estado
        +decimal precio
        +generarQR()
        +validar()
    }

    class Orden {
        +int id
        +decimal total
        +date fecha
        +calcularTotal()
        +confirmar()
    }

    class Pago {
        +int id
        +string metodo
        +string estado
        +procesar()
        +reembolsar()
    }

    Usuario "1" --> "*" Evento : crea
    Usuario "1" --> "*" Boleto : compra
    Orden "1" --> "*" Boleto : contiene
    Orden "1" --> "1" Pago : genera
    Evento "1" --> "*" Boleto : tiene
```

---

## 3. Diagrama de Objetos

```mermaid
graph LR
    subgraph "Instancias"
        U1["usuario1 : Usuario<br/>nombre = 'Carlos'<br/>email = 'carlos@email.com'"]
        E1["evento1 : Evento<br/>titulo = 'Comic Fest'<br/>fecha = '2024-06-15'"]
        B1["boleto1 : Boleto<br/>codigoQR = 'A3X9'<br/>estado = 'activo'"]
    end

    U1 -->|compra| B1
    B1 -->|pertenece| E1
```

---

## 4. Diagrama de Secuencia

```mermaid
sequenceDiagram
    actor Usuario
    participant WebApp
    participant SistemaEventos
    participant DB
    participant PagoAPI

    Usuario->>WebApp: comprar boleto
    WebApp->>SistemaEventos: verificar disponibilidad
    SistemaEventos->>DB: consultar cupo
    DB-->>SistemaEventos: cupo disponible
    SistemaEventos->>PagoAPI: procesar pago
    PagoAPI-->>SistemaEventos: pago aprobado
    SistemaEventos->>DB: registrar boleto
    SistemaEventos-->>Usuario: enviar boleto QR
```

---

## 5. Diagrama de Actividades

```mermaid
flowchart TD
    A([Inicio]) --> B[Seleccionar evento]
    B --> C[Elegir cantidad de boletos]
    C --> D[Ingresar datos]
    D --> E[Procesar pago]
    E --> F{¿Pago aprobado?}
    F -->|No| G[Cancelar]
    G --> H([Fin])
    F -->|Sí| I[Generar boleto]
    I --> J[Enviar boleto]
    J --> H
```

---

## 6. Diagrama de Estados

```mermaid
stateDiagram-v2
    [*] --> Creado
    Creado --> Disponible
    Disponible --> Reservado
    Reservado --> Pagado
    Pagado --> Usado
    Pagado --> Cancelado
    Usado --> [*]
    Cancelado --> [*]
```

---

## 7. Diagrama de Componentes

```mermaid
graph TB
    subgraph "Sistema de Venta de Boletos"
        FW[Frontend Web]
        API[API Backend]
        SP[Servicio de Pagos]
        SN[Servicio de Notificaciones]
        BD[(Base de Datos)]
        QR[Sistema QR]
    end

    FW --> API
    API --> BD
    API --> SP
    API --> SN
    API --> QR
```

---

## 8. Diagrama de Paquetes

```mermaid
graph TB
    subgraph presentation["/presentation"]
        C[controllers]
        V[views]
    end

    subgraph application["/application"]
        S[servicios]
        VAL[validaciones]
    end

    subgraph domain["/domain"]
        E[entidades]
        R[reglas]
    end

    subgraph infrastructure["/infrastructure"]
        DB[base de datos]
        INT[integraciones]
    end

    presentation --> application
    application --> domain
    infrastructure --> domain
```

---

## 9. Diagrama de Despliegue

```mermaid
graph TB
    subgraph "Cliente"
        U[Usuario]
    end

    subgraph "Internet"
        I((Internet))
    end

    subgraph "Infraestructura"
        SW[Servidor Web]
        API[API Backend]
        BD[(Base de Datos)]
    end

    subgraph "Servicios Externos"
        SP[Servicio de Pagos]
    end

    U --> I
    I --> SW
    SW --> API
    API --> BD
    API --> SP
```

---

## 10. Diagrama de Comunicación

```mermaid
graph LR
    U((Usuario)) -->|1: solicita| FE[Frontend]
    FE -->|2: request| API[API]
    API -->|3: query| DB[(DB)]
    API -->|4: procesa| P[Pago]
    API -->|5: notifica| ES[Email Service]
```

---

## 11. Diagrama de Estructura Compuesta

```mermaid
graph TB
    subgraph "Sistema de Eventos"
        GE[GestorEventos]
        GB[GestorBoletos]
        GU[GestorUsuarios]
        GP[GestorPagos]
    end

    GE --> GB
    GU --> GB
    GB --> GP
```
