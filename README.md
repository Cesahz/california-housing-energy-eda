# California Housing & Energy Market — Análisis Exploratorio de Datos

Proyecto de Análisis Exploratorio de Datos (EDA) sobre dos dominios distintos: el mercado inmobiliario de California y el mercado eléctrico español.

El desafío plantea una pregunta central: *¿qué hace que las casas sean tan caras en California?* y una segunda misión de profundización sobre patrones en el mercado energético. Este repositorio contiene dos notebooks independientes que abordan cada dataset con el mismo rigor: exploración de características, limpieza y preprocesamiento, análisis simple y análisis complejo.

## Datasets utilizados

| Dataset | Fuente | Descripción |
|---|---|---|
| California Housing Prices | [Kaggle](https://www.kaggle.com/datasets/camnugent/california-housing-prices/data) | Precios de vivienda en California (1990), a nivel de bloque censal |
| Spanish Electricity Market | [Kaggle](https://www.kaggle.com/datasets/manualrg/spanish-electricity-market-demand-gen-price/data) | Precio, demanda y generación eléctrica diaria en España (2014–2018) |

## Estructura del repositorio

```
├── data/
│   ├── raw/          # Datasets originales, sin modificar
│   ├── processed/    # Datasets curados (post-limpieza)
│   └── recursos/      # Material de apoyo y documentación
├── notebooks/
│   ├── 1_0-california-housing-eda.ipynb
│   └── 2.0-energy-market-eda.ipynb
├── outputs/            # Gráficos y reportes exportados
├── requirements.txt
└── README.md
```

## Notebook 1 — California Housing

Análisis exploratorio del dataset de precios de vivienda en California, a nivel de bloque censal.

**Proceso:** exploración de características → detección y tratamiento de nulos y techos artificiales → feature engineering (ratios estructurales) → correlaciones y pairplot → visualización geográfica → segmentación por proximidad al océano.

**Hallazgos principales:**
- El ingreso medio del vecindario (`median_income`) es el predictor individual más fuerte del precio, con una correlación de Pearson de 0.65.
- La distribución geográfica del precio no es lineal: existen dos clusters de alto valor claramente definidos (Área de la Bahía de San Francisco y costa de Los Ángeles), invisibles al análisis de correlación simple pero evidentes en el mapa geográfico.
- Se detectaron techos artificiales en `median_house_value`, `housing_median_age` y `median_income`. Solo se filtró el de la variable objetivo, ya que las otras dos son predictoras con correlación baja — el costo de eliminar registros no justificaba el beneficio.
- Tras la limpieza, los ratios derivados (`rooms_per_household`, `bedrooms_per_room`) mostraron correlaciones significativamente más altas con el ingreso, evidenciando que los outliers ocultaban relaciones reales.

## Notebook 2 — Spain Energy Market

Análisis de series temporales del mercado eléctrico español, con foco en la relación entre generación renovable y precio.

**Proceso:** aislamiento de señales (estructura longitudinal por ID) → verificación de calidad e integridad temporal → distribuciones univariadas → segmentación por día/mes/estación → correlación y regresión eólica-precio.

**Hallazgos principales:**
- La generación eólica tiene una correlación de -0.50 con el precio SPOT — la relación más fuerte del dataset — consistente con el principio de Orden de Mérito del mercado eléctrico.
- La demanda sigue patrones cíclicos claros (caída en fines de semana, picos en invierno y verano) pero correlaciona débilmente con el precio (0.31): el precio lo determina la disponibilidad de energía renovable, no el nivel de consumo.
- Primavera es la estación con el precio promedio más bajo del año (38.79 €/MWh), no por menor demanda, sino por ser la época de mayor generación eólica.

## Herramientas

Python · Pandas · NumPy · Matplotlib · Seaborn · Jupyter Notebook

## Instalación

```bash
git clone https://github.com/Cesahz/california-housing-energy-eda.git
cd california-housing-energy-eda
pip install -r requirements.txt
jupyter notebook
```