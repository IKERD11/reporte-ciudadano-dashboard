# Dashboard administrativo · Reporte Ciudadano

Panel web del piloto de **Reporte Ciudadano para Cuautla, Morelos**. Permite consultar reportes ciudadanos, darles seguimiento y revisar la actividad del municipio desde una misma interfaz.

**Estado:** piloto de pruebas. No representa atención municipal real ni un canal oficial para emergencias.

- **Dashboard:** https://ikerd11.github.io/reporte-ciudadano-dashboard/
- **App ciudadana:** https://ikerd11.github.io/reporte-ciudadano-app/
- **Código fuente:** https://github.com/IKERD11/reporte-ciudadano-cuautla
- **Actualización de esta guía:** 23 de noviembre de 2026.

## Acerca de este repositorio

Este repositorio publica la versión compilada del dashboard en **GitHub Pages**. Contiene los archivos que descarga el navegador, recursos gráficos y documentación. El código editable se encuentra en `apps/admin` del repositorio fuente.

Aquí no se aloja la base de datos ni se incluyen evidencias privadas, sesiones o claves de servidor. Los correos y contraseñas de seis cuentas de prueba se incluyen expresamente al final de este README.

## Funciones principales

### Resumen

Indicadores de reportes recibidos, pendientes, en atención y atendidos. Incluye mapa, reportes recientes, tendencia semanal y distribución por categoría. Las cifras dependen del municipio y los filtros autorizados.

### Reportes

Listado paginado de 20 registros con folio, incidente, categoría, fecha, estado y responsable. Permite buscar por folio, descripción o dirección y filtrar por categoría, estado e intervalo de fechas.

Incluye exportación CSV de la página actual o de resultados filtrados, con un máximo de 5,000 registros por exportación completa.

Al abrir un reporte se puede consultar su descripción, fotografías disponibles, ubicación e historial. Según los permisos y el estado del reporte, se puede cambiar su prioridad, actualizar su estado y asignar departamento y responsable.

### Mapa

Muestra las ubicaciones de los reportes de la página actual, hasta 20 registros. Los filtros y la paginación permiten consultar otros resultados; no representa automáticamente todos los reportes del municipio a la vez.

### Métricas

Tendencia semanal, distribución por categoría y estado, y tiempo promedio hasta resolución cuando existen datos suficientes. La analítica avanzada y los SLA permanecen pendientes.

### Equipos

Consulta integrantes, roles y casos activos. Las cuentas con permiso de gestión de usuarios pueden administrar los roles municipales de cuentas ya registradas. El rol Ciudadano retira el acceso administrativo y conserva sus reportes.

La interfaz no permite modificar el propio rol ni cuentas de superadministrador. No incluye invitaciones por correo ni creación de roles personalizados.

### Configuración

Consulta categorías y datos del piloto. Las cuentas con permisos correspondientes pueden crear departamentos para utilizarlos en la asignación de reportes.

## Conexión con la app ciudadana

La app y el dashboard comparten el mismo backend de Supabase. Los reportes enviados
por la app aparecen en el panel; los cambios de estado del panel se consultan en
la app junto con su historial y avisos. Los permisos se validan en el servidor.

Resumen, Reportes, Mapa y Métricas se actualizan cada 30 segundos mientras la
pestaña está visible, y al recuperar foco o conexión. Se conservan los filtros.
La actualización se pausa durante operaciones y no reemplaza formularios de
detalle abiertos. La app consulta las novedades al abrir o recargar sus vistas.

El 23 de noviembre de 2026 se verificó el recorrido mediante 21 comprobaciones
SQL de integración y una prueba HTTP con un reporte sintético, retirado después
de la comprobación. También pasaron las suites de administración, avisos y equipos.
No equivale a una prueba de GPS o fotografías en un dispositivo real.

## Guía rápida de uso

1. Abre el dashboard e ingresa el correo y contraseña de una cuenta autorizada.
2. Revisa los indicadores de **Resumen**.
3. Entra a **Reportes** y aplica los filtros necesarios.
4. Abre un reporte para consultar datos, evidencias e historial.
5. Realiza las acciones habilitadas para tu rol: prioridad, estado o asignación.
6. Comprueba el resultado y cierra sesión al terminar, especialmente en equipos compartidos.

Los cambios de gestión quedan registrados en la bitácora. Si otra persona modifica el mismo reporte, el panel puede pedir recargarlo antes de continuar.

## Flujo de seguimiento

El recorrido habitual es:

**Recibido → En revisión → Validado → Asignado → En atención → Resuelto → Cerrado.**

La asignación a un departamento permite avanzar desde Validado. Las opciones de rechazo, duplicado o reapertura dependen del estado actual y de los permisos; no todas las transiciones están disponibles en todo momento.

## Acceso y permisos

El inicio de sesión utiliza **Supabase Auth**. Los permisos se comprueban en el backend y están limitados al municipio y rol de la cuenta. Descargar el sitio o clonar el repositorio no concede permisos adicionales.

Las seis cuentas incluidas tienen rol **MUNICIPAL_ADMIN de Cuautla**; no son superadministradores. Sus contraseñas están publicadas por solicitud del propietario: quienes consulten el repositorio pueden usarlas para acceder al piloto. No reutilizarlas en otros servicios ni asociarlas con información real.

## Tecnología

- **Interfaz:** Next.js y React con exportación estática.
- **Autenticación y datos:** Supabase y PostgreSQL.
- **Mapa:** MapLibre GL JS 6.19.0 y OpenFreeMap/OpenMapTiles con datos de OpenStreetMap; Leaflet como respaldo 2D.
- **Alojamiento:** GitHub Pages con HTTPS.

## Desarrollo y actualización

Para desarrollar o reconstruir el panel, utiliza el repositorio fuente y sus guías:

- [Instalación y colaboración](https://github.com/IKERD11/reporte-ciudadano-cuautla/blob/codex/figma-renovacion-ciudadana/CONTRIBUTING.md).
- [Publicación y actualización web](https://github.com/IKERD11/reporte-ciudadano-cuautla/blob/codex/figma-renovacion-ciudadana/docs/publicacion-web.md).

Esta copia compilada no requiere ejecutar `npm install` para visitar el sitio. Las actualizaciones se generan desde `apps/admin` y se publican aquí. Editar el código fuente en otro repositorio no actualiza automáticamente esta publicación.

## Límites del piloto

- Se trabaja con reportes de prueba y permisos municipales existentes.
- Las INE reales y la operación municipal real continúan deshabilitadas.
- La demostración con datos en memoria está desactivada en la compilación pública; las cuentas de prueba consultan el backend del piloto.
- Las notificaciones con la app cerrada no están implementadas. Los avisos internos se consultan en la app ciudadana.
- Las seis cuentas aprobaron inicio de sesión, rol municipal y consulta de contexto, resumen y reportes. Esto no equivale a una prueba completa de todos los recorridos en navegador.

## Problemas frecuentes

- **No puedo ingresar:** comprueba el correo y la contraseña completos. El dominio `example.test` no recibe correos de recuperación.
- **Mi cuenta no tiene acceso:** una cuenta ciudadana necesita un rol administrativo autorizado para ingresar al panel.
- **No aparecen reportes:** revisa filtros, página y municipio de la cuenta. Puede no haber resultados para esa consulta.
- **Una acción está deshabilitada:** comprueba los permisos y el estado actual del reporte.
- **Falla la carga de datos o del mapa:** comprueba la conexión y vuelve a cargar la página. Datos, fotografías y cartografía requieren sus servicios externos.

## Accesos de prueba

Estas seis cuentas tienen permisos de administrador municipal de Cuautla, Morelos.
Son cuentas sintéticas para las pruebas del piloto.

### Cuenta 1

- Correo: `admin.prueba.01@example.test`
- Contraseña: `Rc!QiRqb655RxmDLQ7FBow1Y48Kq_2i559b9a`

### Cuenta 2

- Correo: `admin.prueba.02@example.test`
- Contraseña: `Rc!m32njnTTqaSDxQjGWOjYJBSp8tz-vVbX9a`

### Cuenta 3

- Correo: `admin.prueba.03@example.test`
- Contraseña: `Rc!x5I-Y_rDNrKFP9JWpftg291sfIKL014O9a`

### Cuenta 4

- Correo: `admin.prueba.04@example.test`
- Contraseña: `Rc!44ed5Gw-kmYx7jrmt5JWp1gQOuez3ziR9a`

### Cuenta 5

- Correo: `admin.prueba.05@example.test`
- Contraseña: `Rc!2mPhbejbfwRVnqIN0IWRd-LO4M2uaVYE9a`

### Cuenta 6

- Correo: `admin.prueba.06@example.test`
- Contraseña: `Rc!ni3_cVe4CJJaCfp1lwg1Z4usngc3gjDG9a`

Detalles de las cuentas: [CUENTAS-PRUEBA.md](CUENTAS-PRUEBA.md).
