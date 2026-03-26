# EDA v2 — Dataset 3: Patient Segmentation — Resumen de Celdas

**Versión enfocada:** 13 celdas de código + setup. Parte de hallazgos del v1 para testear hipótesis y preparar features para modelado.
**Objetivo:** explicar paradojas (Medicare, prevención), validar la cadena causal y construir variables derivadas.

---

## Setup y carga (Celdas 0-3)
- **Celda 0:** Título y subtítulo: versión resumida, enfoque en hallazgos del v1 y feature engineering.
- **Celda 1-2:** Opciones de carga (local, Google Drive, upload) e instalar scipy + sklearn.
- **Celda 3:** Import de librerías (pandas, numpy, matplotlib, seaborn, plotly, scipy.stats, sklearn.preprocessing). Carga CSV, verifica duplicados (0) y resume nulos (solo Primary_Condition 495).

---

## Sección 1: Nulos intencionales y calidad (Celdas 4-6)
- **Celda 4:** Encabezado — "Nulos intencionales y calidad de datos". Explicación: 495 nulos en Primary_Condition coinciden exactamente con pacientes Num_Chronic=0. Son **intencionales**, no errores.
- **Celda 5:** Verificación: comprobación de coincidencia (100%), imputación como 'None', resumen de "Unknown" en City (50.6%), balance de género (1001F vs 999M = perfecto).
- **Celda 6:** Histogramas 2×3 de distribuciones univariadas (Age, BMI, Billing, Visits, Days_Since, Chronic) con línea de media roja punteada.

---

## Sección 2: La cadena principal — Edad→Cronicidad→Facturación (Celdas 7-8)
- **Celda 7:** Encabezado — "La cadena principal: Edad → Cronicidad → Facturación". La relación causal más fuerte: r(Age→Chronic)=0.80 → r(Chronic→Billing)=0.43.
- **Celda 8:** Tres gráficos lado a lado:
  1. Scatter Age vs Num_Chronic con línea de ajuste + r=0.797 anotado (Pearson).
  2. Boxplot Billing por nivel de cronicidad (0/1/2/3) + medias anotadas ($2,598 → $6,124).
  3. Scatter Age vs Billing coloreado por Num_Chronic + línea ajuste + r=0.350. Resumen tabular: conteos y medias por nivel de chronic.

---

## Sección 3: Seguro y costos — La paradoja Medicare (Celdas 9-10)
- **Celda 9:** Encabezado — "Tipo de seguro y costos — la paradoja Medicare". Hipótesis H1 (privado = más caro) refutada: Medicare factura más ($4,458 vs Private $3,628). Razón: Medicare cubre mayores con más cronicidad.
- **Celda 10:** Tres gráficos:
  1. Barplot facturación promedio por tipo (Medicare, Medicaid, Private, Self-Pay) con IC 95%.
  2. Barplot edad promedio por tipo (explica paradoja: Medicare mean=63.8 años).
  3. Barplot cronicidad promedio por tipo. Prueba ANOVA sobre billing (F, p-value). Tabla resumen: N, edad_media, cronicidad_media, billing_media por tipo.

---

## Sección 4: Condiciones médicas (Celdas 11-12)
- **Celda 11:** Encabezado — "Análisis de condiciones médicas".
- **Celda 12:** Tabla agregada por Primary_Condition (N, Facturación media, Edad media, BMI medio, Visitas medias) ordenada por costo descendente; dos gráficos:
  1. Barplot horizontal: facturación media por condición (colores gradiente rojo para alto costo).
  2. Bubble chart: Edad_media (X) vs Facturación_media (Y), tamaño=N pacientes, color=BMI_medio (amarillo-naranja-rojo).

---

## Sección 5: La paradoja preventiva (Celdas 13-14)
- **Celda 13:** Encabezado — "La paradoja preventiva (H3)".

  **EN PALABRAS SIMPLES:** Esperábamos que los pacientes con "prevención" gastaran MENOS. Pero gastan MÁS ($4,096 vs $3,917). ¿Por qué? Porque el flag no mide "buenos hábitos preventivos" sino "el paciente fue al médico y le diagnosticaron una enfermedad". Los enfermos (diagnosticados) son los que hacen seguimiento preventivo, por eso gastan más. Es causalidad invertida.

  **Analogía:** Es como decir "los pacientes en el hospital gastan más que los que no van". Claro, ¡porque el hospital atiende a los más enfermos! No es que el hospital cause gastos, sino que los gastos atraen a los enfermos.

- **Celda 14:** Matriz 2×3 de boxplots comparando Flag=0 vs Flag=1:
  1. Avg_Billing_Amount (paradoja: +$179 con prevención).
  2. Annual_Visits (visitas anuales).
  3. Days_Since_Last_Visit (cuántos días sin ir al médico).
  4. Age (edad).
  5. Num_Chronic_Conditions (número de enfermedades crónicas).
  6. BMI (peso relativo).

  **La evidencia:** Crosstab muestra distribución de enfermedades crónicas (0/1/2/3) por flag:
  - **Con prevención=0:** 30% sanos (0 crónicas) + 50.6% con 1 crónica.
  - **Con prevención=1:** 18.6% sanos + 57.3% con 1 crónica + 13.5% con 3 crónicas.

  **Conclusión clara:** Los "preventivos" tienen MÁS ENFERMEDADES DIAGNOSTICADAS. No son personas más saludables que hicieron prevención. Son enfermos que fueron al médico, fueron detectados, y ahora están en seguimiento. Por eso gastan más.

---

## Sección 6: Análisis demográfico — Género y geografía (Celdas 15-16)
- **Celda 15:** Encabezado — "Análisis demográfico: género y geografía".
- **Celda 16:** Tres gráficos:
  1. Histogramas superpuestos: Billing por Gender (Female rosa $4,100 vs Male azul $3,900); p-value Mann-Whitney anotado.
  2. Boxplot: BMI por Gender (Female mean=32.2 vs Male mean=29.2) con p-value.
  3. Barplot horizontal: Top 12 estados por facturación media (orden descendente), línea punteada roja con media global.

---

## Sección 7: Correlaciones y colinealidad (Celdas 17-18)
**¿Para qué?** Identificar qué variables están relacionadas linealmente entre sí y cuáles son redundantes en modelado. Evitar multicolinealidad: si Weight_kg y BMI son casi lo mismo (r=0.84), incluir ambas en un modelo confunde los coeficientes — hay que elegir una.

- **Celda 17:** Encabezado — "Correlaciones y colinealidad".
- **Celda 18:** Dos gráficos:
  1. Heatmap triangular inferior de correlaciones (anotadas con valores 2 decimales, colormap RdBu centrado): muestra visualmente todos los pares. Valores cercanos a ±1 (rojos/azules intensos) son relaciones fuertes.
  2. Barplot horizontal: Top 12 correlaciones por |r| (colores azul=positiva, rojo=negativa). Líneas grises puntedas en ±0.3 (umbral de "correlación moderada"). Alertas: Weight↔BMI (0.84) y Age↔Chronic (0.80) son las más altas → descartar una en cada par para modelado.

---

## Sección 8: Feature Engineering (Celdas 19-20)
**¿Para qué?** Crear variables nuevas y más útiles a partir de las originales: agrupar edad en categorías administrativas, clasificar BMI según OMS, detectar pacientes inactivos, construir un score de riesgo compuesto. Estas nuevas variables facilitan clustering (Fase 1) y predicción, porque capturan conceptos que el modelo entiende mejor.

- **Celda 19:** Encabezado — "Feature Engineering — preparación para Fases 1 y 2".
- **Celda 20:** Crea 6 variables derivadas y las visualiza en grilla 2×3:
  1. **Age_Group:** Categorías etarias estándar [<35, 35-55, 55-70, 70+] → útil para clustering por grupo demográfico.
  2. **BMI_Category:** Clasificación OMS [Bajo, Normal, Sobrepeso, Obeso] → convierte variable continua en clases clínicas interpretables.
  3. **Inactivity_Flag:** Booleano si >180 días sin visita → identifica pacientes desenganchados.
  4. **Risk_Score:** Índice compuesto normalizado (pesos: Edad 0.2, BMI 0.2, Cronicidad 0.4, Inactividad 0.2) → score único 0-1 que resume riesgo multi-dimensional.
  5. **Annual_Cost_Estimated:** Billing × Visits → costo anual total (proxy de demanda).
  6. **High_Value_Patient:** Booleano si costo anual ≥ P75 → segmentación de pacientes "high-touch".

  6 gráficos + tabla resumen muestran distribuciones y estadísticas de cada variable nueva para validar que capturan lo esperado (ej: Risk_Score aumenta con Age_Group).

---

---

## Sección 9: Conclusiones del EDA v2 (Celdas 21-22)
- **Celda 21:** Markdown con tabla de hipótesis (3 columnas: H#, Resultado, Evidencia):

| Hipótesis | Resultado | Evidencia y significado |
|-----------|-----------|------------------------|
| **H1: Seguro privado = mayor costo** | ❌ REFUTADA | **Medicare factura MÁS ($4,458) que Privado ($3,628).** ¿Por qué? Porque Medicare atiende mayores de 65 (edad media 63.8 años vs 54 en Privado) con más enfermedades crónicas (1.11 vs 1.02). No es el tipo de seguro, sino quién lo usa. El "culpable" es la edad/cronicidad, no el seguro. |
| **H2: Cronicidad → mayor costo** | ✅ CONFIRMADA | **Relación clara y progresiva:** 0 crónicas $2,598 → 1 crónica $3,999 → 2 crónicas $5,058 → 3 crónicas $6,124. Diferencia de $3,526 entre sanos y crónicos severos. **Predictor principal de costo:** este es el factor que más importa para explicar facturación. |
| **H3: Prevención reduce costos** | ❌ REFUTADA (sesgo de selección) | **Preventivos gastan +$179 más ($4,096 vs $3,917).** Pero NO porque la prevención sea ineficaz. Es porque el flag `Preventive_Care_Flag=1` NO mide "personas con buenos hábitos preventivos", sino **"pacientes diagnosticados que están en seguimiento médico"**. Comparación de cronicidad: 30% sanos sin prevención vs 18.6% sanos con prevención. Los preventivos son MÁS ENFERMOS. **Causalidad invertida:** enfermedad detectada → acceso a prevención, no prevención → menos enfermedad. |
| **H7: Desigualdad de género significativa** | ⚠️ LEVE | **Mujeres gastan levemente más ($4,100 F vs $3,900 M, diferencia $200).** BMI femenino mayor (32.2 F vs 29.2 M, diferencia 3 puntos). Pero las diferencias NO son dramaticas. p-value Mann-Whitney: probable que sea significativo estadísticamente pero **el tamaño práctico es pequeño** (5% de diferencia). No es un driver importante. |
| **H8: Edad es el principal driver de costo** | ✅ CONFIRMADA | **Edad → Cronicidad (r=0.80, correlación muy fuerte).** A mayor edad, más enfermedades diagnosticadas. Y cronicidad → costo (r=0.43). Entonces: edad NO impacta directamente costo, sino MEDIADA por cronicidad. La cadena es: `Edad (+años) → Crónicas diagnosticadas (+enferm) → Costo (+$)`. **Edad es el upstream driver**, cronicidad es el mecanismo. |

---

**RESUMEN INTERPRETATIVO DE HALLAZGOS:**

1. **El verdadero culpable de los costos altos es la cronicidad** — no el tipo de seguro, no el género.
   - Medicare no paga más por "ser privado": paga más porque sus beneficiarios son mayores/más enfermos.
   - Mujeres/hombres gastan similar: pequeñas diferencias.

2. **La "prevención" NO reduce costos en este dataset** — porque el flag captura enfermos diagnosticados, no comportamiento preventivo real.
   - Si una variable se llama "Preventive_Care" pero los que la tienen son MÁS enfermos, entonces NO es un indicador de prevención.
   - Reinterpretación: es un indicador de "acceso al sistema de salud / engagement con médicos".

3. **La edad es importante, pero indirectamente** — su efecto sobre costos se debe a que trae más cronicidad.
   - No es "envejecer cuesta dinero", sino "envejecer + enfermedades crónicas = cuesta dinero".

4. **Para modelado predictivo de costos:**
   - **Variable más importante:** `Num_Chronic_Conditions` (r=0.43 directo con billing).
   - **Segundo nivel:** `Age` (mediador de cronicidad).
   - **Tercero:** `Annual_Visits` (r=0.36).
   - **NO usar:** Género (diferencia pequeña), `Preventive_Care_Flag` como indicador de ahorro (sesgo).

---

  También tabla "Estructura de datos para modelado" con target variables, predictores y R² esperado (Billing regresión: ~0.35–0.50; Clustering 3–4 clusters; Inactivity clasificación TBD).

  **Alertas para Fase 1:**
  - ⚠️ **Colinealidad:** `Weight_kg` ↔ `BMI` (r=0.84), `Height_cm` ↔ `BMI` (r=-0.54) → usar solo BMI en modelos, descartar peso y altura.
  - ⚠️ **Sesgo en `Preventive_Care_Flag`:** No usar como "indicador de reducción de costos". Reinterpretarlo como "engagement con sistema" o excluir.
  - ⚠️ **City:** 50.6% "Unknown" → excluir análisis geográfico, usar solo `State`.
  - ⚠️ **Imputación de `Primary_Condition`:** Los 495 nulos (pacientes sanos) ya están imputados como "None".

- **Celda 22:** Guardar dataset engineered `3_patient_segmentation_engineered.csv` con shape y lista de columnas nuevas (Age_Group, BMI_Category, Inactivity_Flag, Risk_Score, Annual_Cost_Estimated, High_Value_Patient).

---

## Matriz de variables por celda

| Variable | Celdas | Análisis |
|----------|--------|----------|
| `Age` | 6,8,16 | Cadena causal, boxplot gender, top states |
| `Num_Chronic_Conditions` | 6,8,14 | Cadena causal, por flag preventiva |
| `Avg_Billing_Amount` | 6,10,12,14,16 | Paradoja Medicare, por condición, por flag |
| `Primary_Condition` | 12 | Bubble chart, tabla stats |
| `Insurance_Type` | 10 | Paradoja Medicare demostrada |
| `Preventive_Care_Flag` | 14 | Paradoja preventiva tests |
| `Gender` | 16 | Histogramas+boxplot billing/BMI |
| `State` | 16 | Top 12 estados |
| Derivadas en 20 | 20 | Age_Group, BMI_Cat, Inactivity, Risk, Cost, High_Value |

---

## Resumen de cambios v1 → v2

| Aspecto | v1 | v2 |
|--------|----|----|
| Celdas | 56 | 23 |
| Código | 43 | 13 |
| Enfoque | Exhaustivo | Hipótesis-driven |
| Validaciones | Univariado/bivariado | Causalidad + feature eng. |
| Output | Exploración | Paradojas refutadas + vars para modelado |
| CSV final | Nada | `*_engineered.csv` con 6 vars derivadas |

---

## Hallazg princiales

1. **Cadena causal confirmada:** Age (r=0.80)→ Chronic (r=0.43)→ Billing. Es el principal driver.
2. **Paradojas refutadas:**
   - Medicare NO paga más por ser "privado" ← es población mayor/crónica.
   - Prevención NO reduce costos ← **SESGO DE SELECCIÓN**: el flag no mide "buenos hábitos", mide "el paciente fue diagnosticado y está en seguimiento médico". Los enfermos diagnosticados gastan más. La causalidad es inversa.
3. **Colinealidad identificada:** Weight+BMI, Height+BMI para excluir en modelado.
4. **Features listos:** 6 variables derivadas para clustering, regresión, clasificación en Fase 1.
