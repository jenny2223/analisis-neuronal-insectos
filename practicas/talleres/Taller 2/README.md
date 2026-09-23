# Taller 2 · De la neurona a la figura

Pipeline completo de spike sorting sobre registros extracelulares sintéticos de una sonda
lineal de 16 canales, desde la señal cruda hasta una figura de resultados con estadística
inferencial.

**Datos:** `taller2_limpia.h5` (registro sin estímulos, para calibrar el pipeline) y
`taller2_ruidosa.h5` (registro con actividad multiunitaria, artefactos y tres condiciones
de estímulo A/B/C).

## Qué hace el pipeline

1. **Lectura y filtrado** — carga del `.h5`, conversión a µV, filtro pasa-banda 300-5000 Hz
   (`sosfiltfilt`).
2. **Detección** — umbral por canal (mediana/MAD), fusión de cruces en ventana de 0.5 ms,
   canal pico.
3. **Extracción de características** — amplitud, media anchura del valle (con interpolación),
   centroide de profundidad, por evento.
4. **Clustering** — K-means (k=16) sobre las características estandarizadas.
5. **Control de calidad** — 7 métricas tipo bombcell (picos/valles, duración, pendiente
   espacial, razón pico/valle, refractario, disparos faltantes, n° de disparos).
6. **Curación** — fusión de grupos sobre-divididos (correlograma cruzado + comparación de
   forma de onda) y etiquetado en `good` / `mua` / `non-somatic` / `noise`.
7. **Verificación contra la verdad sintética** — precisión, exhaustividad, F1 y fracción de
   disparos identificables individualmente, antes y después de curar.
8. **Análisis por estímulo** — raster, PSTH, Z-score, t-test pareado e intervalos de
   confianza por bootstrap para cada unidad `good`.
9. **Figura final** de 4 paneles (ubicación + forma de onda, raster, PSTH poblacional,
   swarmplot) siguiendo las reglas de figura científica.

## Resultado principal

De 15 grupos obtenidos con K-means, 5 se clasificaron como `good` (5046 disparos), lo que
elevó la fracción de disparos identificables individualmente de 0.650 a 0.956 frente a la
verdad sintética (12/15 etiquetas correctas). Dos unidades mostraron selectividad clara por
condición de estímulo (detalle en `Hoja_de_respuestas` dentro del cuaderno).

## Archivos

| Archivo | Contenido |
|---|---|
| `Taller2_<apellido>.ipynb` | cuaderno completo, secciones 1 a 12 |
| `../../prompts/Taller2_prompts.md` | bitácora de prompts de este taller |

## Cómo correrlo

Abrir el cuaderno en Google Colab y subir `taller2_limpia.h5` y `taller2_ruidosa.h5` a
`/content/` antes de ejecutar.
