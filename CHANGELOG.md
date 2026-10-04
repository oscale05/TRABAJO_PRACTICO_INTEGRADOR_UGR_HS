# CHANGELOG

## [Ejercicio 01 Sprint 2] - Setup Sprint 2
- Clonado repositorio y creada rama `Sprint_2` desde `Sprint_1`
- Configurado git user.email y user.name
- Descargado dataset de imágenes `port_log_images.zip` (1.76 MB) y descomprimido en `port_log/data/raw/imgs/port_log_images/`
- Verificados archivos Sprint 1:
  - Raw: 1500 registros
  - Interim (limpio): 474 registros  
  - Summary: 11 registros
- Creado directorio `port_log/data/processed/`
- Instaladas dependencias: easyocr, opencv-python-headless, pillow
- Commit: "Día 8: Ejercicio 01 Sprint 2 - Setup repositorio, descarga imágenes y verificación archivos Sprint 1"

## [Ejercicio 07] - Conclusión final guardada en port_log/reports/conclusion.md
- **Calidad dataset heredado**: 1.500 → 474 registros (68.4% descartados). Errores: horas inválidas 18.78%, fechas inválidas 4.64%, matrículas nulas 91, nulos críticos en velocidad/tonelaje/radar/estado, 88 outliers IQR, 772 sin infracción real.
- **Patrones de infracción**: Turno Tarde 27.6% (mayor), Noche 22.4%, Madrugada 22.2%, Mañana 21.7%. Muelle-D 87 (18.4%), Muelle-B 82, Muelle-F 81. Tipo carga: CONTENEDORES 14.98%, TRIGO 14.77%, GRANOS 13.08%.
- **Impacto sin limpieza**: Cálculos duración erróneos (109 NaN), falsos positivos/negativos infracciones, pérdida trazabilidad (91 matrículas nulas), decisiones sesgadas por outliers, riesgo sanciones normativas.
- **Propuesta mejora**: Validación tiempo real en captura: formatos obligatorios (ISO 8601, 24h, regex matrícula), validación cruzada (egreso ≥ ingreso, velocidad ≤ límite), catálogos normalizados (muelles, carga, orígenes), alertas automáticas exceso >5%, auditoría diaria completitud.

## [Ejercicio 06] - Preguntas analíticas (5 respuestas con formato exacto)
- % infracciones con fecha inválida (1900-01-01): **4.64%**
- % infracciones con hora inválida (00:00): **18.78%**
- Tipo de carga más frecuente: **CONTENEDORES** con **14.98%**
- Origen más frecuente entre infractores: **VALPARAISO** con **71 registros**
- Duración promedio de estadía de buques infractores: **39.64 horas**

## [Ejercicio 05] - Visualizaciones (6 gráficos exportados a JPG)
- **Top 10 matrículas más reincidentes**: barras horizontales, mayor a menor (Top: COSCO-TIGRE 32, ONE-PARANA 29, CMA-ROSARIO 28...) → `port_log/data/interim/plots/top_infractores.jpg`
- **Infracciones por turno**: gráfico de torta (Tarde 27.6%, Noche 22.4%, Madrugada 22.2%, Mañana 21.7%, Desconocido 6.1%) → `port_log/data/interim/plots/turnos.jpg`
- **Infracciones por mes**: barras horizontales ordenado mayor a menor → `port_log/data/interim/plots/meses.jpg`
- **Histograma exceso_velocidad_real con KDE**: distribución con curva de densidad superpuesta → `port_log/data/interim/plots/distribucion_exceso.jpg`
- **Exceso velocidad promedio por muelle**: barras horizontales mayor a menor (MUELLE-D ~3.8, MUELLE-B ~3.2, MUELLE-F ~3.0...) → `port_log/data/interim/plots/exceso_por_muelle.jpg`
- **Fechas válidas vs inválidas**: barras simples (Válidas: ~390, Inválidas 1900-01-01: ~84) → `port_log/data/interim/plots/fechas_invalidas.jpg`
- Todos los gráficos con título, etiquetas en ejes, leyenda cuando corresponde

## [Ejercicio 04] - Clase PortAnalyzer con 6 métodos analíticos
- Definida clase `PortAnalyzer` con encapsulamiento (`_df` privado) y type hints
- `top_infractores(n)`: ranking matrículas con más infracciones (Top 1: COSCO-TIGRE 32, ONE-PARANA 29, CMA-ROSARIO 28...)
- `infracciones_por_turno()`: distribución por turnos (Tarde 131, Noche 106, Madrugada 105, Mañana 103, Desconocido 29)
- `exceso_promedio()`: exceso real promedio = 3.0331
- `exceso_promedio_tolerancia()`: exceso con 5% tolerancia = 2.4727
- `infracciones_por_muelle()`: ordenado alfabéticamente (MUELLE-A 66, MUELLE-B 82, MUELLE-C 81, MUELLE-D 87, MUELLE-E 77, MUELLE-F 81)
- `infractores_por_tipo_carga()`: mayor a menor (CONTENEDORES 71, TRIGO 70, GRANOS 62, HARINA 61, MINERALES 61, MAIZ 55, ACEITE 50, SOJA 44)
- Instanciado objeto `analyzer` e invocados todos los métodos en celdas separadas

## [Ejercicio 03] - Limpieza y normalización de datos
- Normalizadas fechas (fecha_ingreso, fecha_egreso) a YYYY-MM-DD; inválidas → 1900-01-01 (13 fechas inválidas detectadas)
- Normalizadas horas (hora_ingreso, hora_egreso) a formato 24h; inválidas → 00:00 (10 primeras inválidas mostradas: AB:CD, 30:17:00, 26:42:00, etc.)
- Calculada columna duracion_horas (diff datetime_egreso - datetime_ingreso); NaN cuando fechas/horas inválidas (10 primeras mostradas)
- Normalizadas matrículas: mayúsculas, quitar especiales, solo alfanuméricos y guión; inválidas → pd.NA (91 matrículas nulas finales)
- Normalizados muelles: mayúsculas, quitar especiales, formato MUELLE-X (10 primeros mostrados)
- Eliminadas filas con nulos en columnas críticas: matricula, fecha_ingreso, fecha_egreso, muelle, velocidad_ingreso, velocidad_maxima_muelle, tipo_carga, origen (166 filas eliminadas)
- Outliers en tonelaje_declarado y velocidad_ingreso: comparado IQR vs Z-score; elegido IQR (más robusto, no asume normalidad); 88 filas eliminadas (44 + 44)
- Creada exceso_velocidad_real = velocidad_ingreso - velocidad_maxima_muelle (10 primeras mostradas)
- Creada exceso_velocidad = velocidad_ingreso - (velocidad_maxima_muelle * 1.05) con 5% tolerancia (10 primeras mostradas)
- Eliminadas filas sin infracción (exceso_velocidad <= 0): 772 filas eliminadas, quedan 474
- Guardado dataset limpio final en port_log/data/interim/port_movements.csv (474 filas, 20 columnas)
- Exportado resumen estadístico (describe) a port_log/reports/summary_sprint1.csv

## [Ejercicio 02] - Análisis exploratorio de datos (EDA)
- Descargado dataset raw a port_log/data/raw/port_movements.csv (1500 filas, 15 columnas)
- Mostradas 5 primeras y 5 últimas filas en única salida
- Analizados tipos de datos: 12 object, 2 float64, 1 int64
- Identificadas columnas que requieren conversión: fecha_ingreso, fecha_egreso, hora_ingreso, hora_egreso, tonelaje_declarado, matricula, muelle, radar_id, estado_despacho
- Contados valores nulos (orden descendente): velocidad_ingreso (82), tonelaje_declarado (69), radar_id (65), estado_despacho (58), matricula (56), hora_egreso (34)
- Calculada completitud por columna (formato solicitado): movimiento_id 100%, buque_id 100%, matricula 96.27%, radar_id 95.67%, muelle 100%, tipo_carga 100%, origen 100%, fecha_ingreso 96.47%, hora_ingreso 93.53%, fecha_egreso 97.53%, hora_egreso 87.67%, tonelaje_declarado 95.40%, velocidad_ingreso 94.53%, velocidad_maxima_muelle 100%, estado_despacho 96.13%

## [Ejercicio 01] - Inicialización repositorio y estructura de directorios
- Inicializado repositorio Git en rama Sprint_1
- Configurado user.email y user.name
- Agregado remote origin
- Creada estructura de directorios port_log/data/{raw,interim,processed,interim/plots}, port_log/reports
- Commit inicial: "Día 1: Inicialización repositorio y estructura de directorios"
