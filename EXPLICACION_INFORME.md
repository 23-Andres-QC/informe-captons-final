# AeroVision · resumen del informe y del proyecto

**AeroVision: Analítica Aeroportuaria Multicámara para Tracking, Identificación de Aglomeraciones e Insights Comerciales.** Capstone con apoyo de Lima Airport Partners (LAP).

## Problema
Las cámaras CCTV del aeropuerto son independientes y están instaladas para seguridad. Por eso no permiten seguir a un pasajero entre cámaras, medir aglomeraciones frente a las tiendas ni saber cuántos de los que pasan entran. Todo debe hacerse sin rostro ni biometría (Ley 29733).

## Solución: tres partes
1. **Seguimiento por cámara:** YOLO26m detecta a las personas y un seguidor híbrido (movimiento, IoU, color y ResNet18) les da un `local_id` estable.
2. **Multicámara y plano:** la re-identificación sin rostro (YOLO26s-ReID; en Videos, OSNet-AIN + PersonViT) usa una galería global y restricciones físicas para asignar un `global_id` seudónimo. La homografía lleva cada persona al plano 2D en metros. CLIP estima el género, opcional y solo en agregados.
3. **Histórico e insights:** a partir de los eventos por zona se calculan visitas, permanencia, captación, densidad, mapas de calor (KDE), rutas (PrefixSpan) y flujos origen-destino.

## Plataforma
- **Componentes:** modelo en Python, dos backends en Go, PostgreSQL/PostGIS + pgvector y web en Vue, en Docker sobre una computadora local con RTX 4060 (la nube se dejó: cuesta más y el video saldría de la red).
- **Secciones de la web:** En vivo (teléfonos como cámaras), Videos (hasta 10 a la vez con IDs compartidos, sin guardar el video), Insights, Ventas, Registros, Configuración, Cuentas y Solicitudes.
- **Insights:** línea de tiempo con hasta 5 rangos que filtra y compara todo el tablero, y exporta a PDF y CSV.
- **Roles:** superadmin, seguridad y negocio. Negocio solo exporta con un permiso aprobado.
- **Retención:** vectores 24 h y puntos 90 días.

## Pruebas
- **ESAN, Edificios A y B, 3 cámaras fijas:** 16 personas seguidas, 93 % de las detecciones en el plano, error de calibración de 0.28–0.48 m y precisión de 0.99 en la asociación entre cámaras.
- **ESAN, teléfonos en vivo:** misma persona con el mismo ID en 4 teléfonos y género con 91 % de acierto.
- **ESAN, comedor, 2 teléfonos:** la misma persona lleva el mismo ID en las dos cámaras (4 personas en el plano frente a 17 antes); quedan dos errores puntuales.
- **Validación con videos etiquetados** (6 pares, 308 personas):
  - Re-ID entre cámaras: 58 → 78 % de acierto.
  - Misma persona con el mismo ID en dos cámaras: 14 → 36 %.
  - IDs que mezclan personas: 60 → 28 %.
  - IDF1 del seguidor: 35 → 40 %, que sigue siendo el límite.
- **Carga y estrés:**
  - 2 cámaras 1080p: unos 16 cuadros/s por video.
  - El género es lo que más carga la CPU.
  - 7 cámaras de 60 fps, a 5 cuadros/s fijos: 2 min de video en 3 veces su duración, sin saltar cuadros.
  - El cuello de botella es la CPU; la GPU trabaja al 28–38 %.

## Plan de negocio
- **Productos:** misma tecnología con las cámaras que ya existen, procesando grabaciones de noche.
  - **Captación de tiendas:** pasan, se detienen, entran y compran. Diagnóstico S/ 900; monitoreo S/ 149 al mes.
  - **Audiencia de pantallas:** estudio S/ 4 000; certificación S/ 180 al mes por pantalla.
- **Costos:** S/ 43–46 por cámara al mes, margen del 57–77 %, una computadora por cada 10 cámaras.
- **Meta:** sin capital externo, llegar a fin de 2027 a 10 tiendas y 10 pantallas, con unos S/ 3 300 de ingreso recurrente al mes.

## Estructura del informe
Resumen ejecutivo · Introducción · Problema y contexto (variables, restricciones, análisis ético/social/ambiental) · Marco teórico (9 antecedentes) · Requerimientos y casos de uso · Metodología (Partes I–III) · Planificación (Scrum, riesgos) · Algoritmos · Arquitectura · Prueba ESAN A y B · Prueba ESAN comedor · Validación y estrés · Plan de negocio · Referencias.
