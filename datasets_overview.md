# Descripción de Datasets — Big Data II

---

## 1. `1_diabetes_risk_dataset.csv`

**Descripción:** Datos clínicos y de estilo de vida de pacientes individuales orientados a la predicción de riesgo de diabetes.

**Variables (19):** `Patient_ID`, `age`, `gender`, `bmi`, `blood_pressure`, `fasting_glucose_level`, `insulin_level`, `HbA1c_level`, `cholesterol_level`, `triglycerides_level`, `physical_activity_level`, `daily_calorie_intake`, `sugar_intake_grams_per_day`, `sleep_hours`, `stress_level`, `family_history_diabetes`, `waist_circumference_cm`, `diabetes_risk_score`, `diabetes_risk_category`

**Target:** `diabetes_risk_category` (Low Risk / Prediabetes / High Risk)

### Posibilidades de análisis

| Tipo | Ideas |
|------|-------|
| **EDA** | Distribución de glucosa e HbA1c por categoría de riesgo · Correlación BMI/circunferencia de cintura con riesgo · Boxplots de biomarcadores por nivel de actividad física |
| **Regresión** | Predecir `diabetes_risk_score` (continuo) con regresión lineal/Ridge/Lasso |
| **Clasificación** | Predecir `diabetes_risk_category` con Random Forest, XGBoost, SVM · Identificar variables más importantes |

---

## 2. `2_pakistan_healthcare_dataset.csv`

**Descripción:** Métricas operacionales de hospitales en Pakistán (años 2022–2024), incluyendo infraestructura, personal, costos y calidad de atención.

**Variables (35):** Identificadores del hospital (`Hospital_ID`, `Name`, `Sector`, `City`, `Province`, `Hospital_Type`), capacidad (`Total_Beds`, `ICU_Beds`, `Operation_Theatres`), personal (`Total_Doctors`, `Specialists`, `Nurses`), indicadores de calidad (`Patient_Satisfaction_Score`, `Mortality_Rate`, `Infection_Rate`, `Readmission_Rate`), económicos (`Avg_Treatment_Cost`, `Consultation_Fee`, `Surgery_Cost`, `Government_Funding`), operacionales (`Daily_Outpatients`, `Monthly_Admissions`, `Waiting_Time_Minutes`), servicios (`Telemedicine_Service`, `Ambulance_Available`)

### Posibilidades de análisis

| Tipo | Ideas |
|------|-------|
| **EDA** | Comparativa público vs. privado en satisfacción y mortalidad · Distribución geográfica (ciudad/provincia) de capacidad hospitalaria · Correlación entre financiamiento y calidad |
| **Regresión** | Predecir `Patient_Satisfaction_Score` o `Mortality_Rate` a partir de recursos y personal · Modelar costo de tratamiento promedio |
| **Clustering** | Segmentar hospitales por perfil de recursos y rendimiento (K-Means, DBSCAN) |

---

## 3. `3_patient_segmentation_dataset.csv`

**Descripción:** Datos demográficos, clínicos y de utilización de servicios de pacientes en EE.UU., orientados a segmentación y gestión de pacientes crónicos.

**Variables (16):** `PatientID`, `Age`, `Gender`, `State`, `City`, `Height_cm`, `Weight_kg`, `BMI`, `Insurance_Type`, `Primary_Condition`, `Num_Chronic_Conditions`, `Annual_Visits`, `Avg_Billing_Amount`, `Last_Visit_Date`, `Days_Since_Last_Visit`, `Preventive_Care_Flag`

### Posibilidades de análisis

| Tipo | Ideas |
|------|-------|
| **EDA** | Distribución de condiciones por tipo de seguro · Frecuencia de visitas según número de enfermedades crónicas · Análisis de facturación por condición primaria |
| **Regresión** | Predecir `Avg_Billing_Amount` o `Annual_Visits` · Predecir `Days_Since_Last_Visit` (retención de pacientes) |
| **Clustering** | Segmentar pacientes por perfil clínico y de uso → diseño de programas de atención diferenciada |
| **Clasificación** | Predecir `Preventive_Care_Flag` (paciente adherente vs. no adherente) |

---

## 4. `4_student_health_data.csv`

**Descripción:** Datos de salud y bienestar de estudiantes universitarios, combinando métricas fisiológicas (biosensores) con autorreporte, hábitos y nivel de riesgo de salud.

**Variables (14 activas):** `Student_ID`, `Age`, `Gender`, `Heart_Rate`, `Blood_Pressure_Systolic`, `Blood_Pressure_Diastolic`, `Stress_Level_Biosensor`, `Stress_Level_Self_Report`, `Physical_Activity`, `Sleep_Quality`, `Mood`, `Study_Hours`, `Project_Hours`, `Health_Risk_Level`, `Family_members`

> ⚠️ **Nota:** El archivo contiene columnas vacías adicionales al final — limpiar antes de analizar.

### Posibilidades de análisis

| Tipo | Ideas |
|------|-------|
| **EDA** | Correlación entre estrés biosensor vs. autorreporte · Impacto de `Sleep_Quality` y `Physical_Activity` en `Health_Risk_Level` · Distribución de horas de estudio por nivel de riesgo y estado de ánimo |
| **Regresión** | Predecir `Stress_Level_Biosensor` a partir de horas de estudio, sueño y actividad |
| **Clasificación** | Predecir `Health_Risk_Level` (Low/Moderate/High) · Predecir `Mood` a partir de variables fisiológicas |
| **Análisis de discrepancia** | Brecha entre estrés percibido (self-report) y fisiológico (biosensor) como variable derivada |

---

## Resumen comparativo

| Dataset | Filas aprox. | Variables | Dominio | Target principal |
|---------|-------------|-----------|---------|-----------------|
| Diabetes Risk | ≥ 1,000 | 19 | Salud individual | `diabetes_risk_category` |
| Pakistan Healthcare | ≥ 500 | 35 | Gestión hospitalaria | `Patient_Satisfaction_Score` / `Mortality_Rate` |
| Patient Segmentation | ≥ 1,000 | 16 | Salud poblacional EE.UU. | Segmentación / `Avg_Billing_Amount` |
| Student Health | ~ 500 | 14 | Bienestar estudiantil | `Health_Risk_Level` |
