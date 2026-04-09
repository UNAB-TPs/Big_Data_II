# Discurso — EDA v1: Exploración exhaustiva del Dataset 3

> **Contexto:** exploración exhaustiva, sin hipótesis previas. El objetivo era conocer el dataset de cero.

---

## Introducción

"Arrancamos con un dataset de 2.000 pacientes de Estados Unidos, con 16 variables que incluyen datos demográficos, tipo de seguro, condiciones médicas, visitas y facturación. El primer paso fue entender la estructura antes de hacer cualquier supuesto."

---

## Calidad de datos

"Lo primero que encontramos fue una anomalía: 495 valores nulos en `Primary_Condition`. Pero en vez de tratarlos como error, los cruzamos con `Num_Chronic_Conditions` y confirmamos que coinciden exactamente con los pacientes que tienen cero condiciones crónicas. Es decir, los nulos son intencionales — representan pacientes sanos sin diagnóstico primario. El dataset también tiene un 50% de valores 'Unknown' en la ciudad, lo cual nos obliga a trabajar a nivel de estado para análisis geográfico. Llamativamente, el género está perfectamente balanceado: 1001 mujeres y 999 hombres, algo que sugiere un dataset sintético o controlado."

---

## Distribuciones

"Al explorar las distribuciones univariadas vemos que la edad se distribuye uniformemente entre 18 y 90 años. El BMI tiene una media de alrededor de 30, lo que ubica a la población en promedio en sobrepeso/obesidad según la OMS. La facturación media es de unos $4.000 por episodio, con algunos outliers en el extremo alto."

---

## Análisis bivariado — el hallazgo central

"Cuando cruzamos variables, aparece la relación más fuerte del dataset: a mayor edad, más condiciones crónicas. La correlación de Pearson es de 0.80, lo cual es altísimo para datos reales — de hecho, es el par más correlacionado de todo el dataset. Y a mayor cronicidad, mayor facturación: pasamos de $2.598 en pacientes sanos a $6.124 en pacientes con tres condiciones crónicas. Esto define la cadena causal central."

---

## Paradoja Medicare

"Algo que llama la atención: Medicare tiene la facturación más alta del dataset, superando a los seguros privados. La hipótesis inicial era que el seguro privado pagaría más. Pero cuando miramos la edad promedio de los asegurados por Medicare — 63 años contra 54 de privado — se explica todo. Medicare cubre una población más vieja y más enferma. No es el tipo de seguro lo que importa, sino quién lo usa."

---

## Paradoja preventiva

"Otro hallazgo contraintuitivo: los pacientes marcados con `Preventive_Care_Flag=1` gastan más, no menos. Prima facie parecería que la prevención no sirve. Pero la explicación es otra: ese flag no mide buenos hábitos preventivos, mide que el paciente fue al médico, fue diagnosticado, y ahora está en seguimiento. Los enfermos son los que más acceden al sistema. No hay paradoja real, hay sesgo de selección."

---

## Cierre

"Al final de esta exploración tenemos un mapa claro del dataset: la cronicidad es el driver principal de costos, la edad es su causa upstream, y variables como el tipo de seguro o el flag preventivo explican menos de lo que parecen. Estos hallazgos alimentan la versión 2 del análisis."
