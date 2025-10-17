# Pump-It-Competion
Este proyecto implementa un pipeline completo de Machine Learning en Python: desde la exploración de datos (EDA) y la limpieza, hasta la ingeniería de variables, entrenamiento de modelos, evaluación con métricas robustas y explicabilidad de resultados.

# Predicción del estado de bombas de agua en Tanzania

Proyecto de Machine Learning para predecir el estado operativo de puntos de agua (waterpoints) en Tanzania a partir de variables de ubicación, instalación, gestión y características del agua. Los datos provienen de Taarifa y del Ministerio de Agua de Tanzania. 
DrivenData, más información https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/data/

🎯 Objetivo

Clasificar cada bomba en una de tres categorías:

functional

functional needs repair

non functional
Estas etiquetas son las utilizadas en el problema oficial. 
DrivenData

🧠 Datos y variables

El conjunto de datos incluye decenas de atributos por waterpoint. Algunos destacados:

Ubicación y administración: region, region_code, district_code, lga, ward, subvillage, scheme_management, management, public_meeting, permit.

Geometría y demografía: longitude, latitude, gps_height, population.

Instalación y construcción: funder, installer, construction_year, date_recorded.

Extracción y fuente: extraction_type, extraction_type_group, extraction_type_class, source, source_type, source_class.

Calidad y cantidad de agua: water_quality, quality_group, quantity, quantity_group.

Tipo de punto de agua: waterpoint_type, waterpoint_type_group. 
DrivenData

Contexto del origen de los datos (Taarifa): plataforma open source de reporte/triage de incidencias de infraestructuras que agrega información del Tanzania Ministry of Water. 
DrivenData

📦 Estructura del repositorio
├─ notebooks/
│  └─ pump_it_up_modeling.ipynb          # cuaderno principal del proyecto
├─ data/
│  ├─ raw/                                # CSVs originales de DrivenData
│  └─ processed/                          # datos limpios / features
├─ models/                                # artefactos (pkl) del mejor modelo
├─ figures/                               # gráficos exportados
├─ src/
│  ├─ features.py                          # limpieza + ingeniería de variables
│  ├─ train.py                             # entrenamiento + validación
│  └─ infer.py                             # predicción y generación de submission
└─ README.md

🔁 Flujo del proyecto

EDA & Data Quality: inspección de nulos, cardinalidad categórica, coordenadas, años inconsistentes.

Limpieza: normalización de texto, tratamiento de categorías raras, imputación y recodificaciones coherentes.

Ingeniería de variables: derivadas de tiempo (antigüedad de la bomba), agregaciones por región, group/class simplificados, binning para variables muy sesgadas.

Modelado: baseline lineal y modelos por árboles (e.g., Gradient Boosting/Random Forest), con validación cruzada y early stopping cuando aplique.

Selección: se elige el modelo con mejor rendimiento de validación interna y robustez (shift por región/año).

Explicabilidad: importancias (permutation) y curvas parciales para features clave.

Generación de envío (submission).

🧪 Métrica y evaluación

La competición se evalúa con la métrica indicada en la página oficial; el proyecto incluye utilidades locales para monitorizar accuracy/F1 como referencia durante el desarrollo. Para ver el criterio de evaluación vigente, consulta el sitio de la competición. 
DrivenData
+1

📤 Formato de submission

El archivo a enviar es un CSV con dos columnas: id y status_group (la predicción para cada fila). Ejemplo mínimo:

id,status_group
50785,functional
51630,functional
17168,functional
45559,functional


Este formato es el requerido por el concurso. 
DrivenData

▶️ Cómo reproducir

Entorno

conda create -n pumps python=3.10 -y
conda activate pumps
pip install -r requirements.txt


Datos
Descarga los datos desde la página del reto y colócalos en data/raw/. (Necesitas unirte al reto para acceder a las descargas). 
DrivenData

Preprocesado + features

python -m src.features --input data/raw --output data/processed


Entrenamiento

python -m src.train --data data/processed --out models/


Predicción y CSV de envío

python -m src.infer --model models/best.pkl --data data/processed/test.csv \
  --out submissions/submission.csv

🧰 Dependencias (sugeridas)
python>=3.10
pandas, numpy, scikit-learn, lightgbm, xgboost
category_encoders, scipy, matplotlib, seaborn
shap (opcional para explicabilidad)

✅ Buenas prácticas aplicadas

Validación estratificada por región/estado de la bomba para evitar sobreajuste geográfico.

Pipelines de scikit-learn (preprocesado + modelo) con semillas fijas.

Reporte de feature importance y análisis de estabilidad por folds.

Scripts y cuadernos reproducibles para facilitar revisión.

📎 Referencias

Resumen del reto y procedencia de los datos (Taarifa + Ministerio de Agua). 
DrivenData

Detalle de features, etiquetas y formato de submission. 
DrivenData

Contexto sobre Taarifa (dashboard y descripción).
