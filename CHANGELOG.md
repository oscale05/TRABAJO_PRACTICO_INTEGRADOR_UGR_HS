# CHANGELOG

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
