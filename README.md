# Detección de Ataques DoS en Kubernetes (Slowloris y Torshammer)

Este proyecto implementa un sistema de detección de intrusiones (IDS) basado en Machine Learning para identificar ataques de Denegación de Servicio (DoS) 
tipo Slowloris y Torshammer en entornos de Kubernetes. Utiliza el dataset `Kubernetes Intrusion Detection Datasets` y modelos como XGBoost y Regresión Logística, complementado con 
explicabilidad SHAP y la integración de un LLM para interpretaciones en lenguaje natural.

## 1. Descripción del Proyecto

El objetivo principal es construir y evaluar modelos de clasificación capaces de distinguir entre actividad normal y dos tipos específicos de ataques DoS 
en un clúster de Kubernetes: Slowloris y Torshammer. Se aborda el preprocesamiento de datos, la reducción de dimensionalidad con PCA, el entrenamiento y evaluación 
de modelos de alto rendimiento, y la explicabilidad del modelo mediante SHAP, con interpretaciones generadas por un modelo de lenguaje grande (LLM).

[Conoce como ejecutar el proyecto](https://github.com/DariethSV/final_project_IA/blob/main/docs/guia_de_usuario.md)

[Video](https://www.canva.com/design/DAHKc_8rLtw/GxAyOcq0L0QBugWsyR71SA/watch?utm_content=DAHKc_8rLtw&utm_campaign=designshare&utm_medium=link2&utm_source=uniquelinks&utlId=h0e5fe00e02)

## 2. Dataset

El proyecto utiliza el dataset `Kubernetes Intrusion Detection Datasets`, obtenido de Kaggle. Este dataset contiene métricas de observabilidad de un clúster Kubernetes bajo diferentes escenarios: 
actividad normal (etiqueta `0`), ataque Slowloris (etiqueta `1`) y ataque Torshammer (etiqueta `2`). Los archivos PCAP fueron omitidos debido a restricciones de tamaño de GitHub.

## 3. Metodología

### 3.1. Preprocesamiento de Datos

1.  **Carga del Dataset**: Se carga el archivo `boa_dataset_ml_ready_frontend_microservice.csv` en un DataFrame de Pandas.
2.  **Selección de Características**: Se filtran características basadas en su correlación con la variable objetivo `label` (correlación absoluta > 0.1), reduciendo de 225 a 177 características.
3.  **Escalado de Datos**: Las características seleccionadas se escalan utilizando `StandardScaler`.
4.  **Reducción de Dimensionalidad (PCA)**: Se aplica PCA para reducir las 177 características a 20 componentes principales, capturando el 95% de la varianza explicada.
5.  **División de Datos**: Se realiza una división estratificada (80% entrenamiento, 20% prueba) para mantener la proporción de las clases debido al desbalance existente.

### 3.2. Modelos de Machine Learning

Se entrenaron y evaluaron dos modelos de clasificación:

*   **Regresión Logística (con PCA)**: Sirve como línea base para la comparación.
*   **XGBoost (con PCA)**: El modelo principal, conocido por su alto rendimiento y robustez.

### 3.3. Evaluación del Modelo

Los modelos se evaluaron utilizando métricas como:

*   **Accuracy**
*   **Reporte de Clasificación** (Precision, Recall, F1-score por clase)
*   **Matriz de Confusión**
*   **Curvas de Entrenamiento y Validación** (mlogloss y Accuracy para XGBoost).

### 3.4. Explicabilidad del Modelo (SHAP)

Se integró SHAP (SHapley Additive exPlanations) para interpretar las predicciones del modelo XGBoost. Esto permite entender la contribución de cada componente principal a una predicción específica.

### 3.5. Integración con LLM (Groq API)

Se utiliza la API de Groq (modelo `llama-3.1-8b-instant`) para generar explicaciones en lenguaje natural basadas en los valores SHAP de los componentes principales más influyentes. Esto facilita 
la comprensión de por qué el modelo hizo una predicción particular.

## 4. Resultados Clave

*   **Alto Rendimiento**: Ambos modelos, especialmente XGBoost (con un `accuracy` de ~0.999), demostraron una capacidad excepcional para detectar los ataques Slowloris y Torshammer.
*   **Importancia de PCA**: La reducción de dimensionalidad con PCA fue efectiva, manteniendo un alto rendimiento del modelo con un conjunto de características reducido.
*   **Explicabilidad**: SHAP proporcionó información valiosa sobre qué componentes principales influyen más en las predicciones, y el LLM tradujo esto en explicaciones técnicas comprensibles, facilitando el diagnóstico operacional.

Este proyecto sirve como una base sólida para el desarrollo de sistemas de detección de intrusiones más avanzados en entornos de Kubernetes, ofreciendo no solo predicciones precisas sino también una comprensión clara de las razones detrás de ellas.

## Integrantes:
* Valentina Benitez Zapata | Correo: vbenitezz@eafit.edu.co
* Darieth Farid Sánchez Velásquez | Correo: dfsanchezv@eafit.edu.co
