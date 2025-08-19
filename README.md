# Telecom X — Parte 2 · Predicción de Churn (Colab-only)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/flacoca1970/Desafio_2_P2/blob/main/notebooks/TelecomX_P2_colab.ipynb)
## 🎯 Propósito
Predecir qué clientes tienen **mayor probabilidad de cancelar** (churn) y **priorizar acciones de retención** con base en variables relevantes, maximizando la **ganancia neta** de las campañas.
## 📁 Estructura del proyecto
```
TelecomX_P2_repo/
├─ README_P2.md
├─ TelecomX_P2_colab.ipynb          # cuaderno único con todo el flujo
├─ data/
│  └─ df_limpo.csv                  # (cárgalo aquí o en /content en Colab)
└─ reports/
   ├─ README_REPORT_P2.md           # informe autogenerado (al ejecutar)
   ├─ metrics_p2.json               # métricas reales de tu corrida
   └─ clientes_en_riesgo_topN_p2.csv
```
> Usa el **mismo CSV tratado** de la Parte 1 (`df_limpo.csv`).
## 🧼 Preparación de datos
- **Clasificación de variables**
  - **Numéricas**: `MonthlyCharges`, `TotalCharges`, `tenure` (y equivalentes normalizados).
  - **Categóricas**: `Contract`, `PaymentMethod`, `InternetService`, etc.
- **Codificación/normalización**
  - `OneHotEncoder(handle_unknown="ignore")` para categóricas.
  - `StandardScaler` en numéricas.
  - `SimpleImputer` (mediana para numéricas, moda para categóricas).
- **Split**
  - `train/test` **estratificado** (80/20) **antes** de cualquier balanceo.
  - Balanceo opcional **solo en train** (`RandomOverSampler`/`SMOTE`) si es necesario.
- **Justificaciones**
  - **PR-AUC** como métrica principal por **desbalance**.
  - **Calibración** para probabilidades fiables.
  - **Umbral de negocio** que maximiza: `TP×VALUE_RETAIN − (TP+FP)×COST_CONTACT`.
## 🔎 EDA y selección de variables
- Correlación numérica ↔ target (point-biserial aprox).
- **Información mutua** para mixtas.
- (Opcional) Cramér’s V para categóricas ↔ target.
- Gráficos de barras con **top features**.
## 🤖 Modelos
- **Baseline**: Dummy estratificado.
- **Logistic Regression** (interpretable, `class_weight="balanced"`).
- **RandomForest** y **GradientBoosting** (y/o HistGradientBoosting).
- **GridSearchCV** (CV=5, selección por PR-AUC).
- **Calibración**: Isotónica vs Platt.
- **Evaluación**: ROC-AUC, PR-AUC, F1, matriz de confusión a umbral de negocio.
## 💸 Umbral de negocio
Se barre `t∈(0,1)` y se elige el que maximiza la ganancia con tus **parámetros de negocio** (`VALUE_RETAIN`, `COST_CONTACT`).
El cuaderno genera la **lista de clientes en riesgo (Top N)** para cargar en CRM.
## 🛠️ Cómo ejecutar
1. Abre el badge de Colab.
2. Sube `df_limpo.csv` a `/content` o edita `CSV_PATH` en el notebook.
3. Ejecuta todas las celdas. Al final se generan:
   - `reports/README_REPORT_P2.md`, `reports/metrics_p2.json`,
   - `reports/clientes_en_riesgo_topN_p2.csv`.

**Dependencias**: `pandas`, `numpy`, `matplotlib`, `scikit-learn (>=1.1)`, `imbalanced-learn` (opcional).
## 🧭 Conclusión estratégica (plantilla)
- Variables con influencia: **Contract (Month-to-month)**, **PaymentMethod (Electronic check)**, **InternetService (Fiber)**, **tenure** y **cargos** tienden a explicar mayor riesgo (confirmar con tus métricas).
- Recomendaciones:
  1) **Anualización** de contratos *Month-to-month* (oferta/upgrade).
  2) **Migración a auto-pay** desde *Electronic check*.
  3) **Soporte proactivo** para fibra (health-check técnico).
  4) **Onboarding** intensivo en tenures bajos.
  5) **Paquetes flex** para cargos altos sensibles a precio.
## 📌 Notas
- Balanceo **solo en train** (evitar fuga).
- Evitar imputar el target; descartar `Churn` no {{Yes/No}} para entrenar.
- Guardar semillas (`random_state`) para reproducibilidad.

---

© 2025 — Proyecto académico. Ajusta `VALUE_RETAIN` y `COST_CONTACT` según tu realidad.
