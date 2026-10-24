# Port Log — Trabajo Práctico Integrador

**Sprint actual: Sprint 3**

## Objetivo
Aplicar conocimientos de bases de datos relacionales (SQLAlchemy), versionado de datos (DVC) y búsqueda por similitud visual (ChromaDB) para profesionalizar el sistema portuario.

## Introducción y contexto
El Puerto Fluvial de Rosario es uno de los complejos portuarios más importantes de
América del Sur y el principal punto de exportación de granos y derivados de la
Argentina. El sistema de registro de movimientos portuarios acumuló inconsistencias
de formato en el sistema heredado de los años '90.

En el Sprint 3 el sistema se profesionaliza: los datos pasan a una base de datos
relacional con ORM (base `transito_portuario`), los artefactos binarios y el dataset
procesado se versionan con DVC sobre un repositorio S3 de Supabase (auditoría de
versiones), y los buques se identifican por similitud visual con embeddings OpenCLIP
y ChromaDB, sorteando las limitaciones del OCR.

## Repositorio
https://github.com/oscale05/TRABAJO_PRACTICO_INTEGRADOR_UGR_HS (rama `Sprint_3`)

## Grupo 25
- Oscar Alejandro Franco
