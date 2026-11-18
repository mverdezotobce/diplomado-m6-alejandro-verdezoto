# Sistema de decisión para detección de fraude en DolarYa — Grupo 3

**Integrantes:** Grace Castillo, Santiago Segovia y Marcelo Verdezoto  
**Problema:** detección de transacciones fraudulentas.

## 1. Problema y datos

El objetivo es identificar transacciones potencialmente fraudulentas para apoyar la decisión de bloqueo. Se analizaron **2,996 transacciones**, con una tasa de fraude de **10.28%**. Se recalculó `ratio` como monto dividido para ingreso mensual y se creó `madrugada` para identificar transacciones entre las 00:00 y las 05:59.

## 2. Modelos comparados

Los modelos se evaluaron sobre el mismo conjunto de prueba. La exactitud balanceada de esta tabla utiliza un umbral de clasificación de 0.50. Un AUC y una exactitud balanceada más altos son mejores; un Brier más bajo es mejor.

| Modelo | AUC | Exactitud balanceada | Brier |
|---|---:|---:|---:|
| Random Forest | 0.902955 | 0.770359 | 0.046266 |
| Red Neuronal | 0.903911 | 0.743185 | 0.048513 |
| XGBoost | 0.890011 | 0.841967 | 0.051860 |
| Regresión Logística | 0.896490 | 0.834498 | 0.119253 |

**Modelo seleccionado para la explicación y decisión de negocio:** Random Forest.

## 3. Explicabilidad

El análisis SHAP se aplicó al modelo Random Forest. Las cinco variables con mayor importancia SHAP media en esta ejecución son: **dispositivo_nuevo, ratio, distancia_km, monto, intentos_pin**. SHAP describe la contribución de las variables a las predicciones del modelo; no implica causalidad.

La variabilidad de predicciones entre árboles se reporta como dispersión entre estimadores, no como intervalo de confianza estadístico.

## 4. Recomendación de negocio

El umbral se seleccionó usando el conjunto de validación, sin optimizarlo sobre el conjunto de prueba.

- **Umbral de bloqueo seleccionado:** 0.19.
- **Costo en el conjunto de prueba con el modelo:** USD 2,632.82.
- **Costo en el conjunto de prueba sin modelo:** USD 14,825.85.
- **Ahorro estimado en el conjunto de prueba:** USD 12,193.03.

La matriz de costos considera USD 5.00 por cada transacción legítima bloqueada y el monto de cada transacción fraudulenta no detectada como costo de falso negativo.

## 5. Limitaciones y próximos pasos

- Los datos del proyecto son simulados; los resultados no garantizan el mismo desempeño con transacciones reales.
- La evaluación económica depende de los costos asumidos para falsos positivos y falsos negativos.
- El umbral debe revisarse cuando cambien los patrones de fraude o los costos de operación.
- Antes de implementar el modelo en producción, se requiere validación con datos reales, monitoreo de deriva (data drift) y seguimiento de falsos positivos y falsos negativos.
