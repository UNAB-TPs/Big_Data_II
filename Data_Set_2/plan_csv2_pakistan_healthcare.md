# Plan de Analisis — Dataset 2: Pakistan Healthcare

## Descripcion del dataset

Metricas operacionales de hospitales en Pakistan (2022-2024). 35 variables que cubren infraestructura, personal, costos, calidad de atencion y servicios. Cada fila es un hospital en un anio determinado.

**Variables clave:**
- Identificacion: `Hospital_ID`, `Hospital_Name`, `Sector` (Public/Private), `City`, `Province`, `Hospital_Type`, `Year`
- Infraestructura: `Total_Beds`, `ICU_Beds`, `Operation_Theatres`, `Emergency_Department`, `Diagnostic_Labs`, `Medical_Equipment_Score`
- Personal: `Total_Doctors`, `Specialists`, `Nurses`, `Paramedical_Staff`, `Doctor_Patient_Ratio`
- Operacion: `Daily_Outpatients`, `Monthly_Admissions`, `Surgeries_Per_Month`, `Average_Length_of_Stay`, `Emergency_Cases`
- Economico: `Average_Treatment_Cost`, `Consultation_Fee`, `Surgery_Cost`, `Government_Funding`
- Calidad: `Patient_Satisfaction_Score`, `Mortality_Rate`, `Infection_Rate`, `Readmission_Rate`
- Acceso: `Waiting_Time_Minutes`, `Distance_from_City_Center`, `Ambulance_Available`, `Telemedicine_Service`, `Rural_Patients_Percentage`

---

## Hipotesis a investigar

### H1 — Brecha publico vs. privado
Los hospitales privados tienen mejor satisfaccion del paciente, menor mortalidad y menor tasa de infeccion que los publicos, pero a un costo significativamente mayor.

### H2 — Recursos determinan calidad
Hospitales con mayor ratio doctor/paciente, mas camas ICU y mejor puntaje de equipamiento medico tienen mejores outcomes (menor mortalidad, menor readmision).

### H3 — Desigualdad geografica
Las provincias mas desarrolladas (Punjab, Sindh) tienen hospitales mejor equipados y con mejores indicadores de calidad que Balochistan y KPK.

### H4 — El financiamiento publico impacta en resultados
Hospitales con mayor financiamiento gubernamental no necesariamente presentan mejores outcomes — la eficiencia importa mas que el monto bruto.

### H5 — Acceso rural vs. urbano
Hospitales mas alejados del centro de la ciudad atienden mayor porcentaje de pacientes rurales y tienen peores indicadores por falta de recursos especializados.

### H6 — Evolucion temporal
Los indicadores de calidad mejoraron entre 2022 y 2024 a nivel general, pero la brecha publico-privado se mantuvo o se amplio.

### H7 — Telemedicina como factor diferencial
Hospitales con servicio de telemedicina muestran menor tiempo de espera y mayor satisfaccion, especialmente en zonas rurales.

---

## Preguntas de investigacion

1. Cual es la brecha real de calidad entre hospitales publicos y privados en Pakistan?
2. Que variables predicen mejor la satisfaccion del paciente?
3. Existe una relacion costo-efectividad: pagar mas implica mejores resultados clinicos?
4. Como varia la distribucion de recursos entre provincias?
5. El financiamiento gubernamental se traduce en mejores outcomes?
6. Que perfil de hospital tiene la mortalidad mas baja?
7. La telemedicina y ambulancias mejoran los indicadores en zonas rurales?
8. Que tan eficientes son los hospitales en funcion de sus recursos vs. resultados?

---

## Fase 0: EDA (Analisis Exploratorio de Datos)

### 0.1 Carga y limpieza
- Importar librerias (pandas, numpy, matplotlib, seaborn, plotly)
- Cargar CSV, revisar shape, dtypes, head
- Verificar nulos, duplicados, valores unicos por columna
- Estadisticas descriptivas (describe) para numericas y categoricas
- Detectar outliers con IQR y boxplots

### 0.2 Distribuciones univariadas
- Histogramas de variables numericas clave: `Mortality_Rate`, `Patient_Satisfaction_Score`, `Average_Treatment_Cost`, `Total_Beds`
- Countplots de categoricas: `Sector`, `Province`, `Hospital_Type`, `Year`
- Distribucion de variables binarias: `Emergency_Department`, `Telemedicine_Service`, `Ambulance_Available`

### 0.3 Analisis bivariado
- Boxplots: `Mortality_Rate` por `Sector`, por `Province`, por `Hospital_Type`
- Scatter: `Total_Doctors` vs `Patient_Satisfaction_Score`, `Government_Funding` vs `Mortality_Rate`
- Barplots agrupados: satisfaccion promedio por sector y provincia
- Heatmap de correlacion completa (variables numericas)

### 0.4 Analisis temporal
- Lineplot: evolucion de `Mortality_Rate`, `Satisfaction_Score`, `Infection_Rate` por anio
- Comparativa temporal publico vs. privado

### 0.5 Analisis geografico
- Barplots por provincia: camas, doctores, mortalidad, satisfaccion
- Proporcion publico/privado por provincia

---

## Fase 1: Primer analisis (Descriptivo-Comparativo)

**Objetivo:** Responder H1, H3 y H5 con evidencia estadistica.

### 1.1 Comparativa Publico vs. Privado
- Test de hipotesis (t-test / Mann-Whitney) para cada KPI de calidad entre sectores
- Calculo de effect size (Cohen's d)
- Tabla resumen con medias, medianas, p-values

### 1.2 Analisis por provincia
- ANOVA / Kruskal-Wallis para comparar KPIs entre provincias
- Post-hoc (Tukey/Dunn) para identificar que provincias difieren
- Radar chart comparativo por provincia

### 1.3 Perfil rural vs. urbano
- Segmentar hospitales por `Distance_from_City_Center` y `Rural_Patients_Percentage`
- Comparar indicadores entre hospitales urbanos vs rurales
- Efecto de `Ambulance_Available` y `Telemedicine_Service` en zonas rurales

### 1.4 Feature engineering
- Crear `Nurse_to_Doctor_Ratio`, `ICU_per_Bed`, `Cost_per_Admission`, `Staff_Total`
- Crear `Efficiency_Index` = Satisfaction / (Treatment_Cost normalizado)
- Crear `Resource_Score` combinando camas, ICU, equipamiento, personal

---

## Fase 2: Analisis profundo (Modelado predictivo)

**Objetivo:** Predecir KPIs de calidad y entender que factores los determinan. Responder H2 y H4.

### 2.1 Regresion — Predecir `Patient_Satisfaction_Score`
- Variables predictoras: infraestructura, personal, costos, servicios, ubicacion
- Modelos: Linear Regression, Ridge, Lasso, Random Forest Regressor, XGBoost
- Feature importance: que variables impactan mas la satisfaccion
- Cross-validation (5-fold), metricas: R2, RMSE, MAE

### 2.2 Regresion — Predecir `Mortality_Rate`
- Mismo pipeline pero con mortalidad como target
- Comparar si los drivers de mortalidad son distintos a los de satisfaccion
- Analisis de residuos y diagnostico del modelo

### 2.3 Clasificacion — Predecir `Sector` (Publico/Privado)
- Puede un modelo distinguir hospitales publicos de privados solo por sus metricas?
- Random Forest, Logistic Regression, SVM
- Metricas: accuracy, precision, recall, F1, AUC-ROC
- Feature importance: que los diferencia realmente

### 2.4 Clustering — Segmentacion de hospitales
- Seleccion de variables para clustering (estandarizar)
- Metodo del codo y silhouette score para K optimo
- K-Means y comparar con DBSCAN
- Perfilar cada cluster: que tipo de hospital cae en cada grupo
- Cruzar clusters con Sector y Provincia

---

## Fase 3: Analisis avanzado (Eficiencia, causalidad y longitudinal)

**Objetivo:** Tecnicas avanzadas para H4, H6 y H7. Maxima complejidad metodologica.

### 3.1 Analisis de eficiencia (DEA - Data Envelopment Analysis)
- Inputs: `Total_Beds`, `Total_Doctors`, `Government_Funding`, `Medical_Equipment_Score`
- Outputs: `Patient_Satisfaction_Score`, 1/`Mortality_Rate`, `Monthly_Admissions`
- Calcular eficiencia relativa de cada hospital
- Identificar hospitales benchmark y hospitales ineficientes
- Analizar si publicos o privados son mas eficientes dado sus recursos

### 3.2 Analisis longitudinal (Panel Data)
- Estructurar datos como panel: Hospital_ID x Year
- Modelo de efectos fijos: controlar por caracteristicas no observadas del hospital
- Evaluar si la adopcion de telemedicina entre 2022-2024 mejoro outcomes
- Evaluar tendencia temporal de mortalidad y satisfaccion controlando por hospital

### 3.3 Inferencia causal — Propensity Score Matching
- Pregunta: Cual es el efecto causal de ser hospital privado sobre la mortalidad?
- Calcular propensity score (probabilidad de ser privado dado recursos y ubicacion)
- Matching: emparejar hospitales publicos y privados con perfil similar
- Estimar el Average Treatment Effect (ATE) del sector sobre mortalidad y satisfaccion
- Analisis de sensibilidad (Rosenbaum bounds)

### 3.4 Multi-output prediction
- Predecir simultaneamente `Mortality_Rate` + `Satisfaction_Score` + `Infection_Rate`
- MultiOutputRegressor con base XGBoost
- Comparar con modelo individual por target
- Analizar correlaciones entre errores de prediccion (los outcomes estan relacionados?)

### 3.5 Analisis de interacciones y efectos no lineales
- SHAP values para interpretar modelos complejos a nivel global e individual
- Partial Dependence Plots (PDP) para variables clave
- Detectar interacciones: Sector x Province, Funding x Hospital_Type
- Identificar hospitales anomalos (residuos extremos) y analizar por que

---

## Librerias necesarias

```
pandas, numpy, matplotlib, seaborn, plotly
scipy (tests estadisticos)
scikit-learn (modelos, clustering, preprocessing)
xgboost
shap
statsmodels (panel data, regresion)
pyDEA o linprog de scipy (DEA)
causalinference o dowhy (PSM)
```

---

## Entregables esperados por fase

| Fase | Output |
|------|--------|
| EDA | Notebook con visualizaciones, estadisticas descriptivas, hallazgos iniciales |
| Fase 1 | Tests de hipotesis, tablas comparativas, conclusiones estadisticas |
| Fase 2 | Modelos entrenados, metricas, feature importances, clusters perfilados |
| Fase 3 | Eficiencia DEA, panel data, PSM con ATE estimado, SHAP plots |
