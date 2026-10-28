# Informe final del Capstone · estructura

**AeroVision: Analítica Aeroportuaria Multicámara para Tracking, Identificación de Aglomeraciones e Insights Comerciales**

1. **Resumen Ejecutivo.** El problema, la solución propuesta, las pruebas en ESAN, las pruebas de estrés y el plan de negocio.
2. **Introducción.** Contexto, importancia, motivación técnica y objetivos general y específicos.
3. **Planteamiento del Problema y Análisis del Contexto.** Descripción del caso, variables, restricciones técnicas, éticas y legales, y análisis técnico, ético, social y ambiental (retención de 24 horas para los vectores y 90 días para los puntos).
4. **Marco Teórico.** Nueve antecedentes, cuadro comparativo y vacío de investigación.
5. **Especificación de Requerimientos.** Casos de uso y requerimientos funcionales y no funcionales (roles, ventas, videos y solicitudes).
6. **Metodología.** Parte I (seguimiento por cámara), Parte II (re-identificación multicámara, mapa 2D y género agregado) y Parte III (eventos, mapas de calor, rutas y métricas comerciales).
7. **Planificación y Gestión del Proyecto.** Panel Scrum con Gantt por sprints y avance, actas, evidencias del desarrollo, roles y gestión de riesgos.
8. **Algoritmos.** Pseudocódigo de cada parte, del modelo en vivo de tres carriles, de la ubicación desde un teléfono y del procesamiento de tandas de videos (OSNet-AIN y PersonViT).
9. **Arquitectura del Sistema.** Servicios, usuarios y roles, cuentas y solicitudes, despliegue local con GPU, costos frente a la nube, base de datos, backend y frontend con sus nueve secciones.
10. **Primera Prueba de Validación: ESAN, Edificios A y B.** Plano del patio, prueba con tres cámaras fijas y prueba con teléfonos como cámaras en vivo.
11. **Segunda Prueba de Validación: ESAN, Comedor.** Dos teléfonos en el comedor procesados como una tanda de Videos, con el mismo ID en las dos cámaras.
12. **Pruebas de Validación y Estrés del Modelo.** Ajustes medidos con videos etiquetados, cinco pares de cámaras con los tres análisis (solo multitracking, solo género y ambos) y estrés con dos y siete cámaras de 60 fps: cuadros por segundo, CPU, GPU, memoria y potencia.
13. **Plan de Negocio.** Problema, clientes, productos (captación de tienda y audiencia), lo construido, mercado y competencia, FODA, Canvas, precios, costo por cámara con la capacidad medida, pronóstico, validación comercial y cronograma.
14. **Referencias Bibliográficas.**
