# Conclusión Sprint 2 - Validación Visual de Infracciones Portuarias

## 1. Porcentaje de infracciones validadas visualmente

De las **474 infracciones** en el dataset limpio, solo **8 (1.69%)** pudieron validarse visualmente mediante evidencia fotográfica con matching exitoso de matrícula. Las **466 restantes (98.31%)** no cuentan con imagen asociada o el OCR no logró extraer una matrícula coincidente con el dataset.

Este bajo porcentaje de validación visual evidencia una brecha significativa entre el registro tabular de infracciones y la captura de evidencia fotográfica en el sistema portuario actual.

## 2. Grupo de imágenes más útil para OCR: `plates`

El grupo **`plates` (recortes de matrícula)** resultó sustancialmente más útil para el OCR:
- **Tasa de match: 13.33%** (8 matches de 60 imágenes)
- **Grupo `completes`: 2.50%** (1 match de 40 imágenes)

**Por qué `plates` funciona mejor:**
1. **ROI enfocado**: Las imágenes `plates` son recortes automáticos de la zona de matrícula, eliminando ruido de fondo (casco, mar, cielo, estructuras portuarias).
2. **Resolución relativa mayor**: Aunque las imágenes `plates` son más pequeñas en píxeles absolutos (promedio 347x77 vs 637x348), la matrícula ocupa una proporción mucho mayor del frame, resultando en mayor densidad de píxeles por carácter.
3. **Menos interferencias**: Sin texto incidental (nombres de buques, números de contenedores, señales portuarias) que confunda al OCR.
4. **Contraste optimizado**: El recorte automático suele centrar la matrícula con iluminación más uniforme.

## 3. Condiciones de captura que afectaron al matching

Las principales condiciones que degradaron el rendimiento del OCR y matching:

### **Imágenes nocturnas / baja iluminación**
- Muchas imágenes `completes` muestran condiciones de poca luz, generando ruido digital que el preprocesamiento (ecualización + blur) no logra eliminar completamente.
- El OCR detecta falsos positivos en reflejos, luces de navegación y sombras.

### **Distancia excesiva / matrícula pequeña**
- En imágenes `completes`, la matrícula ocupa < 5% del área total, resultando en < 10 píxeles de altura por carácter tras el resize implícito de EasyOCR.
- El threshold de 75% de coincidencia posicional es inalcanzable con tan poca resolución.

### **Desenfoque por movimiento / vibración**
- Buques en movimiento generan motion blur que el kernel gaussiano 5x5 no recupera.
- Caracteres como 'M' vs 'N', '8' vs 'B', '0' vs 'O' se vuelven indistinguibles.

### **Oclusiones y perspectiva**
- Estructuras del muelle, cables, u otros buques ocultan parcialmente la matrícula.
- Ángulos oblicuos deforman la proporción de caracteres.

### **Ruido en imágenes `plates` defectuosas**
- Algunas imágenes `plates` incluyen fondo (patente, casco) por recorte imperfecto del sistema automático.
- Esto explica por qué 52 de 60 `plates` no tuvieron match: OCR detecta ruido como texto.

## 4. Mejoras propuestas

### **Sistema de captura (hardware/software embebido)**
1. **Iluminación IR dedicada** en zona de matrícula para captura nocturna sin depender de luz ambiental.
2. **Trigger por proximidad**: Activar cámara solo cuando buque está en zona óptima (distancia fija, perpendicularidad).
3. **Dual-cámara**: Una wide (contexto `completes`) + una zoom/tele (recorte `plates` nativo) sincronizadas.
4. **Validación en borde**: Rechazar captura si detección de matrícula < 30 px de altura o score de confianza < 0.7.

### **Algoritmo de matching / OCR (software)**
1. **Detección de matrícula previa (YOLO/RT-DETR)**: Localizar ROI de matrícula antes de OCR, en lugar de OCR full-frame.
2. **Ensemble OCR**: Combinar EasyOCR + Tesseract + PaddleOCR con voting por carácter.
3. **Post-procesamiento con diccionario**: Corregir OCR usando lista blanca de matrículas válidas del dataset (Levenshtein + constrained decoding).
4. **Matching fuzzy mejorado**: Usar Jaro-Winkler o weighted Levenshtein en lugar de coincidencia posicional estricta, penalizando menos errores típicos OCR (0↔O, 1↔I, 8↔B).
5. **Aumento de datos sintéticos**: Generar dataset de entrenamiento con fuentes de matrículas, ruido, blur, perspectiva para fine-tuning de modelo OCR especializado.

## Resumen ejecutivo

El sistema actual de captura fotográfica **no es fiable como evidencia única** de infracción (solo 1.69% validación). Los recortes `plates` son 5.3x más efectivos que `completes`, pero aún fallan en 86.7% de casos por ruido residual. La inversión prioritaria debe ser **mejora en captura (iluminación + trigger)** antes que refinamiento algorítmico, ya que el OCR actual ya funciona bien con entrada de calidad.