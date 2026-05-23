
# IE0435 - Proyecto 1-3: Repositorio

> Universidad de Costa Rica · Escuela de Ingeniería Eléctrica  
> IE0435 - Inteligencia Artificial Aplicada a la Ingeniería Eléctrica · I-2026 · Grupo 01  
> Profesor: Marvin Coto Jiménez  
> Autor: Marlon Gutiérrez Vásquez · C33619

---

## Descripción

Sistema de clasificación supervisada para detectar contaminación por granos de arroz en imágenes RGB. Seconvierte imágenes a escala de intensidad, aplica segmentación binaria mediante el algoritmo de Kittler, extrae 17 características geométricas y morfológicas por imagen, y entrena varios modelos de clasificación supervisada. El mejor modelo obtenido fue SVM con kernel RBF (C=1), con Accuracy=0.90 y F1 Score=0.9032 sobre el conjunto de prueba.

---

## Estructura del repositorio
![Estructura del repositorio](reporte/estructura_estructura_proyecto.png)

---
## Instalación

```bash
git clone https://github.com/gutmar-09/IE0435-Proyecto-1-3-Repositorio.git
cd IE0435-Proyecto-1-3-Repositorio
pip install -r requirements.txt
```

---
## Consideración importante para el Path

Los programas fueron desarrollados utilizando rutas de directorio Path correspondientes al computador en el que se realizó el proyecto. Por esta razón, al ejecutar los programas en un computador externo, será necesario modificar dichas rutas para que coincidan con la ubicación específica de los archivos y directorios en el nuevo sistema. De esta manera, se garantiza el correcto funcionamiento de los programas y el acceso adecuado a las imágenes, archivos CSV y modelos utilizados en el proyecto.

---
## Cómo correr el preprocesamiento

Este programa carga las imágenes RGB, calcula la intensidad, aplica el algoritmo de Kittler para segmentación binaria, redimensiona a 128×128 px, genera los archivos CSV y reconstruye las imágenes para verificación.

```bash
python src/preprocesamiento_digital_imagenes.py
```

Los resultados se guardan automáticamente en:
- `conjunto_entrenamiento/imagenes_binarias/`
- `conjunto_entrenamiento/imagenes_reconstruidas/`
- `conjunto_entrenamiento/CSV/CSV.csv`
- `conjunto_prueba/imagenes_binarias/`
- `conjunto_prueba/imagenes_reconstruidas/`
- `conjunto_prueba/CSV/CSV.csv`

---

## Cómo correr el entrenamiento y evaluación

Este programa carga los CSV, extrae las 17 características geométricas, normaliza con StandardScaler, entrena y evalúa los modelos (Árbol de Decisión, Naive Bayes, SVM RBF C=1/10/100, SVM Lineal, KNN k=3/5) e imprime la tabla comparativa de métricas.

```bash
python src/entrenamiento_y_prueba.py
```

Salida esperada en terminal:

```
TABLA COMPARATIVA
=================================================================
                   Accuracy  Precision  Recall      F1
Arbol de Decision    0.5333     0.5294  0.6000  0.5625
Naive Bayes          0.8000     1.0000  0.6000  0.7500
SVM RBF C1           0.9000     0.8750  0.9333  0.9032   - mejor
SVM RBF C10          0.8333     0.7778  0.9333  0.8485
SVM RBF C100         0.5667     0.5455  0.8000  0.6486
SVM Lineal           0.8333     0.8125  0.8667  0.8387
KNN k=3              0.7667     0.7000  0.9333  0.8000
KNN k=5              0.8667     0.8235  0.9333  0.8750
```

El mejor modelo (SVM RBF C=1) se guarda automáticamente en:
```
mejor_modelo/C33619_Marlon_Gutierrez.joblib
```

---


## Licencia

MIT License. Ver [LICENSE](LICENSE).
