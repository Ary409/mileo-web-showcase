# Mileo Web — Gestión de flotas de transporte de carga

Panel web para empresas de transporte: administra camiones, remolques, conductores, viajes, combustible y mantenimiento, y genera los reportes de impuesto al combustible **IFTA** por estado.

> **Este repositorio es una vitrina técnica, sin código fuente.** Mileo es un producto propietario de la empresa para la que trabajé; el código vive en su repositorio privado. Aquí documento la arquitectura, el stack y mi participación. Puedo ampliar cualquier punto en una entrevista técnica.

| | |
|---|---|
| **Mi rol** | Desarrollador frontend de la versión web |
| **Período** | 2026 |
| **Frontend** | React 19 · TypeScript · Vite 6 · React Router 7 |
| **Estado y formularios** | Zustand · React Hook Form · Zod |
| **Comunicación** | Axios contra una API REST (Node.js · Express · MySQL) |
| **Infraestructura** | AWS: S3 + CloudFront (web), EC2 (API), RDS (base de datos) |
| **Tamaño** | 13 módulos funcionales · más de 120 archivos TypeScript |

---

## El producto

Mileo nació como aplicación móvil para conductores y dueños de flota. La versión web lleva esa operación a un panel de escritorio pensado para quien administra la flota desde una oficina.

| Módulo | Qué hace |
|---|---|
| **Acceso** | Inicio de sesión, creación de cuenta, recuperación de contraseña y PIN de seguridad. |
| **Panel** | Indicadores de la flota en una sola vista. |
| **Flota, camiones y remolques** | Inventario de unidades con ficha de detalle por vehículo. |
| **Conductores** | Registro y ficha de cada conductor. |
| **Viajes** | Registro de viajes con su detalle y recorrido. |
| **Combustible** | Cargas de combustible por unidad. |
| **Mantenimiento** | Programación e historial de servicios. |
| **Reportes e IFTA** | Reportes operativos, detalle de millas y galones por estado y enmiendas de declaraciones IFTA. |
| **Notificaciones, perfil y ajustes** | Avisos y configuración de la cuenta. |

---

## Arquitectura

Arquitectura **híbrida**: la interfaz se organiza por funcionalidad y la lógica se separa por capas. La idea es que una pantalla nunca sepa de dónde vienen los datos.

```text
src/
├── app/              Arranque: rutas, layouts, proveedores, stores globales, estilos
├── modules/          Una carpeta por funcionalidad (páginas, componentes, hooks)
├── application/      Casos de uso: orquestan una operación del negocio
├── domain/           Entidades y contratos de repositorio (sin dependencias externas)
├── infrastructure/   Repositorios HTTP que implementan los contratos del dominio
├── core/             Configuración, variables de entorno, tipos y utilidades
└── shared/           Componentes de interfaz, hooks y esquemas reutilizables
```

Flujo de una operación:

```text
Página  →  Caso de uso  →  Contrato (domain)  ←  Repositorio HTTP (infrastructure)  →  API
```

**Por qué así y no solo por capas o solo por funcionalidades**

- Organizar la interfaz por funcionalidad mantiene el mismo modelo mental de la aplicación móvil y hace que cada módulo se pueda trabajar sin tocar los demás.
- Separar `domain`, `application` e `infrastructure` evita que las reglas del negocio queden dentro de componentes. Las páginas dependen de un contrato, no de Axios: cambiar la API o simularla en pruebas no obliga a tocar la interfaz.
- `shared` concentra lo transversal (botones, campos, selectores, barra lateral) para que las pantallas se vean y se comporten igual.

**Por qué Vite y no Next.js.** Es un panel privado detrás de inicio de sesión: no necesita SEO ni renderizado en servidor. Vite entrega archivos estáticos que se sirven desde S3 + CloudFront, sin servidor de Node que mantener para el frontend.

---

## Mi participación

- Construcción de la base del proyecto web y su estructura por capas.
- Pantallas de los módulos operativos y sus vistas de detalle.
- **Conexión con la API:** repositorios HTTP para autenticación, flota, viajes e IFTA, con manejo de sesión y rutas protegidas.
- **Validación de formularios** en el cliente con React Hook Form y esquemas Zod, con mensajes de error por campo.
- Componentes reutilizables: selector con búsqueda, selector de fecha, tarjetas de métricas, navegación lateral y superior.
- Documentación de arquitectura y **guía de despliegue en AWS** (migraciones en RDS, API en EC2 con PM2, frontend en S3 con invalidación de CloudFront).

---

## Contacto

**Jeferson Herrera** — [@Ary409](https://github.com/Ary409)

Mileo y su marca pertenecen a sus respectivos propietarios. Este repositorio no contiene código, credenciales ni datos de la empresa.
