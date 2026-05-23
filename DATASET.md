# DATASET.md — Documentación del Dataset


`IE0435-Proyecto-1-3-Repositorio-Dataset-v3.0`  
Detección de contaminación por granos de arroz en imágenes RGB binarias.

---

## Resumen

La siguiente tabla corresponde a los muestras iniciales tomadas
| Campo | Detalle |
|-------|---------|
| **Tarea** | Clasificación binaria (contaminada / no contaminada) |
| **Total de imágenes originales** | 30 (15 positivas + 15 negativas) |
| **Total tras ampliación** | 120 imágenes de entrenamiento + 30 de prueba |
| **Formato** | PNG, RGB |
| **Resolución procesada** | 128 × 128 píxeles (tras redimensionamiento) |
| **Etiquetas** | 1 = contaminada (con arroz), 0 = no contaminada |

---

## Cómo se recolectó

### Conjunto de entrenamiento (120 imágenes)

El conjunto de entrenamiento se construyó en dos etapas:

**Etapa 1 — Imágenes propias (30 imágenes):**  
Se tomaron 30 imágenes RGB originales: 15 muestras positivas (con granos de arroz) y 15 muestras negativas (sin arroz). Las imágenes fueron capturadas en exteriores durante un día soleado, con el fin de mantener iluminación uniforme y reducir variaciones de intensidad que pudieran afectar la segmentación. Las muestras se nombran `positiva1` a `positiva15` y `negativa1` a `negativa15`.

**Etapa 2 — Ampliación colaborativa (90 imágenes adicionales):**  
Para construir un conjunto más robusto de 120 imágenes, se incorporaron imágenes aportadas por los compañeros Walther Barrantes, Ignacio Montenegro y Sebastián Rojas. Estas fueron tomadas bajo condiciones similares: fondos claros, buena iluminación y contraste adecuado. Se nombraron `positiva16` a `positiva60` y `negativa16` a `negativa60`, completando 60 muestras positivas y 60 negativas.

### Conjunto de prueba (30 imágenes)

El conjunto de prueba es completamente independiente del de entrenamiento. Fue aportado por el estudiante Jorge Loría y nombrado `prueba1` a `prueba30`. Se le aplicó exactamente el mismo pipeline de preprocesamiento.

---

## Distribución del dataset

| Conjunto | Imágenes | Positivas (arroz) | Negativas (sin arroz) |
|----------|----------|-------------------|----------------------|
| Entrenamiento | 120 | 60 (50%) | 60 (50%) |
| Prueba | 30 | 15 (50%) | 15 (50%) |
| **Total** | **150** | **75** | **75** |

El dataset está perfectamente balanceado entre clases en ambos conjuntos.

---

## Condiciones de captura

| Variable | Descripción |
|----------|-------------|
| **Iluminación** | Luz solar natural, exterior, día soleado |
| **Fondo** | Fondos claros para maximizar contraste con objetos |
| **Dispositivos** | Múltiples celulares (al menos 4 estudiantes distintos) |
| **Distancia** | Distinta entre imágenes para mantener escala similar |

---

## Preprocesamiento aplicado

1. Conversión de RGB a imagen de intensidad (escala de grises mediante promedio de canales R, G, B)
2. Segmentación binaria mediante el algoritmo de Kittler (umbral calculado de forma independiente por imagen)
3. Redimensionamiento a 128 × 128 píxeles
4. Conversión a vector fila de 16 384 valores + etiqueta → almacenado en `CSV.csv`
5. Verificación mediante reconstrucción visual de cada imagen desde el CSV

---


## Cómo acceder al dataset

Los datos están incluidos en el repositorio bajo las carpetas `conjunto_entrenamiento/` y `conjunto_prueba/`:

```bash
git clone https://github.com/gutmar-09/IE0435-Proyecto-1-3-Repositorio.git
```

Los archivos CSV con los vectores de características ya procesados se encuentran en:
- `conjunto_entrenamiento/CSV/CSV.csv`
- `conjunto_prueba/CSV/CSV.csv`

Para regenerar los CSV desde las imágenes originales:

```bash
python src/preprocesamiento_digital_imagenes.py
```

