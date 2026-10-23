# CHANGELOG

[Ejercicio 06]
- Conclusión sobre datos tabulares vs imágenes en `port_log/reports/conclusion_sprint2.md`.

[Ejercicio 05]
- Infracciones sin y con imagen asociada.
- Imágenes sin match en el dataset.
- Ratio promedio de coincidencia de los matches.
- Grupo con mayor tasa de match exitoso (plates vs completes).
- Infracciones en estado PENDIENTE sin imagen asociada.

[Ejercicio 04]
- Extracción de matrículas con EasyOCR (`extraer_matricula`) sobre las imágenes.
- Matching alfanumérico contra `port_movements.csv` con umbral de 75% de coincidencia posicional.
- Dataset final en `port_log/data/processed/port_movements_image.csv`.

[Ejercicio 03]
- Conversión a escala de grises en `port_log/data/interim/imgs/03_01_gray`.
- Ecualización de histograma en `port_log/data/interim/imgs/03_02_equalized`.
- Suavizado gaussiano en `port_log/data/interim/imgs/03_03_blur`.
- Detección de bordes Canny (thresholds fijos 50/150) en `port_log/data/interim/imgs/03_04_canny`.

[Ejercicio 02]
- Listado de imágenes con nombre y tamaño en KB.
- Separación de imágenes en grupos `plates` y `completes` por nombre de archivo.
- Diccionario `group_images` con metadatos (OpenCV) guardado en `port_log/data/interim/group_images.json`.
- Estadísticas por grupo: resolución, área y tamaño promedio.
- Función `mostrar_muestra` (grilla de 2 columnas) para visualizar imágenes de cada grupo.

[Ejercicio 01]
- Clonado del repositorio y creación de la rama `Sprint_2` desde `Sprint_1`.
- Descarga y descompresión del dataset de imágenes en `port_log/data/raw/imgs`.
- Verificación de los archivos heredados del Sprint 1.

## [Ejercicio 01] - Inicialización repositorio y estructura de directorios
- Inicializado repositorio Git en rama Sprint_1
- Configurado user.email y user.name
- Agregado remote origin
- Creada estructura de directorios port_log/data/{raw,interim,processed,interim/plots}, port_log/reports
- Commit inicial: "Día 1: Inicialización repositorio y estructura de directorios"
