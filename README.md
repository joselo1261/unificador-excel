# Unificador de hojas Excel

Herramienta web que **unifica varias hojas de un archivo Excel en una sola**, todo desde el navegador. No requiere instalación ni servidor: es una única página HTML que usa [SheetJS (xlsx)](https://sheetjs.com/) cargado por CDN.

## Características

- Subir un archivo `.xlsx` / `.xls` y combinar sus hojas en una sola tabla.
- Vista previa del resultado antes de exportar.
- Exportar el resultado a **XLSX** o **CSV**.
- 100% del lado del cliente: los datos no salen de tu computadora.

## Uso

1. Abrí `index.html` en el navegador (doble clic) o servilo con un servidor estático.
2. Subí tu archivo Excel en la sección **1. Subir archivo**.
3. Seguí el flujo de trabajo para unificar las hojas.
4. Descargá el resultado en **XLSX** o **CSV**.

### Desarrollo local (opcional)

El proyecto incluye configuración para la extensión **Live Server** de VS Code (puerto `5501`):

```
Click derecho sobre index.html → "Open with Live Server"
```

## Tecnología

- HTML + CSS + JavaScript (sin build, sin dependencias instaladas)
- [xlsx 0.18.5](https://cdn.jsdelivr.net/npm/xlsx@0.18.5/) vía CDN

## Licencia

Uso personal / interno.
