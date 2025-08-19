# Informe de Resultados — Telecom X · Parte 2

## 1. Datos y preparación
- Filas: **7267** | Columnas: **21**
- Columnas numéricas: **3** | categóricas: **16**
- Split: **train/test 80/20** estratificado; oversampling: **No**

## 2. Selección de variables (resumen)
- Top 10 numéricas por correlación (abs):
  - customer.tenure: -0.3522
  - account.TotalCharges: -0.1995
  - account.MonthlyCharges: 0.1934
- Top 20 features por Información Mutua (ver gráfico en el notebook).

## 3. Modelado y evaluación
- **Mejor modelo (CV por PR-AUC)**: **gb** con **0.6612**
- En test: **ROC-AUC**=0.8481, **PR-AUC**=0.6639, **calibración**=isotonic

## 4. Umbral y negocio
- Threshold óptimo: **0.030** → Ganancia estimada: **31635.00** (VALUE_RETAIN=100.0, COST_CONTACT=5.0)

**Matriz de confusión (umbral negocio):**

|       | Pred No | Pred Yes |
|-------|---------|----------|
| Real No  | 313 | 722 |
| Real Yes | 3 | 371 |

## 5. Conclusiones
- Factores con mayor influencia alineados con la Parte 1 (contrato mes a mes, e-check, fibra).
- El umbral maximiza **ganancia**, priorizando recall si `VALUE_RETAIN` es alto respecto a `COST_CONTACT`.
- Recomendación: campañas por segmento con anualización, migración a auto-pay y soporte fibra proactivo.