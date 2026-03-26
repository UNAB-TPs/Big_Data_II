# EDA v1 — Dataset 3: Patient Segmentation — Resumen de Celdas

**Versión completa:** 43 celdas de código + setup. Exploración exhaustiva de todas las variables sin hipótesis predefinidas.
**Objetivo:** entender la estructura completa, detectar anomalías y generar intuiciones iniciales.

---

## Setup y carga (Celdas 0-5)
- **Celda 0:** Título y descripción general del dataset (2000 pacientes, 16 variables, segmentación de pacientes US).
- **Celda 1-3:** Opciones de carga (local, Google Drive, upload).
- **Celda 4:** Import de librerías estándar (pandas, numpy, matplotlib, seaborn, plotly).
- **Celda 5:** Carga del CSV y resumen inicial de dimensiones (2000×16).

---

## Sección 1: Primera vista del dataset (Celdas 6-11)
- **Celda 6:** Encabezado — "Primera vista del dataset".
- **Celda 7:** Dimensiones, primeras 5 filas y tipos de datos básicos.
- **Celda 8:** Info detallado con conteos no-nulos por columna.
- **Celda 9:** Clasificación de columnas: numéricas vs categóricas vs datetime.
- **Celda 10:** Estadísticas descriptivas (describe) para variables numéricas.
- **Celda 11:** Estadísticas para categóricas (unique, count, top, freq).

---

## Sección 2: Calidad de datos (Celdas 12-17)
- **Celda 12:** Encabezado — "Valores nulos, duplicados y calidad".
- **Celda 13:** Tabla de nulos por columna: `Primary_Condition` 495 (24.75%), resto 0.
- **Celda 14:** Duplicados en filas (0) y por PatientID (0) — dataset limpio.
- **Celda 15:** Conteo de únicos por cualquier (PatientID, Age, Gender, etc.).
- **Celda 16:** Conteo de "Unknown" en State y City (City: 50.6%, State: 0%).
- **Celda 17:** Heatmap de nulos para visualizar patrón de faltantes.

---

## Sección 3: Distribuciones univariadas (Celdas 18-22)
- **Celda 18:** Encabezado — "Distribuciones univariadas".
- **Celda 19:** Grilla 2×3 histogramas (Age, BMI, Avg_Billing_Amount, Annual_Visits, Days_Since_Last_Visit, Weight_kg) con media/mediana.
- **Celda 20:** Conteplots 2×2 para categóricas (Gender, Insurance_Type, Primary_Condition, Num_Chronic_Conditions) con anotaciones de conteo.
- **Celda 21:** Countplot specifico para Preventive_Care_Flag (0 vs 1).
- **Celda 22:** Histograma de BMI con líneas OMS (18.5, 25, 30) y resumen de categorías (bajo peso, normal, sobrepeso, obesidad).

---

## Sección 4: Detección de outliers (Celdas 23-25)
- **Celda 23:** Encabezado — "Detección de outliers".
- **Celda 24:** Boxplots 1×5 para Age, BMI, Avg_Billing_Amount, Annual_Visits, Days_Since_Last_Visit (rojo para outliers detectados visualmente).
- **Celda 25:** Tabla con método IQR (Q1, Q3, IQR, límites, conteo de outliers). Resultado: Age/Annual_Visits/Days sin outliers; BMI 1 (0.05%); Billing 28 (1.40%).

---

## Sección 5: Análisis bivariado (Celdas 26-33)
- **Celda 26:** Encabezado — "Análisis bivariado".
- **Celda 27:** Boxplots 1×3: Billing vs Insurance_Type, vs Primary_Condition, vs Preventive_Care_Flag. Busca diferencias de costo por grupo.
- **Celda 28:** Boxplots 1×2: Annual_Visits vs Insurance_Type y vs Primary_Condition.
- **Celda 29:** Boxplot: BMI por Primary_Condition (ordenado por mediana) con líneas de corte (25, 30).
- **Celda 30:** Scatter Age vs Billing coloreado por Insurance_Type (puntos según tipo).
- **Celda 31:** Plotly scatter interactivo: BMI vs Num_Chronic_Conditions coloreado por Primary_Condition.
- **Celda 32:** Barplot horizontal: Facturación promedio por condición primaria (ordenado).
- **Celda 33:** Barplot: Visitas anuales promedio vs Num_Chronic_Conditions (relación clara).

---

## Sección 6: Matriz de correlación (Celdas 34-36)
- **Celda 34:** Encabezado — "Matriz de correlación".
- **Celda 35:** Heatmap de correlación de Pearson (triangular inferior, anotado, centrado en 0, colormap coolwarm).
- **Celda 36:** Tabla ordenada: top 10 correlaciones por |r|. Destaca Weight↔BMI (0.84), Age↔Num_Chronic (0.80), Height↔BMI (-0.54).

---

## Sección 7: Análisis por condición médica (Celdas 37-41)
- **Celda 37:** Encabezado — "Análisis por condición médica".
- **Celda 38:** Barplot horizontal: conteo de pacientes por condición primaria (Hypertension 210, Obesity 183, etc.).
- **Celda 39:** Tabla resumen: facturación, visitas, cronicidad, BMI, edad por condición (8 condiciones + "None").
- **Celda 40:** Crosstab heatmap: Primary_Condition vs Insurance_Type (matriz de conteos).
- **Celda 41:** Crosstab (porcentaje normalizado): Primary_Condition vs Preventive_Care_Flag; barplot stacked 100% (sin prevención vs con).

---

## Sección 8: Análisis de seguro y costos (Celdas 42-46)
- **Celda 42:** Encabezado — "Análisis de seguro y costos".
- **Celda 43:** Violin plot: Avg_Billing_Amount por Insurance_Type (distribuciones suave de densidad).
- **Celda 44:** Boxplot: Days_Since_Last_Visit por Insurance_Type.
- **Celda 45:** Barplot agrupado: Facturación promedio por Insurance_Type y Gender lado a lado.
- **Celda 46:** Pie chart: proporción de cada tipo de seguro con % anotados. Tabla de conteos.

---

## Sección 9: Prevención y comportamiento (Celdas 47-51)
- **Celda 47:** Encabezado — "Prevención y comportamiento".
- **Celda 48:** Tabla de medias: Facturación, Visitas, Días desde última visita por Preventive_Care_Flag (0 vs 1); barplots 1×3 lado a lado.
- **Celda 49:** Scatter interactivo: Days_Since_Last_Visit vs Annual_Visits coloreado por Preventive_Care_Flag. Busca relación actividad-preventiva.
- **Celda 50:** Histogramas superpuestos (alpha 0.5): distribución de Days_Since_Last_Visit por flag (compara patrones).
- **Celda 51:** Crea `Inactivity_Flag` (>180 días); crosstab y visualización de inactividad por tipo de seguro; pie chart de proporción general activos vs inactivos.

---

## Sección 10: Análisis demográfico (Celdas 52-54)
- **Celda 52:** Encabezado — "Análisis demográfico".
- **Celda 53:** Histogramas superpuestos 1×2: Age por Gender y BMI por Gender (compara distribuciones hombre vs mujer).
- **Celda 54:** Crea `Age_Group` (bins: 18-30, 31-45, 46-60, 60+); barplot de facturación promedio por grupo; resumen de distribución por grupo.

---

## Sección 11: Conclusiones preliminares (Celda 55)
- **Celda 55:** Markdown con 8 hallazgos clave:
  1. Estructura limpia, 495 nulos en Primary_Condition (=pacientes sin condición), 50.6% City unknown.
  2. Seguro: Medicare 45.3%, Private 27.3%, Medicaid 24.2%, Self-Pay 3.3%.
  3. Medicare facturación más alta ($4,458 vs Private $3,628) — NO por tipo sino por edad de beneficiarios.
  4. **Cronicidad es el driver:** 0 condiciones $2,598 → 3 condiciones $6,124. r(Age→Chronic)=0.80.
  5. Condiciones: Hypertension, Obesity, Anxiety más frecuentes; Hypertension ($4,690) y Diabetes ($4,680) más caras.
  6. **Prevención sin efecto:** preventivos gastan más ($4,096 vs $3,917) — sesgo de selección probable.
  7. Género: mujeres BMI más alto (32.2 vs 29.2), facturación levemente mayor.
  8. Inactividad: 50.6% pacientes >180 días sin visita, sin correlación clara con prevención.
  9. Próximos pasos: imputar nulos, clustering, regresión para predecir billing, clasificación de riesgo.

---

## Matriz de variables por celda

| Variable principal | Celdas | Tipo de análisis |
|--------------------|--------|------------------|
| `Age` | 10,19,22,30,36,53,54 | Univariado, bivar, correlación, demográfico |
| `BMI` | 19,22,24,25,29,53 | Univariado, outliers, by condition |
| `Avg_Billing_Amount` | 19,24,25,27,32,43,45,48 | Univariado, bivariado, por grupo |
| `Annual_Visits` | 19,24,28,33,36,48 | Univariado, por condición, por prevención |
| `Primary_Condition` | 11,15,20,27,29,31,38-40 | Distribución, bivariado, crosstab |
| `Num_Chronic_Conditions` | 10,15,20,24,33,36,51 | Univariado, outliers, bivar |
| `Insurance_Type` | 11,15,20,27,28,40,43,44,45,46 | Distributed, comparativa |
| `Preventive_Care_Flag` | 15,21,27,41,48,49,50,51 | Univariado, por condición, impacto |
| `Days_Since_Last_Visit` | 10,24,25,44,49,50,51 | Univariado, inactividad |
| `Gender` | 9,11,15,20,53 | Distribución, demographic |
| `State` / `City` | 11,15,16 | Calidad (Unknown %) |

---

## Resumen metodológico

- **Propósito:** exploración exhaustiva sin hipótesis previas.
- **Flujo:** calidad → univariado → bivariado → correlaciones → segmentación conceptual.
- **Herramientas:** box/violin plots, scatter, heatmaps, crosstabs, histogramas.
- **Hallazgo clave:** `Age → Num_Chronic → Avg_Billing` es la relación causal más fuerte (r=0.80 → r=0.43).
