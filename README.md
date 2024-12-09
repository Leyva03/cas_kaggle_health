# Health Insurance Cross Sell Prediction 🏠 🏥

## Autores

Josep Riballo Moreno y Pau Leyva García 

## Contexto

Este proyecto aborda el desafío de predecir si los asegurados de una compañía de seguros de salud estarán interesados en adquirir también un seguro de vehículo. Este modelo es clave para optimizar las estrategias de comunicación y maximizar los ingresos al contactar de manera efectiva a los clientes interesados.

Las pólizas de seguro operan bajo el concepto de compartir riesgos entre un grupo de clientes. Por ejemplo, los costos de hospitalización pueden ser cubiertos a través de las primas regulares que pagan los asegurados. Este principio se aplica tanto al seguro de salud como al de vehículos, donde la prima asegura la cobertura de costos en caso de accidentes.

Dado este contexto, la predicción precisa de clientes interesados en el seguro de vehículo permitirá a la empresa optimizar sus esfuerzos de marketing y alcanzar sus objetivos de negocio.

---

## Descripción del Dataset

El conjunto de datos, obtenido de [Kaggle](https://www.kaggle.com/datasets/anmolkumar/health-insurance-cross-sell-prediction), contiene información demográfica, de vehículo y pólizas de seguro. Las variables principales son:

| **Variable**            | **Descripción**                                                             |
|--------------------------|-----------------------------------------------------------------------------|
| **id**                  | ID único para el cliente                                                   |
| **Gender**              | Género del cliente                                                        |
| **Age**                 | Edad del cliente                                                          |
| **Driving_License**     | 0: Sin licencia de conducir, 1: Con licencia de conducir                   |
| **Region_Code**         | Código único para la región del cliente                                    |
| **Previously_Insured**  | 1: Ya tiene seguro de vehículo, 0: No tiene seguro                         |
| **Vehicle_Age**         | Edad del vehículo                                                         |
| **Vehicle_Damage**      | 1: Ha sufrido daños en el pasado, 0: No ha tenido daños                    |
| **Annual_Premium**      | Prima anual pagada por el cliente                                          |
| **Policy_Sales_Channel**| Canal utilizado para la captación del cliente                              |
| **Vintage**             | Días que el cliente lleva asociado a la compañía                          |
| **Response**            | 1: Cliente interesado, 0: Cliente no interesado                           |

---

## Proceso del Proyecto

### 1. Análisis Exploratorio de Datos (EDA)

Se realizó una revisión exhaustiva del dataset para identificar patrones y relaciones clave:
- **Distribución de clases:** Fuerte desbalance entre clientes interesados (`Response=1`) y no interesados (`Response=0`).
- **Valores nulos:** No se encontraron valores nulos.
- **Transformaciones:** Se realizaron escalados y codificación de variables categóricas (`Gender`, `Vehicle_Age`, `Vehicle_Damage`) para su uso en los modelos.

---

### 2. Técnicas de Balanceo

Dado el desbalance en la variable objetivo (`Response`), se aplicó la técnica **SMOTE** (Synthetic Minority Oversampling Technique) para generar muestras sintéticas de la clase minoritaria (`1`). Esto permitió mejorar el aprendizaje del modelo al equilibrar las proporciones de ambas clases.

---

### 3. Modelos Probados

Se evaluaron tres modelos principales con sus respectivas optimizaciones:

#### **3.1. Regresión Logística**
Modelo base utilizado para comparar resultados.
- Buen desempeño en **accuracy** general (87.50% en validación).
- Bajo desempeño en la detección de la clase minoritaria (`1`), con valores casi nulos en recall y f1-score sin oversampling.
- Mejora con SMOTE, pero sigue siendo inferior a los modelos basados en árboles.

#### **3.2. Random Forest**
Modelo de árboles que captura relaciones no lineales.
- Desempeño inicial con **AUC-ROC** de 0.8639 en validación.
- Optimización de hiperparámetros mejoró el **AUC-ROC** a 0.9037.
- Mejor balance entre recall y precisión en la clase `1`, sacrificando algo de precisión en la clase `0`.

#### **3.3. XGBoost**
Modelo basado en boosting, altamente eficiente.
- **AUC-ROC** de 0.8981 en validación tras optimización.
- Excelente **recall** (98%) en la clase `1`, lo que garantiza la detección de casi todos los clientes interesados.
- Sacrificio en precisión en la clase `0`, generando falsos positivos.

---

### 4. Resultados y Comparativa

| Métrica       | Regresión Logística (SMOTE) | Random Forest (Optimizado) | XGBoost (Optimizado) |
|---------------|-----------------------------|----------------------------|-----------------------|
| **AUC-ROC (Val)** | 0.8353                      | 0.9037                     | 0.8981                |
| **Recall (Clase 1)** | 87%                         | 97%                        | 98%                   |
| **Precision (Clase 1)** | 45%                         | 75%                        | 74%                   |
| **Accuracy (Val)** | 87.50%                      | 79.27%                     | 81.49%                |

---

### 5. Recomendaciones

En función de los resultados obtenidos:
- **Random Forest**: Recomendado si se busca un balance entre precisión y recall en ambas clases.
- **XGBoost**: Ideal si el objetivo es maximizar el recall en la clase `1` (clientes interesados), cumpliendo el objetivo de minimizar falsos negativos.

#### **Ajustes adicionales según la estrategia de negocio:**
- Modificar el umbral (**threshold**) para ajustar el balance entre falsos positivos y recall.
- Considerar modelos híbridos o estrategias combinadas si los falsos positivos generan un alto impacto operativo.

---

## Uso del Proyecto

### **Requisitos**
1. **Python 3.8+**
2. Librerías requeridas: `numpy`, `pandas`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `imblearn`.

### **Instrucciones**
1. Clonar el repositorio:  
   ```bash
   git clone https://github.com/Leyva03/cas_kaggle_health
   ```
2. Instalar las dependencias:  
   ```bash
   pip install -r requirements.txt
   ```
3. Ejecutar el notebook:

### **Archivos Principales**
- `CasKaggle.ipynb`: Script principal que implementa el flujo del proyecto.
- `train.csv`, `test.csv`: Datasets de entrenamiento y prueba.
- `README.md`: Detalles del proyecto.
- `requirements.txt`: Dependencias del proyecto.

---

## Conclusión

El modelo **XGBoost optimizado** es el más adecuado para este caso, ya que prioriza la detección de clientes interesados (recall de 98%). Dependiendo de la estrategia de negocio, ajustes en el umbral o combinaciones de modelos pueden reducir los falsos positivos, maximizando la efectividad de las campañas de marketing.