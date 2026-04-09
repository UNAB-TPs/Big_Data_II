# Discurso — EDA v2: Hipótesis, paradojas y feature engineering

> **Contexto:** versión enfocada. Partimos de los hallazgos del v1 para testear hipótesis, cuantificar relaciones y preparar el dataset para los modelos.

---

## Introducción

"En esta segunda versión del EDA no exploramos de cero — llegamos con preguntas concretas. Teníamos cinco hipótesis que queríamos confirmar o refutar, y el objetivo final era dejar el dataset listo para las fases de clustering y predicción."

---

## Calidad de datos — imputación correcta

"Lo primero que hacemos en este notebook es resolver el problema de los nulos. Verificamos formalmente que los 495 nulos en `Primary_Condition` corresponden exactamente a los pacientes con cero condiciones crónicas, y los imputamos como 'None'. A partir de este momento, todos los análisis de condiciones incluyen a los 2.000 pacientes, sin pérdida silenciosa de datos."

---

## La cadena causal

"El hallazgo más importante del v1 era cualitativo. Acá lo cuantificamos. La correlación entre edad y cronicidad es r=0.80, con p < 0.001. La correlación entre cronicidad y facturación es r=0.43. Y la relación entre edad y facturación es r=0.35 — más débil que la cadena mediada. Esto confirma que la edad no impacta directamente el costo: lo hace a través del mecanismo de acumulación de condiciones crónicas. La cadena es `Edad → Cronicidad → Costo`, no un efecto directo. Visualmente, lo vemos también en que los puntos de alto costo son casi siempre los más crónicos, independientemente de la edad exacta."

---

## Paradoja Medicare — confirmada con ANOVA

"Testeamos formalmente si la diferencia de facturación entre los cuatro tipos de seguro es estadísticamente significativa. El ANOVA da F significativo, lo que confirma que las diferencias no son ruido. Pero la explicación sigue siendo la misma: Medicare se lleva la facturación más alta porque su población tiene en promedio 64 años y 1.1 condiciones crónicas, mientras los privados tienen 54 años y 1.02. No es el contrato de seguro, es el perfil del paciente."

---

## Paradoja preventiva — refutada con Mann-Whitney

"Aplicamos Mann-Whitney sobre seis variables comparando pacientes con y sin flag preventivo. El resultado es consistente: los marcados como preventivos tienen más condiciones crónicas, más visitas, y gastan más. El crosstab lo muestra claramente: entre los que tienen prevención, solo el 18.6% son sanos, contra el 30% entre los que no tienen prevención. La causalidad está invertida: no es que la prevención cause gasto, sino que los enfermos diagnosticados son quienes más interactúan con el sistema. El flag no mide prevención, mide *engagement* médico."

---

## Condiciones médicas

"El análisis por condición muestra que Hypertension y Diabetes son las condiciones más costosas, mientras que Anxiety y Depression son las más frecuentes pero con menor costo unitario. Esto tiene implicancias directas para estrategias de gestión de riesgo: los pacientes hipertensos o diabéticos son los que más impactan la facturación."

---

## Demografía y geografía

"En cuanto a género, las mujeres tienen un BMI promedio significativamente mayor (32.2 vs 29.2), y una facturación levemente superior. La diferencia existe, es estadísticamente significativa, pero el tamaño del efecto es pequeño — no es un driver relevante. Para el análisis geográfico, filtramos los estados con al menos 10 pacientes dado el 50% de Unknown en City, y encontramos que hay variabilidad estatal real aunque moderada."

---

## Feature Engineering

"En la sección final construimos seis variables derivadas pensadas para las fases siguientes. `Age_Group` agrupa en cuatro rangos etarios clínicamente relevantes. `BMI_Category` aplica la clasificación OMS. `Inactivity_Flag` marca a los pacientes que no visitaron el sistema en más de 180 días — el 50% del dataset. El `Risk_Score` es un índice compuesto normalizado donde la cronicidad pesa el doble que el resto, dando un número único entre 0 y 1 por paciente. `Annual_Cost_Estimated` multiplica la facturación por las visitas anuales para estimar el impacto económico total. Y `High_Value_Patient` es un booleano para los que están por encima del percentil 75 de costo anual — el cuartil más costoso del sistema."

---

## Conclusión y recomendaciones para modelado

"Salimos de este notebook con cinco conclusiones concretas. Uno: la cronicidad es el predictor más importante de costo. Dos: la edad importa, pero como causa upstream de la cronicidad. Tres: el tipo de seguro no predice costos — predice el perfil del paciente. Cuatro: `Preventive_Care_Flag` es un indicador de engagement médico, no de reducción de costos, y hay que tratarlo como tal en los modelos. Y cinco: tenemos dos pares de variables colineales que hay que resolver antes de entrenar — `Weight_kg`/`BMI` con r=0.84 y `Height_cm`/`BMI` con r=-0.54. La regla es: quedarse con BMI y descartar las otras dos. El dataset engineered está guardado y listo para la Fase 1."
