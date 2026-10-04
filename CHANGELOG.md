# CHANGELOG

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
