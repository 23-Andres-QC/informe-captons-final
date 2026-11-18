# Guion de la exposición — TrackFlow

| Orden | Expositor | Diapositivas | Tiempo aproximado |
|---|---|---|---|
| 1 | Ema López | 1–6, 8–10 | 5 min |
| 2 | Gael Almeyda | 11–17 | 5 min |
| 3 | Andrés Quiliche | Flujo (diap. 7, movida) + 18–19 + demo | 3 min + 5 min de demo |
| 4 | Leslie Neciosup | 20–29 | 7 min |

Los números de diapositiva son los de la versión actual (antes de mover el Flujo). El texto es para decirlo con las propias palabras, no para leerlo: las cifras en **negrita** son las que conviene decir exactas.

---

## 1. Ema López — Problema, planificación, antecedentes y algoritmos

### Diap. 1 — Portada

Buenos días. Somos el equipo de TrackFlow: Leslie Neciosup, Andrés Quiliche, Gael Almeyda y yo, Ema López, de la Universidad ESAN. Les presentamos TrackFlow, inteligencia comercial y operativa mediante visión artificial: una plataforma que convierte las cámaras que un lugar ya tiene en información sobre cómo se mueven las personas.

### Diap. 2 — Índice

La exposición tiene cuatro bloques. Yo presento el problema, cómo planificamos el proyecto, los antecedentes y los algoritmos. Gael sigue con los datos, las pruebas y los casos de uso. Andrés explica el flujo y la arquitectura del sistema y hace la demo en vivo. Y Leslie cierra con la ética, el mercado y el plan de negocio.

### Diap. 3 — ¿Qué problema buscamos resolver?

Un aeropuerto, un centro comercial o una tienda reciben miles de personas al día, pero casi no saben qué hacen dentro. Queremos responder cinco preguntas: cuántas personas pasan, por qué zonas se mueven, cuántas ven cada local y se detienen frente a él, cuánto tiempo se quedan y dónde se concentran, y si esas visitas terminan en compras.

### Diap. 4 — ¿Cómo afecta a las organizaciones?

Cada vacío tiene un costo. Si no se conoce el flujo, los espacios se distribuyen mal. Si no se miden las visitas, no se sabe qué tan atractivo es un local. Sin datos de permanencia, el comportamiento del visitante es una incógnita. Sin indicadores de conversión, los resultados comerciales no se pueden medir. Y las aglomeraciones frente a las tiendas hacen que los clientes desistan de comprar.

### Diap. 5 — La solución: TrackFlow

Nuestra respuesta es TrackFlow, una plataforma de analítica multicámara que usa las cámaras existentes, sin instalar sensores nuevos. Detecta a las personas, las sigue entre cámaras con un identificador seudónimo, ubica su posición en el plano del lugar y entrega visitas, permanencia, captación y mapas de calor. No usa reconocimiento facial, no guarda imágenes de las personas ni estima su género. Lo que ven en pantalla es el ingreso a la plataforma.

### Diap. 6 — Planificación del proyecto

Trabajamos con Scrum, y Leslie Neciosup cumple el rol de Scrum Master. El proyecto tiene **16 sprints semanales** que cierran los miércoles. En el panel registramos **96 tareas** y, al sprint 14, llevamos completadas el **87,5 %**. Todo el código está en un repositorio Git, con **291 commits** de tres autores.

### Diap. 8 — Antecedentes revisados

Revisamos nueve trabajos que resuelven partes del problema. Giannopoulou y su equipo detectan personas en el aeropuerto de Atenas con un mAP de **0,87**. Zhao y su equipo identifican grupos en el aeropuerto de Shanghai. Mendes y su equipo re-identifican personas entre cámaras sin usar el rostro, con **82 %** de acierto en un entorno comercial real. Y Yang y su equipo verifican las identidades sobre un plano común, con un MOTA de **95,2 %** en WILDTRACK y MMPTRACK. Los demás aportan la caída de los modelos entre dominios, la evaluación de eventos, una arquitectura aeroportuaria, el grafo origen-destino y el seguimiento local en aeropuertos.

### Diap. 9 — Diferencia con los antecedentes

La conclusión es que cada antecedente resuelve una pieza por separado y se evaluó en su propio escenario: Atenas, Shanghai, Dublín o AirportPassenger. Ninguno ofrece un pipeline único que se pueda desplegar. TrackFlow junta detección, seguimiento, re-identificación, mapa 2D, minería de patrones e insights en un solo sistema, que corre en una computadora local con GPU y con privacidad por diseño, conforme a la Ley N.° 29733.

### Diap. 10 — Algoritmos del sistema

El sistema tiene tres partes. En la **Parte I**, cada cámara detecta a las personas con YOLO26m y las sigue con un seguidor híbrido que asocia los cuadros con el algoritmo húngaro: el resultado es un ID local estable por cámara. En la **Parte II**, un modelo de re-identificación compara la apariencia del cuerpo y une a la misma persona entre cámaras, con reglas físicas de tiempo y distancia; además, una homografía lleva cada posición de la imagen al plano, en metros. En la **Parte III**, sobre el histórico, una máquina de eventos, KDE, PrefixSpan y un grafo origen-destino producen los insights por zona. Para el en vivo usamos un modelo de dos carriles con memoria de identidades, y para los videos subidos, YOLO26 a 1280 px con OSNet-AIN y PersonViT.

*Transición:* Gael les muestra ahora con qué datos probamos todo esto.

---

## 2. Gael Almeyda — Dataset, pruebas, casos test y casos de uso

### Diap. 11 — Dataset

No entrenamos modelos desde cero: todos son preentrenados. Los de re-identificación, OSNet-AIN y PersonViT, vienen preentrenados en MSMT17, un conjunto de personas etiquetadas. Para las pruebas de estrés usamos **WILDTRACK**, un conjunto público de siete cámaras que graban la misma plaza. Y para validar usamos **12 videos etiquetados**: seis pares de cámaras de interiores, en 1080p a 23 cuadros por segundo y de unos cinco minutos, con **308 personas** etiquetadas cuadro por cuadro. De ellos salen **4 372 recortes** para comparar los modelos de re-identificación, que describen a cada persona con un vector de 512 dimensiones.

### Diap. 12 — Re-identificación y seguimiento

Aquí comparamos el modelo antes y después de la validación. En la re-identificación dentro de una misma cámara, con OSNet-AIN la persona más parecida dos segundos después es la misma el **92,4 %** de las veces; combinando OSNet con PersonViT sube a **94,3 %**, y a diez segundos llega a **82,8 %**. CLIP-ReID apenas sumaba medio punto y duplicaba el tiempo, por eso lo descartamos. En el seguimiento, el acierto entre cámaras pasa de **58 a 78 %**, los cambios de ID bajan un **34 %**, de 4 422 a 2 940, cada persona pasa de 3,8 a 2,9 IDs, los IDs que mezclan personas bajan de **60 a 28 %** y el IDF1 sube de 35 a **40 %**.

### Diap. 13 — Detector y rendimiento

En 16 cámaras de interiores etiquetadas, el detector encontró el **69 %** de las personas, y el **70 %** de lo que marcó era realmente una persona. La cifra real es algo mayor, porque las etiquetas dibujan el cuerpo completo aunque esté tapado. En rendimiento, con dos cámaras 1080p la GPU procesa unos **17 cuadros por segundo por video**, con un uso cercano al 68 % y 2,8 GB de memoria: la GPU no es el cuello de botella.

### Diap. 14 — Casos test

Resumimos la validación en nueve casos de prueba. Los dos primeros miden el detector: 69 % de recall y 70 % de precisión, y 52 % de recall en personas de menos de 100 píxeles, que es su punto débil. Del CT-03 al CT-07 se mide la re-identificación, el seguimiento y la asociación entre cámaras, con las mejoras que acabamos de ver. El CT-08 mide la carga: 17 cuadros por segundo por video. Y el CT-09 es el estrés: con las siete cámaras de WILDTRACK a 60 cuadros por segundo, el **85 %** de las detecciones lleva un ID global. El límite que reconocemos es el seguimiento en lugares muy llenos.

### Diap. 15 — Resumen de casos de uso

El sistema cubre **11 casos de uso** para dos actores: el usuario comercial y el administrador. Van desde iniciar sesión y monitorear en vivo hasta reproducir el histórico, configurar zonas y cámaras, consultar mapas de calor, insights y flujos, exportar reportes y procesar videos multicámara. De ellos salen los requerimientos funcionales y los no funcionales: seguridad y control de acceso, protección de datos, rendimiento, integridad y disponibilidad.

### Diap. 16 — CU-02: Monitorear personas en vivo

Un ejemplo: el administrador entra a la pantalla En vivo, ve el plano 2D con las personas y las zonas, activa las capas de trayectorias o de cámaras, selecciona a una persona y revisa su recorrido y sus eventos. Es el requerimiento RF-02, y Andrés lo va a mostrar en la demo.

### Diap. 17 — CU-04: Configurar zonas del mapa

El segundo ejemplo es configurar una zona. El administrador pulsa «Editar» y «+ Zona», marca los vértices sobre el plano y define el nombre, el tipo, el color y el local; el sistema valida el polígono y registra la zona. Esas zonas son las que después usan los insights para medir visitas y permanencia.

*Transición:* Andrés continúa con el flujo y la arquitectura del sistema.

---

## 3. Andrés Quiliche — Flujo, arquitectura y demo

### Diap. 7 (movida) — Flujo del sistema

Este es el flujo completo, en cinco pasos. Uno: en cada cámara se detecta y se sigue a cada persona. Dos: el tracking multicámara reconoce a la misma persona cuando pasa de una cámara a otra y le mantiene un solo ID. Tres: cada posición se proyecta al plano del lugar. Cuatro: con esas trayectorias se obtienen rutas y patrones, como los flujos entre zonas y el mapa de calor. Y cinco: todo termina en insights, como visitas por tienda, captación, permanencia, densidad por zona y horas de congestión. Los modelos son preentrenados: YOLO26m para detectar, un seguidor híbrido y un modelo de re-identificación.

### Diap. 18 — Arquitectura general y módulos web

Todo corre en una computadora local con una GPU RTX 4060, con Docker Compose. El usuario entra por HTTPS a la web, hecha en Vue 3 con TypeScript y servida por nginx. La web habla con el backend, escrito en Go con arquitectura hexagonal, que guarda sitios, sesiones, tráfico, ventas y cuentas en PostgreSQL con PostGIS. Los teléfonos se unen como cámaras con un código QR y mandan sus imágenes a camara-web; el modelo en vivo, en Python sobre la GPU, las procesa y manda las detecciones a backend-vivo, que guarda la memoria de identidades en otra base con pgvector. Los videos grabados se procesan por lotes y se publican por la API. A la izquierda están los nueve módulos de la web: En vivo, Videos, Insights, Ventas, Registros, Configuración, Teléfonos, Cuentas y Solicitudes.

### Diap. 19 — Base de datos

La base principal agrupa las tablas en seis bloques: espacio (sitios, cámaras, locales y zonas), sesiones y trayectorias, analítica, tráfico, ventas, y cuentas y seguridad. Las trayectorias guardan solo puntos e IDs seudónimos: no hay imágenes, rostros ni género. La memoria del en vivo está aparte, en vivo-db, con los vectores de apariencia, que se borran a las 24 horas.

Ahora veamos el sistema funcionando.

### Demo en vivo

**Antes de exponer:** modelo en vivo encendido, laptop y teléfonos en la misma red, enlace de la cámara abierto y aceptado en cada teléfono, sesión iniciada en la web y una sesión guardada a mano para el plan B.

1. **Teléfonos.** «Cualquier teléfono se vuelve una cámara, sin instalar ninguna aplicación: escanea el QR o abre el enlace con el código de unión, y empieza a transmitir.» Unir uno o dos teléfonos.
2. **En vivo.** Pulsar «Activar en vivo» y elegir la zona. «Cada persona aparece en el plano con un punto de su color y su ID global.» Caminar frente a un teléfono y luego frente al otro: «Al pasar de una cámara a otra conserva el mismo ID.»
3. **Terminar y guardar.** «La captura queda en Registros, con su fecha, duración, personas y puntos.»
4. **Insights.** Abrir esa captura: personas, permanencia, captación, densidad, mapa de movimiento y de calor. Marcar un rango en la línea de tiempo: «Todo el tablero se recalcula solo para ese tramo.» Exportar a PDF.
5. **Videos** (si sobra tiempo). «También se suben grabaciones por tandas; se procesan todas a la vez y no se guardan.»

**Plan B:** si falla la red o el modelo, abrir en Registros una sesión ya guardada (por ejemplo, la del pasillo del Edificio D del 9 de noviembre) y mostrarla con «Mapear en el plano» e «Insights».

*Transición:* Leslie cierra con la ética, el mercado y el plan de negocio.

---

## 4. Leslie Neciosup — Ética, mercado, negocio y cierre

### Diap. 20 — Ética y normas aplicables

Un sistema que observa personas tiene que cuidar su privacidad desde el diseño. TrackFlow no usa reconocimiento facial, no graba en el flujo analítico, usa identificadores seudónimos, controla el acceso por rol, registra cada acción en una bitácora y borra los vectores de apariencia a las 24 horas; tampoco estima el género ni otros atributos de las personas. Nos alineamos con la **Ley N.° 29733**, de protección de datos personales, y la **Ley N.° 31814**, sobre el uso de la inteligencia artificial, y tomamos como referencia las normas ISO/IEC 42001, 27001 y 27701, y la ISO 31000 de gestión de riesgos.

### Diap. 21 — Mercado y competencia

El mercado mundial de conteo de personas vale unos **US$ 1 450 millones** en 2025 y crece cerca de **13 % al año**. En el Perú, el mercado total (TAM) son unos **34 900 locales** comerciales: si todos pagaran el monitoreo de S/ 149 por tienda, serían unos **S/ 62,3 millones al año**. El mercado atendible (SAM) son unos **10 500 locales** de Lima con una cámara IP utilizable, unos **S/ 18,7 millones al año**. Y el mercado objetivo (SOM) para 2031 son **750 locales y 350 pantallas**, unos **S/ 2,1 millones al año**. Frente a la competencia —V-Count, FootfallCam, ShopperTrak y Xovis, que venden sensores, o Hikvision y Axis, que cuentan dentro de la cámara—, TrackFlow usa las cámaras que ya existen y sigue el recorrido completo, no solo el cruce de la puerta. Las consultoras locales cobran entre S/ 3 000 y 10 000 por un estudio puntual.

### Diap. 22 — Análisis FODA y estrategias cruzadas

Nuestras fortalezas son una plataforma completa, que funciona con las cámaras existentes, con privacidad por diseño y un costo fijo bajo. Las debilidades: somos un equipo a medio tiempo, la precisión todavía no se mide en tiendas, el detector tiene licencia AGPL y aún no hay clientes que paguen. Las oportunidades: un mercado que crece cerca de 13 % al año, recintos sin medición de audiencia, JCDecaux en el Jorge Chávez y fondos como ProInnóvate. Y las amenazas: competidores que ya aprovechan el CCTV, la analítica incluida en las cámaras, las exigencias de la Ley 29733 y cámaras de calidad variable. De ahí salen cuatro estrategias: usar el contacto en LAP para llegar a las tiendas del terminal y a JCDecaux; financiar las pruebas de precisión con ProInnóvate e incubadoras; procesar en la red del cliente para reforzar la privacidad; y vender estudios cortos pagados mientras resolvemos la licencia del detector.

### Diap. 23 — Organigrama

El proyecto tiene al docente del curso como patrocinio académico y a LAP como socio externo. Yo soy líder del proyecto, Data Engineer y Scrum Master; Andrés Quiliche es Data Scientist, a cargo de los modelos de visión; Gael Almeyda lleva la comunicación y la documentación; y Ema López, la integración, el repositorio y los casos de uso.

### Diap. 24 — Business Model Canvas

Nuestra propuesta de valor es medir la captación de cada tienda —cuántos pasan, se detienen, entran y compran— y la audiencia de cada pantalla, con las cámaras que ya existen. Los segmentos son aeropuertos, centros comerciales, retail y boticas, universidades y dueños de pantallas. Llegamos a ellos con una demo en el teléfono, a través de LAP, de integradores de CCTV y de ACCEP. Los ingresos vienen de un diagnóstico de **S/ 900**, una prueba de vitrina de **S/ 1 200**, el monitoreo de **S/ 149 al mes** por tienda, estudios de audiencia de **S/ 4 000** y la certificación de pantallas a **S/ 180 al mes**. Los socios clave son LAP, los integradores de CCTV, ESAN y una asesoría legal.

### Diap. 25 — Costos

Procesar una cámara de tienda nos cuesta unos **S/ 43 al mes**, y una de pantalla, **S/ 46**: la mayor parte es la computadora, S/ 26 entre amortización y energía, más el traslado del video, el soporte y la nube central. Una tienda paga S/ 149 y tiene en promedio 1,5 cámaras, unos S/ 99 por cámara, así que el margen bruto es de **57 % en tiendas y 74 % en pantallas**. Cada 10 cámaras se necesita una PC con RTX 4060 de S/ 4 579, que cuesta S/ 255 al mes, y la línea base de costos del proyecto es de **S/ 19 534**.

### Diap. 26 — Costos en la nube

También cotizamos una máquina virtual con GPU T4 en Google Cloud, AWS y Azure, con sus calculadoras oficiales y en dólares. Encendida 240 horas al mes cuesta entre **US$ 129 y 137**; 360 horas, entre 192 y 201; y todo el mes, entre **387 y 392**, casi lo mismo en los tres. Por eso el procesamiento va en una computadora local, y la nube queda para la web, la base y los respaldos.

### Diap. 27 — Estado de resultados: VAN y TIR

Con una inversión inicial de S/ 19 534, los flujos crecen de S/ 30 800 en 2027 a S/ 927 000 en 2031. Descontados a un COK de 20 %, el **VAN es de S/ 798 311**, la **TIR de 294 %** y la relación beneficio-costo de **41,9**; la inversión se recupera en 2027, el primer año. Y el resultado aguanta: con un COK de 40 %, el VAN sigue en S/ 423 588, y aun si los flujos fueran la mitad o la cuarta parte, seguiría positivo, con S/ 389 388 y S/ 184 927.

### Diap. 28 — Bibliografía

Estas son las referencias principales: los antecedentes que revisamos y la documentación de los modelos que usamos, como YOLO26, OSNet, PersonViT y MSMT17. La lista completa está en el informe.

### Diap. 29 — Cierre

Para terminar: no se trata solamente de observar personas, sino de comprender cómo se utilizan los espacios. Eso es TrackFlow. Muchas gracias; quedamos atentos a sus preguntas.

---

## Preguntas probables

| Pregunta | Responde | Respuesta corta |
|---|---|---|
| ¿Qué tan preciso es? | Gael | El detector encuentra el 69 % de las personas con 70 % de precisión; el Re-ID acierta 94,3 % a 2 s. El límite es el seguimiento en lugares llenos (IDF1 de 40 %). |
| ¿Entrenaron los modelos? | Gael | No: son preentrenados (YOLO26 en COCO; OSNet-AIN y PersonViT en MSMT17) y se validaron con 12 videos etiquetados. |
| ¿Cuántas cámaras soporta? | Andrés | Dos cámaras 1080p a unos 17 cuadros por segundo cada una en tiempo real; las siete de WILDTRACK, por lotes a 5 cuadros por segundo, en 3 veces la duración del video. |
| ¿Cómo cuidan la privacidad? | Leslie | Sin reconocimiento facial ni imágenes guardadas, IDs seudónimos, vectores borrados a las 24 h, sin género, acceso por rol y bitácora. |
| ¿Por qué local y no en la nube? | Leslie | Una T4 encendida todo el mes cuesta unos US$ 390; una PC local atiende 10 cámaras por S/ 255 al mes. |

---

## Ajustes pendientes en las diapositivas

- **Diap. 7 (Flujo):** la figura todavía dice YOLO11m, ByteTrack, TransReID y «Modelo entrenado». Reemplazarla por `figuras/metodologia/flujo.png` del repositorio del informe.
- **Diap. 18 (Arquitectura):** el cuadro de modelo-vivo todavía dice «YOLO26, Re-ID, CLIP». Reemplazarlo por `figuras/arquitectura/arquitectura.png`.
- **Diap. 6:** cambiar 284 commits por 291.
- **Diap. 21:** igualar las cifras con el informe: S/ 62,3 M (no 62,0), ~34 900 locales en Perú (no 34 700), ~10 500 locales en Lima (no 10 400) y S/ 18,7 M (no 18,6). Axis es de «Suecia», no «Suicia».
- **Diap. 12:** «CLIP-RelD» → «CLIP-ReID».
- **Índice y encabezados:** con el Flujo movido antes de la Arquitectura, el índice y los números de sección de cada diapositiva (03, 04, 05…) deben seguir el nuevo orden; hoy las dos de Antecedentes dicen 04 y 05.
