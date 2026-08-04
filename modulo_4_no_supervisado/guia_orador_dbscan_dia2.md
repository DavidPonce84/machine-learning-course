# 🎤 GUÍA EXTENSA DEL ORADOR (SPEAKER NOTES)
## Módulo 4 · Día 2 · DBSCAN — Detección de Anomalías por Densidad
### Instructor: David Ponce
### Auditorio: Ingenieros y Petrofísicos de Schlumberger (SLB)
### Universidad de Las Américas (UDLA)

---

> **📌 Cómo usar esta guía:** Cada diapositiva tiene 5 secciones: qué se ve en pantalla, profundización conceptual para ti como instructor, guion verbal que puedes leer textualmente, qué hacer con tu cuerpo/pantalla en ese momento, y notas con anécdotas o alertas. Al final, una sección de **Preguntas Frecuentes Anticipadas** con respuestas listas.

---

## DIAPOSITIVA 1 — PORTADA

### 🖼️ Visual / Contenido en Pantalla
Fondo oscuro institucional. Título: "Módulo 4: Machine Learning No Supervisado". Subtítulo: "Día 2: DBSCAN — Detectando Anomalías por Densidad". Instructor: David Ponce. Schlumberger (SLB). Agosto 2026.

### 🧠 Profundización Conceptual (Para el Docente)
El Día 2 es la continuación natural del Día 1 (K-Means). Mientras K-Means asume que todos los puntos pertenecen a algún grupo, DBSCAN introduce el concepto de RUIDO — puntos que no pertenecen a ningún cluster. Esta es una transición pedagógica clave: pasamos de "todo debe clasificarse" a "hay datos que simplemente son basura y debemos aislarlos". El cambio de mentalidad es el objetivo principal del día.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Buenos días a todos. Bienvenidos al Día 2 de nuestro Módulo 4 de Machine Learning No Supervisado. Ayer aprendimos a usar K-Means para agrupar pozos sin etiquetas previas — y funcionó muy bien con datos limpios. Pero hoy vamos a enfrentar la realidad del campo petrolero: los sensores fallan, los transmisores se desconectan, y los datos llegan corruptos. Vamos a aprender DBSCAN, un algoritmo que no solo agrupa, sino que también detecta automáticamente qué datos son BASURA y los aísla para que no contaminen nuestros modelos."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Mantén la portada mientras confirmas que todos están conectados. Transmite energía: este día resuelve un problema real que ellos enfrentan diariamente.

### 💡 Nota para el Instructor / Anécdota / Alerta
Anécdota personal: "En mi experiencia con datos de campo, al menos el 5-10% de los registros SCADA tienen algún tipo de corrupción. Si no los detectas, cualquier modelo predictivo que construyas sobre esos datos estará tomando decisiones basadas en fantasmas."

---

## DIAPOSITIVA 2 — RECAP K-MEANS

### 🖼️ Visual / Contenido en Pantalla
Dos columnas: ✅ Lo bueno de K-Means (agrupó 50 pozos, Yellowbrick, Z-Score) y ❌ Lo malo (fuerza outliers a clusters, distorsiona centroides). Recuadro dorado con pregunta detonante: "¿Qué pasa cuando tenemos datos CORRUPTOS que no deberían estar en ningún grupo?"

### 🧠 Profundización Conceptual (Para el Docente)
K-Means minimiza la suma de distancias al cuadrado (inercia). Matemáticamente, un outlier con valor extremo tiene una distancia ENORME a cualquier centroide. K-Means, para minimizar la inercia global, mueve los centroides hacia esos outliers, distorsionando TODOS los clusters. Un solo dato corrupto puede arruinar la agrupación de 500 pozos sanos. DBSCAN resuelve esto porque no minimiza inercia — evalúa densidad local.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Ayer K-Means nos dio resultados espectaculares: separamos 50 pozos en 3 categorías — alta producción, maduros y agotados. Yellowbrick nos ayudó a elegir K=3 científicamente, y el Z-Score evitó que el caudal dominara al water-cut.

Pero quiero que piensen en algo: ¿qué habría pasado si en nuestro dataset de 50 pozos hubiera UN solo registro corrupto? Imaginen un sensor que reporta Qo=99,999 barriles por día. K-Means no tiene la opción de decir 'este dato no pertenece a ningún grupo'. Lo fuerza a entrar en algún cluster. Y al hacerlo, JALA el centroide hacia ese valor absurdo, contaminando potencialmente a todos los pozos sanos de ese grupo.

Ese es el problema que DBSCAN viene a resolver. Y de eso trata nuestra clase de hoy."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Señala la columna ❌ con énfasis. Haz una pausa después de la pregunta detonante para que el concepto cale.

### 💡 Nota para el Instructor / Anécdota / Alerta
Pregunta interactiva opcional: "Levanten la mano: ¿quién ha recibido alguna vez un reporte de producción con un número ridículamente alto o bajo que sabían que era error del sensor?" Esto conecta inmediatamente con su experiencia.

---

## DIAPOSITIVA 3 — ANALOGÍA: LA FIESTA DEL BARRIO

### 🖼️ Visual / Contenido en Pantalla
Dos escenarios comparados: K-Means (organizador obsesivo que fuerza a todos) vs DBSCAN (organizador inteligente que aísla al intruso). Frase: "DBSCAN no obliga a nadie a pertenecer donde no debe."

### 🧠 Profundización Conceptual (Para el Docente)
Esta analogía es crucial. El salto mental de "todo debe clasificarse" a "hay cosas inclasificables" es el corazón filosófico del aprendizaje no supervisado con ruido. K-Means es un algoritmo de partición: divide el espacio en K regiones de Voronoi y TODO punto cae en alguna. DBSCAN es un algoritmo de densidad: solo asigna puntos que están en zonas densas; el resto es ruido.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Vamos a usar una analogía que les va a quedar grabada. Imaginen que organizan una fiesta en su casa. Tienen gente en la parrilla, gente en el karaoke, y gente en la piscina. Tres grupos naturales.

K-Means es como ese amigo obsesivo con el control que dice: '¡TODOS deben estar en un grupo!'. Ve a un señor que nadie invitó, que está borracho en una esquina, y lo agarra del brazo y lo sienta a la fuerza con el grupo de la parrilla. El grupo queda contaminado. Las conversaciones se vuelven incómodas. La dinámica se arruina.

DBSCAN es el amigo inteligente que mira al señor de la esquina y dice: 'Está solo. A 20 metros de todos. No lo molesto. No es parte de ningún grupo. Es ruido.'

Esa es la diferencia fundamental. DBSCAN respeta que hay puntos que simplemente NO PERTENECEN."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Gesticula la diferencia: para K-Means, haz gesto de agarrar y jalar. Para DBSCAN, gesto de manos abiertas "lo dejo en paz".

### 💡 Nota para el Instructor / Anécdota / Alerta
Refuerza que "ruido" no es un insulto al dato — es una categoría técnica. En DBSCAN, ruido = "no hay suficiente evidencia de densidad para agruparte". Es una decisión matemática, no un juicio de valor.

---

## DIAPOSITIVA 4 — SENSORES QUE MIENTEN

### 🖼️ Visual / Contenido en Pantalla
Tabla con 4 fallas típicas: transductor BHP desconectado (-999 psi), spike eléctrico (99,999 bpd), sensor congelado (287.3 psi × 3 días), medidor atascado (Qw=0 por 6 meses). Cada una con su ejemplo real SLB.

### 🧠 Profundización Conceptual (Para el Docente)
Cada una de estas fallas tiene una firma física distinta:
- **-999:** Es un código centinela (sentinel value). El PLC/RTU de Schlumberger está programado para enviar -999 cuando el transductor pierde la señal de 4-20 mA. NO es una medición — es un mensaje de error.
- **99,999:** Es un spike de un solo registro. Típicamente causado por interferencia electromagnética en el cableado del medidor multifásico Vx Spectra cuando una bomba ESP arranca.
- **Sensor congelado:** El transmisor de presión tiene un diafragma que puede trabarse mecánicamente. La lectura se congela en el último valor bueno. La desviación estándar se vuelve CERO — otra señal de alerta.
- **Medidor atascado:** El water-cut meter usa un sensor de capacitancia. Las incrustaciones de carbonato de calcio (scale) se acumulan en el sensor y falsean la lectura a cero.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Veamos la realidad del campo. Estos no son ejemplos de libro de texto — son fallas que ustedes han visto en sus operaciones diarias.

El -999 en BHP: no es que el pozo tenga presión negativa. Es el código que el RTU envía cuando el cable del transductor se dañó durante la completación.

El 99,999 bpd: no es que el pozo de repente produjo como Ghawar. Es una interferencia electromagnética cuando arrancó la bomba ESP y el Vx Spectra captó el ruido.

La presión congelada en 287.3 psi por 3 días seguidos: ningún yacimiento se comporta así. El transmisor simplemente se trabó.

Y el water-cut en cero por 6 meses: en un campo maduro, eso es imposible. El sensor tiene incrustaciones de carbonato.

Estos NO son 'outliers estadísticos' que puedas ignorar. Son DATOS CORRUPTOS. Y si entrenas un modelo de Machine Learning con esto, el modelo aprenderá patrones FALSOS."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Señala cada fila de la tabla. Haz una pausa después de cada falla para que los ingenieros asientan — ellos reconocen estos problemas.

### 💡 Nota para el Instructor / Anécdota / Alerta
Si algún alumno pregunta "¿y por qué no simplemente filtramos Qo > 10000?", dile: "Excelente pregunta. Lo vamos a cubrir en la diapositiva 12. Pero adelanto: el filtro simple no detecta combinaciones anómalas. Quédate con esa duda."

---

## DIAPOSITIVA 5 — LA PREGUNTA + DBSCAN

### 🖼️ Visual / Contenido en Pantalla
Pregunta central enorme: "¿Cómo detectamos automáticamente datos anómalos sin etiquetas ni reglas manuales?" Debajo: DBSCAN con su significado en inglés y traducción.

### 🧠 Profundización Conceptual (Para el Docente)
DBSCAN fue publicado en 1996 por Ester, Kriegel, Sander y Xu. Es el algoritmo de clustering basado en densidad más citado en la literatura. La idea clave: un cluster es una región de ALTA DENSIDAD separada de otras regiones de alta densidad por regiones de BAJA DENSIDAD. El ruido son los puntos que caen en las regiones de baja densidad.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Entonces la pregunta del millón es: ¿cómo detectamos estas anomalías de forma automática, sin que un ingeniero tenga que revisar manualmente miles de registros?

La respuesta se llama DBSCAN: Density-Based Spatial Clustering of Applications with Noise. En español: Agrupamiento Espacial Basado en Densidad con Detección de Ruido.

Fíjense que el propio nombre del algoritmo incluye la palabra NOISE. A diferencia de K-Means, DBSCAN fue diseñado desde cero para convivir con datos corruptos. No es una ocurrencia tardía — es su propósito fundamental."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Señala la palabra "NOISE" en el nombre del algoritmo. Es el punch line de esta slide.

### 💡 Nota para el Instructor / Anécdota / Alerta
DBSCAN significa literalmente "agrupamiento espacial basado en densidad de aplicaciones con ruido". El paper original ya reconocía que los datos reales TIENEN ruido.

---

## DIAPOSITIVA 6 — LOS 2 PARÁMETROS

### 🖼️ Visual / Contenido en Pantalla
Dos tarjetas grandes: EPSILON (ε) — radio de búsqueda, y MinPts — vecinos mínimos. Consecuencias de valores extremos para cada uno. Analogía del estacionamiento.

### 🧠 Profundización Conceptual (Para el Docente)
**Epsilon (ε):** Es el parámetro más crítico y el más difícil de calibrar. Un ε muy pequeño significa que solo puntos extremadamente cercanos se consideran vecinos → todo es ruido. Un ε muy grande significa que puntos lejanos se consideran vecinos → todo es un solo cluster gigante.

**MinPts:** La regla heurística es MinPts ≥ dimensiones + 1. Para datos en 2D, MinPts ≥ 3. En la práctica se usa MinPts = 4 para 2D, y MinPts = 2×dimensiones para datos de alta dimensionalidad. Valores más altos de MinPts hacen a DBSCAN más robusto al ruido pero pueden fusionar clusters cercanos.

### 🗣️ Qué decir (Guion Verbal Extenso)
"DBSCAN solo necesita DOS parámetros. Vamos a entenderlos:

EPSILON — piensen en un círculo imaginario alrededor de cada punto. Epsilon es el RADIO de ese círculo. La pregunta es: ¿qué tan lejos estoy dispuesto a mirar para considerar que otro punto es mi 'vecino'?

MinPts — de todos los puntos que cayeron dentro de mi círculo, ¿cuántos necesito como mínimo para decir 'esto es un grupo, no una casualidad'?

Usemos la analogía del estacionamiento del mall: epsilon es la distancia que caminas para considerar que dos autos están 'juntos'. MinPts es cuántos autos necesitas para decir 'esto es un grupo de amigos que llegaron juntos'. Si ves UN auto solo a 50 metros de todos los demás — no es un grupo, es alguien que estacionó lejos. RUIDO."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Con las manos, dibuja un círculo en el aire al explicar epsilon. Para MinPts, muestra contar con los dedos.

### 💡 Nota para el Instructor / Anécdota / Alerta
**Alerta didáctica:** Los estudiantes suelen confundir MinPts con K. NO es lo mismo. K en K-Means es "en cuántos grupos divido". MinPts en DBSCAN es "cuánta evidencia necesito para considerar que esto ES un grupo". DBSCAN descubre K solo; no se lo das.

---

## DIAPOSITIVA 7 — LOS 3 TIPOS DE PUNTOS

### 🖼️ Visual / Contenido en Pantalla
Tres tarjetas con iconos: 🔵 Core Point, 🟢 Border Point, 🔴 Noise (-1). Cada una con definición breve y ejemplo petrolero.

### 🧠 Profundización Conceptual (Para el Docente)
- **Core Point:** Si en un radio ε alrededor del punto p hay al menos MinPts puntos (incluyendo a p mismo), entonces p es un core point. Estos puntos forman el "esqueleto" denso del cluster.
- **Border Point:** p no es core point (tiene < MinPts vecinos), pero está a distancia ≤ ε de algún core point. Están en la periferia del cluster.
- **Noise Point:** No es core ni border. No pertenece a ningún cluster.

**Algoritmo:** DBSCAN empieza en un core point y expande el cluster agregando todos los puntos alcanzables por densidad (todos los core points conectados + sus border points). Luego salta a otro core point no visitado. Los puntos que nunca son visitados son ruido.

### 🗣️ Qué decir (Guion Verbal Extenso)
"DBSCAN clasifica cada punto en una de tres categorías. Esto es fundamental:

CORE POINT — el corazón del cluster. Si dibujo un círculo de radio epsilon alrededor de este punto y encuentro al menos MinPts vecinos, este punto es un CORE. En términos petroleros: un pozo que tiene al menos 4 vecinos con presión y caudal similares. Es el centro de la acción.

BORDER POINT — está en la orilla. No tiene suficientes vecinos para ser core, pero está cerquita de uno que sí lo es. Como un pozo en el borde del yacimiento: su comportamiento es mixto, pero sigue conectado al grupo.

NOISE — y este es el protagonista del día. El punto que NO es core NI border. DBSCAN le asigna la etiqueta -1. En nuestro contexto: un sensor que reportó una lectura físicamente imposible. DBSCAN lo aísla."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Para Noise, haz un gesto de "descartar" con la mano. El -1 debe quedar grabado como "esto es basura".

### 💡 Nota para el Instructor / Anécdota / Alerta
Énfasis clave: "-1 no es un error del algoritmo. Es INFORMACIÓN. Significa: este registro no se parece a nada. Investígalo."

---

## DIAPOSITIVA 8 — CALIBRANDO EPSILON

### 🖼️ Visual / Contenido en Pantalla
4 pasos numerados para calibrar ε. Diagrama conceptual de la curva k-distance con el codo marcado. FAQ: ¿qué pasa si pongo ε antes/después del codo?

### 🧠 Profundización Conceptual (Para el Docente)
El gráfico de k-distance es la herramienta estándar para calibrar ε. El procedimiento:
1. Para cada punto, calcula la distancia a su k-ésimo vecino más cercano (donde k = MinPts - 1, típicamente 3 si MinPts=4)
2. Ordena todas estas distancias de menor a mayor
3. Grafica: el codo es el punto donde la pendiente cambia abruptamente

**¿Por qué funciona?** Los puntos en zonas densas tienen distancias pequeñas a sus vecinos. Los puntos aislados tienen distancias enormes. La transición entre estos dos regímenes es el codo. Poner ε en el codo significa: "todo lo que esté en zona densa se agrupa; todo lo que tenga distancia mayor queda como ruido".

### 🗣️ Qué decir (Guion Verbal Extenso)
"Epsilon no se adivina. Hay un método matemático para calibrarlo: el gráfico de k-distancias.

Paso 1: Para cada punto del dataset, calculamos la distancia a su 4to vecino más cercano. Si el punto está en una zona densa, esa distancia será pequeña. Si está aislado, será enorme.

Paso 2: Ordenamos todas esas distancias de menor a mayor.

Paso 3: Las graficamos. Verán una curva que empieza plana — son los puntos en zonas densas — y de repente se DISPARA hacia arriba. Ese quiebre es EL CODO.

Paso 4: El valor de distancia en el codo es nuestro ε.

Piensen en ordenar personas por altura: 1.60, 1.65, 1.68, 1.72, 1.75... todos similares. De repente: 2.20. ¡Un jugador de basketball! Ahí pones el límite."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Traza la curva con el dedo: plana, plana, plana... ¡disparo! Señala el punto de quiebre.

### 💡 Nota para el Instructor / Anécdota / Alerta
**FAQ incluida en la slide:** Si ε es muy pequeño → casi todo es ruido. Si ε es muy grande → todo se fusiona en un cluster. El codo es el balance óptimo. Si un alumno pregunta "¿y si no hay un codo claro?", responde: "Excelente pregunta. Significa que tus datos no tienen una estructura de densidad clara. Puede que todos los puntos estén uniformemente distribuidos. En ese caso, DBSCAN no es la herramienta adecuada."

---

## DIAPOSITIVA 9 — DBSCAN vs K-MEANS

### 🖼️ Visual / Contenido en Pantalla
Tabla comparativa de 6 características. Escenarios petroleros para cada algoritmo.

### 🧠 Profundización Conceptual (Para el Docente)
DBSCAN tiene ventajas claras sobre K-Means cuando hay ruido, pero NO es universalmente superior:
- DBSCAN sufre con datos de densidades muy variables (algunos clusters densos, otros dispersos). Un solo ε no puede capturar ambos.
- DBSCAN es más lento que K-Means en datasets grandes (O(n log n) con índices espaciales, O(n²) sin ellos vs O(n) de K-Means).
- DBSCAN no funciona bien en alta dimensionalidad (>10 dimensiones) porque el concepto de "distancia" se diluye (maldición de la dimensionalidad).

### 🗣️ Qué decir (Guion Verbal Extenso)
"Comparemos cara a cara:

K-Means asume clusters esféricos. DBSCAN acepta cualquier forma. ¿Por qué? Porque DBSCAN no usa centroides — usa densidad. Si tus datos forman una serpiente, DBSCAN la sigue; K-Means la parte en bolitas.

K-Means fuerza outliers a clusters. DBSCAN los marca como -1. Si tienes datos corruptos, K-Means se contamina; DBSCAN los aísla.

K-Means necesita que le digas K. DBSCAN descubre la cantidad de clusters solo. Aunque ojo: tienes que calibrar ε, que es un parámetro distinto.

Ambos necesitan escalado. Eso NO se negocia. Sin StandardScaler, ninguno de los dos funciona.

En la práctica petrolera: si tienes 500 pozos con datos limpios y quieres categorizarlos → K-Means. Si estás analizando telemetría de un pozo con interferencia y sospechas datos corruptos → DBSCAN."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Señala cada fila de la comparación. Para la última, haz el gesto de "depende" con las manos.

### 💡 Nota para el Instructor / Anécdota / Alerta
**Pregunta frecuente:** "¿Puedo usar DBSCAN para todo y olvidarme de K-Means?" Respuesta: No. K-Means es más rápido, más simple de explicar a gerencia, y cuando tus datos están limpios y son esféricos, funciona perfectamente. Son herramientas complementarias, no competidoras.

---

## DIAPOSITIVA 10 — ¿POR QUÉ SOLO 2 VARIABLES?

### 🖼️ Visual / Contenido en Pantalla
3 puntos con iconos explicando que el dataset tiene 2 columnas, que 2D es pedagógico, y que DBSCAN escala a N dimensiones. Recuadro importante con el código features = ['WHP','Qo',...].

### 🧠 Profundización Conceptual (Para el Docente)
**Esta es una de las preguntas más importantes que surgirán.** Los estudiantes vieron K-Means con 7 variables y ahora DBSCAN con solo 2. Pueden pensar que DBSCAN está limitado a 2D. NO es así.

La razón es puramente pedagógica y del dataset:
- `telemetria_anomala.csv` tiene solo 3 columnas: Registro_ID, WHP_psi, Qo_bpd. Es un dataset de UN pozo monitoreado en el tiempo.
- Con 2 variables podemos VISUALIZAR el ruido. Los estudiantes VEN los puntos -1 separados de la nube principal. En 7D no podríamos dibujarlo.
- DBSCAN internamente usa exactamente la misma lógica que K-Means: recibe un array X_scaled de shape (n_samples, n_features) y calcula distancias en ese espacio.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Me van a hacer esta pregunta, así que la respondo antes de que la hagan: 'David, ¿por qué K-Means usó 7 variables y DBSCAN solo 2? ¿DBSCAN no puede con más variables?'

Tres razones:

Primero, el dataset. telemetria_anomala.csv tiene 1,000 registros de UN SOLO pozo. Sus columnas son: ID, presión en cabeza, y caudal de crudo. Punto. No hay water-cut, no hay GOR, no hay BHP. Es telemetría simple.

Segundo, pedagogía. Con 2 variables, ustedes PUEDEN VER el ruido. Los puntos -1 aparecen volando lejos de la nube principal. Es visualmente obvio. Si trabajáramos en 7 dimensiones, no podríamos graficarlo — pero DBSCAN internamente SÍ estaría usando las 7.

Tercero, y lo más importante: DBSCAN FUNCIONA CON N DIMENSIONES. Si mañana tienen un dataset con water-cut, GOR, BHP, temperatura, vibración, y 10 variables más, DBSCAN las usa TODAS. Exactamente igual que K-Means. La única diferencia es que ESTE dataset de práctica tiene 2.

Para que quede claro: features = ['WHP_psi','Qo_bpd','Water_Cut','GOR','BHP_psi'] y DBSCAN calculará distancias en 5D. Sin problema."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Enfatiza el punto 3 con las manos: "DBSCAN escala a N dimensiones". Muestra el código features con tus dedos contando variables.

### 💡 Nota para el Instructor / Anécdota / Alerta
Si alguien insiste: "En el paper original de DBSCAN, los autores lo probaron con datasets de hasta 10 dimensiones. El algoritmo no tiene límite teórico de dimensionalidad. El límite práctico viene de la maldición de la dimensionalidad: en espacios de más de 10-15 dimensiones, TODAS las distancias se vuelven similares y el concepto de 'densidad' se diluye."

---

## DIAPOSITIVA 11 — CASO REAL SLB

### 🖼️ Visual / Contenido en Pantalla
Contexto: Vx Spectra con interferencia electromagnética. Tabla con 4 registros reales (2 normales, 2 anómalos) y el veredicto de DBSCAN.

### 🧠 Profundización Conceptual (Para el Docente)
El Vx Spectra es un medidor multifásico de Schlumberger que usa tecnología de resonancia magnética nuclear y un Venturi para medir caudales de petróleo, agua y gas sin separación. Está expuesto a interferencia electromagnética cuando equipos de alta potencia (como variadores de frecuencia de bombas ESP) operan cerca.

La anomalía "WHP normal, Qo mínimo" es físicamente una violación de la curva IPR: a mayor presión en cabeza, menor debería ser el caudal, pero no CERO. Un caudal de 50 bpd con 350 psi de cabeza sugiere que la válvula del choke está cerrada o el sensor de caudal está fallando.

La anomalía "WHP baja, Qo alto" viola la ecuación de flujo: sin presión que empuje, el fluido no puede vencer la presión hidrostática de la columna.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Veamos un caso concreto que podrían encontrar en sus operaciones.

Tienen un Vx Spectra midiendo Qo, Qw, Qg cada minuto. Un día, el variador de frecuencia de la bomba ESP genera una干扰 electromagnética. El medidor empieza a reportar lecturas erráticas.

DBSCAN recibe estos datos. Sin que nadie le diga qué es 'normal' y qué no, el algoritmo mira la densidad de puntos:

Registro 1: WHP=348, Qo=1180 → Cluster 0. DBSCAN dice: 'Este punto está en la zona densa. Es normal.'

Registro 2: WHP=352, Qo=52 → RUIDO -1. DBSCAN dice: 'Aquí hay 350 psi de presión, pero el caudal es casi cero. Esta combinación no ocurre en la zona densa. Algo raro pasa.'

Registro 3: WHP=81, Qo=1790 → RUIDO -1. DBSCAN dice: '¿80 psi empujando 1800 barriles? Físicamente imposible. Este dato está corrupto.'

Registro 4: WHP=355, Qo=1220 → Cluster 0. Normal.

DBSCAN detectó DOS anomalías SIN reglas manuales, SIN umbrales predefinidos, SIN etiquetas. Solo mirando la densidad."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Señala cada fila de la tabla en secuencia. Enfatiza "SIN reglas manuales".

### 💡 Nota para el Instructor / Anécdota / Alerta
Pregunta retórica: "¿Cuántas reglas 'if-else' necesitarían para cubrir todos los modos de falla posibles? ¿10? ¿100? DBSCAN no necesita ninguna."

---

## DIAPOSITIVA 12 — ¿POR QUÉ ANÁLISIS UNIVARIADO NO BASTA?

### 🖼️ Visual / Contenido en Pantalla
Tabla con 3 ejemplos donde el filtro simple falla. Frase clave: "El filtro univariado no detecta NINGUNA de estas anomalías."

### 🧠 Profundización Conceptual (Para el Docente)
**Este es el punto que conecta DBSCAN con el Módulo 2 (EDA y limpieza).** En el Módulo 2 aprendieron a limpiar outliers univariados: "si Qo > 10000, reemplázalo". Pero eso solo detecta anomalías en UNA variable a la vez.

DBSCAN detecta anomalías MULTIVARIADAS: combinaciones de valores que individualmente son plausibles pero juntos son imposibles. Es la diferencia entre:
- "Este caudal es muy alto" (univariado)
- "Esta presión es normal Y este caudal es normal, pero JUNTOS no tienen sentido físico" (multivariado)

Esta es la respuesta a la pregunta: "¿Por qué usar DBSCAN si ya limpiamos outliers en el Módulo 2?"

### 🗣️ Qué decir (Guion Verbal Extenso)
"Alguien podría decir: 'David, en el Módulo 2 ya aprendimos a limpiar outliers. Filtramos Qo > 10000, reemplazamos -999, y listo. ¿Para qué necesito DBSCAN?'

Excelente pregunta. Miren esta tabla.

Registro A: WHP=350, Qo=1500. Si filtro WHP > 0, pasa. Si filtro Qo < 10000, pasa. Todo 'normal'. Y efectivamente, ES normal.

Registro B: WHP=350, Qo=50. ¿WHP > 0? Sí. ¿Qo < 10000? Sí. ¿Qo > 0? Sí. PASA TODOS LOS FILTROS. Pero esta combinación es FÍSICAMENTE IMPOSIBLE: con 350 psi de presión en cabeza, el caudal no puede ser 50 bpd. Algo está estrangulando el pozo o el sensor de caudal está dañado.

Registro C: WHP=80, Qo=1800. Misma historia. Pasa todos los filtros univariados. Pero 80 psi no pueden empujar 1800 barriles contra la presión hidrostática de la columna de producción.

El filtro univariado NO DETECTA NINGUNA de estas anomalías. Porque cada variable por separado está 'en rango'. DBSCAN ve las DOS variables a la vez y detecta que la combinación no ocurre en la zona densa.

Esa es la diferencia entre limpieza univariada (Módulo 2) y detección multivariada de anomalías (Módulo 4). Son complementarias, no redundantes."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Para cada registro B y C, haz el gesto de "pasa... pasa... ¡pero es imposible!" Señala la contradicción física.

### 💡 Nota para el Instructor / Anécdota / Alerta
**Refuerzo clave:** "La limpieza del Módulo 2 elimina errores OBVIOS (-999, 99999). DBSCAN detecta errores SUTILES que solo se ven en combinación. Ambos son necesarios. Primero limpian lo obvio con reglas de dominio. Luego aplican DBSCAN para lo que las reglas no capturan."

---

## DIAPOSITIVA 13 — CÓDIGO DEL PIPELINE

### 🖼️ Visual / Contenido en Pantalla
3 bloques de código: escalar, calibrar ε, entrenar. Estilo terminal oscuro.

### 🧠 Profundización Conceptual (Para el Docente)
El pipeline es notablemente corto. DBSCAN en sklearn requiere solo 3 líneas de código real. La complejidad está en la CALIBRACIÓN (k-distance graph), no en la implementación.

**Consideraciones de implementación:**
- `eps` está en unidades Z después de StandardScaler. Un eps de 0.25 significa "busca vecinos en un radio de 0.25 desviaciones estándar".
- `min_samples` incluye el punto mismo. `min_samples=4` significa "el punto + 3 vecinos".
- `fit_predict` devuelve un array donde -1 es ruido. Los clusters se numeran desde 0.
- DBSCAN no tiene `predict` para nuevos puntos. No puedes "entrenar una vez y predecir después" como con K-Means. Para nuevos datos, debes re-entrenar.

### 🗣️ Qué decir (Guion Verbal Extenso)
"El código es sorprendentemente simple. Solo 3 pasos:

Paso 1: Escalar. Misma regla que K-Means. StandardScaler().fit_transform(). Sin esto, no funciona.

Paso 2: Calibrar ε. Usamos NearestNeighbors para calcular distancias al 4to vecino, ordenamos, graficamos, y buscamos el codo. En nuestro caso, el codo está en ε ≈ 0.25.

Paso 3: Entrenar. DBSCAN(eps=0.25, min_samples=4).fit_predict(X_scaled). Una sola línea. El resultado es un array donde -1 significa RUIDO.

Y ya está. Tres líneas de código. La verdadera habilidad no está en escribir más código, sino en entender qué significan los parámetros y cómo calibrarlos."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Cuenta con los dedos: 1, 2, 3. Enfatiza la brevedad del código.

### 💡 Nota para el Instructor / Anécdota / Alerta
**Advertencia:** DBSCAN de sklearn no tiene `predict()` para nuevos datos. Si un alumno pregunta "¿cómo clasifico datos nuevos sin re-entrenar?", la respuesta es: usa `HDBSCAN` (una evolución moderna de DBSCAN con `approximate_predict`) o re-entrena con todo el dataset.

---

## DIAPOSITIVA 14 — OBJETIVOS DEL DÍA

### 🖼️ Visual / Contenido en Pantalla
Checklist de 6 objetivos con checkmarks.

### 🧠 Profundización Conceptual (Para el Docente)
Verificación de aprendizaje. Al final de la clase práctica, cada estudiante debería poder responder estas 6 preguntas sin ayuda. Si alguno no puede, necesita refuerzo.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Al final de esta clase, quiero que cada uno de ustedes pueda decir:

✅ Sé explicar por qué K-Means falla cuando hay sensores dañados — y no es culpa del algoritmo, es que no fue diseñado para eso.

✅ Sé diferenciar un Core Point de un Border Point y de un Noise Point — y sé que -1 no es un error, es información valiosa.

✅ Sé calibrar Epsilon SIN adivinar — usando el gráfico de k-distancias y buscando el codo.

✅ Sé entrenar DBSCAN en Python y aislar datos corruptos — en 3 líneas de código.

✅ Sé interpretar resultados en contexto petrolero real — y explicarle a un colega por qué ese punto -1 probablemente es un sensor dañado.

✅ Sé que DBSCAN funciona con 2, 5, 10 o más variables — no está limitado a 2D."

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Lee cada objetivo con convicción. Haz una pausa después de cada uno.

### 💡 Nota para el Instructor / Anécdota / Alerta
Si el tiempo lo permite, haz una ronda rápida: "Levanten la mano si se sienten seguros con el objetivo 1... objetivo 2..." Esto te da feedback inmediato de qué reforzar en el laboratorio.

---

## DIAPOSITIVA 15 — CIERRE

### 🖼️ Visual / Contenido en Pantalla
Frase central: "¡Manos al Código! Abrimos Google Colab: 02_dbscan_outliers_estudiante.ipynb". Recordatorios clave en bullets. Logo UDLA.

### 🧠 Profundización Conceptual (Para el Docente)
Cierre de transición a la práctica. Los 4 recordatorios son los conceptos que NO deben olvidar cuando estén codeando.

### 🗣️ Qué decir (Guion Verbal Extenso)
"Con esto cerramos la teoría y abrimos Google Colab. Cuatro cosas que quiero que recuerden mientras codean:

Uno: -1 es ruido, es anomalía, es ¡alerta de sensor! No lo ignoren.

Dos: DBSCAN no necesita que le digas cuántos clusters hay. Él solo los descubre.

Tres: el escalado SIGUE siendo obligatorio. Sin StandardScaler, DBSCAN tampoco funciona.

Cuatro: DBSCAN funciona con 2, 5, 10 o más variables. No se dejen engañar por el ejemplo de hoy.

Abran el notebook 02_dbscan_outliers_estudiante.ipynb. Vamos a detectar anomalías en datos reales de telemetría. ¡Manos al código!"

### 🎬 Qué hacer en pantalla / Lenguaje corporal
Cambia a la pestaña de Google Colab. Muestra el notebook abierto. Energía alta para el laboratorio.

### 💡 Nota para el Instructor / Anécdota / Alerta
Asegúrate de haber verificado que el comando `!wget` del notebook funcione antes de la clase. Ten el dataset descargado localmente como backup.

---

# ❓ PREGUNTAS FRECUENTES ANTICIPADAS (FAQ)

---

## FAQ 1: ¿Por qué usamos solo 2 variables si K-Means usó 7?

**Respuesta:** DBSCAN puede usar tantas variables como quieras. El dataset `telemetria_anomala.csv` simplemente tiene 2 columnas numéricas (WHP, Qo). Si tuviera 7, DBSCAN las usaría todas. La limitación no es del algoritmo — es del dataset de práctica.

**Profundización:** Con 2 variables podemos VISUALIZAR el ruido en un plano 2D. Con 7 variables no podríamos graficarlo directamente (necesitaríamos PCA). El día de hoy es pedagógico: primero entendemos el concepto en 2D, y mañana (Día 3) usaremos PCA para visualizar clusters en datos de alta dimensionalidad.

---

## FAQ 2: ¿Para qué usar DBSCAN si en el Módulo 2 ya aprendimos a limpiar outliers, spikes y datos corruptos?

**Respuesta:** Son complementarios, no redundantes. La limpieza del Módulo 2 es UNIVARIADA: "si Qo > 10000, es un spike → reemplázalo". DBSCAN es MULTIVARIADO: detecta combinaciones anómalas que individualmente parecen normales.

**Ejemplo concreto:** WHP=350 y Qo=50. Filtro univariado: ¿WHP en rango? Sí (0-500). ¿Qo en rango? Sí (0-10000). PASA. Pero físicamente es imposible: con 350 psi de presión no produces solo 50 bpd. DBSCAN detecta que esta COMBINACIÓN es anómala porque no ocurre en la zona densa.

**Flujo recomendado:** Primero limpien outliers obvios con reglas de dominio (Módulo 2). Luego apliquen DBSCAN para detectar anomalías sutiles que las reglas no capturan (Módulo 4).

---

## FAQ 3: ¿DBSCAN reemplaza la limpieza manual de datos?

**Respuesta:** No. DBSCAN DETECTA anomalías — no las CORRIGE automáticamente. Tú como ingeniero debes decidir qué hacer con los puntos -1: ¿los eliminas? ¿los interpolas? ¿investigas el sensor? DBSCAN te dice "este punto es sospechoso", pero la decisión de ingeniería es tuya.

---

## FAQ 4: ¿Cómo sé si mi ε está bien calibrado?

**Respuesta:** Tres validaciones:
1. El número de puntos -1 debe ser razonable (típicamente < 10% del dataset). Si es > 30%, ε es muy pequeño.
2. El número de clusters debe tener sentido físico. Si todo es un solo cluster (o solo ruido), ε está mal.
3. Visualmente (si son 2D o 3D): los puntos -1 deben verse claramente separados de las zonas densas.

---

## FAQ 5: ¿Qué hago si el gráfico k-distance NO tiene un codo claro?

**Respuesta:** Significa que tus datos no tienen una estructura de densidad bien definida — posiblemente están uniformemente distribuidos. En ese caso DBSCAN no es la herramienta adecuada. Opciones: (a) prueba con más variables, (b) usa OPTICS (una evolución de DBSCAN que no requiere ε fijo), o (c) reconsidera si el clustering basado en densidad es el enfoque correcto.

---

## FAQ 6: ¿DBSCAN funciona con variables categóricas (ej. tipo de pozo, zona geográfica)?

**Respuesta:** DBSCAN usa distancias euclidianas, que requieren variables numéricas. Para variables categóricas necesitas: (a) codificarlas (one-hot encoding) y luego aplicar DBSCAN, o (b) usar una métrica de distancia personalizada (DBSCAN de sklearn acepta `metric='precomputed'`).

---

## FAQ 7: ¿Por qué MinPts = 4 y no 3 o 5?

**Respuesta:** La regla general es MinPts ≥ dimensiones + 1. Para 2D, MinPts ≥ 3. Usamos 4 por ser un valor conservador y común en la literatura. MinPts más grande = DBSCAN más robusto al ruido pero puede fusionar clusters cercanos. MinPts más pequeño = más sensible a variaciones locales. Con experiencia calibrarás este parámetro, pero 4 es un excelente punto de partida para 2D.

---

## FAQ 8: ¿Puedo usar DBSCAN con los mismos datos que usé para K-Means (50 pozos, 7 variables)?

**Respuesta:** Absolutamente. Cambia features a las 7 columnas, escala, calibra ε con NearestNeighbors en 7D, y entrena DBSCAN. La diferencia es que ahora los pozos que sean "raros" (diferentes a todos los demás) aparecerán como -1 en lugar de ser forzados a un cluster. Esto puede revelar pozos con comportamiento anómalo que K-Means camufló.

---

## FAQ 9: En el EDA del Módulo 2 corregimos el spike de 99,999 bpd manualmente. ¿DBSCAN habría detectado eso?

**Respuesta:** Sí. DBSCAN habría marcado ese registro como -1 porque 99,999 está extremadamente lejos de la zona densa de caudales normales (1000-2000 bpd). De hecho, DBSCAN lo habría detectado SIN que tú tuvieras que escribir la regla `df['Qo_bpd'] > 10000`. Esa es la ventaja: no necesitas conocer todas las reglas de antemano.

---

## FAQ 10: ¿DBSCAN es mejor que K-Means?

**Respuesta:** Ninguno es "mejor" en abstracto. Son herramientas para problemas diferentes:
- Datos limpios, clusters compactos, sabes cuántos grupos hay → K-Means
- Sospechas datos corruptos, clusters con formas arbitrarias, no sabes cuántos grupos hay → DBSCAN

Un buen científico de datos petrolero conoce AMBOS y sabe cuándo usar cada uno.
