# Conclusión - Sprint 1: Port Log Analysis

## 1. Evaluación de la calidad del dataset heredado

El dataset original contenía **1.500 registros** de movimientos portuarios. Tras el proceso de limpieza y normalización, el dataset final quedó con **474 registros válidos**, lo que implica que se descartaron **1.026 registros (68.4%)**.

### Tipos de error más frecuentes:
- **Horas inválidas (18.78% de infracciones)**: valores como `AB:CD`, `30:17:00`, `26:42:00`, formatos 12h/24h mezclados, y nulos (34 en hora_egreso).
- **Fechas inválidas (4.64% de infracciones)**: formatos inconsistentes (`32/13/2021`, `21-05-2020`), días/meses imposibles, y nulos.
- **Matrículas inválidas (91 nulas finales)**: caracteres especiales (`@#$%`), espacios, nulos (56 originales).
- **Valores nulos en columnas críticas**: velocidad_ingreso (82), tonelaje_declarado (69), radar_id (65), estado_despacho (58), matricula (56).
- **Outliers en variables numéricas**: 88 registros eliminados por IQR en tonelaje_declarado y velocidad_ingreso.
- **Registros sin infracción real (772 eliminados)**: exceso_velocidad ≤ 0 tras aplicar tolerancia del 5%.

## 2. Patrones de infracción detectados

### Por turno:
La mayor concentración de infracciones ocurre en **Tarde (27.6%)**, seguida de **Noche (22.4%)**, **Madrugada (22.2%)** y **Mañana (21.7%)**. El turno de tarde muestra mayor volumen, posiblemente por mayor tráfico portuario en ese horario.

### Por muelle:
Los muelles con más infracciones son **MUELLE-D (87, 18.4%)**, **MUELLE-B (82, 17.3%)** y **MUELLE-F (81, 17.1%)**. MUELLE-D destaca con el exceso de velocidad promedio más alto (~3.8).

### Por tipo de carga:
**CONTENEDORES (14.98%)** es el tipo de carga más frecuente en infracciones, seguido de **TRIGO (14.77%)** y **GRANOS (13.08%)**. Las cargas de alto valor/urgencia (contenedores, trigo) tienden a presentar más excesos de velocidad.

## 3. Impacto de incorporar datos sin limpieza

Incorporar estos datos sin limpieza al nuevo sistema tendría consecuencias graves:
- **Cálculos erróneos de duración**: 109 registros con duración NaN por fechas/horas inválidas.
- **Falsos positivos/negativos en infracciones**: horas `AB:CD` o `30:17` generarían errores en clasificación por turno y cálculo de excesos.
- **Pérdida de trazabilidad**: 91 matrículas nulas impiden identificar buques infractores reincidentes.
- **Decisiones operativas sesgadas**: outliers en tonelaje y velocidad distorsionan estadísticas de capacidad y seguridad.
- **Incumplimiento normativo**: reportes oficiales basados en datos sucios podrían derivar en sanciones.

## 4. Propuesta concreta de mejora

**Implementar validación en tiempo real en el sistema de captura portuaria:**
- Controles de formato obligatorios al ingreso: fecha (ISO 8601), hora (24h HH:MM), matrícula (regex alfanumérico+guión).
- Validación cruzada: fecha_egreso ≥ fecha_ingreso, velocidad_ingreso ≤ límite físico del muelle.
- Catálogos normalizados: muelles (MUELLE-A..F), tipos de carga, orígenes predefinidos (dropdown).
- Alertas automáticas: exceso de velocidad > 5% tolerancia notifica a control de tránsito.
- Auditoría diaria: reporte de completitud y calidad de datos capturados en las últimas 24h.
