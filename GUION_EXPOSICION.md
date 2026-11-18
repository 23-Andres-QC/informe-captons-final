# Guion de la exposición — TrackFlow

Para decirlo con las propias palabras, no para leerlo. Va en el orden de la exposición: la diapositiva 7 (Flujo) se pasa después de la 17. Los números de diapositiva son los del PDF actual. En cada una, «En pantalla» lista lo que se ve en el orden en que conviene señalarlo, y luego va el texto. Las cifras en **negrita** se dicen exactas y son siempre las del informe; donde la diapositiva dice otra cosa, la lista del final lo indica. Las líneas en cursiva que empiezan con «Si…» se dicen solo si la diapositiva no se corrigió a tiempo.

## Reparto y tiempos

| Orden | Expositor | Diapositivas | Contenido | Tiempo |
|---|---|---|---|---|
| 1 | Ema López | 1–6 y 8–10 | Portada, índice, problema y solución (3), planificación, antecedentes (2), algoritmos | 6 min |
| 2 | Gael Almeyda | 11–17 | Dataset, pruebas y test (2), casos test, casos de uso (3) | 5 min 45 s |
| 3 | Andrés Quiliche | 7 (movida), 18–19 y demo | Flujo del sistema, arquitectura y base de datos, demo en vivo | 3 min 15 s + 5 min de demo |
| 4 | Leslie Neciosup | 20–29 | Ética, mercado, FODA, organigrama, Canvas, costos (2), VAN y TIR, bibliografía, cierre | 7 min 30 s |
| | | | **Total** | **≈ 27 min** |

Los tiempos suponen unas 165 palabras por minuto. Para quedar en 25 minutos, se resumen en una frase: los seis antecedentes complementarios (diap. 8), el tercer gráfico de la diap. 13 (solo el 85 %), los ingresos del Canvas (diap. 24, se señalan sin leer) y la tabla de escenarios (diap. 27, solo «siempre positivo»).

---

## 1. Ema López — Problema, planificación, antecedentes y algoritmos (6 min)

### Diap. 1 — TRACKFLOW · Inteligencia comercial y operativa mediante visión artificial

**En pantalla:** el logo del ojo y «Capstone Project · 2026»; el título y el subtítulo; la mascota con el teléfono; a la derecha, una foto con cajas «persona · 0.96» y el aviso «EN VIVO · Detección de flujo»; abajo, el equipo y la Universidad ESAN.

Buenos días. Somos el equipo de TrackFlow, de la Universidad ESAN: Leslie Neciosup, Andrés Quiliche, Gael Almeyda y yo, Ema López.

TrackFlow convierte lo que ven las cámaras en datos sobre cómo se mueve la gente. En la foto, cada persona lleva una caja y el número es la confianza del detector. Nació del caso del aeropuerto Jorge Chávez, con el apoyo de Lima Airport Partners, LAP.

### Diap. 2 — Índice

**En pantalla:** doce tarjetas numeradas, de «01 Problema y solución» a «12 Bibliografía».

Son doce puntos en cuatro bloques. Yo presento el problema, la planificación, los antecedentes y los algoritmos. Gael, los datos, las pruebas y los casos de uso. Andrés, el flujo, la arquitectura y una demo en vivo. Y Leslie, la gestión y el plan de negocio.

*Si el índice todavía pone el Flujo en el 06: «El flujo del sistema lo verán con Andrés, junto a la arquitectura».*

### Diap. 3 — ¿Qué problema buscamos resolver?

**En pantalla:** la foto de un centro comercial de varios pisos; a la derecha, cinco franjas con ícono: caminante, ruta, tienda, cronómetro y carrito.

Un aeropuerto o un centro comercial recibe miles de personas al día y sabe poco de lo que hacen adentro. Tiene cámaras, pero son de seguridad y cada una trabaja sola.

Queremos responder cinco preguntas, una por franja: cuánta gente pasa; por dónde va entre zonas; cuántos pasan frente a cada local; cuánto se quedan y dónde se amontonan; y si esas visitas terminan en compras. Lo difícil es seguir a la misma persona de una cámara a otra sin ver su cara.

### Diap. 4 — ¿Cómo afecta a las organizaciones?

**En pantalla:** dos columnas, «Situación» e «Impacto», con cinco filas unidas por flechas; a la derecha, un tablero de vuelos.

Cada fila va de una situación a su impacto. Sin conocer el flujo, los espacios se reparten mal. Sin medir visitas, no se sabe qué tan atractivo es un local. Sin datos de permanencia, el visitante es una incógnita. Sin conversión —cuántos de los que entran compran—, no se miden los resultados. Y con aglomeraciones frente a la tienda, el cliente se va sin comprar.

Hoy todo esto se vigila a ojo, en pantallas divididas.

### Diap. 5 — La solución: TrackFlow

**En pantalla:** la pantalla de ingreso de TrackFlow sobre una foto del Jorge Chávez; a la derecha, la definición en el recuadro celeste.

El recuadro la define: una plataforma de analítica multicámara —une lo que ven varias cámaras— que usa las que ya existen y convierte sus imágenes en información comercial sobre cómo se mueve la gente.

Funciona en tres partes: detecta y sigue a cada persona en cada cámara; la reconoce al pasar a otra, le deja un solo código y la ubica en el plano, en metros; y con esos recorridos calcula visitas, permanencia, captación y mapas de calor.

A la izquierda, el ingreso: cada usuario ve solo lo que su rol permite. Y por diseño no reconoce rostros, no guarda imágenes de las personas ni estima su género.

### Diap. 6 — Planificación del proyecto

**En pantalla:** el diagrama de Gantt del panel Scrum (filas Leslie, Quiliche, Ema, Gael y Reuniones; semanas S5 a S16, con la S14 resaltada); el resumen del panel (96 tareas, 84 completadas, 12 sin iniciar, avance 87,5 %); abajo, cuatro tarjetas: Scrum Master, 16 sprints, 96 tareas y commits.

Usamos un enfoque híbrido: las entregas del curso tienen fecha fija y el desarrollo avanza con Scrum. Leslie Neciosup es la Scrum Master.

Son **16 sprints** semanales que cierran los miércoles. En el Gantt, cada color es una persona, cada columna una semana y cada rombo, una reunión de cierre. El panel registra **96 tareas**. Al corte del informe, el sprint 11, íbamos al 95 % de lo planificado, un leve atraso que corregimos priorizando lo indispensable. Hoy, en el sprint 14, van **84 de 96**: el **87,5 %**. Y el código está en GitHub, con **291 commits** al 17 de noviembre.

### Diap. 8 — Antecedentes revisados

**En pantalla:** tabla de nueve antecedentes con las columnas Dominio, Métrica clave y Aporte.

Revisamos nueve trabajos. Para leer la tabla: el mAP mide qué tan bien detecta un modelo, de 0 a 1; el MOTA, la exactitud del seguimiento, descontando personas perdidas, falsas alarmas y cambios de identidad; y el F1 equilibra aciertos y omisiones.

Tres son la base. Giannopoulou detecta con YOLO en el aeropuerto de Atenas, con un mAP de **0,87**: respalda nuestro detector. Zhao, en Shanghai, reconoce grupos en **3,21 a 4,49 segundos** solo con posiciones; de él tomamos los mapas de calor y las rutas frecuentes. Y Mendes re-identifica sin rostro en un comercio real, con **82 %** de acierto: lo más cercano a lo nuestro.

De los demás, Jin muestra que un modelo cae de 86,94 a 72,05 de F1 al cambiar de entorno, y por eso validamos con videos propios; y Yang verifica posiciones sobre un plano común, con un MOTA de **95,2 %**. El resto aporta la evaluación de eventos, la arquitectura de Dublín, el grafo origen-destino y un seguidor para aeropuertos.

### Diap. 9 — Diferencia con los antecedentes

**En pantalla:** arriba, tres imágenes: «Monitoreo operativo en vivo [1]», «Conteo y mapas de densidad [8]» y «Arquitectura de tracking [9]»; abajo, el cuadro celeste «Antecedentes» y el azul «TrackFlow».

Las imágenes son de tres antecedentes: el monitoreo en vivo de Atenas, los mapas de densidad de Wong y Law y el seguidor de Zhou.

El cuadro celeste es la brecha: cada uno resuelve una pieza, evaluada en su propio escenario, y ninguno entrega una cadena completa que se pueda instalar.

El azul es nuestra diferencia: detección, seguimiento, re-identificación, mapa 2D, minería e insights en un solo pipeline; en una computadora local con GPU; y con la privacidad en el diseño, con la Ley N.° 29733 como marco.

### Diap. 10 — Algoritmos del sistema

**En pantalla:** tabla de tres columnas (Parte, Algoritmo, Resultado) con seis filas: I. Seguimiento local, II. Multicámara, II. Mapa, III. Histórico, En vivo y Videos.

Cada fila dice qué algoritmo usamos y qué produce.

**Parte I.** YOLO26m marca a cada persona con una caja. El seguidor híbrido predice dónde estará y compara movimiento, solape de cajas, tamaño y apariencia; el algoritmo húngaro empareja de forma óptima las cajas nuevas con las trayectorias. Sale el local_id, estable dentro de cada cámara.

**Parte II.** YOLO26s-ReID resume el cuerpo en un vector de números, y una galería global compara esos vectores entre cámaras tras aplicar reglas físicas: nadie está a la vez en dos cámaras que no se ven entre sí. Sale el global_id, un seudónimo que solo vale en esa sesión. Y la homografía lleva el punto de los pies al plano del piso: la posición en metros.

**Parte III.** Sin volver al video, una máquina de eventos marca si alguien pasó frente a un local, entró, se quedó, salió o hizo cola; KDE dibuja el mapa de calor; PrefixSpan halla las rutas más repetidas; y el grafo origen-destino cuenta cuántos van de una zona a otra.

Las dos últimas filas son variantes: en vivo, desde teléfonos, con dos carriles y memoria de identidades; y Videos, para grabaciones de cualquier origen, con detección a 1280 píxeles y el Re-ID de OSNet-AIN con PersonViT.

*Transición:* Con qué datos probamos todo esto, y qué tan bien funciona, se los muestra Gael.

---

## 2. Gael Almeyda — Dataset, pruebas, casos test y casos de uso (5 min 45 s)

### Diap. 11 — Dataset

**En pantalla:** cuatro tarjetas con ícono (MSMT17, Wildtrack, 12 videos etiquetados y «Mall», vacía) y cuatro cifras abajo: 308, 4 372, 512 y 0.

Empiezo por la última cifra: **0** modelos entrenados. Todos son preentrenados: el detector, en el conjunto público COCO, y el Re-ID en MSMT17, la primera tarjeta, un conjunto público de personas etiquetadas entre cámaras. Nuestros videos no son para entrenar, sino para medir.

Medimos con **12 videos etiquetados**: seis pares de cámaras de interiores que miran el mismo lugar, en 1080p, a 23 cuadros por segundo y de unos cinco minutos. Tienen **308 personas** marcadas cuadro por cuadro, con el mismo código en ambas cámaras: la respuesta correcta. De cuatro pares salen **4 372 recortes** para comparar modelos de Re-ID. Y **512** es el tamaño del vector con que el Re-ID describe a una persona.

Para el estrés, **WILDTRACK**: siete cámaras 1080p a 60 cuadros por segundo sobre una misma plaza concurrida, en Zúrich.

*La tarjeta «Mall» está vacía: no se menciona.*

### Diap. 12 — Re-identificación y seguimiento

**En pantalla:** a la izquierda, barras de acierto a 2 s y a 10 s de cuatro modelos (OSNet-AIN; una sin nombre, que es PersonViT; CLIP-ReID; y OSNet + PersonViT); a la derecha, las tarjetas «58 → 78 % Acierto entre cámaras» y «−34 % Cambios de ID», y la tabla «Seguimiento (12 videos)» con Antes y Actual.

El gráfico mide esto: tomo a una persona y busco, entre todas las que ve esa cámara, la más parecida dos segundos después, o diez. Si es ella misma, es acierto; a diez segundos cuesta más, porque ya cambió de pose.

OSNet-AIN logra **92,4** y **80 %**; la barra sin nombre es PersonViT, 93,4 y 81; CLIP-ReID, 93,2 y 81,2. Promediar OSNet y PersonViT sube a **94,3** y **82,8 %**. Sumar CLIP-ReID ganaba medio punto y duplicaba el tiempo: lo descartamos.

La tarjeta azul es entre cámaras: la más parecida en la otra cámara es la misma persona el **78 %** de las veces, frente al **58 %** del modelo anterior.

La tabla es el seguimiento. El IDF1, la fracción del tiempo en que alguien conserva su código, sube de **35 a 40 %**. Los cambios de ID bajan **34 %**, de **4 422 a 2 940**, y cada persona recibe **2,9** códigos en vez de **3,8**: le dimos más peso a la apariencia, justo donde fallaba: al recuperar a alguien perdido o cuando dos personas se cruzaban. Y los IDs que mezclan a varias personas bajan de **60 a 28 %**, en un par revisado recorte por recorte: ahora cada tramo nuevo debe parecerse a cada tramo de la identidad, no solo a su promedio.

### Diap. 13 — Detector y rendimiento

**En pantalla:** tres gráficos de barras («Detector: personas encontradas (%)», «Cuadros/s por par de cámaras» y «7 cámaras: detecciones con ID (%)») y cinco tarjetas: 69 %, 70 %, 17, 68 % y 2.8 GB.

El recall es cuántas de las personas reales encontró el detector: el **69 %**. La precisión, cuánto de lo que marcó era de verdad una persona: el **70 %**. Se midió en 16 cámaras de interiores, y la cifra real es algo mayor, porque las etiquetas dibujan el cuerpo entero aunque lo tape un estante.

El primer gráfico muestra su punto débil, la distancia: encuentra el **52 %** de las personas de menos de 100 píxeles de alto, el 66 % de las de 100 a 200 y el 69 % en total. Por eso Videos detecta a 1280 píxeles.

El segundo son cinco pares de cámaras 1080p: de 13 a 18 cuadros por segundo por video, y el más lento es el de más gente. En promedio, **17**, con la GPU al **68 %** y **2,8 GB** de memoria. El cuello de botella no es la GPU, sino la CPU, que corre el seguidor.

El tercero son las siete cámaras de WILDTRACK: a 1,5 cuadros por segundo, el 76 % de las detecciones lleva ID global; a 5 fijos, el **85 %**, y dos minutos de video toman seis. Con muchas cámaras, conviene procesar por lotes.

### Diap. 14 — Casos test sobre los datasets

**En pantalla:** tabla de nueve casos, CT-01 a CT-09, con Componente, Dataset, Métrica y Resultado.

La tabla ordena la validación en nueve casos, uno por pieza —detector, Re-ID, seguimiento, asociación entre cámaras, carga y estrés—, cada uno con su dataset y su métrica. Las cifras ya las vieron; la lectura de conjunto: cada cambio se aceptó solo si mejoraba la medición, y el límite que reconocemos es el seguimiento en lugares muy llenos, con un IDF1 de 40 %. Subirlo exige un Re-ID dentro del propio seguidor, que es más lento.

### Diap. 15 — Resumen de casos de uso

**En pantalla:** once tarjetas, CU-01 a CU-11 (CU-02 y CU-04 en azul); la tarjeta «2 actores: Comercial · Administrador»; y los cuadros de requerimientos funcionales y no funcionales.

Hay **11 casos de uso** y **dos actores**: el usuario comercial, que consulta mapas de calor, insights, flujos y reportes, y en la plataforma es el rol negocio; y el administrador, que monitorea, configura el mapa y procesa videos: los roles superadmin y seguridad.

De los casos salen los requerimientos. Los funcionales dicen qué hace el sistema, de cámaras y zonas a reportes en PDF y CSV; los no funcionales, cómo debe hacerlo: con seguridad, protección de datos, rendimiento, integridad y disponibilidad. Los dos en azul son los ejemplos.

### Diap. 16 — Ejemplo 1 · CU-02: Monitorear personas en vivo

**En pantalla:** el diagrama del caso de uso (actor «Usuario administrador»; «Monitorear operación en vivo» con «Visualizar mapa 2D», «Activar capas», «Seleccionar persona» y «Consultar recorrido y eventos»); debajo, la captura de la pantalla En vivo; a la derecha, «Actor: usuario administrador · RF-02» y cinco pasos con ícono.

El primero es monitorear en vivo: actor administrador, requisito RF-02. En el diagrama, «include» es lo que siempre ocurre —ver el mapa 2D— y «extend», lo opcional: activar capas o seleccionar a una persona.

Los cinco pasos: entra a «En vivo»; ve el plano 2D con personas y zonas; activa las capas de trayectoria o de cámaras; selecciona a una persona; y revisa su recorrido y sus eventos. Si no hay datos, el sistema avisa «sin observaciones». Abajo está esa pantalla, que Andrés mostrará funcionando.

*Si sigue la captura antigua: «Es de las pruebas de campo, cuando el sistema todavía se llamaba AeroVision».*

### Diap. 17 — Ejemplo 2 · CU-04: Configurar zonas del mapa

**En pantalla:** la captura de Configuración con el plano y la lista de zonas; cuatro pasos numerados; a la derecha, el diagrama con «Configurar zonas del mapa», «Crear zona», «Modificar zona», «Eliminar zona» y «Validar polígono y datos».

El segundo es configurar zonas, también del administrador: requisito RF-03. Pulsa «Editar» y «+ Zona», marca los vértices en el plano, define nombre, tipo, color y local, y el sistema valida y registra la zona. En el diagrama, crear y modificar siempre incluyen esa validación: un polígono inválido se rechaza.

El tipo es la clave: INTERIOR es el adentro de una tienda y FRONTAGE, la franja de enfrente, ambas atadas a un local. Con ellas la Parte III distingue a quien solo pasó de quien entró: de ahí sale la captación.

*Si sigue la captura antigua: «Es de la versión anterior; la actual ya no tiene el panel de género».*

*Transición:* Ya vieron qué hace cada usuario; Andrés les muestra cómo fluyen los datos por dentro y cómo está construido el sistema.

---

## 3. Andrés Quiliche — Flujo, arquitectura y demo (3 min 15 s + 5 min de demo)

### Diap. 7 (se pasa después de la 17) — Flujo del proyecto simplificado

**En pantalla:** cinco columnas numeradas y unidas por flechas: 1 «Flujo algorítmico por cámara» (recortes de personas con cajas de colores y la pila de modelos), 2 «Tracking multicámara» (tres cámaras con la misma persona, ID 0427), 3 «Mapeo del aeropuerto» (plano con Tiendas, Zona B, Food Court, Llegadas y Embarque, y un recorrido punteado), 4 «Rutas y patrones» (flujos, mapa de calor y grafo A-B-C-D) y 5 «Insights» (seis tarjetas).

Este es el camino de los datos, de izquierda a derecha.

Uno: en cada cámara se detecta y se sigue a cada persona, cada una con su color, con YOLO26m, nuestro seguidor y el Re-ID, todos preentrenados.

*Si sigue la figura antigua: «La pila de la figura es de una versión anterior: hoy son YOLO26m, nuestro seguidor y el Re-ID, y ningún modelo se entrenó».*

Dos: la misma persona pasa por tres cámaras y conserva un solo código, el 0427. Eso lo hace la galería global con sus reglas físicas.

Tres: cada posición se lleva al plano con la homografía. El punteado es un recorrido: de las tiendas al patio de comidas y a embarque.

Cuatro: de muchos recorridos salen los patrones: los flujos entre zonas, el mapa de calor y el grafo origen-destino, donde cada flecha cuenta cuántas personas pasaron de una zona a otra.

Cinco: los insights: visitas por tienda, captación, permanencia, densidad, horas de congestión y rutas predominantes. Esas cifras son de ejemplo; las reales las verán en la demo.

Y algo clave: los vectores de apariencia no se guardan con el análisis. En lo grabado viven solo en memoria, y en el en vivo la memoria de identidades los borra a las 24 horas. Lo que queda son puntos con un código seudónimo.

### Diap. 18 — Arquitectura general · Modulos Web

**En pantalla:** a la izquierda, el menú de la web con sus nueve módulos; a la derecha, el diagrama: arriba, el recuadro punteado «Procesamiento por lotes en la GPU, publicado por la API» (Videos grabados, Build, Insights); abajo, el recuadro «Computadora local con GPU RTX 4060 · Docker Compose» con web, backend, db, camara-web, modelo-vivo, backend-vivo y vivo-db; afuera, Usuario y Teléfonos.

Todo corre en una computadora local con una GPU RTX 4060: los servicios, en contenedores de Docker, y el modelo en vivo, directo sobre la GPU.

Sigo al usuario. Entra por HTTPS a la web, hecha en Vue 3 con TypeScript y servida por nginx, que revisa su sesión y su rol antes de dejarlo pasar a cada servicio. Detrás está el backend en Go, con arquitectura hexagonal: las reglas del negocio viven en un núcleo que no depende ni de la web ni de la base. Guarda sitios, tráfico, ventas y cuentas en PostgreSQL con PostGIS, que entiende zonas y puntos en metros.

Ahora los teléfonos. Mandan sus imágenes por WebSocket a camara-web; el modelo en vivo, en Python, detecta y reconoce a las personas y entrega detecciones y memoria a backend-vivo, que guarda esa memoria en otra base, con pgvector, hecha para buscar vectores parecidos.

*Si sigue la figura antigua: «En modelo-vivo ya no va CLIP: era el que estimaba el género, y lo quitamos».*

Arriba, lo grabado: el Build corre las Partes I y II y publica la sesión por la API, e Insights corre la Parte III sobre sus puntos. Y a la izquierda, los nueve módulos: cada rol ve solo los suyos; seguridad opera En vivo, Videos y Teléfonos, y negocio, Insights y Ventas.

### Diap. 19 — Base Datos

**En pantalla:** arriba, db (PostgreSQL 17 + PostGIS) con seis bloques: Espacio, Sesiones y trayectorias, Analítica (Parte III), Tráfico, Ventas, y Cuentas y seguridad; abajo, vivo-db (PostgreSQL 17 + pgvector) con personas, vistas y numeracion; al pie, «En rojo, las columnas que referencian otra tabla. Sin imágenes, rostros ni género».

Son dos bases. db guarda los sitios y todo lo que se analiza en ellos: el espacio, con cámaras, locales y zonas; las sesiones con sus identidades, tramos y puntos; los eventos y resultados de la Parte III; el tráfico, en ventanas de 5, 15 y 60 minutos; las ventas, unidas a la visita de cada compra; y las cuentas con su bitácora. Lo rojo son referencias entre tablas: por ellas cada evento se rastrea hasta su punto, su tramo, su cámara y su sesión.

Abajo, vivo-db es la memoria del en vivo: por persona, un vector de 512 valores, sus cámaras y cuándo se la vio; se borra a las 24 horas. Como dice el pie, no hay imágenes, rostros ni género, y los puntos se borran a los 90 días.

Ahora veamos el sistema funcionando.

### Demo en vivo (5 min)

**Antes de exponer:**

- Computadora enchufada, con los contenedores arriba (web, backend, db, camara-web, backend-vivo y vivo-db) y el modelo en vivo encendido en la GPU.
- Laptop y teléfonos en la misma red Wi-Fi. En cada teléfono, la página de cámara abierta (`https://<IP-de-la-computadora>:8444`), con el certificado y el permiso de cámara aceptados y el código de unión listo. Teléfonos cargados, apoyados fijos y apuntando a donde se va a caminar.
- Web con la sesión ya iniciada (no escribir contraseñas frente al público) y estas pestañas abiertas: Teléfonos, En vivo y Registros.
- Para empezar con códigos limpios: en Teléfonos, «Olvidar a todos» (vacía la memoria y la numeración vuelve a 1).
- Plan B listo: en Registros, ubicada la captura en vivo del pasillo del Edificio D del 9 de noviembre.
- Ensayar quién camina (Ema o Gael) y por dónde, para pasar de un teléfono al otro.

**Pasos y qué decir:**

1. **Teléfonos (30 s).** Mostrar la lista de cámaras conectadas. «Cualquier teléfono se vuelve cámara desde el navegador, sin instalar nada: lee el QR y se une con un código que viaja dentro de la conexión, nunca en la dirección. Aquí aparece conectado.»
2. **Activar en vivo (1 min 30 s).** Pulsar «Activar en vivo» y elegir la zona. «La captura se guarda solo en esta zona, y los teléfonos toman sus cámaras.» Un compañero camina frente al primer teléfono: «Cada persona lleva un punto de su color sobre la cabeza y su ID global, sin una caja que la tape. Un carril dibuja y sigue sin esperar, y el otro, en paralelo, compara el cuerpo con la memoria al segundo de verlo.» Camina hacia el otro teléfono: «Pasa a la otra cámara y conserva el mismo ID y el mismo color: la memoria lo reconoció por su ropa y su forma, no por su cara.» Señalar el plano: «Y aquí está en el plano, en metros: sin calibrar el teléfono, la distancia se estima por su tamaño en la imagen.»
3. **Terminar y guardar (30 s).** Pulsar «■ Terminar y guardar» e ir a Registros. «La captura queda aquí con su fecha, duración, personas y puntos. Se guardan puntos y códigos, no video.»
4. **Insights (2 min).** En la fila de la captura, «Insights». «Este es el tablero: personas, permanencia, captación, densidad, el mapa de movimiento y el de calor, rutas y flujos.» Arrastrar sobre la línea de tiempo: «Marco un tramo y el tablero se recalcula solo para ese tramo; puedo marcar hasta cinco, y una tabla los compara con la sesión completa.» Pulsar «Exportar PDF»: «El reporte sale en PDF con los mapas de cada tramo; el rol negocio solo puede exportar con un permiso temporal aprobado.»
5. **Videos (si sobran 30 s).** «También se suben grabaciones de cualquier origen, hasta diez a la vez: se procesan juntas, la misma persona lleva el mismo ID en todas, y el video se borra al terminar; queda solo el resumen.»

**Plan B:**

- Si falla la red o el modelo: en Registros, abrir la captura del pasillo del Edificio D del 9 de noviembre con «Mapear en el plano» (reproduce el seguimiento y el mapa de calor) y luego «Insights», y seguir desde el paso 4. «Les muestro una captura que hicimos en el campus con dos teléfonos.»
- Si solo responde un teléfono: que la persona salga del cuadro y vuelva: recupera su mismo ID, que es la memoria funcionando.
- Si el plano tarda en dibujar: quedarse en la vista de cámara y explicar los códigos sobre cada persona.

*Transición:* Ya lo vieron funcionando; Leslie les cuenta cómo cuidamos a las personas, cómo nos organizamos y cómo lo vamos a vender.

---

## 4. Leslie Neciosup — Ética, mercado, negocio y cierre (7 min 30 s)

### Diap. 20 — Ética y normas aplicables

**En pantalla:** a la izquierda, seis prácticas con ícono: sin reconocimiento facial, sin grabación en el flujo analítico, identificadores seudónimos, control de acceso por rol, vectores borrados en 24 h y bitácora de actividades; a la derecha, seis tarjetas: Ley N.° 29733, Ley N.° 31814, ISO/IEC 42001, ISO/IEC 27001, ISO/IEC 27701 e ISO 31000.

Un sistema que sigue personas tiene que cuidarlas desde el diseño. A la izquierda, lo que ya hace TrackFlow: no reconoce rostros; en el flujo de análisis no graba, las imágenes se procesan y se descartan; usa seudónimos, códigos que no dicen quién eres; cada rol ve solo lo suyo; los vectores de apariencia se borran a las 24 horas; y una bitácora registra cada ingreso y cada cambio.

A la derecha, el marco: la **Ley N.° 29733**, de protección de datos personales, y la **Ley N.° 31814**, sobre inteligencia artificial; y cuatro normas ISO: 42001 para gestionar la IA, 27001 para la seguridad de la información, 27701 para la privacidad y 31000 para los riesgos. No estamos certificados: las usamos para ordenar los controles. Y una matriz legal marca lo pendiente antes de producción; lo más delicado, los menores de edad.

### Diap. 21 — Mercado y competencia

**En pantalla:** a la izquierda, tres tarjetas de arriba abajo: TAM, SAM y SOM 2031; a la derecha, la tabla de competidores (Competidor, País de origen, Tipo, Precio de referencia); abajo, «US$ 1 450 M · Mercado mundial 2025» y «13 % · Crecimiento anual».

De contexto, abajo: el conteo de personas mueve en el mundo unos **US$ 1 450 millones** en 2025 y crece cerca de **13 % al año**.

En el Perú, el mercado total (TAM) es lo que se vendería si todos los locales que contamos, de centros comerciales a boticas de cadena, pagaran el monitoreo de S/ 149 al mes: unos **34 900 locales** y **S/ 62,3 millones al año**. El mercado atendible (SAM) es lo que podemos atender hoy: como máximo, los locales de Lima con una cámara IP usable, unos **10 500** y **S/ 18,7 millones**; como mínimo, las tiendas de los centros comerciales, S/ 16,4 millones. Y el mercado objetivo (SOM) es nuestra meta: **750 locales y 350 pantallas en 2031**, unos **S/ 2,1 millones al año**, del 7 al 8 % del atendible.

En la tabla, la competencia: sensores propios como V-Count o Xovis, contadores en la puerta como FootfallCam, la analítica de las cámaras Hikvision y Axis, y consultoras. Usar el CCTV existente ya no es exclusivo nuestro; nos defiende el recorrido completo frente a la vitrina, la audiencia de cada pantalla dentro del recinto y estar en Lima con precios en soles.

### Diap. 22 — Análisis FODA y estrategias cruzadas

**En pantalla:** a la izquierda, los cuatro cuadrantes (Fortalezas, Debilidades, Oportunidades y Amenazas); a la derecha, cuatro estrategias: FO, DO, FA y DA.

El FODA separa lo interno, fortalezas y debilidades, de lo externo, oportunidades y amenazas. Lo valioso son los cruces de la derecha.

**FO** usa una fortaleza para tomar una oportunidad: con la plataforma y nuestro contacto en LAP, llegamos a las tiendas del terminal y a JCDecaux, que opera los espacios publicitarios del nuevo Jorge Chávez. **DO** cubre una debilidad con una oportunidad: con ProInnóvate e incubadoras financiamos las pruebas de precisión en tiendas que aún faltan, mientras trabajamos a medio tiempo. **FA** responde a una amenaza con una fortaleza: ante la Ley 29733, vendemos privacidad por diseño y procesamos dentro de la red del cliente. Y **DA** reduce debilidades frente a amenazas: como aún no hay clientes pagados, empezamos con estudios cortos y pagados, y resolvemos la licencia AGPL del detector antes del primer contrato.

### Diap. 23 — Organigrama

**En pantalla:** arriba, «Docente del curso · Patrocinio académico» y, unida con línea punteada, «LAP · Socio externo»; al centro, la líder del proyecto; abajo, tres integrantes con su rol.

El docente del curso es nuestro patrocinio académico, y LAP, nuestro socio externo. Yo soy la líder del proyecto, Data Engineer y Scrum Master. Andrés es Data Scientist, a cargo de los modelos de visión; Gael lleva la comunicación y la documentación; y Ema, la integración, el repositorio y los casos de uso.

### Diap. 24 — Business Model Canvas

**En pantalla:** los nueve bloques: Socios clave, Actividades clave, Recursos clave, Propuesta de valor (al centro, en azul), Relación, Canales, Segmentos y, abajo, Estructura de costos y Fuentes de ingresos.

El Canvas resume en nueve bloques cómo creamos, entregamos y cobramos valor. Los segmentos: aeropuertos, centros comerciales, retail y boticas, universidades y dueños de pantallas. La propuesta de valor, al centro: decirle a una tienda en qué paso pierde a la gente —pasan, se detienen, entran, compran— y a un dueño de medios cuánta gente ve cada pantalla, con las cámaras que ya existen. Llegamos con una demo en el teléfono, por LAP, por integradores de CCTV y por ACCEP, el gremio de centros comerciales; y la relación es un diagnóstico, un informe mensual y una cuenta dedicada.

Los ingresos: diagnóstico, **S/ 900**; prueba de vitrina, **S/ 1 200**; monitoreo, **S/ 149 al mes** por tienda; estudio de audiencia, **S/ 4 000**; y certificación, **S/ 180 al mes** por pantalla. Por dentro: procesamos, calibramos y controlamos la calidad con el software, una PC con RTX 4060 y cuatro fundadores, junto a socios como LAP, ESAN y una asesoría legal.

### Diap. 25 — Costos

**En pantalla:** foto de cámaras; la tabla «Concepto por cámara al mes» con columnas Tienda y Pantalla; abajo, tres tarjetas: S/ 4 579, S/ 255 y S/ 19 534.

La unidad de costo es la cámara. Procesamos por lotes: tomamos lo que ya grabó el NVR, el grabador del cliente, lo procesamos a 5 cuadros por segundo y borramos el video.

Por cámara al mes: la computadora, entre amortización y energía, **S/ 26**; el traslado del video, S/ 3; el soporte y control de calidad, S/ 10 en tienda y 14 en pantalla; y la nube central, para la web, la base y los respaldos, S/ 4 o 3. Total: **S/ 43** por cámara de tienda y **S/ 46** por pantalla. Una tienda paga S/ 149 y suponemos 1,5 cámaras por tienda: unos S/ 99 por cámara. El margen bruto, lo que queda del precio tras el costo, es **57 %** en tiendas y **74 %** en pantallas.

Abajo: cada 10 cámaras, una PC con RTX 4060 de **S/ 4 579**, que cuesta **S/ 255** al mes. Y **S/ 19 534** es la línea base de costos del proyecto: las horas del equipo, la computadora, el primer despliegue en la nube y una reserva del 10 %.

### Diap. 26 — Costos a nivel de nube

**En pantalla:** foto de cámaras y la tabla de horas al mes (240, 360 y 720) por Google Cloud, AWS y Azure.

¿Y la nube? Cotizamos una máquina virtual con una GPU T4 en las calculadoras oficiales de Google Cloud, AWS y Azure, en dólares al mes. Encendida 240 horas, cuesta entre **US$ 129 y 137**; 360 horas, entre 192 y 201; y todo el mes, entre **387 y 392**. Los tres quedan a menos de 6 % entre sí. Nuestra computadora sale unos **US$ 70 al mes**: la nube cuesta casi el doble con 240 horas y unas cinco veces y media todo el mes. Y así, además, el video no sale de la red del cliente.

### Diap. 27 — Estado de resultados: VAN y TIR

**En pantalla:** cuatro tarjetas (S/ 798 311 VAN con COK 20 %, 294 % TIR, 41.9 relación beneficio-costo y 2027 recuperación); la tabla por año (ingresos, flujo y valor presente); la tabla de escenarios; y el gráfico «Flujo para el equipo (miles de S/)».

¿Crea valor el negocio? Tres conceptos. El COK, costo de oportunidad del capital, es lo mínimo que le exigimos a nuestro dinero; suponemos un **20 %** anual. El VAN trae los flujos futuros a valor de hoy con ese COK y les resta la inversión: si es positivo, el negocio crea valor. Y la TIR es la tasa con la que el VAN sería cero: si supera al COK, conviene.

Invertimos **S/ 19 534**, y los flujos para el equipo van de S/ 30 800 en 2027 a S/ 927 000 en 2031, como muestra el gráfico; la última fila los trae a hoy. Resultado: **VAN de S/ 798 311**; **TIR de 294 %**; y relación beneficio-costo de **41,9**: lo descontado es casi 42 veces lo invertido. Se recupera en **2027**, el primer año.

La TIR es alta porque la inversión es pequeña: el software ya se hizo en el curso. Por eso la tabla de escenarios la pone a prueba: con un COK de **40 %**, el VAN sigue en S/ 423 588; con los flujos a la mitad, S/ 389 388; y a la cuarta parte, S/ 184 927. Siempre positivo. El límite que reconocemos: son proyecciones, todavía sin ventas reales.

### Diap. 28 — Bibliografía

**En pantalla:** veintiocho referencias en dos columnas.

Las referencias: los nueve antecedentes, los modelos y métodos que usamos, la Ley 29733, la guía PMBOK y las fuentes del mercado. La lista completa está en el informe.

### Diap. 29 — Cierre

**En pantalla:** el logo de TrackFlow sobre la foto de una cámara de seguridad y la frase de cierre.

Para terminar, la frase que nos guió: no se trata solamente de observar personas, sino de comprender cómo se utilizan los espacios. Eso es TrackFlow. Muchas gracias; quedamos atentos a sus preguntas.

---

## Preguntas probables

| Pregunta | Responde | Respuesta corta |
|---|---|---|
| ¿Qué tan preciso es el sistema? | Gael | Se mide por pieza. El detector encuentra el 69 % de las personas con 70 % de precisión (52 % de las de menos de 100 px); el Re-ID acierta el 94,3 % a 2 s y el 78 % entre cámaras. El límite es el seguimiento en lugares llenos: IDF1 de 40 % y 28 % de IDs que mezclan personas; mejorarlo exige un Re-ID dentro del seguidor, que es más lento. |
| ¿Entrenaron algún modelo? | Gael | No: todos son preentrenados (YOLO26 en COCO, ResNet18 en ImageNet; YOLO26s-ReID, OSNet-AIN y PersonViT en MSMT17). Los 12 videos etiquetados sirven para elegir y medir: cada cambio se aceptó solo si mejoraba la medición. Un seguidor especializado como AirMOTR necesitaría video anotado del propio aeropuerto. |
| ¿Cómo reconocen a la misma persona sin su cara? | Andrés | Por el cuerpo y la ropa: el Re-ID resume cada vista en un vector de 512 números y una galería global compara los tramos de todas las cámaras. Antes, reglas físicas descartan lo imposible (dos cámaras sin solape a la vez, pasos fuera de tiempo). Solo se une si el parecido supera el umbral, le gana claramente a la segunda opción y la preferencia es mutua. |
| ¿Qué pasa si alguien sale y vuelve? | Andrés | En lo grabado, quien no se ve en 120 s se cierra y se borran sus vistas: si vuelve, es otra persona para el sistema. En el en vivo, la memoria de identidades lo reconoce y le devuelve su ID y su color, hasta 24 horas después de verlo por última vez. |
| ¿Cómo ubican a una persona en el plano con un teléfono? | Andrés | Si el teléfono está calibrado, con una homografía (se calibra con una grabación de alguien caminando, suponiendo 1,70 m de altura). Si no, se estima la distancia por la altura aparente de la persona y el ángulo por su posición en la imagen, desde la posición y el rumbo del teléfono en el plano. En las grabaciones de ESAN se usaron referencias de cada cámara: 24 de 24 cuadros reconocidos. |
| ¿Cuántas cámaras soporta una computadora? | Andrés | Procesando como en vivo, dos cámaras 1080p a unos 17 cuadros por segundo cada una. Las siete cámaras de 60 fps de WILDTRACK se procesan completas por lotes a 5 cuadros por segundo, en 3 veces la duración del video. El límite es la CPU, que corre el seguidor; la GPU va al 28–38 %. En el negocio, una PC atiende 10 cámaras procesando por lotes; en tiendas eso supone un modo liviano, sin Re-ID, que todavía está por construir. |
| ¿Los vectores de apariencia no son datos biométricos? | Leslie | Es un punto abierto y lo decimos así: en la matriz legal (L3) es una brecha por verificar, de prioridad alta. No usamos el rostro ni guardamos recortes, y los vectores del en vivo se borran a las 24 horas, pero antes de producción hace falta una opinión legal escrita. |
| ¿Y los menores de edad? | Leslie | Desplegar sin cambios incumpliría el reglamento de la Ley 29733 (L4): los menores de 14 años necesitan el consentimiento de sus padres. La propuesta es privacidad por diseño para todos, en vez de intentar detectar a los niños. |
| ¿Por qué una computadora local y no la nube? | Leslie | Una T4 a demanda cuesta de US$ 129 a 137 al mes con 240 horas y de 387 a 392 todo el mes; la PC, unos US$ 70. Además, el video no sale de la red y el costo es fijo. Con pocas horas hay opciones más baratas (Vast.ai o máquinas spot), pero pueden detenerse en cualquier momento. La nube queda para la web, la base y los respaldos. |
| ¿En qué se diferencian de la analítica que ya traen las cámaras? | Leslie | Hikvision o Axis cuentan cruces de una línea dentro de la cámara. TrackFlow sigue el recorrido completo, con cámaras de cualquier marca —pasan, se detienen, entran y compran— y mide la audiencia de cada pantalla dentro del recinto, con soporte en Lima y precios en soles. |
| ¿Por qué la TIR es tan alta? ¿Es creíble? | Leslie | Porque la inversión es pequeña (S/ 19 534): el software se hizo en el curso. Por eso se lee con el VAN y la sensibilidad: con flujos a la cuarta parte, el VAN sigue en S/ 184 927. La prueba de estrés financiera da lo mismo: si los locales pagan S/ 99 y no 149, si se consigue la mitad de locales, si el costo por cámara se duplica o si la publicidad vende la mitad, la caja de 2031 queda entre S/ 609 mil y 728 mil (base: S/ 927 mil); y si nadie compra en 2027, la reserva sigue intacta. Son proyecciones sin ventas todavía. |
| ¿Por qué ya no estiman el género? | Leslie | Porque inferirlo por la apariencia produce clasificaciones binarias y estereotipadas que además pueden fallar, y el reglamento de la Ley 29733 cuenta la identidad de género entre los datos sensibles. Se quitó del procesamiento y de los reportes: no se calcula ni se guarda. Algunas capturas de las pruebas de campo aún lo muestran porque son de la versión anterior, AeroVision. |

---

## Ajustes pendientes en las diapositivas

Verificados contra el PDF actual (29 páginas) y el informe. El guion ya usa las cifras del informe; esto es lo que conviene corregir en las diapositivas.

**Contenido (corregir antes de exponer)**

1. **Diap. 7 (Flujo):** la pila de modelos todavía dice YOLO11m, ByteTrack, TransReID y «Modelo entrenado». Reemplazar la figura por `figuras/metodologia/flujo.png` del repositorio del informe (YOLO26m, Seguidor, Re-ID y «Modelos preentrenados»). Las cifras de las tarjetas de Insights (128,450 visitas, 32.4 %, 4.6 min, 8:00–10:00) son de ejemplo y no salen del informe (también están en la figura del informe): marcarlas como ejemplo o no citarlas.
2. **Diap. 18 (Arquitectura):** el cuadro de modelo-vivo todavía dice «YOLO26, Re-ID, CLIP». Reemplazar la figura por `figuras/arquitectura/arquitectura.png` («YOLO26 y Re-ID»).
3. **Diap. 16 y 17 (casos de uso):** los diagramas dicen «Sistema AeroVision»: cambiarlos por `figuras/casos-de-uso/cu-02.png` y `cu-04.png` («Sistema TrackFlow»). Las capturas llevan la marca AeroVision, y la de la 17 muestra además «Privacidad del género · Tamaño mínimo de grupo», que ya no existe: cambiarla por `figuras/arquitectura/web-configuracion.png`. Para la 16 no hay en el repositorio del informe una captura de En vivo con la marca nueva: tomarla de la web local.
4. **Diap. 21 (Mercado):** igualar con el informe: ~34 900 locales (no 34 700), ~S/ 62,3 M (no 62,0), ~10 500 locales (no 10 400) y ~S/ 18,7 M (no 18,6). El TAM dice «(mercado global)», pero sus cifras son del Perú: poner «(Perú)»; el mundial ya está en la tarjeta de US$ 1 450 M. El SAM dice «(Perú: aeropuertos y retail)», pero en el informe los aeropuertos no se cuentan y el SAM es un rango: como mínimo las 9 155 tiendas de 91 centros comerciales (~S/ 16,4 M) y como máximo los ~10 500 locales de Lima con cámara IP (~S/ 18,7 M). Axis es de «Suecia», no «Suicia». «-S/ 18.6 M» y «-S/ 2.1 M» → «~S/»; «= 13 %» → «~13 %». La columna «País de origen» no está en el informe (no tiene fuente). En Xovis, el informe dice «más licencia».
5. **Diap. 6 (Planificación):** «284 commits» → «291 commits» (informe, al 17 de noviembre). «87,5 % completadas al sprint 14» no está en el informe, cuyo corte de gestión es el sprint 11 (60 de 96 completadas, 63 %; avance promedio 65,5 %; SPI 0,95): poner la fecha en la tarjeta («al 17 de noviembre») o actualizar el informe. «3 autores» tampoco está en el informe y el equipo es de cuatro: quitarlo.
6. **Diap. 12 (Re-ID y seguimiento):** «CLIP-RelD» → «CLIP-ReID». La segunda barra no tiene nombre: es «PersonViT» (93,4 / 81,0). En la tabla «Seguimiento (12 videos)», la fila «IDs que mezclan personas» (60 → 28 %) se midió en un par de 5 min revisado recorte por recorte, no en los 12 videos: separarla o anotarlo.
7. **Diap. 11 (Dataset):** la tarjeta «Mall» está vacía y no corresponde a ningún dato del informe: quitarla o cambiarla por un dato del informe, por ejemplo «Grabaciones de campo en ESAN» (patio, comedor y pasillo del Edificio D; 149 y 128 personas el 9 de noviembre). «Wildtrack · Dataset de 7 camaras» → «WILDTRACK · 7 cámaras 1080p a 60 fps».
8. **Diap. 27 (VAN y TIR):** el título «Estado de resultados» no corresponde a lo que se muestra (flujos de caja y su evaluación); el informe la llama «Evaluación financiera: VAN, TIR y COK». El eje del gráfico dice 1 a 5: poner 2027 a 2031.
9. **Diap. 13 (Detector y rendimiento):** los ejes de los tres gráficos no tienen nombres: 1–3 son «< 100 px, 100–200 px, todas»; 1–5 son los pares «P1 a P5»; 1–3 son los ritmos «1,5 c/s, adaptado, 5 c/s».
10. **Diap. 28 (Bibliografía):** faltan las fuentes de cifras que se muestran: Mordor Intelligence (US$ 1 450 M y ~13 %, diap. 21), WILDTRACK (Chavdarova et al., CVPR 2018; diap. 11, 13 y 14) y CLIP-ReID (Li et al., AAAI 2023; diap. 12). Intel Market Research figura como 2025; en el informe es 2026.

**Forma y numeración**

11. **Índice (diap. 2) y encabezados:** con el Flujo después de los casos de uso, el índice queda: 01 Problema y solución · 02 Planificación · 03 Antecedentes · 04 Algoritmos y dataset · 05 Pruebas y test de algoritmos · 06 Casos test · 07 Casos de uso · 08 Flujo del sistema · 09 Arquitectura del sistema · 10 Planificación y gestión del proyecto · 11 Plan de negocio · 12 Bibliografía. Encabezados a cambiar: diap. 7 «03» → «08»; diap. 8 y 9 «04» y «05» → «03»; diap. 10 y 11 «06» → «04»; diap. 12 y 13 «07» → «05»; diap. 14 «07» → «06»; diap. 15 a 17 «08» → «07». La diap. 21 (Mercado) es del plan de negocio: «11 · Plan de negocio» en vez de «10».
12. **Número de página:** en las diap. 3, 4, 6, 8, 22, 27 y 28 se ve «‹#›» en lugar del número, y la 10 dice «13»: poner el número real o quitarlo.
13. **Diap. 9:** falta cerrar el paréntesis de «(Ley N.° 29733».
14. **Diap. 18 y 19:** «Modulos Web» → «Módulos web»; «Base Datos» → «Base de datos».
15. **Diap. 26:** falta decir qué se cotizó y en qué moneda («máquina virtual con GPU T4, US$ al mes»); los decimales llevan un espacio de más («129, 35» → «129,35»).
16. **Diap. 22:** en «Mercado crece 13 % al año» el «~» no se ve en el PDF (queda un hueco), y «Ley N.° 29733» se parte en dos líneas.
17. **Diap. 14:** los códigos CT-01 a CT-09 no existen en el informe (ahí, «casos de prueba» son las pruebas de campo en ESAN); las cifras sí coinciden. Si el jurado busca un CT en el informe, la respuesta es que la tabla resume la sección 7, «Pruebas de validación y estrés del modelo».
18. **Diap. 17:** el informe detalla como segundo ejemplo el CU-08, consultar insights (usuario comercial), para tener uno por actor; el CU-04 está en el Anexo. Se puede quedar el CU-04, pero entonces los dos ejemplos son del administrador.
19. **Diap. 1:** la foto tiene una etiqueta «grupo · 3», y TrackFlow no detecta grupos. Es decorativa; conviene quitarla para que no lleve a una pregunta.
