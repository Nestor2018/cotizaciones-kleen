# Cotizaciones Kleen's

Generador de cotizaciones para PERFEC KLEEN, S.A. de C.V. Un único archivo HTML,
sin build ni servidor: se abre en el navegador, se editan los campos en la barra
superior y el documento se actualiza en vivo.

**Publicado en:** https://nestor2018.github.io/cotizaciones-kleen/

## Uso

1. Completá los campos de la barra superior (cliente, número, fecha, costos).
2. Usá los interruptores para mostrar u ocultar la página "Nosotros", la segunda
   línea de servicio y el bloque de firma.
3. Presioná **Descargar PDF** y elegí "Guardar como PDF" en el diálogo de
   impresión.

Los valores quedan guardados en el navegador (`localStorage`), así que al volver
a abrir el archivo siguen ahí. **Restablecer** vuelve a los valores iniciales.

## Cómo se calcula

| Concepto      | Fórmula                                      |
| ------------- | -------------------------------------------- |
| Costo mensual | costo quincenal × 2                          |
| Subtotal      | mensual línea 1 + mensual línea 2 (si aplica) |
| IVA           | subtotal × 16 %                              |
| Total mensual | subtotal + IVA                               |

## Notas técnicas

- **El PDF se genera con `window.print()`**, no con html2canvas/jsPDF. El
  documento son tres páginas Carta exactas (816 × 1056 px a 96 dpi); imprimir
  produce texto vectorial seleccionable y un archivo de ~200 KB, mientras que
  rasterizar daría texto borroso y varios megas.
- **Sólo la última página visible** lleva `break-after: auto`. Sin eso, apagar
  la página "Nosotros" dejaba una hoja en blanco al final del PDF.
- **El logo va embebido como data-URI**, recortado de sus márgenes transparentes
  (el original tenía 48 % del ancho y 26 % del alto en vacío) y cuantizado a 64
  colores: 32 KB en lugar de 182 KB. Por eso el archivo no depende de `assets/`.
- El archivo no contiene datos reales de clientes ni tarifas: los valores por
  defecto son genéricos y lo que se escribe vive sólo en el navegador.

## Estructura

```
index.html                        generador completo y autocontenido
assets/kleens-logo-stacked.png    logo original de marca
assets/kleens-logo-trimmed.png    logo recortado (el que va embebido)
```

El diseño original proviene de un proyecto de Claude Design; este repositorio es
la versión editable y publicable.
