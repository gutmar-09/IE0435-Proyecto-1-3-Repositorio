# MODEL_CARD.md

---

## 1. Model name + version

| Campo | Detalle |
|-------|---------|
| **Nombre** | IE0435: Proyecto 1-3 Repositorio|
| **Archivo** | `C33619_Marlon_Gutierrez.joblib` |
| **Versión** | v3.0.0 |
| **Algoritmo** | SVM con kernel RBF, C=1 |
| **Pipeline** | StandardScaler → SVM RBF C=1 |
| **Framework** | scikit-learn (Python) |
| **Fecha** | Mayo 2026 |
| **Autor** | Marlon Gutiérrez Vásquez · C33619 · marlon.gutierrez@ucr.ac.cr |
| **Curso** | IE0435 - Inteligencia Artificial Aplicada a la Ingeniería Eléctrica, UCR |

---

## 2. Intended use / Out-of-scope

### Uso previsto
- Clasificación binaria de imágenes para detectar si una muestra está contaminada con granos de arroz (clase 1) o no lo está (clase 0).


### Fuera del alcance (out-of-scope)
- Detección de contaminantes distintos al arroz (el modelo solo fue entrenado para arroz vs. sin arroz).
- Contraluces o ambientes oscuros.
- Imágenes con fondos texturizados, oscuros o con colores similares al arroz.
- Detección de arroz en imágenes con objetos parcialmente tapados o escenas complejas.
- Correcta detección cuando los elementos están muy cerca, el algoritmo tiene mejor desempeño cuando los elementos están bien separados entre ellos.

---

## 3. Data summary

| Aspecto | Detalle |
|---------|---------|
| **Total imágenes** | 150 (120 entrenamiento + 30 prueba) |
| **Clases** | 1 = contaminada con arroz, 0 = no contaminada |
| **Balance** | Perfectamente balanceado: 50% positivas, 50% negativas en ambos conjuntos |
| **Resolución procesada** | 128 × 128 px (segmentadas y redimensionadas) |
| **Representación** | Vector de 17 características geométricas por imagen |

### Cómo se recolectó
Las primeras 30 imágenes (15 positivas + 15 negativas) fueron tomadas por el autor del proyecto en exteriores bajo luz solar directa, sobre fondos claros, con ángulo cenital. Para ampliar a 120 imágenes de entrenamiento, se incorporaron imágenes de tres compañeros del curso (Walther Barrantes, Ignacio Montenegro, Sebastián Rojas) siguiendo el mismo protocolo. El conjunto de prueba (30 imágenes) fue aportado íntegramente por Jorge Loría y es completamente independiente.



## 4. Labeling process

| Campo | Detalle |
|-------|---------|
| **Tipo de etiquetado** | Etiquetado binario a nivel de imagen (no bounding boxes) |
| **Nombre de mustras** | Convención de nombres de archivo: `positiva*.png` = clase 1, `negativa*.png` = clase 0, `prueba*.png` con etiqueta asignada manualmente |
| **Formato de etiqueta** | Última columna del CSV: 1 (contaminada) o 0 (no contaminada) |


### Control de calidad
- Las imágenes segmentadas fueron inspeccionadas visualmente de forma individual antes de generar el CSV, para verificar que la segmentación representara correctamente cada muestra.
- Se detectaron y corriguieron casos donde imágenes negativas presentaban pequeños puntos negros (ruido) que podrían confundirse con características de clase positiva.
- La reconstrucción de cada imagen desde el CSV fue comparada contra la imagen segmentada original para garantizar integridad de los datos almacenados.
---

## 5. Metrics

### Métricas reportadas

| Métrica | Fórmula | Descripción |
|---------|---------|-------------|
| Accuracy | (VP+VN)/(VP+VN+FP+FN) | Porcentaje total de clasificaciones correctas |
| Precision | VP/(VP+FP) | Proporción de predicciones positivas correctas |
| Recall | VP/(VP+FN) | Capacidad de detectar muestras positivas reales |
| F1 Score | 2×(P×R)/(P+R) | Media armónica entre Precision y Recall |

### Cómo se midieron
- Evaluación sobre el **conjunto de prueba** (30 imágenes, completamente independiente del entrenamiento).
- El modelo fue entrenado únicamente con los 120 ejemplos del conjunto de entrenamiento.

### Resultados comparativos en conjunto de prueba

| Modelo | Accuracy | Precision | Recall | F1 Score |
|--------|----------|-----------|--------|----------|
| Árbol de Decisión | 0.5333 | 0.5294 | 0.6000 | 0.5625 |
| Naive Bayes | 0.8000 | 1.0000 | 0.6000 | 0.7500 |
| **SVM RBF C=1** | **0.9000** | **0.8750** | **0.9333** | **0.9032** |
| SVM RBF C=10 | 0.8333 | 0.7778 | 0.9333 | 0.8485 |
| SVM RBF C=100 | 0.5667 | 0.5455 | 0.8000 | 0.6486 |
| SVM Lineal | 0.8333 | 0.8125 | 0.8667 | 0.8387 |
| KNN k=3 | 0.7667 | 0.7000 | 0.9333 | 0.8000 |
| KNN k=5 | 0.8667 | 0.8235 | 0.9333 | 0.8750 |

### Criterio de selección
El modelo SVM RBF C=1 fue seleccionado por obtener el mayor F1 Score (0.9032), indicando el mejor equilibrio entre Precision y Recall. Se descartó C=100 a pesar de mayor Recall, pues su Accuracy y F1 evidencian sobreajuste.

---

## 6. Ethical / Safety notes

- **Sesgo por iluminación**: el dataset fue capturado exclusivamente bajo luz solar exterior. El modelo puede degradarse significativamente ante iluminación artificial, ambientes interiores, sombras fuertes o variaciones de brillo no vistas durante el entrenamiento. Este es el sesgo más relevante del sistema.
- **Sesgo por fondo**: la elección deliberada de fondos claros facilita la segmentación pero limita la aplicabilidad del modelo en escenarios reales con fondos variables.
- **Sesgo de colaboración**: los cuatro aportantes son estudiantes del mismo curso y probablemente siguieron condiciones muy similares de captura, lo que reduce la diversidad real del dataset más de lo que los números sugieren.
- **Privacidad**: el dataset no contiene imágenes de personas ni datos personales identificables.
- **Transparencia sobre IA usada**: en el desarrollo del proyecto se utilizó ChatGPT para generación de figuras y depuración de código, y Claude para definición de características, estructura del pipeline de entrenamiento y para el desarrollo del repositorio.

---

## 7. Limitations

| Limitación | Impacto esperado |
|------------|-----------------|
| **Iluminación no controlada** | Alta degradación en interiores o con luz artificial; el umbral de Kittler puede fallar si la distribución de intensidades cambia significativamente |
| **Objetos pequeños** | Granos de arroz muy pequeños o lejanos pueden no ser detectados por el proceso de segmentación |
| **Dataset pequeño** | Con solo 150 imágenes, la varianza de las métricas es alta; una muestra diferente de prueba podría dar resultados distintos |
| **Distancia entre objetos y cámara** | Por un tema de perspectiva, el algoritmo funcionaría mejor si se define una misma distancia entre las muestras y la cámara, de esta forma se tienen muestras más estandar con menos variantes" |

---

## 8. Reproducibility

### Hardware usado para entrenamiento
| Campo | Detalle |
|-------|---------|
| **Equipo** | Computadora personal |
| **SO** | Windows |
| **Python** | 3.x |
| **Dependencias** | Ver `requirements.txt` |

### Comandos exactos para reproducir resultados

```bash
# 1. Clonar el repositorio
git clone https://github.com/gutmar-09/IE0435-Proyecto-1-3-Repositorio.git
cd IE0435-Proyecto-1-3-Repositorio

# 2. Instalar dependencias
pip install -r requirements.txt

# 3. Ajustar los path dentro de los programas a donde se guardó el repositirio

# 4. Generar imágenes segmentadas y archivos CSV
python src/preprocesamiento_digital_imagenes.py

# 5. Entrenar todos los modelos, evaluar en conjunto de prueba
#    y guardar el mejor modelo en mejor_modelo/
python src/entrenamiento_y_prueba.py
```


### Resultado esperado
Al ejecutar `entrenamiento_y_prueba.py` se debe obtener en terminal la tabla comparativa de métricas y el archivo `mejor_modelo/C33619_Marlon_Gutierrez.joblib` con el pipeline SVM RBF C=1 entrenado, obteniendo Accuracy=0.9000 y F1=0.9032 sobre el conjunto de prueba.

---

*Model Card generada para el curso IE0435, UCR, I-2026. Autor: Marlon Gutiérrez Vásquez, C33619.*