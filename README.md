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

Antes de escribir las medidas definitivas, se deben confirmar los nombres y tipos de las columnas, las claves entre tablas y cómo se identifica una operación completada. También hay que confirmar en qué tabla está el importe para evitar sumarlo dos veces si el total existe tanto en la operación como en su detalle.

## Diseño propuesto del tablero

Una sola página ejecutiva, en formato panorámico, con filtros de periodo, sede, tipo de propiedad, servicio y estado. El filtro de estado debe permitir analizar las operaciones completadas y comparar con otros estados cuando corresponda.

| Zona | Visual | Qué permite ver |
|---|---|---|
| Encabezado | Título y filtros | Periodo, sede, propiedad, servicio y estado seleccionados |
| Indicadores | Tarjetas: importe completado, cantidad de operaciones completadas e importe promedio por operación | Tamaño del resultado y referencia rápida |
| Aporte por segmento | Barras horizontales por tipo de propiedad, separadas por servicio cuando los campos lo permitan | Qué categorías concentran el importe y cuáles lideran |
| Evolución | Línea mensual de cantidad de operaciones, con una serie por sede | Tendencia y diferencias entre sedes a lo largo del tiempo |
| Cruce de categorías | Matriz con categoría de propiedad en filas, servicio en columnas e importe completado como valor; aplicar formato condicional | Qué combinaciones de categoría y servicio generan mayor importe |

Se priorizan barras, líneas y una matriz porque facilitan comparar importes, seguir cambios mensuales y detectar combinaciones destacadas. No se propone un gráfico de pastel: la comparación de muchas categorías es más clara con barras.

## Definiciones de negocio por validar

- **Importe completado:** suma del importe asociado a operaciones cuyo estado corresponde a completado.
- **Operaciones completadas:** conteo distinto del identificador de operación, filtrado al estado completado.
- **Importe promedio por operación:** importe completado dividido entre operaciones completadas.
- **Operaciones por mes y sede:** conteo distinto de operaciones agrupado por mes calendario y sede.
- **Segmento:** combinación de tipo de propiedad, servicio y categoría disponible en el catálogo.

Los nombres de columnas, el valor exacto del estado completado y las relaciones del modelo se documentarán después de inspeccionar los datos en Power Query y la vista Modelo. No se deben presentar cifras ni conclusiones hasta validar las medidas en Power BI.

## Estética

Tomar como referencia los ejemplos compartidos: fondo oscuro azul petróleo o grafito, paneles en tonos pizarra, títulos en dorado suave y acentos turquesa y naranja. Mantener buen contraste, pocos colores y espacio suficiente para que las etiquetas se lean durante la presentación.

## Guion breve para presentar

> Este tablero resume el desempeño de las operaciones de la inmobiliaria. Primero mostramos el importe y el volumen de operaciones completadas. Las barras comparan el aporte por tipo de propiedad y servicio; así se identifica qué segmentos concentran más importe. La línea muestra cómo cambia el número de operaciones mes a mes en cada sede, lo que ayuda a detectar tendencias o diferencias entre ubicaciones. Finalmente, la matriz cruza categoría y servicio para encontrar las combinaciones que más aportan. Los filtros permiten enfocar el análisis en un periodo o sede específica. Las conclusiones se basan en las cifras que queden validadas en el modelo.

## Estado del trabajo

- [x] Conexión de Power BI Desktop con PostgreSQL/Neon.
- [x] Identificación de las cinco tablas disponibles.
- [ ] Inspeccionar columnas, tipos de datos y valores de estado.
- [ ] Revisar y confirmar relaciones entre tablas.
- [ ] Crear medidas y validar que no dupliquen importes.
- [ ] Construir los visuales y revisar que los filtros interactúen correctamente.
- [ ] Reemplazar este estado por hallazgos reales después de verificar el tablero.
