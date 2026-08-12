# Cotizaciones Kleen's

Generador de cotizaciones para PERFEC KLEEN, S.A. de C.V. Un único archivo HTML,
sin build ni servidor: se abre en el navegador, se editan los campos en la barra
superior y el documento se actualiza en vivo.

**Publicado en:** https://nestor2018.github.io/cotizaciones-kleen/

## Uso

1. Completá los campos de la barra superior (cliente, número, fecha, IVA).
2. **Hacé clic sobre cualquier texto de las hojas y escribí encima.** Títulos,
   párrafos, celdas de la tabla, condiciones y membrete son editables en el
   lugar; se resaltan al pasar el mouse.
3. Agregá servicios con **+ Agregar línea** debajo de la tabla y quitalos con la
   **×** que aparece al costado de cada fila.
4. Usá los interruptores para mostrar u ocultar la página "Nosotros" y el bloque
   de firma.
5. Presioná **Descargar PDF** y elegí "Guardar como PDF" en el diálogo de
   impresión.

Todo queda guardado en el navegador (`localStorage`), así que al volver a abrir
el archivo sigue ahí. **Restablecer** vuelve a los valores y textos iniciales.

## Qué se edita dónde

| Mecanismo               | Alcance                                                                 |
| ----------------------- | ----------------------------------------------------------------------- |
| Barra superior          | Lo que alimenta cálculos o se repite en varias páginas                  |
| Clic sobre el documento | Los ~50 textos literales y las celdas de cada línea de servicio         |

## Cómo se calcula

| Concepto              | Fórmula                                               |
| --------------------- | ----------------------------------------------------- |
| Costo mensual (línea) | cantidad × costo quincenal c/u × 2 quincenas          |
| Subtotal              | suma del costo mensual de todas las líneas            |
| IVA                   | subtotal × porcentaje configurable (16 % por defecto)  |
| Total mensual         | subtotal + IVA                                        |

La columna **Costo quincenal** es **por unidad**, no el total de la línea: la
cantidad la multiplica.

## Cuánto entra en la hoja

La hoja es de alto fijo (Carta), así que las líneas no son infinitas en la
práctica: entran **3 con el bloque de firma visible y 5 sin él**. Al pasarse
aparece un aviso en la barra superior, porque lo que sobresale se recorta en el
PDF en vez de saltar a otra página.

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
- **Los textos editables usan `contenteditable="plaintext-only"`** para que
  pegar desde Word no inyecte marcado; en Firefox, que no lo soporta, hay un
  manejador de `paste` equivalente. Enter confirma la edición en vez de insertar
  un salto: la hoja mide 11 in exactas y un salto empujaría contenido fuera del
  área imprimible.
- **De los textos sólo se guardan las diferencias** contra el marcado, no las
  cadenas completas. Si más adelante se reformula una frase del template, el
  texto nuevo llega a quien nunca editó esa cadena en particular.
- **El porcentaje de IVA es un campo, no una etiqueta.** Antes decía "IVA 16 %"
  con el 0,16 fijo en el código: editar el rótulo no habría cambiado la cuenta.
- **Las celdas numéricas se reformatean al salir del campo, nunca al tipear.**
  Reescribir el nodo bajo el cursor mandaría el cursor al principio de la celda.
- **El aviso de desborde mide el pie contra el borde de la hoja**, no
  `scrollHeight`. Un ítem flex no se encoge por debajo de su contenido
  (`min-height: auto`), así que la columna crece con las filas y nunca reporta
  desborde; lo que sí se desplaza es el pie que va debajo.
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
