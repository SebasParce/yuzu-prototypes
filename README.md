# yuzu-prototypes

Repo estático que sirve `proto.yuzulab.io`. Aloja los prototipos personalizados generados por prospecto durante la campaña de cold outreach de Yuzu Lab. Ver `prototype-generation.md` (fuera de este repo, en la carpeta de lineamientos) para el proceso completo de theming por prospecto.

## Estructura

```
/public
  index.html               ← placeholder de la raíz del subdominio, no editar
  base-home-health.html    ← plantilla genérica, categoría Home health (Medicare-certified)
  base-home-care.html      ← plantilla genérica, categoría Home care / non-medical
  [categoria]-[slug].html  ← prototipos ya personalizados por prospecto (se van agregando acá)
vercel.json                ← fuerza modo estático, sin build step
package.json                ← metadata mínima, sin dependencias
```

Las plantillas `base-*.html` son la fuente de verdad. Nunca se editan directamente ni se despliegan tal cual — siempre se copian, se re-marcan con el logo/color/nombre del prospecto, y se guardan como un archivo nuevo siguiendo la convención de abajo.

## Convención de nombres

```
[categoria]-[slug-nombre-prospecto].html
```

- `categoria`: `hh` (home health, Medicare-certified) o `hc` (home care, non-medical)
- `slug-nombre-prospecto`: nombre de la agencia en minúsculas, sin espacios ni caracteres especiales, separado por guiones

Ejemplo: `hc-sunrise-companion-care.html` → queda accesible en `proto.yuzulab.io/hc-sunrise-companion-care` (la extensión `.html` se oculta automáticamente por `cleanUrls` en `vercel.json`).

## Cómo se genera y despliega un prototipo nuevo

1. Copiar el archivo base correspondiente (`base-home-health.html` o `base-home-care.html`) a un archivo nuevo dentro de `/public`, ya renombrado según la convención.
2. Editar en la copia: las tres variables de color (`--accent`, `--accent-deep`, `--accent-soft`), el logo dentro de `<template id="logo-template">`, y el texto de `id="client-name-slot"`. Ver el detalle completo de este paso en `prototype-generation.md`.
3. Commit y push a este repo (o `vercel --prod` directo si se despliega sin pasar por git, ver más abajo).
4. La URL resultante es siempre predecible: `proto.yuzulab.io/[nombre-de-archivo-sin-extension]`.

## Despliegue

Este repo está conectado a Vercel. Cualquier push a la rama principal dispara un deploy automático de todo `/public`, así que basta con hacer commit de los archivos nuevos o editados.

Si se necesita desplegar sin pasar por git (por ejemplo desde un entorno de automatización sin acceso de push):

```bash
vercel --prod --yes --token=$VERCEL_TOKEN
```

Ejecutado desde la raíz de este proyecto. Requiere el CLI de Vercel instalado y un token con permisos de deploy sobre este proyecto específico (`yuzu-prototypes`).

## Qué no va en este repo

- Datos reales de pacientes o de agencias más allá de nombre y marca visual (logo/color). Los datos de ejemplo dentro de cada prototipo (nombres de pacientes, montos, fechas) son siempre ilustrativos, nunca información real de ningún prospecto o cliente.
- Credenciales o tokens de ningún tipo, ni siquiera en `.env` versionado — usar variables de entorno del lado de quien ejecuta el deploy.
