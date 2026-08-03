# Guía del Orador (Speaker Notes) - Módulo 2: EDA & Data Cleaning
### Diseñada para: David Ponce (Tu Guion Paso a Paso en Clase)

Esta guía te da el guion exacto, las anécdotas y las acciones en pantalla para cada diapositiva de las presentaciones del Módulo 2. Imprímela o tenla en una pantalla secundaria durante tus clases.

---

## 📅 DÍA 1: Limpieza SCADA y Estadística Descriptiva

### 🎴 Diapositiva 1: ¿Qué es el Análisis Exploratorio de Datos (EDA)?
* **Qué decir**: 
  > *"Buenas tardes a todos. Bienvenidos a este módulo. Hoy vamos a hablar del alma de cualquier proyecto de datos: el EDA. En inteligencia artificial hay una regla de oro: 'Garbage In, Garbage Out'. Si entrenamos un modelo de machine learning con datos sucios, nos dará predicciones erróneas. El EDA no es solo hacer gráficos bonitos; es entender la salud de nuestro pozo y de nuestro yacimiento a través de los datos antes de tomar decisiones."*
* **Acción en pantalla**: Muestra la diapositiva y mantén contacto visual.
* **Anécdota**: Comenta que el 80% del tiempo de un científico de datos real se va en limpiar y entender los datos, no en entrenar algoritmos complejos.

### 🎴 Diapositiva 2: ¿Qué desafíos presentan los datos de telemetría de pozos?
* **Qué decir**:
  > *"Los datos que ustedes manejan en Schlumberger provienen de sensores expuestos a presiones extremas, vibraciones y fluidos corrosivos. Por eso, las fallas son normales. Hoy veremos cómo se reflejan estas fallas en los datos: pérdidas de señal que generan nulos, descalibraciones que marcan valores extraños constantes (como el famoso -999 psi en las BHP), spikes de caudal por burbujas de gas, y las paradas operativas que distorsionan nuestras estadísticas."*
* **Acción en pantalla**: Apunta a los términos técnicos de la slide (BHP, WHP, spikes).
* **Pregunta detonante**: *"¿Quién de ustedes ha tenido que lidiar con un reporte de presión de fondo que de la nada marca -999?"* (Espera que sonrían o asientan).

### 🎴 Diapositiva 3: ¿Cómo automatizar el diagnóstico con ydata-profiling?
* **Qué decir**:
  > *"Revisar columna por columna en Excel o Python toma horas. Por eso, el primer paso que daremos es usar AutoEDA con la librería 'ydata-profiling'. Con una sola línea de código, esta herramienta genera un reporte web completo en HTML que nos muestra nulos, correlaciones y variables sospechosas al instante."*
* **Acción en pantalla**: Muestra los beneficios listados en la slide (alertas, duplicados).

### 🎴 Diapositiva 4: ¿Cómo detectar outliers de forma visual y matemática?
* **Qué decir**:
  > *"¿Cómo sabemos si un dato es un error físico o una variación operativa real? Usamos herramientas estadísticas. El Boxplot nos ayuda a ver la distribución de forma gráfica. Matemáticamente, usaremos el Rango Intercuartílico (IQR). Todo dato que supere 1.5 veces el IQR por encima del cuartil 3 será catalogado como un outlier potencial que debemos investigar."*
* **Acción en pantalla**: Haz el gesto con las manos dibujando una caja en el aire explicando los límites.

### 🎴 Diapositiva 5: ¿Cómo tratar los vacíos de telemetría sin perder información?
* **Qué decir**:
  > *"Si un sensor falla un día, no podemos borrar toda la fila de producción porque perderíamos los datos de caudal de ese día. Borrar no es una opción en series de tiempo. Rellenar con el promedio general puede aplanar curvas físicas de presión. Usaremos la interpolación lineal, asumiendo que los cambios físicos en el pozo son graduales en el tiempo. Pero atención: siempre agrupando por pozo. No queremos interpolar presiones del Pozo-01 con las del Pozo-02."*
* **Acción en pantalla**: Enfatiza la palabra **"Agrupando por Pozo"** (groupby).

### 🎴 Diapositiva 6: ¿Qué preguntas responderemos hoy en el notebook de Colab?
* **Qué decir**:
  > *"Ahora es su turno. Pasemos a la práctica en Google Colab. Hoy responderemos preguntas reales: buscaremos sensores rotos, limpiaremos un spike gigante de producción del Pozo-02 y generaremos nuestro primer reporte automático de AutoEDA. Abramos el notebook de estudiantes."*
* **Acción en pantalla**: Cierra la presentación, abre el navegador y proyecta Google Colab. Guíalos a cargar el dataset y ejecutar la primera celda.

---

## 📅 DÍA 2: KPIs de Yacimiento, Correlaciones y Streamlit

### 🎴 Diapositiva 1: ¿Qué es la Ingeniería de Características (Feature Engineering)?
* **Qué decir**:
  > *"Bienvenidos al Día 2. Ayer aprendimos a limpiar datos. Hoy aprenderemos a potenciarlos. El algoritmo de ML es muy inteligente, pero no sabe física de petróleo. Si le damos las tasas de crudo y agua por separado, le costará entender qué es el avance de agua. Nosotros, como ingenieros, debemos facilitarle el trabajo creando nuevas variables con base física."*
* **Acción en pantalla**: Muestra el concepto de "incorporar el conocimiento del experto".

### 🎴 Diapositiva 2: ¿Cómo calculamos e interpretamos KPIs físicos clave?
* **Qué decir**:
  > *"Hoy calcularemos tres indicadores críticos: el corte de agua (Water-Cut) para monitorear acuíferos; la GOR para vigilar el comportamiento del gas disuelto; y el Índice de Productividad (PI) para diagnosticar si la formación está sufriendo daño físico o taponamiento. Veremos que cuando un pozo se cierra, la matemática de división por cero puede romper nuestro código, y aprenderemos a prevenirlo en Python."*
* **Acción en pantalla**: Señala las fórmulas de la diapositiva. Explica brevemente el término de presión de drawdown (P_estática - BHP).

### 🎴 Diapositiva 3: ¿Cómo analizar correlaciones físicas entre variables de subsuelo?
* **Qué decir**:
  > *"En yacimientos, nada ocurre de forma aislada. La presión de fondo (BHP) está íntimamente ligada al caudal que puede aportar el pozo. Usaremos la correlación de Pearson y mapas de calor para verificar matemáticamente lo que dicta la física de reservorios: a mayor contrapresión (BHP alta), menor caudal aportado."*
* **Acción en pantalla**: Haz énfasis en que correlación no implica causalidad, pero en física nos ayuda a validar la calidad del dataset.

### 🎴 Diapositiva 4: ¿Cómo desplegar micro-apps dinámicas con Streamlit?
* **Qué decir**:
  > *"Si le mostramos código en consola a un gerente, no captará el impacto de nuestro trabajo. Los tomadores de decisiones quieren tableros interactivos sencillos. Hoy aprenderemos a usar Streamlit para construir un dashboard interactivo web en menos de 20 líneas de código y exponerlo directamente a internet."*
* **Acción en pantalla**: Comenta con entusiasmo lo fácil que es crear widgets en Streamlit.

### 🎴 Diapositiva 5: ¿Qué preguntas responderemos hoy en el notebook de Colab?
* **Qué decir**:
  > *"Vayamos a la práctica. Hoy calcularemos los KPIs, programaremos el control de divisiones por cero, crearemos el mapa de calor de correlaciones y levantaremos nuestra primera aplicación interactiva web en vivo. ¡Manos a la obra!"*
* **Acción en pantalla**: Abre el notebook del Día 2 en Google Colab.

---

## 📅 DÍA 3: Registros Geofísicos y Optimización de Perforación

### 🎴 Diapositiva 1: ¿Cómo visualizar registros geofísicos (Well Logs) verticales?
* **Qué decir**:
  > *"Último día del Módulo 2. Hoy dejamos las series de tiempo y entramos al espacio geológico. Al graficar registros de pozo, el eje Y representa profundidad, por lo que debe graficarse de forma vertical e invertido, tal como ocurre en el subsuelo. Aprenderemos a compartir el eje Y entre paneles para poder correlacionar las lecturas a la misma profundidad."*
* **Acción en pantalla**: Señala el movimiento de descenso vertical en la diapositiva.

### 🎴 Diapositiva 2: ¿Cómo identificar reservorios mediante firmas de GR y ILD?
* **Qué decir**:
  > *"Para buscar petróleo, cruzamos firmas. El Gamma Ray nos indica si estamos ante una arcilla densa (radiación alta) o una arena porosa (radiación baja). La resistividad nos indica si los poros de la arena contienen agua salada conductora (resistividad baja) o petróleo aislante (resistividad alta). Hoy programaremos un filtro lógico para pintar automáticamente de amarillo las zonas donde coincidan GR bajo e ILD alto: nuestro reservorio productor."*
* **Acción en pantalla**: Muestra en la slide las condiciones de corte: GR < 40 e ILD > 15.

### 🎴 Diapositiva 3: ¿Cómo corregir derrapes por cavernas en sensores de densidad?
* **Qué decir**:
  > *"El sensor de densidad bulk (RHOB) presiona una zapata contra la roca. Pero si la pared del pozo se derrumba (washout), la zapata queda flotando en el lodo de perforación. El sensor nos reportará una densidad falsa equivalente a la del agua o lodo (~1.0 g/cc). Hoy detectaremos estas anomalías físicas y aplicaremos limpieza con Pandas."*
* **Acción en pantalla**: Muestra cómo una descalibración a 1.0 desvía cualquier promedio volumétrico si no se corrige.

### 🎴 Diapositiva 4: ¿Cómo optimizar mecánicamente la velocidad de perforación (ROP)?
* **Qué decir**:
  > *"En perforación, tiempo es dinero. Queremos maximizar la ROP. Pero si aplicamos demasiado peso sobre la broca (WOB) o demasiada rotación (RPM), la fricción (Torque) se dispara y podemos romper la broca. Hoy visualizaremos el mapa cromático de ROP para encontrar la 'ventana dulce' de perforación óptima y detectaremos fallas lógicas de sensores del taladro."*
* **Acción en pantalla**: Explica la diferencia física entre ROP, WOB y RPM.

### 🎴 Diapositiva 5: ¿Qué preguntas responderemos hoy en el notebook de Colab?
* **Qué decir**:
  > *"Pasemos a Colab. Hoy graficaremos verticalmente, sombrearemos reservorios, limpiaremos la curva de densidad bulk y optimizaremos la perforación mecánicamente detectando sensores de RPM descalibrados. ¡A programar!"*
* **Acción en pantalla**: Abre el notebook del Día 3 en Google Colab y guía la práctica.
