# AeroVision · guía de estudio del informe y del proyecto

**AeroVision: Analítica Aeroportuaria Multicámara para Tracking, Identificación de Aglomeraciones e Insights Comerciales.** Capstone con apoyo de Lima Airport Partners (LAP).

---

## 1. El problema
- El aeropuerto tiene decenas de cámaras CCTV **independientes**, instaladas para seguridad y no para analítica.
- Por eso no se puede responder:
  - Cuánta gente se aglomera frente a las tiendas.
  - Cuántos de los que pasan **entran**.
  - Cuánto tiempo se quedan.
  - Qué rutas siguen.
- **Restricción central:** seguir a una persona entre cámaras **sin rostro ni biometría** (Ley 29733 de Protección de Datos Personales).
- **Vacío de investigación:** los 9 antecedentes del marco teórico resuelven piezas por separado (congestión, trayectorias, Re-ID sin rostro, multivista, CCTV en aeropuertos), pero ninguno integra todo en un pipeline desplegable con un marco ético latinoamericano.

## 2. La idea en una frase
Detectar a cada persona en cada cámara, seguirla dentro de la cámara, reconocerla entre cámaras por su **cuerpo y ropa**, ubicarla en un **plano en metros** y, con esas trayectorias, calcular métricas de negocio. No se guarda video ni rostro.

---

## 3. Parte I · Seguimiento dentro de una cámara (`local_id`)
**Objetivo:** que una persona conserve el mismo ID mientras está en el campo de visión de una cámara, aunque se cruce con otra, se tape o se gire.

**Paso a paso por cada cuadro:**
1. **Detectar.** YOLO26m, solo la clase *person*, devuelve cajas `(x1, y1, x2, y2, confianza)`.
   - Un lote lleva un cuadro de cada cámara del mismo instante.
   - Se acepta desde una confianza de 0.30.
   - Las detecciones débiles **solo pueden alargar** un track existente; nunca crean uno nuevo.
2. **Limpiar.** Se descartan las cajas metidas en un 92 % o más dentro de otra y las de menos de 40 px de alto.
3. **Predecir.** Cada track guarda su caja y una velocidad amortiguada, con las que predice dónde estará en el cuadro siguiente.
4. **Comparar** cada track con cada detección:
   - **Movimiento:** distancia entre la caja predicha y la detectada.
   - **IoU:** cuánto se solapan las cajas.
   - **Escala:** cambio de tamaño en escala logarítmica, que tolera que la persona se acerque o se aleje.
   - **Apariencia:** solo cuando hace falta (ver abajo).
5. **Asociación en cascada** (5 etapas, cada una resuelta con el algoritmo **Hungarian**):
   1. **Principal:** detecciones con confianza ≥ 0.35 contra los tracks vistos en el cuadro anterior.
      - Puntaje: `S = wm·movimiento + wo·IoU + ws·escala + wa·apariencia`.
   2. **Segunda etapa (idea de ByteTrack):** una detección débil solo prolonga un track libre si el movimiento (0.55) y el IoU (0.12) son muy coherentes.
   3. **Recuperación:** las detecciones sobrantes se comparan con los tracks **perdidos**, usando su galería, la posición esperada y la escala.
   4. **Actualización:** cada track asignado actualiza su caja, su velocidad y, si la vista es buena, su galería.
   5. **Creación:** un track nuevo necesita 3 detecciones con confianza ≥ 0.60 en una ventana de 5 cuadros, para no publicar IDs fugaces.
   - Las columnas ficticias del Hungarian permiten que un track quede sin pareja sin robarle la detección a otro.

**Apariencia, sin rostro:**
- **Color:** el recorte se divide en cabeza, torso y piernas (pesos 0.20 / 0.52 / 0.28), con histogramas HSV y Lab. Se tapa lo que cubre otra persona.
- **Vector profundo:** ResNet18 de ImageNet sobre un recorte de 64×128, promediado en 4 franjas horizontales (2 048 valores).
- **Mezcla calibrada:** `S_app = clip(γ·[(1−ω)·color + ω·profundo] + β)`.
  - Modelo base: ω = 0.20, γ = 1.123, β = −0.075.
  - **Videos:** ω = 0.8.
- **Galería multivista:** cada track guarda hasta 24 vistas buenas (frente, costado, espalda) y se compara con la **más parecida**.
- **Cálculo selectivo:** la apariencia solo se calcula cuando cambia una decisión:
  - una detección cerca pero con poco solape;
  - dos personas que compiten por el mismo track;
  - una recuperación;
  - una persona nueva;
  - el refresco de la galería cada 5 cuadros.

**Estados de un track:**
- **Tentativo:** todavía no se publica.
- **Confirmado:** tiene `local_id`.
- **Perdido:** se guarda hasta 120 cuadros (8 s a 15 fps) por si reaparece; si no, se borra.
- Si un track se recupera o pasa por un cruce, se marca **en riesgo** y la Parte II lo verifica de inmediato.
- Los tiempos se definen a 15 fps y se escalan con los fps reales de cada cámara.

---

## 4. Parte II · Entre cámaras y en el plano (`global_id`)
**Objetivo:** que la misma persona tenga **el mismo ID en todas las cámaras** y una posición en metros.

**4.1 Sincronizar.**
- El tiempo común de cada cuadro es `t = (k−1)/fps + desfase`.
- Los cuadros a menos de 0.15 s entre sí se procesan juntos.

**4.2 Calibrar (homografía).**
- Con ≥ 4 puntos del suelo vistos en la imagen y en el plano se estima la matriz `H` de cada cámara.
- La persona se ubica por el **punto de sus pies**: centro inferior de la caja `(u, v)`.
- Su posición en el plano es `[X', Y', W'] = H·[u, v, 1]`, y luego `X = X'/W'`, `Y = Y'/W'`.
- **Validación:** se reservan ≥ 2 puntos de control que no entran al cálculo. Si su error supera 0.75 m, se recalibra.
- **Dos modos:**
  - **Visual-temporal:** sin homografía; solo apariencia, tiempo y conectividad.
  - **Calibrado:** además posición, velocidad y dirección en el plano.

**4.3 Tracklets y Re-ID.**
- **Tracklet:** el tramo de un `local_id` (cámara, tiempos, posiciones, velocidad, dirección y vistas). Si reaparece tras más de 2 s, es otro tracklet.
- **Re-ID:** YOLO26s-ReID (vector de 512) en el modelo base; en **Videos**, el promedio de OSNet-AIN y PersonViT.
- **Prototipo:** el promedio normalizado de los vectores de sus vistas.
- **Muestreo:** una vista buena (confianza ≥ 0.55, ≥ 40 px, < 20 % tapada) cada 0.4 s, hasta tener 10 por tracklet y 40 por identidad.
  - Después solo verifica cada 2 s, o al instante si el seguidor la marcó en riesgo.
- **Cambio de persona:** si 2 vistas seguidas se parecen < 0.40 a su prototipo, el tracklet se corta, porque el seguidor le pasó el ID a otra persona.

**4.4 Galería global.**
- Hay una sola memoria para todas las cámaras.
- Cada persona es una ficha con sus vistas de **todas** las cámaras, dónde y cuándo se vio por última vez.
- Así, alguien visto de espaldas en una cámara se reconoce de frente en otra.

**4.5 Antes de comparar apariencia: lo físicamente posible.** No se pueden fusionar dos identidades si:
1. Tienen dos tracklets a la vez en la misma cámara.
2. Están a la vez en dos cámaras que no se solapan.
3. Pasan entre cámaras sin conexión o fuera de la ventana `[tmin, tmax]` de esa transición.
4. Reaparecen en la misma cámara después de 120 s.
5. En modo calibrado: la velocidad requerida supera 4 m/s, la dirección no cuadra, o dos cámaras solapadas la ubican a más de 1.5 m.
- Además, solo son candidatas las identidades vistas en los últimos 90 s.

**4.6 Regla de fusión.**
- Similitud coseno entre fichas ≥ **0.60**.
- **Y** enlace promedio entre todos sus tracklets ≥ **0.55**, para que un solo tramo parecido no encadene a personas distintas.
- Si toda la evidencia viene de la misma cámara se exige **0.70**, porque mismo fondo y misma luz hacen que todos se parezcan más.
- **Sin ambigüedad:** la mejor candidata debe ganarle a la segunda por ≥ 0.05; si no, se espera.
- **Preferencia mutua:** la otra ficha también tiene que preferirla.
- Al fusionar se queda el ID más antiguo. Si luego resulta imposible, se separa. Todo queda en una bitácora.
- **En Videos:** cada tramo nuevo debe parecerse a **cada** tramo de la identidad, no solo al promedio. Así un error no "se come" a otras personas: los IDs mezclados bajaron de 60 a 28 %.

**4.7 Identidad nueva o no.**
- Se une a una conocida si, con ≥ 4 vistas, coincide con ella.
- Recibe un ID nuevo solo si estuvo visible ≥ 2 s con ≥ 5 vistas.
- Si pasó fugazmente y no coincide con nadie, **no se cuenta**.

**4.8 Seudónimo, cierre y privacidad.**
- El `global_id` vale solo en esa sesión.
- Al terminar, los IDs se **reconcilian** (se renumeran en orden de aparición) y se exportan como UUID v5.
- Quien no se ve en 120 s (re-entrada) y 90 s (candidatos) se **cierra**:
  - se borran sus vectores;
  - quedan solo sus puntos y tiempos;
  - si vuelve, es otra persona para el sistema.
- No se guardan video, recortes, rostros ni datos civiles.

**4.9 Género (opcional, solo agregado).**
- **Cómo vota:** CLIP *zero-shot* compara el cuerpo completo con dos frases de referencia. En el modelo base vota cada 0.6 s, con un máximo de 12 votos por tracklet.
- **Cuándo se fija:** con ≥ 2 votos, una confianza ≥ 0.55 y un margen ≥ 0.10. Si no se cumple, queda «sin determinar».
- **Cómo se publica:**
  - el backend cuenta por género y borra el de cada persona;
  - una categoría con menos de 5 personas se oculta;
  - si ocultarla permitiría deducirla a partir del total, se oculta también la siguiente.

**4.10 Zonas.**
- **INTERIOR:** dentro de la tienda (entrada real).
- **FRONTAGE:** franja de enfrente (exposición o *pass-by*).
- **Otras:** pasillo, check-in, seguridad, puerta, cola…
- Un punto se asigna con *point-in-polygon*. Si cae en varias zonas, gana INTERIOR y luego la de menor área.

**Salida:** la tabla `trajectory_points`, con `global_id`, tiempo, cámara, `local_id`, tracklet, x, y, zona, velocidad, dirección y confianza.

---

## 5. Parte III · Histórico e insights
Trabaja solo con las trayectorias, sin volver a correr la visión.

1. **Consolidar:** une las vistas simultáneas de la misma persona desde varias cámaras, ponderadas por confianza, y quita duplicados y saltos imposibles.
2. **Dos trayectorias por persona:**
   - espacial: `(X, Y, t)`;
   - semántica, por zonas: Entrada → Check-in → Seguridad → Tienda → Puerta.
3. **Eventos:**

| Evento | Cuándo ocurre |
|---|---|
| EXPOSURE | Cruza el FRONTAGE de una tienda |
| ENTER | Pasa de afuera o del FRONTAGE al INTERIOR |
| DWELL | Se queda en el INTERIOR un tiempo mínimo |
| EXIT | Sale del INTERIOR |
| RETURN | Vuelve a entrar tras salir |
| QUEUE | Se queda en una zona de cola con velocidad baja |

4. **Métricas:**
   - **Permanencia** = t(EXIT) − t(ENTER).
   - **Visitas** = personas únicas con ENTER.
   - **Exposición** = personas únicas con EXPOSURE.
   - **Captación** = expuestos que luego entran ÷ expuestos. Sin expuestos se reporta «no disponible», no 0.
   - **Densidad** = personas en la zona ÷ área (pers/m²).
   - **Congestión:** cantidad + densidad + duración + velocidad. Mucha gente rápida es flujo; mucha gente lenta y persistente es aglomeración o cola.
5. **Mapas de calor (KDE):** suaviza las posiciones.
   - De **ocupación:** pesa el tiempo.
   - De **visitantes únicos:** no infla a quien se queda quieto.
6. **Rutas frecuentes (PrefixSpan):** busca las secuencias de zonas que más se repiten, tras comprimir las repeticiones seguidas de una misma zona.
7. **Grafo origen-destino:** el peso de cada flecha Zi → Zj es el número de personas únicas que hicieron ese paso.
8. **Tráfico y embudo:**
   - Las posiciones se agregan cada 0.2 s en ventanas de 5, 15 y 60 min.
   - **Se detiene:** ≥ 5 s. **Interesado:** ≥ 30 s.
   - Embudo: **Pasan → Se detienen → Entran → Compran**.
9. **Ventas:**
   - Cada ticket se empareja con la persona que estaba en el INTERIOR a esa hora, con una tolerancia de 90 s.
   - Un ticket por persona y visita; primero las parejas más probables.
   - **Conversión** = tickets ÷ visitantes.
   - **Pronóstico:** Holt-Winters con banda del 80 %.

---

## 6. Los tres modos del sistema

**6.1 Build (video grabado):** corre las Partes I, II y III completas sobre las cámaras fijas y publica la sesión.

**6.2 En vivo con teléfonos** (tres carriles, para que las cajas no esperen a los modelos lentos):
- **Rápido:** detecta, arma la caja con YOLO26s-pose, sigue y publica. Oculta a quien no se mueve en 3 s (maniquíes, afiches).
- **IDs:** muestrea el cuerpo y compara con una **memoria de identidades** (tras 1 s, o 3 s si hay duda).
  - Si coincide, recupera su ID y su color.
  - Durante 60 s puede fusionar un ID nuevo con uno ya visto.
  - La memoria se guarda cada 2 s y olvida a las 24 h.
- **Género:** CLIP cada 0.3 s; se fija con 2 votos de certeza ≥ 0.80.
- **Ubicación desde un teléfono:**
  - **Con calibración:** homografía. Se calibra con una grabación de alguien caminando: la altura de 1.70 m fija el horizonte y la focal se ajusta con dos referencias del plano.
  - **Sin calibración:** distancia `d = f · 1.70 / alto_en_px` (con `f = 0.78 · ancho`), ángulo `α = atan((u − ancho/2)/f)`, y posición = teléfono + `d·(cos(θ−α), sin(θ−α))`.

**6.3 Videos (subir grabaciones de cualquier origen):**
- Hasta 10 videos forman una **tanda** que comparte un asociador: la misma persona tiene el mismo ID en todos.
- Por defecto procesa como **cámara en vivo**: si no alcanza al video, salta cuadros.
- **Ajustes más sensibles que el Build:**
  - detector a 1280 px en la GPU, con confianza 0.10;
  - tracks desde 0.15 de confianza y 12 px de alto;
  - apariencia con peso 0.8;
  - Re-ID OSNet-AIN + PersonViT;
  - género desde el primer voto, cada 1 s.
- **Análisis a elegir:** *solo multitracking*, *solo género* o *ambos*.
- Decodifica en la GPU (NVDEC) y prepara la pasada siguiente mientras procesa la actual.
- El video se borra al terminar; queda solo el resumen.

---

## 7. Casos de uso (CU) y requisitos
| CU | Qué hace |
|---|---|
| CU-01 | Iniciar y cerrar sesión |
| CU-02 | Monitorear personas y trayectorias en vivo sobre el plano |
| CU-03 | Reproducir el histórico de movimiento de una sesión |
| CU-04 | Configurar zonas del mapa (nombre, tipo, color) |
| CU-05 | Configurar cámaras y su cobertura |
| CU-06 | Ver una cámara en vivo |
| CU-07 | Consultar el mapa de calor |
| CU-08 | Consultar insights por zona; marcar hasta 5 rangos en la línea de tiempo y compararlos |
| CU-09 | Analizar flujos entre zonas |
| CU-10 | Exportar el reporte (CSV o PDF, o enviarlo por correo) |
| CU-11 | Procesar videos multicámara |

- **Funcionales:**
  - RF-01: roles.
  - RF-02 a RF-05: en vivo, configuración del mapa, histórico e insights.
  - RF-06: multicámara seudónimo.
  - RF-07: ventas.
  - RF-08: videos (hasta 10).
  - RF-09: solicitudes.
- **No funcionales:**
  - RNF-01: seguridad.
  - RNF-02: privacidad.
  - RNF-03: integridad.
  - RNF-04 y RNF-05: rendimiento y fluidez.
  - RNF-06: disponibilidad.
  - RNF-07: portabilidad.
  - RNF-08: usabilidad.
  - RNF-09: mantenibilidad.
  - RNF-10: compatibilidad.
  - RNF-11: precisión.
  - RNF-12: rendimiento del video.
  - RNF-13: reproducibilidad (SHA-256 de pesos y configuración).

---

## 8. Arquitectura y plataforma
- **Servicios:**
  - modelo de visión en Python;
  - `backend` en Go: API de sitios, zonas, sesiones, insights, ventas y cuentas;
  - `backend-vivo` en Go: video de los teléfonos y Videos;
  - PostgreSQL/PostGIS + pgvector;
  - web en Vue.
  - Todo en Docker, en una computadora local con **RTX 4060 (8 GB)**.
- **Por qué local y no nube:** la primera versión estuvo en Google Cloud sin GPU. La computadora propia cuesta unos US$ 70 al mes (S/ 4 500 en 36 meses + energía), lo mismo que la nube interrumpible y mucho menos que la nube a demanda. Además, el video no sale de la red y el costo es fijo.
- **Roles:**
  - **Superadmin:** todo; crea cuentas y aprueba solicitudes.
  - **Seguridad:** En vivo, Videos y Teléfonos.
  - **Negocio:** Insights y Ventas; exporta solo con un permiso temporal aprobado.
  - **Operador:** une su teléfono con un código.
- **Seguridad:**
  - no hay registro público;
  - contraseñas con argon2id y sesión en cookie HttpOnly;
  - 8 intentos fallidos bloquean la cuenta 15 min;
  - bitácora;
  - el código del teléfono viaja por WebSocket, nunca en la URL.
- **Retención:** vectores 24 h y puntos 90 días.
- **Insights en la web:**
  - tarjetas de métricas, mapa de movimiento, mapa de calor, zonas, rutas, flujos y tráfico por franja;
  - **línea de tiempo** con hasta 5 rangos (arrastrar, escribir m:ss o clic en la línea). Con rangos:
    - todo el tablero muestra solo esos tramos;
    - los mapas colorean cada rango;
    - una tabla compara los rangos con la sesión completa;
    - el PDF trae los mapas de cada caso.

---

## 9. Pruebas y lo que demuestran
**9.1 ESAN, Edificios A y B, tres cámaras fijas (126 s).**
- 16 personas seguidas.
- 93 % de las detecciones ubicadas en el plano.
- Error de calibración de 0.28–0.48 m.
- Asociación entre cámaras con precisión 0.99 y recall 0.86.
- Muestrear el Re-ID ahorró 34 % de llamadas sin perder precisión.
- **Demuestra:** el pipeline completo funciona con cámaras calibradas.

**9.2 ESAN, teléfonos en vivo (6 de octubre).**
- 5 capturas, 28 personas.
- La que más se vio conservó su ID en 4 teléfonos.
- Género con 91 % de acierto sobre 169 recortes.
- **Demuestra:** los teléfonos sirven como cámaras y la memoria mantiene la identidad.

**9.3 ESAN, comedor, dos teléfonos.**
- Procesados como tanda de Videos: 4 personas en el plano, frente a 17 sin IDs compartidos.
- Quedan dos errores: un ID duplicado por unos segundos y dos caminantes que comparten un ID.
- **Demuestra:** dos cámaras dan el mismo ID a la misma persona.

**9.4 Validación de cada pieza** (6 pares de cámaras etiquetados, 308 personas).

| Pieza | Antes | Ahora |
|---|---|---|
| Detector | 960 px | 1280 px en Videos (+14 % de personas, sobre todo las lejanas) |
| Re-ID: la más parecida en la otra cámara es la misma | 58 % (YOLO26s-ReID) | 78 % (OSNet-AIN) |
| Tramos bien unidos entre cámaras | 31 % | 59 % |
| Re-ID a 2 s / 10 s (misma cámara) | OSNet 92.4 / 80.0 % | OSNet + PersonViT 94.3 / 82.8 % (CLIP-ReID descartado: +0.5 punto y el doble de tiempo) |
| Seguimiento: IDF1 | 35 % | 40 % |
| Seguimiento: cambios de ID | 4 422 | 2 940 (−34 %) |
| Misma persona con el mismo ID en dos cámaras | 14 % | 36 % (2.6 veces) |
| IDs que mezclan personas | 60 % | 28 % |

- **Limitación honesta:** con un IDF1 de 40 %, el seguimiento dentro de la cámara sigue siendo el límite en lugares llenos.

**9.5 Carga y estrés.**
- **2 cámaras 1080p a 23 fps:** unos 16 cuadros/s por video (32 en total), alrededor del 70 % de los cuadros.
- **El género es lo que más pesa:** 1.7 núcleos de CPU sin CLIP y 4.4 con él.
- **2 cámaras de 60 fps con mucha gente:** 7.7 cuadros/s por video.
- **7 cámaras de 60 fps, sin el reloj del tiempo real:**

| Ritmo | Cuadros/s por cámara | Tiempo (veces el video) | Continuidad (p90) | Con ID global |
|---|---|---|---|---|
| 1.5 fijos | 1.5 | 1.3 | 0.31 | 76 % |
| Adaptado | 3.9 | 3.0 | 0.17 | 82 % |
| **5 fijos** | **5.0** | **3.0** | **0.15** | **85 %** |

- **Conclusión:** el cuello de botella es la CPU (el seguidor); la GPU trabaja al 28–38 %. Para muchas cámaras conviene procesar **por lotes a 5 cuadros/s**, no en tiempo real.

---

## 10. Ética y gestión
- **Privacidad por diseño:** sin rostro, sin video guardado, seudónimos por sesión, vectores borrados al cerrar una identidad y género solo agregado con umbral de 5.
- **Riesgos cubiertos:** menores de edad, re-identificación indirecta, retención de datos, transparencia, sesgo algorítmico y mal uso del sistema.
- **Gestión:** Scrum por sprints con Gantt, actas, roles y matriz de riesgos.

---

## 11. Plan de negocio
- **Problema que se cobra:**
  - una tienda no sabe cuántos de los que pasan entran y compran;
  - quien vende publicidad no sabe cuánta gente ve cada pantalla.
- **Cómo se entrega:** con las cámaras que ya existen; la computadora procesa las grabaciones del NVR **de noche**, a 5 cuadros/s, y borra el video.
- **Productos:**

| Producto | Oferta | Precio |
|---|---|---|
| Captación | Diagnóstico de 2 semanas | S/ 900 |
| Captación | Prueba de vitrina | S/ 1 200 |
| Captación | Monitoreo mensual (2 cámaras) | S/ 149 al mes |
| Audiencia | Estudio de un lugar | S/ 4 000 |
| Audiencia | Certificación de una pantalla | S/ 180 al mes |
| Extra | Valor de locales | S/ 15 000 |

- **Costo:** S/ 43 por cámara de tienda y S/ 46 por cámara de pantalla al mes, con margen del 57–77 %. Una computadora (S/ 255 al mes) atiende 10 cámaras.
- **Capacidad:** se procesa de dos en dos cámaras (32 cuadros/s), así que diez pantallas se procesan en unas 5 horas de noche.
- **Crecimiento sin capital externo:**
  - para fines de 2027: 10 tiendas y 10 pantallas;
  - en 2027 entran unos S/ 51 mil;
  - a fin de año, el ingreso recurrente de unos S/ 3 300 al mes cubre el 73 % del pago a los fundadores.

---

## 12. Estructura del informe
1. Resumen ejecutivo
2. Introducción y objetivos
3. Problema y contexto (variables, restricciones, análisis ético, social y ambiental)
4. Marco teórico (9 antecedentes)
5. Requerimientos y casos de uso
6. Metodología (Partes I, II y III)
7. Planificación (Scrum y riesgos)
8. Algoritmos
9. Arquitectura
10. Prueba en ESAN, Edificios A y B
11. Prueba en ESAN, comedor
12. Validación y estrés
13. Plan de negocio
14. Referencias
