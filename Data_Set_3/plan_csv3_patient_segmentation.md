# Plan de Analisis — Dataset 3: Patient Segmentation

## Descripcion del dataset

Datos demograficos, clinicos y de utilizacion de servicios de salud de pacientes en EE.UU. 16 variables que cubren perfil del paciente, condicion medica, seguro, facturacion y comportamiento de visitas. Cada fila es un paciente individual.

**Variables clave:**
- Demograficas: `PatientID`, `Age`, `Gender`, `State`, `City`
- Antropometricas: `Height_cm`, `Weight_kg`, `BMI`
- Seguro: `Insurance_Type` (Private, Medicare, Medicaid)
- Clinicas: `Primary_Condition` (Arthritis, Depression, Asthma, Hypertension, Anxiety, Heart Disease, COPD, Obesity, None), `Num_Chronic_Conditions`
- Utilizacion: `Annual_Visits`, `Avg_Billing_Amount`, `Last_Visit_Date`, `Days_Since_Last_Visit`
- Prevencion: `Preventive_Care_Flag` (0/1)

---

## Hipotesis a investigar

### H1 — El tipo de seguro determina el costo
Los pacientes con seguro privado tienen facturacion promedio mayor que Medicare/Medicaid, independientemente de su condicion clinica.

### H2 — Cronicidad = mayor costo y mayor uso
Pacientes con mas condiciones cronicas (`Num_Chronic_Conditions`) visitan mas frecuentemente al medico y generan mayor facturacion.

### H3 — Adherencia a prevencion reduce costos
Pacientes con `Preventive_Care_Flag = 1` tienen menor facturacion y menos dias desde la ultima visita (estan mas activos en el sistema).

### H4 — Perfiles de paciente diferenciados
Existen segmentos claros de pacientes que se pueden agrupar por combinacion de edad, cronicidad, seguro y uso de servicios — y cada segmento requiere estrategias de atencion distintas.

### H5 — Obesidad como multiplicador
Pacientes con BMI alto (>30) tienen mayor numero de condiciones cronicas y mayor costo, especialmente en combinacion con ciertas condiciones primarias.

### H6 — Riesgo de abandono
Pacientes con alto `Days_Since_Last_Visit` y bajo `Preventive_Care_Flag` representan un segmento de riesgo de abandono del sistema de salud.

### H7 — Desigualdad por genero
Existen diferencias significativas en facturacion, frecuencia de visitas o condiciones primarias entre hombres y mujeres.

### H8 — Edad como driver principal
La edad es el predictor mas fuerte de costo, cronicidad y frecuencia de visitas, por encima del tipo de seguro o la condicion primaria.

---

## Preguntas de investigacion

1. Que factores determinan el costo de atencion de un paciente?
2. Existen segmentos naturales de pacientes y cuales son sus perfiles?
3. La prevencion reduce costos? Cuanto?
4. Que condiciones primarias son las mas costosas?
5. Que perfil de paciente tiene mayor riesgo de abandono del sistema?
6. El BMI esta asociado a mayor carga de enfermedad cronica?
7. Hay desigualdad en facturacion o acceso segun genero o tipo de seguro?
8. Se puede predecir que pacientes necesitan intervencion proactiva?

---

## Fase 0: EDA (Analisis Exploratorio de Datos)

### 0.1 Carga y limpieza
- Importar librerias (pandas, numpy, matplotlib, seaborn, plotly)
- Cargar CSV, revisar shape, dtypes, head
- Verificar nulos, duplicados, valores unicos por columna
- Analizar proporcion de "Unknown" en `State` y `City`
- Parsear `Last_Visit_Date` a datetime
- Estadisticas descriptivas para numericas y categoricas

### 0.2 Distribuciones univariadas
- Histogramas: `Age`, `BMI`, `Avg_Billing_Amount`, `Annual_Visits`, `Days_Since_Last_Visit`
- Countplots: `Gender`, `Insurance_Type`, `Primary_Condition`, `Preventive_Care_Flag`
- Distribucion de `Num_Chronic_Conditions`
- Deteccion de outliers en `BMI`, `Avg_Billing_Amount`

### 0.3 Analisis bivariado
- Boxplots: `Avg_Billing_Amount` por `Insurance_Type`, por `Primary_Condition`, por `Preventive_Care_Flag`
- Scatter: `Age` vs `Avg_Billing_Amount`, `BMI` vs `Num_Chronic_Conditions`
- Barplots: visitas anuales promedio por condicion, costo promedio por tipo de seguro
- Heatmap de correlacion (variables numericas)

### 0.4 Analisis de condiciones
- Frecuencia de cada `Primary_Condition`
- Cronicidad promedio por condicion
- Condiciones mas costosas (ranking por `Avg_Billing_Amount`)
- Cruce condicion x seguro

### 0.5 Analisis de comportamiento
- Distribucion de `Days_Since_Last_Visit` — identificar pacientes inactivos
- Relacion entre `Preventive_Care_Flag` y frecuencia de visitas
- Patron temporal de visitas (mes de `Last_Visit_Date`)

---

## Fase 1: Primer analisis (Descriptivo-Comparativo)

**Objetivo:** Responder H1, H2, H3 y H7 con tests estadisticos.

### 1.1 Comparativa por tipo de seguro
- ANOVA / Kruskal-Wallis para `Avg_Billing_Amount` entre Private, Medicare, Medicaid
- Post-hoc (Tukey/Dunn) para identificar pares con diferencia significativa
- Controlar por condicion y edad (ANCOVA si aplica)
- Tabla resumen con medias, medianas, intervalos de confianza, p-values

### 1.2 Efecto de la cronicidad
- Correlacion `Num_Chronic_Conditions` con `Avg_Billing_Amount` y `Annual_Visits`
- Segmentar por niveles de cronicidad (0, 1, 2, 3+) y comparar costos
- Regresion simple: costo ~ cronicidad + edad

### 1.3 Prevencion vs. no prevencion
- Test de hipotesis: `Avg_Billing_Amount` entre Flag=0 y Flag=1
- Comparar `Annual_Visits` y `Days_Since_Last_Visit` entre grupos
- Calcular ahorro estimado asociado a prevencion

### 1.4 Analisis de genero
- Comparar costos, visitas y condiciones por genero
- Chi-cuadrado para distribucion de condiciones por genero
- Deteccion de desigualdades significativas

### 1.5 Feature engineering
- Crear `BMI_Category` (bajo peso, normal, sobrepeso, obeso)
- Crear `Age_Group` (joven, adulto, adulto mayor)
- Crear `Risk_Score` = f(Num_Chronic_Conditions, BMI, Age, Days_Since_Last_Visit)
- Crear `Inactivity_Flag` = 1 si Days_Since_Last_Visit > umbral (ej. 180 dias)

---

## Fase 2: Analisis profundo (Segmentacion y Modelado)

**Objetivo:** Segmentar pacientes y predecir costos y comportamiento. Responder H4, H5 y H8.

### 2.1 Clustering — Segmentacion de pacientes
- Seleccion de variables: `Age`, `BMI`, `Num_Chronic_Conditions`, `Annual_Visits`, `Avg_Billing_Amount`, `Days_Since_Last_Visit`, `Preventive_Care_Flag`
- Estandarizacion (StandardScaler)
- Metodo del codo + silhouette score para K optimo
- K-Means clustering
- Alternativa: clustering jerarquico (dendrograma)
- Perfilado de cada cluster: edad media, condiciones predominantes, costo promedio, tipo de seguro, adherencia
- Nombres descriptivos para cada segmento (ej: "Cronico costoso", "Joven saludable", "Abandonador")

### 2.2 Regresion — Predecir `Avg_Billing_Amount`
- Variables predictoras: Age, BMI, Num_Chronic_Conditions, Annual_Visits, Insurance_Type, Primary_Condition, Preventive_Care_Flag
- Encoding de categoricas (OneHotEncoder / TargetEncoder)
- Modelos: Linear Regression, Ridge, Random Forest, XGBoost, Gradient Boosting
- Cross-validation (5-fold), metricas: R2, RMSE, MAE
- Feature importance: que variables determinan el costo

### 2.3 Clasificacion — Predecir `Preventive_Care_Flag`
- Que perfil de paciente adhiere a cuidados preventivos?
- Modelos: Logistic Regression, Random Forest, XGBoost
- Metricas: accuracy, precision, recall, F1, AUC-ROC
- Analisis de umbral optimo (curva precision-recall)
- Feature importance: que los diferencia

### 2.4 Clasificacion — Predecir riesgo de inactividad
- Target: `Inactivity_Flag` (derivado de Days_Since_Last_Visit > 180)
- Modelos de clasificacion
- Identificar pacientes en riesgo de abandonar el sistema
- Curva ROC y analisis de costo-beneficio de intervenir

---

## Fase 3: Analisis avanzado (Modelos complejos, interpretabilidad y valor clinico)

**Objetivo:** Maxima profundidad analitica. Responder H6 y generar valor actionable.

### 3.1 Survival Analysis — Tiempo hasta proxima visita
- Modelar `Days_Since_Last_Visit` como tiempo hasta evento
- Kaplan-Meier por segmento de paciente, por condicion, por seguro
- Cox Proportional Hazards: que factores aceleran o retrasan la proxima visita
- Identificar grupos con mayor hazard de abandono

### 3.2 Multi-target classification
- Predecir simultaneamente `Primary_Condition` + `Preventive_Care_Flag` + `Inactivity_Flag`
- Classifier chains vs. modelos independientes
- Evaluar si predecir condicion mejora la prediccion de comportamiento

### 3.3 Analisis de valor del paciente (CLV adaptado a salud)
- Estimar costo proyectado por paciente: `Avg_Billing_Amount` x `Annual_Visits` x anos esperados en el sistema
- Segmentar por valor: alto costo/alta frecuencia vs. bajo costo/baja frecuencia
- Identificar pacientes "prevenibles" vs. pacientes con costo estructural
- ROI estimado de intervenciones preventivas por segmento

### 3.4 Interpretabilidad avanzada
- SHAP values para el modelo de costos: explicaciones globales e individuales
- Partial Dependence Plots: como cambia el costo predicho al variar edad, BMI, cronicidad
- LIME para explicar predicciones individuales de pacientes extremos
- Analisis de interacciones: BMI x Condicion, Edad x Seguro

### 3.5 Modelo de recomendacion de intervencion
- Dado el perfil del paciente, que intervencion se recomienda?
  - Cronico inactivo -> reenganche proactivo
  - Alto BMI sin prevencion -> programa preventivo
  - Alto costo con baja satisfaccion -> revision de tratamiento
- Clasificar pacientes en acciones sugeridas basado en clustering + reglas de negocio
- Estimar impacto economico de implementar las recomendaciones

### 3.6 Analisis de fairness
- El modelo de costos predice igual de bien para hombres y mujeres?
- Metricas de equidad: disparate impact, equal opportunity
- Desglose de errores por genero, seguro y condicion
- Ajustes si se detecta sesgo

---

## Librerias necesarias

```
pandas, numpy, matplotlib, seaborn, plotly
scipy (tests estadisticos)
scikit-learn (modelos, clustering, preprocessing, metrics)
xgboost
shap
lime
lifelines (survival analysis)
imbalanced-learn (si hay desbalance)
fairlearn (analisis de fairness)
```

---

## Entregables esperados por fase

| Fase | Output |
|------|--------|
| EDA | Notebook con visualizaciones, distribucion de condiciones, perfiles demograficos |
| Fase 1 | Tests estadisticos, tablas comparativas, feature engineering |
| Fase 2 | Segmentos de pacientes perfilados, modelos de costo y adherencia |
| Fase 3 | Survival analysis, SHAP/LIME, CLV por segmento, modelo de recomendacion, fairness audit |
