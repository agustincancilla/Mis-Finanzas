# Mis Finanzas

App de finanzas personales de Agus: https://agustincancilla.github.io/Mis-Finanzas/

- Pestañas (en este orden, lo pidió Agus): Mes, Tarjetas, Inversiones, Comida, Auto, Deudas.
- **Línea rápida** arriba (se puede dictar con el micrófono del teclado): separa por coma o punto y entiende gastos ("súper 25000 visa", "nafta 30 lucas", "tele 300000 en 6 cuotas mp", "ayer…"), cambios de precio ("subió el gym a 50000", "el kilo de pollo está a 9500") y ganancias ("hoy hice 95 mil"). Siempre muestra una hoja para confirmar antes de guardar.
- **Mes:** ingresos ("Cobrás"), gastado, te queda y ahorro estimado. Lo que ganaste (semana contra el mínimo) y "Ganancia neta de trabajar" (`netoTrabajo`: cobra − auto − monotributo; lo que falta del mes se estima con el tope de GNC y el seguro sin pagar). El "sueldo neto" de $1.500.000 de la planilla era una aproximación y ya no se usa. Gastos fijos de dos tipos: "pago" (se tilda al pagarlo y crea el gasto) y "tope" (se llena con los gastos de esa categoría). Día a día, por categoría y plata que sale en el mes (efectivo + resúmenes que vencen).
- **Ingresos:** Agus maneja con Didi y cobra por día (lun a sáb), mínimo $500.000 por semana. Modo "diario": reparte el mínimo semanal entre los días de trabajo; los días con ganancia anotada usan lo real.
- **Tarjetas:** Visa Macro (cierre variable: se cargan las fechas del resumen; si falta, estima a 7 días hábiles) y Mercado Pago (cierra 5, vence 10). Cada gasto con tarjeta y cada cuota cae en el resumen que corresponde. `resumenes[clave].base` = lo que ya venía antes de usar la app; `.monto` = monto real que dice el resumen. Plata disponible = billeteras (Naranja X y Mercado Pago); calcula si alcanza sumando lo que cobra antes de cada vencimiento.
- **Inversiones:** saldos por cuenta (ordenadas de más a menos, en USD; IOL en pesos con el dólar cargado a mano), reparto contra el objetivo 50/30/20 y fotos mensuales.
- **Comida:** carnicería y verdulería con precio por unidad; "subió X%" desde el último cambio.
- **Auto:** GNC (cargas, promedio, por día de trabajo, % de lo que cobra, contra el tope), peajes, mantenimiento, seguro; últimos 6 meses; service y repuestos con link a la Libreta del Voyage.
- **Con qué se paga:** en la línea rápida, si no se aclara, va a efectivo/débito (`medioDefault`). Los fijos de tipo pago usan su medio (Seguro, Claro y Spotify: Visa; Claude: débito de Naranja X). `reglas` (pestaña Tarjetas, "Siempre con tarjeta"): palabras que se anotan con tarjeta solas (GNC, peajes, repuestos y PedidosYa: Visa; Mercado Libre: tarjeta de Mercado Pago). "Café" va con Comida y súper (Agus lo compra con el súper).
- Clave en el celu: `mis-finanzas-v1`. Gastos con `previo: true` son cuotas que ya venían de antes: cuentan en las tarjetas pero no en los gastos del mes.

## Cómo trabajar con Agus

- Todo lo que se le muestre va en español rioplatense. Solo quedan en inglés nombres de comandos y archivos.
- Agus no es programador: explicar en pocas líneas qué se va a hacer antes de un cambio grande y esperar su OK; los pasos que tenga que hacer él, uno por uno.
- Muchas veces trabaja desde el celu (sesión en la nube): no hay acceso a sus datos ni a otros repos.

## Cómo está hecha

App web instalable (PWA) que anda sin internet, en GitHub Pages. Este repo es **público**: nunca subir datos personales (el `.gitignore` bloquea los `.json` salvo el manifest).

- `index.html` tiene todo el HTML, CSS y JS. `manifest.webmanifest`, `sw.js`, `icons/`.
- Rutas relativas (la app vive en `agustincancilla.github.io/<repo>/`).
- Los datos viven solo en el `localStorage` del celu. Botón "Datos y copias" para descargar/cargar copias.
- Fechas con la fecha **local** (no `toISOString()`, que en Argentina después de las 21 h da el día siguiente).

## Al cambiar algo

1. Cambiar `index.html`.
2. **Subir `VERSION` en `sw.js`** (`v1` → `v2`…), si no el celu sigue mostrando la vieja.
3. Si cambia la forma de los datos, que `normalize()` siga aceptando las copias viejas: Agus tiene datos cargados que no se pueden perder.
4. Commit (terminar el mensaje con `Co-Authored-By: Claude Opus 5.5 <noreply@anthropic.com>`) y push a `main`.
5. Avisarle: el celu baja la versión nueva cuando tiene internet y la muestra **la segunda vez** que abre la app.
