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
- Bootstrap 5.3.3
- GitHub Actions
- GitHub Pages

Versiones verificadas durante la auditoría:

- `astro@7.3.5`
- `vite@8.3.1`
- `sass@1.81.0`
- `bootstrap@5.3.3`

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

### PENDIENTE — Dependabot

#### #114 — braces
Cadena verificada: `sass@1.81.0 → @parcel/watcher@2.5.0 → micromatch@4.0.8 → braces@3.0.3`.
No se encontraron referencias a `braces`, `micromatch` ni `@parcel/watcher` dentro de `dist/`.
Clasificación actual: dependencia transitiva asociada al entorno de build, sin evidencia de exposición directa en el contenido estático publicado.

#### #115 — http-cache-semantics
Cadena verificada: `astro@7.3.5 → http-cache-semantics@4.2.0`.
No se encontraron referencias a `http-cache-semantics` dentro de `dist/`.
Clasificación actual: dependencia transitiva de Astro, sin evidencia de exposición directa en el sitio estático publicado.
Las alertas #114 y #115 deben permanecer abiertas hasta disponer de una remediación segura. No usar `Dismiss alert` únicamente para ocultarlas.

## Deuda técnica SCSS / Sass

### CONFIRMADO
`src/styles/global.scss` utiliza actualmente `@import` para cargar módulos de Bootstrap, variables propias y estilos del proyecto.
El build funciona correctamente.
Con `--quiet-deps`, desaparecen los avisos procedentes de dependencias externas, pero permanecen las advertencias de depreciación de `@import` del código raíz.

### PENDIENTE
Migrar en una intervención específica desde `@import` hacia `@use` / `@forward`.
La migración debe realizarse en una rama independiente, con build completo, comparación visual y validación de Bootstrap.
No realizar esta migración directamente sobre `master`.

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
- Dependabot #114: PENDIENTE DE REMEDIACIÓN
- Dependabot #115: PENDIENTE DE REMEDIACIÓN
- Migración Sass `@import`: PENDIENTE / DEUDA TÉCNICA
