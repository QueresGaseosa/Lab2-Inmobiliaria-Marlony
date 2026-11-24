# Lab 2 — Inmobiliaria Marlony

## Propósito

Construir en Power BI un tablero ejecutivo para que la administración de la inmobiliaria pueda entender qué tipos de propiedad y servicios generan más importe, cómo cambia la actividad por mes y sede, y cuáles son los segmentos de mayor aporte.

El análisis responde estas preguntas:

1. ¿Qué tipos de propiedad o servicios concentran mayor importe completado?
2. ¿Cómo cambia el número de operaciones por mes y sede?
3. ¿Qué segmentos generan más importe por categoría?

## Fuente y tablas

- Motor: PostgreSQL alojado en Neon.
- Base de datos: `inmobiliaria`.
- Tablas visibles en el navegador de Power BI: `public.catalogo`, `public.clientes`, `public.detalle_operacion`, `public.operaciones` y `public.sedes`.
- Las credenciales no se guardan en este repositorio.

## Revisión inicial de calidad de datos

Revisión hecha con las vistas y muestras compartidas. No se identifican valores vacíos ni IDs repetidos dentro de las tablas de catálogo, clientes, operaciones o sedes. Antes del modelado, aplicar estas verificaciones en Power Query:

- Asignar tipos: IDs como entero; importes, precios, descuentos y cantidades como número; fechas como fecha/hora según corresponda; nombres, categorías, estado, canal y segmento como texto.
- Para `clientes.fecha_registro` y `operaciones.fecha`, confirmar interpretación **día/mes/año** con configuración regional Español (El Salvador). Las fechas compartidas siguen ese orden.
- En `operaciones`, revisar el registro con `operacion_id = 85`: su fecha aparece como `1/8/2026`, entre operaciones del 25/8/2026 y 3/9/2026. Parece un dato fuera de secuencia; confirmar el valor correcto con la fuente antes de editarlo.
- En `detalle_operacion`, no quitar duplicados por `operacion_id`: una operación puede tener varias líneas de detalle. El identificador de línea es `detalle_id`.
- Confirmar qué representa `descuento` (importe por unidad, importe total de la línea o porcentaje) antes de calcular el importe neto. No asumir su fórmula.
- Revisar en la vista Modelo que las claves se relacionen correctamente: catálogo con detalle por `producto_id`; operaciones con detalle por `operacion_id`; clientes con operaciones por `cliente_id`; sedes con operaciones por `sede_id`.

### Tablas revisadas

- `catalogo`: seis productos; cuatro pertenecen a la categoría `Alquiler` y dos a `Servicio`. Los precios de lista son numéricos.
- `sedes`: tres sedes, con IDs 1–3.
- `clientes`: 30 filas; los segmentos `Ocasional`, `Nuevo` y `Frecuente` aparecen en la muestra. Las fechas abarcan de agosto a octubre de 2025.
- `operaciones`: 100 filas; se observan los estados `completada`, `pendiente` y `cancelada`. La operación 85 queda pendiente de validar por fecha.
- `detalle_operacion`: 133 líneas para 100 operaciones; hay varias operaciones con más de un producto/servicio asociado. Es una relación de detalle esperada, no evidencia por sí sola de duplicados.

## Diseño propuesto del dashboard

Una sola página ejecutiva, en formato panorámico, con filtros de periodo, sede, tipo de propiedad, servicio y estado. El análisis principal usa operaciones completadas.

| Zona | Visual | Qué permite ver |
|---|---|---|
| Encabezado | Título y filtros | Periodo, sede, propiedad, servicio y estado seleccionados |
| Indicadores | Tarjetas: importe completado, cantidad de operaciones completadas e importe promedio por operación | Tamaño del resultado y referencia rápida |
| Aporte por segmento | Barras horizontales por producto/tipo, separadas por categoría `Alquiler` o `Servicio` cuando sea legible | Qué tipos de propiedad o servicios concentran el importe |
| Evolución | Línea mensual de cantidad de operaciones, con una serie por sede | Tendencia y diferencias entre sedes a lo largo del tiempo |
| Cruce de categorías | Matriz con producto/tipo en filas, categoría en columnas e importe completado como valor; aplicar formato condicional | Qué combinaciones generan mayor importe |

Se priorizan barras, líneas y una matriz porque facilitan comparar importes, seguir cambios mensuales y detectar combinaciones destacadas. No se propone un gráfico de pastel: la comparación de varias categorías es más clara con barras.

## Definiciones de negocio por validar

- **Importe completado:** importe neto asociado a operaciones con estado `completada`, calculado después de confirmar cómo se aplica `descuento`.
- **Operaciones completadas:** conteo distinto de `operacion_id` filtrado al estado `completada`.
- **Importe promedio por operación:** importe completado dividido entre operaciones completadas.
- **Operaciones por mes y sede:** conteo distinto de operaciones agrupado por mes calendario y sede.
- **Segmento:** producto/tipo de catálogo y su categoría (`Alquiler` o `Servicio`).

No presentar cifras ni conclusiones hasta validar la fecha de la operación 85, la semántica del descuento y las relaciones del modelo en Power BI.

## Estética

Tomar como referencia los ejemplos compartidos: fondo oscuro azul petróleo o grafito, paneles en tonos pizarra, títulos en dorado suave y acentos turquesa y naranja. Mantener buen contraste, pocos colores y espacio suficiente para que las etiquetas se lean durante la presentación.

## Guion breve para presentar

> Este tablero resume el desempeño de las operaciones de la inmobiliaria. Primero mostramos el importe y el volumen de operaciones completadas. Las barras comparan el aporte por tipo de propiedad y servicio; así se identifica qué segmentos concentran más importe. La línea muestra cómo cambia el número de operaciones mes a mes en cada sede, lo que ayuda a detectar tendencias o diferencias entre ubicaciones. Finalmente, la matriz cruza el producto y la categoría para encontrar las combinaciones que más aportan. Los filtros permiten enfocar el análisis en un periodo o sede específica. Las conclusiones se basan en las cifras validadas en el modelo.

## Estado del trabajo

- [x] Conexión de Power BI Desktop con PostgreSQL/Neon.
- [x] Identificación de las cinco tablas.
- [x] Revisión inicial de datos de muestra; se marcó una fecha para confirmar.
- [ ] Inspeccionar columnas y tipos directamente en Power Query.
- [ ] Confirmar valor de descuento y relaciones del modelo.
- [ ] Crear medidas y validar que no dupliquen importes.
- [ ] Construir los visuales y revisar que los filtros interactúen correctamente.
- [ ] Incorporar hallazgos reales después de verificar el tablero.
