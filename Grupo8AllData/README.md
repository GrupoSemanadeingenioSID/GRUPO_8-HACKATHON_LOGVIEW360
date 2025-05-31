# Hackathon Banco del Occidente – Proyecto “All Data”

## Descripción
Este proyecto, desarrollado por el grupo 8 “All Data” durante la Hackathon del Banco del Occidente, aborda la problemática de identificar y predecir fallos en transacciones financieras a partir de sus logs. Se compone de dos fases principales:

## Video Presentacion
Link: https://youtu.be/fFlks13-ELc?si=kNkk-1heZIfTHTO3 

1. **Preprocesamiento de logs:**  
   - Estandarización de formatos  
   - Extracción y creación de variables (número de verificaciones, frecuencia de IP, etc.)  
   - Unificación de trazabilidad entre fases de rechazo y ejecución

2. **Modelo predictivo y evaluación:**  
   - División en conjuntos de entrenamiento y prueba  
   - Construcción de un pipeline de preprocesamiento + regresión logística  
   - Métricas de desempeño (reporte de clasificación, precisión ~71%, recall real ~88%)

## Estructura del notebook
1. **Problemática Hackathon**  
   Breve introducción al reto planteado por el banco y objetivos del equipo.

2. **Importando librerías**  
   - `pandas`, `numpy`, `re`, `pathlib`  
   - Componentes de scikit-learn:  
     - `Pipeline`, `ColumnTransformer`  
     - `OneHotEncoder`, `StandardScaler`  
     - `train_test_split`, `classification_report`  
     - `LogisticRegression`

3. **Convirtiendo formatos en una estructura homogénea**  
   - Lectura de archivos de logs  
   - Normalización de nombres de campos y tipos de datos

4. **Feature engineering**  
   - Cálculo de `n_verificaciones` a partir de listas de verificaciones realizadas  
   - Tabla de frecuencia de direcciones IP (`ip_freq`)

5. **Unificación de trazabilidad**  
   - Combinación de DataFrames:  
     - `rechazoInit`: transacciones rechazadas en SecuCheck  
     - `dfNoEx`: transacciones aceptadas pero no ejecutadas  
   - Generación de un único DataFrame consolidado

6. **Pipeline y modelo predictivo**  
   ```python
   preprocessor = ColumnTransformer([
       ('cat', OneHotEncoder(handle_unknown='ignore'), ['origen', 'operation']),
       ('num', StandardScaler(), ['n_verificaciones', 'ip_freq'])
   ])

   pipeline = Pipeline([
       ('pre', preprocessor),
       ('clf', LogisticRegression(max_iter=1000))
   ])
   pipeline.fit(X_train, y_train)
