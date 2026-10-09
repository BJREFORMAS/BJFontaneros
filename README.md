# B&J Reformas y Servicios Integrales S.L.

Repositorio de la web corporativa de B&J Reformas y Servicios Integrales S.L.

Sitio en producción: https://bjreformas.com/

> Este README refleja el baseline técnico auditado el 03/10/2026.
> La información histórica no verificada no debe considerarse configuración vigente.

## Baseline técnico verificado

### CONFIRMADO — Arquitectura web

- Framework: Astro 7.3.5.
- Sitio generado como web estática.
- Salida de producción: `dist/`.
- Despliegue mediante GitHub Actions y GitHub Pages.
- El workflow de despliegue utiliza Node.js 24.
- Repositorio GitHub: `BJREFORMAS/BJFontaneros`.
- Rama principal: `master`.
- El despliegue con Node 24 fue validado correctamente el 03/10/2026.

### CONFIRMADO — Tecnologías principales

- Astro
- TypeScript
- SCSS / Sass
- Bootstrap 5.3.8
- GitHub Actions
- GitHub Pages

Versiones verificadas durante la auditoría:

- `astro@7.3.5`
- `vite@8.3.1`
- `sass@1.81.0`
- `bootstrap@5.3.8`

## Desarrollo local

Comandos verificados:
- `npm install`
- `npm run dev` → `astro dev`
- `npm run start` → `astro dev`
- `npm run build` → `astro build`
- `npm run preview` → `astro preview`

## Integración WebToLead / Zoho CRM

### CONFIRMADO

- El formulario de `/presupuestos/` crea correctamente un Posible cliente en Zoho CRM.
- Los campos del formulario llegan correctamente a Zoho CRM.
- Se ejecuta el workflow `Nuevo lead web`.
- Se genera la notificación `Aviso Lead Web BJ`.
- El aviso llega correctamente al buzón corporativo.

Campos comprobados:
- Nombre
- Teléfono
- Dirección / ciudad
- Correo electrónico
- Tipo de servicio
- Fuente del posible cliente
- Descripción

La prueba funcional realizada el 03/10/2026 fue posteriormente eliminada de Zoho CRM.

### PENDIENTE — Redirección posterior al formulario

Después del envío correcto, Zoho muestra actualmente su página estándar de confirmación en lugar de redirigir a `https://bjreformas.com/gracias`.
El HTML de producción contiene `returnURL` apuntando a `/gracias`, pero la configuración del formulario web `Solicitud HOME360` no pudo auditarse porque el usuario actual no dispone de permisos suficientes para administrar formularios web.
No modificar permisos ni configuración de Zoho sin auditoría previa.

## Seguridad de dependencias

### RESUELTO — Dependabot

Estado actual verificado: **0 alertas Dependabot abiertas**.

#### #114 — braces — RESUELTA
Estado previo verificado: `sass@1.81.0 → @parcel/watcher@2.5.0 → micromatch@4.0.8 → braces@3.0.3`.
La dependencia vulnerable estaba asociada al entorno de build y no se encontraron referencias a `braces`, `micromatch` ni `@parcel/watcher` dentro de `dist/`.

Remediación aplicada:
- `@parcel/watcher` actualizado de `2.5.0` a `2.6.0`.
- Eliminada la cadena transitiva `micromatch → braces`.
- Build validado correctamente.
- PR #4 fusionado y despliegue de producción completado correctamente.
- Alerta cerrada automáticamente por Dependabot.

#### #115 — http-cache-semantics — RESUELTA
Estado previo verificado: `astro@7.3.5 → http-cache-semantics@4.2.0`.
No se encontraron referencias a `http-cache-semantics` dentro de `dist/`.

Remediación aplicada:
- `http-cache-semantics` actualizado de `4.2.0` a `4.3.0`.
- `astro@7.3.5` continúa como dependencia principal.
- `npm audit` validado con **0 vulnerabilidades** tras la actualización.
- Build validado correctamente.
- PR #5 fusionado y despliegue de producción completado correctamente.
- Alerta cerrada automáticamente por Dependabot.

## Deuda técnica SCSS / Sass

### CONFIRMADO

`src/styles/global.scss` utiliza actualmente `@import` para cargar módulos de Bootstrap, variables propias y estilos del proyecto.

La configuración actual depende del orden y del ámbito global de Sass:

- `_variables.scss` define overrides propios de Bootstrap, incluyendo `$primary`, `$secondary`, `$navbar-nav-link-padding-x` y `$offcanvas-horizontal-width`.
- `_style.scss` utiliza `$primary` y el mixin de Bootstrap `media-breakpoint-down()`.
- Los módulos de Bootstrap se cargan de forma selectiva y en un orden concreto desde `global.scss`.

El build funciona correctamente con `sass@1.81.0` y `bootstrap@5.3.8`, aunque mantiene advertencias de depreciación relacionadas con `@import` y con SCSS interno de Bootstrap.

### PRUEBA NO INTEGRADA — Sass 1.105.1

Se probó de forma aislada la actualización de `sass@1.81.0` a `sass@1.105.1`.

Resultado verificado:

- El build continuó funcionando correctamente.
- `npm audit` permaneció en 0 vulnerabilidades.
- Sass pasó a utilizar `chokidar@5.0.0` y `readdirp@5.1.1`.
- Los avisos repetitivos de depreciación aumentaron de 188 a 248.
- Aparecieron nuevas advertencias `if-function` procedentes del SCSS de Bootstrap.
- La actualización no resolvió la deuda asociada a `@import`.

La prueba fue revertida completamente y `sass@1.105.1` no se integró en `master`.

### PENDIENTE

No realizar una sustitución mecánica de `@import` por `@use` / `@forward`.

La futura migración SCSS debe diseñar explícitamente cómo se gestionarán:

- los overrides de variables de Bootstrap;
- los mixins utilizados por estilos propios;
- el orden de carga de módulos;
- la compatibilidad visual y funcional de los componentes Bootstrap.

La intervención deberá realizarse en una rama independiente, con build completo, comparación visual y validación funcional antes de cualquier integración en `master`.

## Política de cambios
- No modificar producción directamente.
- Trabajar mediante ramas específicas.
- Ejecutar `npm run build` antes de integrar cambios.
- Revisar el diff antes de hacer commit.
- Integrar cambios mediante Pull Request.
- No publicar tokens, credenciales, secretos ni valores sensibles en el repositorio.
- Aplicar el principio de mínimo privilegio a credenciales de acceso.

## LEGACY / información histórica no vigente
El README anterior contenía una referencia a B&J como servicio ubicado en Benito Juárez, CDMX.
Esa información no corresponde al proyecto actual y se considera LEGACY.
También deben revalidarse antes de documentarse como hechos vigentes afirmaciones históricas como puntuaciones concretas de Lighthouse u otras métricas no comprobadas durante la auditoría actual.

## Estado del baseline
Baseline auditado: **03/10/2026**

- Web en producción: CONFIRMADO
- Build Astro: CONFIRMADO
- GitHub Pages: CONFIRMADO
- Node 24 en deploy: CONFIRMADO
- WebToLead → Zoho CRM: CONFIRMADO
- Workflow Zoho → correo: CONFIRMADO
- Redirección `/gracias`: PENDIENTE
- Dependabot #114: RESUELTO
- Dependabot #115: RESUELTO
- Migración Sass `@import`: PENDIENTE / DEUDA TÉCNICA
