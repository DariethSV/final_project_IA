## 1. Requisitos

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

### 1.1. Claves API

Este proyecto requiere la configuración de dos claves API:

*   **Kaggle API Key**: Para descargar el dataset `kube-ids0` directamente desde Kaggle.
*   **Groq API Key**: Para la integración del LLM y la generación de explicaciones en lenguaje natural.

**Instrucciones para configurar las claves API en Google Colab:**

1.  Abre Google Colab.
2.  En el panel izquierdo, busca el icono de '🔑' (Secrets).
3.  Añade un nuevo secreto con el nombre `KAGGLE_API_TOKEN` y pega tu API KEY de Kaggle en el campo valor
4.  Añade otro secreto con el nombre `GROQ_API_KEY` y pega tu API KEY de Groq en el campo de valor.

## 2. Cómo Ejecutar el Proyecto

1.  **Clonar el Repositorio**: Clona este repositorio en tu entorno local o directamente en Google Colab. Todo los procedimientos del proyecto (EDA, preprocesamiento, PCA, entrenamiento, validación y prueba del modelo...) se encuentra en un solo Notebook titulado `project_AI.ipynb`

2.  **Abrir en Google Colab**: Abre el archivo `project_AI.ipynb` en Google Colab.

3.  **Instalar Dependencias**: Asegúrate de que todas las librerías mencionadas en la sección de requisitos estén instaladas. La primera celda del notebook suele incluir los comandos `pip install` necesarios.

4.  **Configurar Claves API**: Configura tus claves de Kaggle y Groq en los 'Secrets' de Colab, como se explica en la sección 1.1.

5.  **Ejecutar Todas las Celdas**: Ejecuta todas las celdas del notebook en orden. Esto realizará los siguientes pasos automáticamente:
    *   Descargará el dataset desde Kaggle.
    *   Realizará todo el preprocesamiento de datos (selección de características, escalado, PCA).
    *   Entrenará y evaluará los modelos XGBoost y Regresión Logística.
    *   Generará visualizaciones de explicabilidad SHAP.
    *   Integrará el LLM para obtener explicaciones detalladas de las predicciones.
Al ejecutar todas las celdas se le pedirá permiso para acceder a los secretos de Google Colab, deberá otorgar el acceso y vuelvo a ejecutar todas las celdas