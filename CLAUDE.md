# CLAUDE.md

Guía para Claude Code (claude.ai/code) al trabajar en este repositorio.

## Qué es

**Unificador de hojas Excel**: herramienta web de un solo archivo que combina varias hojas de un libro Excel en una sola tabla y la exporta a XLSX o CSV. Todo corre en el navegador (client-side); los datos del usuario nunca salen de su máquina.

- Idioma de la interfaz y los comentarios: **español (es-AR)**.
- Sin build, sin backend, sin dependencias instaladas (`node_modules`).

## Arquitectura

Todo el proyecto vive en un único archivo: **[index.html](index.html)** (~1140 líneas), con tres bloques:

1. **`<style>`** — CSS con variables de tema (`:root`, `--primary: #1E3A89`, etc.). Sin framework CSS.
2. **`<body>`** — markup de la UI: dropzone de subida, opciones, pasos del flujo, resumen y modal.
3. **`<script>`** — toda la lógica en JS vanilla, sin módulos.

La única dependencia externa es **SheetJS (xlsx 0.18.5)** cargada por CDN:
```html
<script src="https://cdn.jsdelivr.net/npm/xlsx@0.18.5/dist/xlsx.full.min.js"></script>
```
Expone el global `XLSX`, usado para leer (`XLSX.read`) y escribir (`XLSX.utils`, `XLSX.write`) libros.

El único otro archivo del proyecto es **[favicon.svg](favicon.svg)** (ícono verde de Excel), referenciado desde `index.html` con una URL versionada para romper la caché del navegador:
```html
<link rel="icon" type="image/svg+xml" href="/favicon.svg?v=2" />
```

## Flujo de datos

```
Usuario suelta/elige archivo
  → handleFile()        lee el File como ArrayBuffer
  → readWorkbook()      XLSX.read → objeto workbook
  → validateWorkbook()  verifica que las hojas sean unificables (mismas columnas/encabezados)
  → mergeValidatedSheets()  concatena las filas en una sola tabla
  → renderSummary() / renderRowControl()  muestra el resultado
  → handleDownload() → generateAndDownloadFile()  exporta a XLSX o buildCsvBlob() a CSV
```

### Funciones clave (todas en el `<script>` de index.html)

| Función | Rol |
|---------|-----|
| `handleFile` / `resetAll` | Entrada del archivo y reseteo del estado |
| `readWorkbook` | Parseo del Excel con SheetJS |
| `validateWorkbook` | Reglas de validación entre hojas (encabezados, columnas) |
| `mergeValidatedSheets` | Unificación de las filas |
| `normalizeHeaders` / `trimTrailingEmptyCells` / `findDuplicates` | Limpieza de datos |
| `renderSummary` / `renderRowControl` | Render del resultado y controles de filas |
| `generateAndDownloadFile` / `buildCsvBlob` / `csvEscape` | Exportación XLSX y CSV |
| `setStepState` / `showStepActive` / `showStepDone` / `markStepError` | Indicador visual de pasos (leer → validar → unificar → descargar) |
| `showModal` / `closeModal` | Modal de errores/detalles |

Las opciones del usuario se leen de los checkboxes `#ignoreEmptySheets` y `#trimHeaders`.

## Convenciones

- **Un solo archivo**: toda la lógica de la app va dentro de `index.html` (el único otro asset es `favicon.svg`). No introducir build tools, bundlers ni `package.json` salvo pedido explícito.
- **JS vanilla**: sin frameworks ni librerías nuevas. Si hace falta algo de Excel, ya está `XLSX` disponible.
- **IDs como contrato**: el JS referencia elementos por `id` (ver lista arriba). Al editar el markup, mantené los `id` o actualizá ambos lados.
- **Estilos**: usar las variables CSS de `:root` existentes en vez de colores hardcodeados.
- **Idioma**: textos de UI, mensajes de estado y comentarios en español.

## Desarrollo

No hay comandos de build ni test. Para probar:

- Abrir `index.html` directo en el navegador, **o**
- VS Code + extensión **Live Server** (configurada en [.vscode/settings.json](.vscode/settings.json), puerto `5501`): click derecho sobre `index.html` → "Open with Live Server".

## Deploy

Desplegado en **Vercel** (`https://unificador-excel-two.vercel.app`), redeploy automático al hacer push a `main`. Al cambiar el favicon, actualizá `favicon.svg` **y** bumpeá el `?v=` en `index.html` para que el navegador lo vuelva a bajar (la caché de favicons es muy persistente y no se limpia con un refresh normal).

## Git

Repositorio en `main`, remoto: `https://github.com/joselo1261/unificador-excel`. Commits y mensajes en español.
