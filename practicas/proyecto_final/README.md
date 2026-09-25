# Proyecto final · Frecuencia temporal en el LGd del ratón

**Pareja 5** · Jenny Michelle Serna Merchan · Carol Tatiana Corrales Roa
**Área:** LGd (núcleo geniculado lateral dorsal, tálamo visual)
**Comparación asignada:** frecuencia temporal de rejillas en movimiento, 1 Hz vs 15 Hz

## Pregunta

¿Qué fracción de neuronas del LGd responde a rejillas en movimiento, y es esa fracción mayor
a 15 Hz que a 1 Hz?

## Resultados principales

| Medida | Resultado | IC 95 % jerárquico |
|---|---|---|
| Eje común: rejillas en movimiento vs espontáneo | 11,6 % (35/301) | 5,0 – 29,8 % |
| Responden a 1 Hz (dirección preferida) | 4,7 % (14/301) | 1,4 – 13,2 % |
| Responden a 15 Hz (dirección preferida) | 6,0 % (18/301) | 2,5 – 14,5 % |
| Diferencia 15 Hz − 1 Hz | +1,3 pp · p = 0,45 · d = 0,68 | −2,1 a 5,2 |

Hay una tendencia a favor de 15 Hz, consistente con la predicción, pero no es distinguible de
cero. Un intervalo que ignora al ratón (t-test) es entre 2 y 3,4 veces más estrecho que el
jerárquico.

## Contenido de esta carpeta

| Archivo | Qué es |
|---|---|
| `pipeline_LGd.ipynb` | Cuaderno con todo el análisis, de la descarga de los datos a las figuras |
| `figuras/` | Cada panel de las Figuras 1 y 2 en PNG |
| `poster_LGd.pdf` | Póster final (A0) |

## Cómo reproducir

1. Abrir `pipeline_LGd.ipynb` en Google Colab.
2. *Entorno de ejecución → Ejecutar todas*.

El cuaderno descarga los datos solo (archivo del LGd, unos 35 MB, con `gdown`); no hace falta
subir nada. El bootstrap de 10 000 réplicas puede tardar varios minutos.

Los datos no se incluyen en el repositorio porque no son nuestros (ver *Datos*).

## Estructura del cuaderno

| Sección | Qué hace | Panel |
|---|---|---|
| Descarga y configuración común | Carga los datos y define, una sola vez, índices de ensayos, umbral y funciones de conteo | — |
| Pasos 1 a 6 (sesión L6) | Z-score con la condición preferida y fracción respondedora por ratón | — |
| Eje común | Fracción que responde a rejillas en movimiento vs espontáneo | 2A |
| Comparación 1 vs 15 Hz | Z en la dirección preferida dentro de cada frecuencia | 2D |
| Neurona ejemplo | Forma de onda, raster y PSTH | 1C, 1D |
| Validación del umbral | Rasters de neuronas cerca de Z = 2,5 y tabla de sensibilidad | Métodos |
| Población | PSTH por clase de estímulo y mapa de calor | 2B, 2C |
| Estadística | Bootstrap jerárquico, permutación, d de Cohen y t-test ingenuo | 2E |
| Punto extra | Histograma de frecuencia temporal preferida | Extra |

## Decisiones de análisis

- Cada neurona se analiza solo con los ensayos de su propio ratón.
- Se excluyen los barridos en blanco de las rejillas en movimiento.
- **Ventana:** de 0 a la duración real de cada ensayo (~2 s). La basal sale de trozos de 2 s
  de pantalla gris; para estímulos de 0,25 s se usa basal en ventanas de 0,25 s.
- **Z-score:** (tasa evocada media − basal media) / DE de la basal, en la condición preferida
  de cada neurona.
- **Umbral:** Z ≥ 2,5, el común del curso. Es conservador (hay falsos negativos visibles en
  los rasters). Con umbrales de 1,5 a 3,0, la ventaja de 15 Hz se mantuvo (0,3 a 1,3 pp).
- **Estadística:** bootstrap jerárquico de tres niveles (ratón → neurona → ensayo, 10 000
  réplicas); valor p por permutación pareada a nivel de neurona; d de Cohen a nivel de ratón
  (n = 5).

## Uso del modelo de lenguaje

Usamos un modelo de lenguaje (Claude) para generar y revisar el código de cada paso, dándole
primero la estructura del archivo de datos. Errores que detectamos y corregimos:

1. La fracción del eje común se calculaba promediando todas las combinaciones de rejillas
   (2,0 %) en lugar de usar la condición preferida (11,6 %), como exige el enunciado.
2. La comparación 1 vs 15 Hz usaba una ventana de 0,5 s, que cubre solo medio ciclo a 1 Hz
   y sesgaba el resultado a favor de 15 Hz.
3. Varias celdas redefinían las mismas variables con ventanas distintas, y una incluía los
   barridos en blanco, así que el resultado dependía del orden de ejecución.

Verificamos cada paso con los controles del enunciado, con datos sintéticos de respuesta
conocida y comprobando que el bootstrap sorteara los ratones.

## Datos

Siegle JH et al. (2021). Survey of spiking in the mouse visual system reveals functional
hierarchy. *Nature* 592, 86–92. Datos del Allen Brain Observatory, Visual Coding Neuropixels.
