# Informe final del Capstone · explicación

**AeroVision: Analítica Aeroportuaria Multicámara para Tracking, Identificación de Aglomeraciones e Insights Comerciales**
Informe Final · Capstone Project · Universidad ESAN

Autores:
- Neciosup Saavedra, Leslie (22200111)
- Quiliche Chavez, Andrés (22200144)
- Almeyda Razo, Gael (25101915)
- Isables Lopez Flores, Ema (25100805)

El informe está en LaTeX, se edita en Overleaf y vive en este repositorio (`github.com/23-Andres-QC/informe-captons-final`), separado del código del sistema (`github.com/23-Andres-QC/Aeropuerto`).

---

## 1. Archivos

| Archivo o carpeta | Contenido |
|---|---|
| `informe_aeropuerto_2026.tex` | **El informe** |
| `figuras/portada/` | Logo de ESAN |
| `figuras/antecedentes/` | `antecedente-1.png` … `antecedente-9.png`, una por antecedente del Marco Teórico |
| `figuras/casos-de-uso/` | `cu-01.png` … `cu-11.png`, diagramas de los casos de uso |
| `figuras/metodologia/` | `flujo.png`, flujo de la metodología |
| `figuras/gestion/` | `gantt.png` y `actas.png` |
| `figuras/arquitectura/` | `arquitectura.png` y `base-de-datos.png` (modelo entidad-relación de las tablas reales) |
| `figuras/validacion/` | Plano del patio de ESAN y capturas de la prueba de validación |
| `modelo informe.pdf` | Informe de otro grupo (*NeuroSegment AI*) usado como modelo de formato. No está versionado |

**Formato** (igual al del modelo): A4, Computer Modern a 12 pt, márgenes de 2,5 cm, párrafos separados por una línea, encabezado «Informe Final · Capstone Project» con número de página al pie, secciones numeradas, subtítulos con viñeta cuadrada, apartados numerados, «Figura N» y «Cuadro N», y algoritmos en pseudocódigo con «Entrada» y «Salida».

**Compilar:** en Overleaf, o con `pdflatex informe_aeropuerto_2026.tex` dos veces para armar el índice. Una figura cuyo archivo aún no existe aparece como un recuadro «Imagen pendiente» y no detiene la compilación.

---

## 2. Capítulos

1. **Resumen Ejecutivo.** El problema doble (congestión frente a las tiendas y falta de visibilidad del comportamiento del pasajero), la solución en tres partes y los resultados de la prueba en ESAN.
2. **Introducción.** Contexto, importancia, motivación técnica, objetivo general y siete objetivos específicos.
3. **Planteamiento del Problema.** Variables de entrada, salida, técnicas y de desempeño; restricciones técnicas, éticas y legales; análisis técnico, ético, social y ambiental con seis salvaguardas.
4. **Marco Teórico.** Nueve antecedentes, cuadro comparativo y vacío de investigación.
5. **Especificación de Requerimientos.** Once casos de uso, seis requerimientos funcionales y trece no funcionales.
6. **Metodología.** Parte I (seguimiento local), Parte II (re-identificación multicámara, mapa 2D y género), Parte III (eventos, KDE, PrefixSpan, origen-destino y métricas), validación por componente y trazabilidad.
7. **Planificación y Gestión.** Scrum, Gantt, actas, roles y gestión de riesgos.
8. **Algoritmos.** Un cuadro resumen y seis algoritmos en pseudocódigo: seguimiento local, asociación multicámara, proyección al plano y género, procesamiento histórico, modelo en vivo de dos carriles y ubicación desde un teléfono.
9. **Arquitectura del Sistema.** Diagrama de servicios y componentes; base de datos (`db` con PostGIS y `vivo-db` con pgvector), backend (Go, hexagonal), frontend (Vue 3) y despliegue (Docker Compose en una VM de Google Cloud).
10. **Primera Prueba de Validación: ESAN, Edificios A y B.** Plano del patio, prueba con tres cámaras fijas (16 personas, 93 % de detecciones en el plano, precisión 0,99 entre cámaras) y prueba con teléfonos como cámaras en vivo.
11. **Referencias Bibliográficas.**

---

## 3. Pendiente

- **Conclusiones:** por escribir.
- **Capturas de la prueba con teléfonos.** Los nombres de archivo ya están en el informe; basta subir cada imagen a `figuras/validacion/` con su nombre:

| Archivo | Captura |
|---|---|
| `telefono-camara.png` | La página de cámara abierta en un teléfono |
| `telefonos-modelo.png` | La sección Teléfonos con las cajas, ID y género del modelo |
| `en-vivo-plano.png` | El modo en vivo con las personas sobre el plano del patio |
| `registros.png` | La sección Registros con las capturas guardadas |
| `insights-captura.png` | Insights de una captura en vivo |

- **Referencias incompletas:** Yang, Ryan, Wong y Law, y Zhou no tienen revista, DOI ni enlace.
