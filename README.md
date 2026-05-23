# Detección de Ataques DoS en Kubernetes (Slowloris y Torshammer)

Este proyecto implementa un sistema de detección de intrusiones (IDS) basado en Machine Learning para identificar ataques de Denegación de Servicio (DoS) 
tipo Slowloris y Torshammer en entornos de Kubernetes. Utiliza el dataset `Kubernetes Intrusion Detection Datasets` y modelos como XGBoost y Regresión Logística, complementado con 
explicabilidad SHAP y la integración de un LLM para interpretaciones en lenguaje natural.

## 1. Descripción del Proyecto

El objetivo principal es construir y evaluar modelos de clasificación capaces de distinguir entre actividad normal y dos tipos específicos de ataques DoS 
en un clúster de Kubernetes: Slowloris y Torshammer. Se aborda el preprocesamiento de datos, la reducción de dimensionalidad con PCA, el entrenamiento y evaluación 
de modelos de alto rendimiento, y la explicabilidad del modelo mediante SHAP, con interpretaciones generadas por un modelo de lenguaje grande (LLM).

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

## 4. Requisitos

Para ejecutar este notebook, necesitarás tener instaladas las siguientes librerías de Python:

*   `pandas`
*   `scikit-learn`
*   `xgboost`
*   `matplotlib`
*   `seaborn`
*   `shap`
*   `groq`
*   `kaggle`

Puedes instalarlas usando `pip` o ejecutando la primera celda que aparece en el Notebook:

```bash
%pip install pandas scikit-learn xgboost matplotlib seaborn shap groq kaggle
```

### 4.1. Claves API

Este proyecto requiere la configuración de dos claves API:

*   **Kaggle API Key**: Para descargar el dataset `kube-ids0` directamente desde Kaggle.
*   **Groq API Key**: Para la integración del LLM y la generación de explicaciones en lenguaje natural.

**Instrucciones para configurar las claves API en Google Colab:**

1.  Abre Google Colab.
2.  En el panel izquierdo, busca el icono de '🔑' (Secrets).
3.  Añade un nuevo secreto con el nombre `KAGGLE_API_TOKEN` y pega tu API KEY de Kaggle en el campo valor
4.  Añade otro secreto con el nombre `GROQ_API_KEY` y pega tu API KEY de Groq en el campo de valor.


## 5. Cómo Ejecutar el Proyecto

1.  **Clonar el Repositorio**: Clona este repositorio en tu entorno local o directamente en Google Colab. Todo los procedimientos del proyecto (EDA, preprocesamiento, PCA, entrenamiento, validación y prueba del modelo...) se encuentra en un solo Notebook titulado `project_AI.ipynb`

2.  **Abrir en Google Colab**: Abre el archivo `project_AI.ipynb` en Google Colab.

3.  **Instalar Dependencias**: Asegúrate de que todas las librerías mencionadas en la sección de requisitos estén instaladas. La primera celda del notebook suele incluir los comandos `pip install` necesarios.

4.  **Configurar Claves API**: Configura tus claves de Kaggle y Groq en los 'Secrets' de Colab, como se explica en la sección 4.1.

5.  **Ejecutar Todas las Celdas**: Ejecuta todas las celdas del notebook en orden. Esto realizará los siguientes pasos automáticamente:
    *   Descargará el dataset desde Kaggle.
    *   Realizará todo el preprocesamiento de datos (selección de características, escalado, PCA).
    *   Entrenará y evaluará los modelos XGBoost y Regresión Logística.
    *   Generará visualizaciones de explicabilidad SHAP.
    *   Integrará el LLM para obtener explicaciones detalladas de las predicciones.
Al ejecutar todas las celdas se le pedirá permiso para acceder a los secretos de Google Colab, deberá otorgar el acceso y vuelvo a ejecutar todas las celdas



## 6. Resultados Clave

*   **Alto Rendimiento**: Ambos modelos, especialmente XGBoost (con un `accuracy` de ~0.999), demostraron una capacidad excepcional para detectar los ataques Slowloris y Torshammer.
*   **Importancia de PCA**: La reducción de dimensionalidad con PCA fue efectiva, manteniendo un alto rendimiento del modelo con un conjunto de características reducido.
*   **Explicabilidad**: SHAP proporcionó información valiosa sobre qué componentes principales influyen más en las predicciones, y el LLM tradujo esto en explicaciones técnicas comprensibles, facilitando el diagnóstico operacional.

Este proyecto sirve como una base sólida para el desarrollo de sistemas de detección de intrusiones más avanzados en entornos de Kubernetes, ofreciendo no solo predicciones precisas sino también una comprensión clara de las razones detrás de ellas.
