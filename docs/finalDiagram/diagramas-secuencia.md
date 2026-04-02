# Diagramas de Secuencia - Sistema de Gestión de Eventos

Este documento contiene los diagramas de secuencia que representan las interacciones temporales entre actores y el sistema para cada tipo de usuario.

## 1. Diagrama de Secuencia - Administrador

```mermaid
sequenceDiagram
    actor Admin as Administrador
    participant Login
    participant Sistema
    participant Dashboard
    participant DBEventos as DB Eventos
    participant DBUsuarios as DB Usuarios
    participant DBOrganizadores as DB Organizadores
    participant ModuloPagos as Módulo Pagos
    participant Soporte

    %% Autenticación
    Admin->>Login: Acceder al sistema
    activate Login
    Admin->>Login: Ingresar credenciales
    Login->>Sistema: Validar credenciales
    activate Sistema
    Sistema-->>Login: Autenticación exitosa
    Login-->>Admin: Redirigir a Dashboard
    deactivate Login
    
    %% Gestión de Eventos
    Admin->>Dashboard: Ver eventos
    Dashboard->>DBEventos: Solicitar lista de eventos
    activate DBEventos
    DBEventos-->>Dashboard: Retornar eventos
    deactivate DBEventos
    Dashboard-->>Admin: Mostrar eventos
    
    Admin->>Dashboard: Seleccionar evento específico
    Dashboard->>DBEventos: Obtener detalles del evento
    activate DBEventos
    DBEventos-->>Dashboard: Retornar detalles
    deactivate DBEventos
    Dashboard-->>Admin: Mostrar detalles del evento
    
    Admin->>Dashboard: Ver órdenes del evento
    Dashboard->>Sistema: Consultar órdenes
    Sistema-->>Dashboard: Retornar órdenes
    Dashboard-->>Admin: Mostrar órdenes
    
    Admin->>Dashboard: Ver boletos de la orden
    Dashboard->>Sistema: Consultar boletos
    Sistema-->>Dashboard: Retornar boletos
    Dashboard-->>Admin: Mostrar boletos
    
    %% Gestión de Usuarios
    Admin->>Dashboard: Ver usuarios
    Dashboard->>DBUsuarios: Solicitar lista de usuarios
    activate DBUsuarios
    DBUsuarios-->>Dashboard: Retornar usuarios
    deactivate DBUsuarios
    Dashboard-->>Admin: Mostrar usuarios
    
    Admin->>Dashboard: Seleccionar usuario específico
    Dashboard->>DBUsuarios: Obtener perfil del usuario
    activate DBUsuarios
    DBUsuarios-->>Dashboard: Retornar datos del usuario
    deactivate DBUsuarios
    Dashboard-->>Admin: Mostrar perfil del usuario
    
    Admin->>Dashboard: Ver boletos del usuario
    Dashboard->>Sistema: Consultar boletos del usuario
    Sistema-->>Dashboard: Retornar boletos
    Dashboard-->>Admin: Mostrar boletos
    
    %% Gestión de Organizadores
    Admin->>Dashboard: Ver organizadores
    Dashboard->>DBOrganizadores: Solicitar lista de organizadores
    activate DBOrganizadores
    DBOrganizadores-->>Dashboard: Retornar organizadores
    deactivate DBOrganizadores
    Dashboard-->>Admin: Mostrar organizadores
    
    Admin->>Dashboard: Seleccionar organizador específico
    Dashboard->>DBOrganizadores: Obtener datos del organizador
    activate DBOrganizadores
    DBOrganizadores-->>Dashboard: Retornar datos
    deactivate DBOrganizadores
    Dashboard-->>Admin: Mostrar perfil del organizador
    
    Admin->>Dashboard: Ver eventos del organizador
    Dashboard->>DBEventos: Consultar eventos por organizador
    activate DBEventos
    DBEventos-->>Dashboard: Retornar eventos
    deactivate DBEventos
    Dashboard-->>Admin: Mostrar eventos del organizador
    
    %% Gestión de Transacciones y Pagos
    Admin->>Dashboard: Ver transacciones
    Dashboard->>ModuloPagos: Solicitar historial de transacciones
    activate ModuloPagos
    ModuloPagos-->>Dashboard: Retornar transacciones
    Dashboard-->>Admin: Mostrar transacciones
    
    Admin->>Dashboard: Gestionar pagos
    Dashboard->>ModuloPagos: Solicitar opciones de gestión
    ModuloPagos-->>Dashboard: Retornar opciones
    Dashboard-->>Admin: Mostrar panel de gestión de pagos
    deactivate ModuloPagos
    
    %% Soporte
    Admin->>Dashboard: Acceder a soporte
    Dashboard->>Soporte: Abrir módulo de soporte
    activate Soporte
    Soporte-->>Admin: Mostrar panel de soporte
    
    Admin->>Soporte: Listar eventos con tickets
    Soporte->>DBEventos: Consultar eventos
    DBEventos-->>Soporte: Retornar eventos
    Soporte-->>Admin: Mostrar lista de eventos
    
    Admin->>Soporte: Ver tickets de soporte
    Soporte->>Sistema: Consultar tickets
    Sistema-->>Soporte: Retornar tickets
    Soporte-->>Admin: Mostrar tickets generales
    deactivate Soporte
    deactivate Sistema
```

## 2. Diagrama de Secuencia - Organizador

```mermaid
sequenceDiagram
    actor Org as Organizador
    participant Login
    participant Sistema
    participant Dashboard
    participant DBEventos as DB Eventos
    participant Perfil
    participant Soporte
    participant Reportes
    participant ModuloPagos as Módulo Pagos

    %% Autenticación y Registro
    Org->>Login: Acceder al sistema
    activate Login
    
    alt Usuario nuevo
        Org->>Login: Seleccionar registro
        Login->>Sistema: Solicitar formulario de registro
        activate Sistema
        Sistema-->>Login: Retornar formulario
        Login-->>Org: Mostrar formulario (incluye validación de identidad)
        Org->>Login: Enviar datos + documentación
        Login->>Sistema: Validar y crear cuenta
        Sistema-->>Login: Cuenta creada (pendiente aprobación)
        Login-->>Org: Confirmación de registro
    else Usuario existente
        Org->>Login: Ingresar credenciales
        Login->>Sistema: Validar credenciales
        Sistema-->>Login: Autenticación exitosa
        Login-->>Org: Redirigir a Dashboard
    end
    deactivate Login
    
    %% Ver Eventos Públicos
    Org->>Dashboard: Ver eventos públicos
    Dashboard->>DBEventos: Solicitar eventos públicos
    activate DBEventos
    DBEventos-->>Dashboard: Retornar eventos
    deactivate DBEventos
    Dashboard-->>Org: Mostrar catálogo de eventos
    
    Org->>Dashboard: Seleccionar evento específico
    Dashboard->>DBEventos: Obtener detalles del evento
    activate DBEventos
    DBEventos-->>Dashboard: Retornar detalles
    deactivate DBEventos
    Dashboard-->>Org: Mostrar información del evento
    
    %% Gestión de Mis Eventos
    Org->>Dashboard: Acceder a "Mis Eventos"
    Dashboard->>DBEventos: Consultar eventos del organizador
    activate DBEventos
    DBEventos-->>Dashboard: Retornar mis eventos
    deactivate DBEventos
    Dashboard-->>Org: Mostrar lista de mis eventos
    
    Org->>Dashboard: Seleccionar evento específico
    Dashboard->>DBEventos: Obtener dashboard del evento
    activate DBEventos
    DBEventos-->>Dashboard: Retornar métricas y datos
    deactivate DBEventos
    Dashboard-->>Org: Mostrar dashboard del evento
    
    %% Gestión de Perfil
    Org->>Dashboard: Acceder a "Mi Perfil"
    Dashboard->>Perfil: Solicitar datos del perfil
    activate Perfil
    Perfil->>Sistema: Consultar datos del organizador
    Sistema-->>Perfil: Retornar datos
    Perfil-->>Org: Mostrar perfil
    
    Org->>Perfil: Modificar datos
    Perfil->>Sistema: Validar y actualizar datos
    Sistema-->>Perfil: Confirmación de actualización
    Perfil-->>Org: Datos actualizados
    
    opt Borrar cuenta
        Org->>Perfil: Solicitar borrado de cuenta
        Perfil->>Sistema: Confirmar acción
        Sistema-->>Perfil: Solicitar confirmación final
        Perfil-->>Org: Mostrar advertencia
        Org->>Perfil: Confirmar borrado
        Perfil->>Sistema: Eliminar cuenta
        Sistema-->>Perfil: Cuenta eliminada
        Perfil-->>Org: Redirigir a página de salida
    end
    deactivate Perfil
    
    %% Soporte
    Org->>Dashboard: Acceder a soporte
    Dashboard->>Soporte: Abrir módulo de soporte
    activate Soporte
    Soporte-->>Org: Mostrar panel de soporte
    
    Org->>Soporte: Listar tickets
    Soporte->>Sistema: Consultar tickets del organizador
    Sistema-->>Soporte: Retornar tickets
    Soporte-->>Org: Mostrar lista de tickets
    
    Org->>Soporte: Seleccionar ticket específico
    Soporte->>Sistema: Obtener detalles del ticket
    Sistema-->>Soporte: Retornar detalles
    Soporte-->>Org: Mostrar conversación del ticket
    deactivate Soporte
    
    %% Reportes y Cobros
    Org->>Dashboard: Acceder a reportes
    Dashboard->>Reportes: Solicitar módulo de reportes
    activate Reportes
    Reportes->>Sistema: Consultar datos de eventos
    Sistema-->>Reportes: Retornar métricas
    Reportes-->>Org: Mostrar reportes financieros y estadísticas
    
    Org->>Reportes: Solicitar cobro
    Reportes->>ModuloPagos: Iniciar proceso de cobro
    activate ModuloPagos
    ModuloPagos->>Sistema: Validar saldo disponible
    Sistema-->>ModuloPagos: Retornar saldo
    ModuloPagos-->>Reportes: Mostrar opciones de cobro
    Reportes-->>Org: Mostrar formulario de cobro
    
    Org->>Reportes: Confirmar cobro
    Reportes->>ModuloPagos: Procesar transferencia
    ModuloPagos-->>Reportes: Confirmación de transferencia
    Reportes-->>Org: Cobro exitoso
    deactivate ModuloPagos
    deactivate Reportes
    deactivate Sistema
```

## 3. Diagrama de Secuencia - Usuario Normal

```mermaid
sequenceDiagram
    actor User as Usuario
    participant Login
    participant Sistema
    participant Eventos
    participant DBEventos as DB Eventos
    participant Compra
    participant Pago
    participant Perfil
    participant Ayuda

    %% Autenticación
    User->>Login: Acceder a la aplicación
    activate Login
    
    alt Usuario nuevo
        User->>Login: Seleccionar registro
        Login->>Sistema: Solicitar formulario de registro
        activate Sistema
        Sistema-->>Login: Retornar formulario
        Login-->>User: Mostrar formulario de registro
        User->>Login: Enviar datos de registro
        Login->>Sistema: Crear cuenta
        Sistema-->>Login: Cuenta creada
        Login-->>User: Confirmación de registro
    else Usuario existente
        User->>Login: Ingresar credenciales
        Login->>Sistema: Validar credenciales
        Sistema-->>Login: Autenticación exitosa
        Login-->>User: Redirigir a inicio
    end
    deactivate Login
    
    %% Exploración de Eventos
    User->>Eventos: Ver catálogo de eventos
    Eventos->>DBEventos: Solicitar eventos disponibles
    activate DBEventos
    DBEventos-->>Eventos: Retornar eventos
    deactivate DBEventos
    Eventos-->>User: Mostrar lista de eventos
    
    User->>Eventos: Seleccionar evento específico
    Eventos->>DBEventos: Obtener detalles del evento
    activate DBEventos
    DBEventos-->>Eventos: Retornar detalles y disponibilidad
    deactivate DBEventos
    Eventos-->>User: Mostrar información del evento
    
    %% Proceso de Compra
    User->>Eventos: Iniciar compra
    Eventos->>Compra: Redirigir a módulo de compra
    activate Compra
    Compra-->>User: Mostrar tipos de boletos disponibles
    
    User->>Compra: Elegir tipo de boleto y cantidad
    Compra->>Sistema: Reservar boletos temporalmente
    Sistema-->>Compra: Boletos reservados (timeout 10 min)
    Compra-->>User: Mostrar resumen de compra
    
    User->>Compra: Proceder al pago
    Compra->>Pago: Iniciar proceso de pago
    activate Pago
    Pago-->>User: Mostrar opciones de pago
    
    User->>Pago: Ingresar datos de pago
    Pago->>Sistema: Procesar transacción
    Sistema->>Sistema: Validar pago
    
    alt Pago exitoso
        Sistema-->>Pago: Pago confirmado
        Pago->>Sistema: Generar boletos
        Sistema-->>Pago: Boletos generados
        Pago-->>Compra: Pago exitoso
        Compra-->>User: Mostrar confirmación y detalles del boleto
        
        User->>Compra: Descargar boleto
        Compra->>Sistema: Generar PDF del boleto
        Sistema-->>Compra: PDF generado
        Compra-->>User: Descargar archivo PDF
    else Pago fallido
        Sistema-->>Pago: Error en pago
        Pago-->>Compra: Transacción fallida
        Compra->>Sistema: Liberar boletos reservados
        Compra-->>User: Mostrar error y opciones
    end
    deactivate Pago
    deactivate Compra
    
    %% Ver Mis Boletos
    User->>Sistema: Acceder a "Mis Boletos"
    Sistema->>Sistema: Consultar boletos del usuario
    activate Sistema
    Sistema-->>User: Mostrar lista de boletos comprados
    
    User->>Sistema: Seleccionar boleto específico
    Sistema-->>User: Mostrar información del boleto
    
    User->>Sistema: Descargar boleto
    Sistema->>Sistema: Generar PDF
    Sistema-->>User: Descargar PDF del boleto
    deactivate Sistema
    
    %% Gestión de Perfil
    User->>Sistema: Acceder a "Mi Perfil"
    Sistema->>Perfil: Solicitar datos del perfil
    activate Perfil
    Perfil-->>User: Mostrar perfil del usuario
    
    User->>Perfil: Modificar datos
    Perfil->>Sistema: Actualizar información
    Sistema-->>Perfil: Confirmación de actualización
    Perfil-->>User: Datos actualizados
    
    opt Borrar cuenta
        User->>Perfil: Solicitar borrado de cuenta
        Perfil->>Sistema: Confirmar acción
        Sistema-->>Perfil: Solicitar confirmación
        Perfil-->>User: Mostrar advertencia
        User->>Perfil: Confirmar borrado
        Perfil->>Sistema: Eliminar cuenta
        Sistema-->>Perfil: Cuenta eliminada
        Perfil-->>User: Redirigir a página de salida
    end
    deactivate Perfil
    
    %% Ayuda y Soporte
    User->>Sistema: Acceder a ayuda
    Sistema->>Ayuda: Abrir módulo de ayuda
    activate Ayuda
    Ayuda-->>User: Mostrar opciones de ayuda
    
    opt Generar ticket de soporte
        User->>Ayuda: Generar ticket de soporte
        Ayuda->>Sistema: Crear ticket
        Sistema-->>Ayuda: Ticket creado
        Ayuda-->>User: Confirmación y número de ticket
    end
    deactivate Ayuda
    
    %% Información Legal
    User->>Sistema: Ver términos y condiciones
    Sistema-->>User: Mostrar términos y condiciones
    
    User->>Sistema: Ver política de privacidad
    Sistema-->>User: Mostrar política de privacidad
    deactivate Sistema
```

## 4. Diagrama de Secuencia - Proceso de Inicio de Sesión (Detallado)

```mermaid
sequenceDiagram
    actor User as Usuario/Organizador/Admin
    participant UI as Interfaz Login
    participant Auth as Servicio Autenticación
    participant Firebase
    participant DB as Base de Datos
    participant Email as Servicio Email

    %% Flujo Principal: Login exitoso
    User->>UI: Acceder a login
    activate UI
    UI-->>User: Mostrar formulario de login
    
    User->>UI: Ingresar email y contraseña
    UI->>UI: Validar formato de campos
    
    alt Campos válidos
        UI->>Auth: Enviar credenciales
        activate Auth
        Auth->>Auth: Encriptar contraseña
        Auth->>DB: Consultar usuario por email
        activate DB
        DB-->>Auth: Retornar hash de contraseña
        deactivate DB
        
        Auth->>Auth: Comparar hash con contraseña encriptada
        
        alt Credenciales correctas
            Auth->>DB: Actualizar último acceso
            activate DB
            DB-->>Auth: Confirmación
            deactivate DB
            Auth->>Auth: Generar token de sesión
            Auth-->>UI: Retornar token y datos de usuario
            UI-->>User: Redirigir a dashboard
            Note over User,UI: Login exitoso
        else Credenciales incorrectas
            Auth-->>UI: Error de autenticación
            UI-->>User: Mostrar mensaje de error
            Note over User,UI: Reintentar o recuperar contraseña
        end
        deactivate Auth
    else Campos inválidos
        UI-->>User: Mostrar errores de validación
    end
    
    %% Flujo alternativo: Olvidó contraseña
    User->>UI: Hacer clic en "Olvidé mi contraseña"
    UI-->>User: Mostrar formulario de recuperación
    
    User->>UI: Ingresar email
    UI->>Auth: Solicitar recuperación de contraseña
    activate Auth
    Auth->>DB: Verificar si email existe
    activate DB
    DB-->>Auth: Email encontrado
    deactivate DB
    
    Auth->>Auth: Generar token único de recuperación
    Auth->>DB: Guardar token con expiración
    activate DB
    DB-->>Auth: Token guardado
    deactivate DB
    
    Auth->>Email: Enviar email con enlace de recuperación
    activate Email
    Email-->>User: Email de recuperación enviado
    Email-->>Auth: Confirmación de envío
    deactivate Email
    Auth-->>UI: Confirmación de solicitud
    UI-->>User: Mostrar mensaje de confirmación
    deactivate Auth
    
    Note over User: Usuario revisa su correo
    
    User->>Email: Hacer clic en enlace de recuperación
    Email->>UI: Abrir página de nueva contraseña (con token)
    activate UI
    UI-->>User: Mostrar formulario de nueva contraseña
    
    User->>UI: Ingresar nueva contraseña
    UI->>Auth: Enviar nueva contraseña + token
    activate Auth
    Auth->>DB: Validar token (no expirado)
    activate DB
    DB-->>Auth: Token válido
    deactivate DB
    
    Auth->>Auth: Encriptar nueva contraseña
    Auth->>DB: Actualizar contraseña en BD
    activate DB
    DB-->>Auth: Contraseña actualizada
    deactivate DB
    
    Auth->>DB: Invalidar token usado
    activate DB
    DB-->>Auth: Token invalidado
    deactivate DB
    
    Auth-->>UI: Contraseña actualizada exitosamente
    UI-->>User: Mostrar confirmación y redirigir a login
    deactivate Auth
    deactivate UI
    
    %% Flujo alternativo: Autenticación con Firebase (Google/Facebook)
    User->>UI: Hacer clic en "Iniciar sesión con Google/Facebook"
    UI->>Firebase: Iniciar flujo OAuth
    activate Firebase
    Firebase-->>User: Redirigir a proveedor (Google/Facebook)
    
    User->>Firebase: Autorizar aplicación
    Firebase->>Firebase: Validar autorización
    Firebase->>Auth: Retornar token de Firebase + datos de usuario
    deactivate Firebase
    activate Auth
    
    Auth->>DB: Buscar usuario por email del proveedor
    activate DB
    
    alt Usuario existe
        DB-->>Auth: Usuario encontrado
        Auth->>DB: Actualizar último acceso
        DB-->>Auth: Confirmación
    else Usuario nuevo
        DB-->>Auth: Usuario no existe
        Auth->>DB: Crear nuevo usuario con datos del proveedor
        DB-->>Auth: Usuario creado
    end
    deactivate DB
    
    Auth->>Auth: Generar token de sesión
    Auth-->>UI: Retornar token y datos de usuario
    UI-->>User: Redirigir a dashboard
    deactivate Auth
    deactivate UI
```

---

## Notas adicionales

- Todos los diagramas incluyen manejo de errores y flujos alternativos donde sea relevante
- Los tiempos de timeout y reservas son configurables según las necesidades del negocio
- La autenticación con Firebase permite integración con Google y Facebook como se especifica en los requerimientos
- Los tokens de recuperación de contraseña tienen un tiempo de expiración para mayor seguridad
- Las transacciones de pago incluyen validaciones y rollback en caso de fallo
