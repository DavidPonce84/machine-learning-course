# 🛢️ Guía Celda por Celda: Módulo 4 - Notebook 1: Clustering K-Means & Diagnósticos Visuales

**Curso:** Machine Learning Aplicado a la Industria Petrolera (SLB / SLB Training)  
**Módulo:** Módulo 4 - Machine Learning No Supervisado (Día 1)  
**Notebook Target:** `01_clustering_kmeans_estudiante.ipynb` / `01_clustering_kmeans_solucion.ipynb`  

---

## 📌 Resumen de la Dinámica y Metodología
En esta sesión exploramos el notebook **celda por celda**, abordando:
1. **El Porqué (Fundamentos de Ingeniería y Matemáticas):** Por qué se elige cada técnica, qué problema físico resuelve en la industria petrolera y cómo impacta en las métricas del modelo.
2. **El Cómo (Lógica del Código y Sintaxis Python):** Explicación detallada parámetro por parámetro, funciones involucradas y estructura de datos resultante.

---

## 📘 Celda 0: Título y Objetivos de la Sesión

### 📄 Contenido de la Celda (Markdown):
```markdown
# 🛢️ Segmentación de Pozos mediante Aprendizaje No Supervisado
## Módulo 4 · Clustering K-Means & Diagnósticos Visuales · Capacitación SLB

### Objetivos de la sesión:
1. Comprender la necesidad física y matemática de estandarizar variables de entrada.
2. Determinar la cantidad óptima de grupos (K) mediante el método del codo y Yellowbrick.
3. Entrenar e interpretar un modelo K-Means sobre 50 pozos petroleros.
```

### 💡 El Porqué (Contexto Operativo & Negocio):
- **Aclaración Importante (ID vs. Etiqueta u Objetivo $y$):**
  - La columna `Pozo` (`Pozo-01`, `Pozo-02`, ...) es solo un **Identificador Único (ID)** o nombre de la entidad. No es una etiqueta de Machine Learning.
  - Una **Etiqueta (Target $y$)** en Aprendizaje Supervisado sería una clasificación previa dada por expertos, por ejemplo: `"Categoría de Salud: Alto Rendimiento / Conificación de Agua / Declinado"`.
- **Desafío en Yacimientos:** En la práctica real no tenemos esa columna de diagnóstico $y$. Solo tenemos variables operativas continuas $X$ (`Qo_bpd`, `WHP_psi`, `BHP_psi`, `Water_Cut`).
- **Solución No Supervisada:** K-Means **crea la etiqueta por nosotros** (agrupando en `Cluster 0, 1, 2`) basándose exclusivamente en la cercanía matemática de los datos físicos, sin requerir diagnósticos previos hechos a mano.

---

## 🐍 Celda 1: Importación de Librerías y Configuración de Estilo

### 💻 Código de la Celda:
```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

sns.set_theme(style="whitegrid")
```

### 💡 El Porqué (Justificación Técnica):
- `StandardScaler`: K-Means calcula distancias euclidianas. Si no estandarizamos, una variable con valores grandes (como presiones de $2000 \text{ psi}$) dominará totalmente sobre variables pequeñas (como `Water_Cut` entre $0.0$ y $0.85$).
- `KMeans`: Algoritmo partitioning que divide $N$ observaciones en $K$ clusters minimizando la suma de cuadrados intra-cluster (Inercia).
- `sns.set_theme`: Establece un estándar visual consistente para los gráficos del informe de ingeniería.

### ⚙️ El Cómo (Lógica & Sintaxis):
- `from sklearn.preprocessing import StandardScaler`: Importa el transformador de datos de Scikit-Learn que resta la media ($\mu$) y divide por la desviación estándar ($\sigma$), dejando la distribución con $\mu=0$ y $\sigma=1$.
- `from sklearn.cluster import KMeans`: Importa el estimador principal para el clustering.
- `sns.set_theme(style="whitegrid")`: Aplica una cuadrícula de fondo gris claro/blanca a todas las figuras de Matplotlib/Seaborn.

---

## 📥 Celda 2: Carga y Descarga de Datos

### 💻 Código de la Celda:
```python
!wget -q https://raw.githubusercontent.com/DavidPonce84/machine-learning-course/main/modulo_4_no_supervisado/data/pozos_clustering.csv -O pozos_clustering.csv

df_pozos = pd.read_csv('pozos_clustering.csv')
df_pozos.head()
```

### 💡 El Porqué (Justificación Técnica):
- Garantiza la **reproducibilidad** inmediata tanto en Google Colab como en Jupyter local. El dataset `pozos_clustering.csv` contiene 50 pozos con variables clave de producción.

### ⚙️ El Cómo (Lógica & Sintaxis):
- `!wget`: Comando del sistema operativo/shell invocado desde el notebook (`!`). La bandera `-q` (quiet) oculta la barra de progreso de descarga y `-O` especifica el nombre de salida local.
- `pd.read_csv(...)`: Carga el archivo delimitado por comas a la memoria en forma de `DataFrame` de Pandas.
- `df_pozos.head()`: Retorna por defecto los primeros 5 registros de la tabla para inspección visual de las columnas (`Well_ID`, `Qo_bpd`, `Qw_bpd`, `WHP_psi`, `BHP_psi`, `Water_Cut`).

---

## 📊 Celda 3: Inspección de Escalas Físicas (`describe`)

### 💻 Código de la Celda:
```python
df_pozos.describe().round(2)
```

### 💡 El Porqué (Fundamento Físico y Matemático):
- **La Ecuación de Distancia Euclidiana:** K-Means calcula la distancia entre un punto $x$ y un centroide $\mu_k$:
  $$d(x, \mu_k) = \sqrt{\sum_{i=1}^{P} (x_i - \mu_{k,i})^2}$$
- **Conflicto de Magnitudes:**
  - `Qo_bpd` (Caudal de crudo): Varía en el rango de $100$ a $2500 \text{ bpd}$ (diferencias de $\approx 1000^2 = 1,000,000$).
  - `Water_Cut` (Corte de agua): Varía entre $0.00$ y $0.85$ (diferencias de $\approx 0.5^2 = 0.25$).
- **Consecuencia:** Sin estandarizar, K-Means tratará a `Water_Cut` como "ruido despreciable" e ignorará por completo el comportamiento del agua en la segmentación.

### ⚙️ El Cómo (Lógica & Sintaxis):
- `df_pozos.describe()`: Calcula automáticamente estadísticas descriptivas (`count`, `mean`, `std`, `min`, `25%`, `50%`, `75%`, `max`) para todas las columnas numéricas.
- `.round(2)`: Redondea la salida a 2 decimales para evitar saturación de dígitos flotantes en pantalla.

---

## 🎨 Celda 3.1: Exploración Visual 2D (Caudal de Petróleo vs Corte de Agua)

### 💻 Código de la Celda:
```python
plt.figure(figsize=(8, 5))
sns.scatterplot(data=df_pozos, x="Qo_bpd", y="Water_Cut", s=80, color="navy", alpha=0.7)
plt.title("Dispersión 2D: Caudal de Crudo (Qo) vs Corte de Agua (Water Cut)")
plt.xlabel("Qo_bpd (Barriles de Petróleo por Día)")
plt.ylabel("Water_Cut (Fracción 0-1)")
plt.show()
```

### 💡 El Porqué (Importancia Crítica antes de Elegir el Algoritmo):
- **¿Qué representan los Ejes $X$ e $Y$ en la pantalla?:**
  - **Eje $X$ (Horizontal):** `Qo_bpd` (Caudal de Crudo de 0 a 2,500 bpd).
  - **Eje $Y$ (Vertical):** `Water_Cut` (Corte de Agua de 0.00 a 1.00).
  - **Cada punto individual:** Representa un **pozo específico del campo**.
- **Visualización 2D vs. Espacio Real 5D:**
  - Para visualizar en una pantalla de computadora, proyectamos el plano 2D de las dos variables operativas más críticas (`Qo` vs `Water_Cut`).
  - Sin embargo, en el código Python de K-Means, el algoritmo **trabaja en un hiperespacio de 5 Dimensiones simultáneas** ($\mathbb{R}^5$): `Qo_bpd`, `Qw_bpd`, `WHP_psi`, `BHP_psi` y `Water_Cut`.
- **K-Means vs. DBSCAN (Naturaleza de los Datos):**
  - **K-Means:** Asume de forma estricta que los grupos son **esféricos/convexos** y continuos.
  - **DBSCAN (Cuaderno 2):** No requiere formas esféricas; agrupa por **densidad** y detecta ruido/outliers en formas irregulares o anulares.

### ⚙️ El Cómo (Lógica & Sintaxis):
- `sns.scatterplot()`: Renderiza el gráfico de puntos 2D. `s=80` define el tamaño de las burbujas, `color="navy"` el tono azul petróleo y `alpha=0.7` agrega transparencia para detectar solapamiento de puntos.

---

## 👁️ Celda 3.2: Matriz de Dispersión Multivariada (`pairplot`)

### 💻 Código de la Celda:
```python
features = ['Qo_bpd', 'Qw_bpd', 'WHP_psi', 'BHP_psi', 'Water_Cut']

# Pairplot multivariado de las variables de producción
sns.pairplot(df_pozos[features], corner=True, diag_kind="kde")
plt.suptitle("Matriz de Dispersión Multivariada de Pozos", y=1.02)
plt.show()
```

### 💡 El Porqué (Justificación Técnica):
- Evalúa de forma simultánea **todas las combinaciones de pares de variables** en un solo panel gráfico de 5x5.
- Permite detectar correlaciones lineales y agrupamientos en múltiples planos cartesianos simultáneamente.
- La diagonal (`diag_kind="kde"`) muestra las curvas de densidad de probabilidad Kernel (KDE) de cada variable individual, revelando si la distribución es bimodal o trimodal.

### ⚙️ El Cómo (Lógica & Sintaxis):
- `sns.pairplot(...)`:
  - `corner=True`: Elimina el triángulo superior duplicado de la matriz para una lectura limpia y ligera.
  - `diag_kind="kde"`: Sustituye los histogramas diagonales de barras por curvas suaves de densidad continua (Kernel Density Estimation).

---

## ⚖️ Celda 4: Estandarización de Variables con `StandardScaler`

### 💻 Código de la Celda:
```python
features = ['Qo_bpd', 'Qw_bpd', 'WHP_psi', 'BHP_psi', 'Water_Cut']
scaler = StandardScaler()
X_scaled = scaler.fit_transform(df_pozos[features])
df_scaled = pd.DataFrame(X_scaled, columns=features)
df_scaled.describe().round(3)
```

### 💡 El Porqué (Fundamento Físico, Matemático y Significado de los Valores Negativos):

#### 💡 Demostración Reveladora: ¿Por qué hay que escalar si en la gráfica 2D ya se ven agrupados?
Esta es una de las mayores ilusiones ópticas en Data Science:

1. **La Ilusión Óptica de los Ejes:** Matplotlib estira automáticamente los píxeles de la pantalla para que el eje Y ($0.0 \text{ a } 1.0$) tenga la misma altura visual que el eje X ($0 \text{ a } 2500$). **Tus ojos ven los grupos separados gracias al ajuste visual de la pantalla**, pero la matemática de K-Means **no ve la pantalla**, solo ve números puros.
2. **Ejemplo Matemático de por qué K-Means se confunde sin escalar:**
   - **Pozo A (Grupo Arriba-Izquierda):** $Q_o = 200 \text{ bpd}$, $\text{Water\_Cut} = 0.85$.
   - **Pozo B (Grupo Arriba-Izquierda):** $Q_o = 300 \text{ bpd}$, $\text{Water\_Cut} = 0.85$.
   - **Pozo C (Grupo Abajo-Izquierda):** $Q_o = 205 \text{ bpd}$, $\text{Water\_Cut} = 0.10$.
   
   - **Distancia entre Pozo A y Pozo B (Mismo grupo visual):**
     $$d(A, B) = \sqrt{(300 - 200)^2 + (0.85 - 0.85)^2} = \sqrt{100^2 + 0} = \mathbf{100}$$
   - **Distancia entre Pozo A y Pozo C (¡Grupos totalmente opuestos!):**
     $$d(A, C) = \sqrt{(205 - 200)^2 + (0.10 - 0.85)^2} = \sqrt{25 + 0.56} = \mathbf{5.05}$$

3. **Conclusión Impactante:** Sin estandarizar, K-Means diría que el Pozo A está **20 veces más cerca del Pozo C** (un pozo con solo $10\%$ de agua) que del Pozo B (que tiene $85\%$ de agua), simplemente porque una diferencia de $5 \text{ bpd}$ en crudo eclipsa una diferencia monumental de $75\%$ en corte de agua.

#### 1. ¿Qué hace exactamente `StandardScaler`?
El objetivo de `StandardScaler` es transformar los datos midiendo cada valor en unidades de **desviación estándar ($\sigma$) respecto a la media ($\mu$) del campo**:
$$Z = \frac{X - \mu}{\sigma}$$
Donde:
- $X$: El valor real del pozo (ej. $1500 \text{ bpd}$ de crudo).
- $\mu$: La media o promedio de todos los 50 pozos (ej. $1800 \text{ bpd}$).
- $\sigma$: La desviación estándar o variabilidad del conjunto (ej. $400 \text{ bpd}$).

#### 2. ¿Por qué aparecen VALORES NEGATIVOS?
Un valor estandarizado ($Z$) **NO significa que el pozo tenga producción o presión negativa en el mundo real**. Representa la **posición relativa respecto al promedio del campo**:
- 🟢 **Valores Positivos ($Z > 0$):** El pozo está **POR ENCIMA del promedio**.
  - Ejemplo: $Z_{Qo} = +1.5 \rightarrow$ El pozo produce $1.5$ desviaciones estándar **más** petróleo que el pozo promedio del campo.
- ⚪ **Valor Cero ($Z = 0$):** El pozo es **EXACTAMENTE IGUAL al promedio del campo**.
- 🔴 **Valores Negativos ($Z < 0$):** El pozo está **POR DEBAJO del promedio**.
  - Ejemplo: Si la media del campo es $1800 \text{ bpd}$ y un pozo produce solo $1000 \text{ bpd}$, la resta $X - \mu = 1000 - 1800 = -800$. Al dividir entre $\sigma$, el resultado $Z$ es **negativo** (ej. $Z_{Qo} = -0.911$).

#### 3. Analogía Visual Sencilla:
Imagina que la estatura promedio en un equipo de trabajo es $1.75\text{ m}$. Si a la estatura de cada persona le restamos $1.75\text{ m}$:
- Una persona de $1.90\text{ m}$ pasa a ser $+0.15\text{ m}$ (encima del promedio).
- Una persona de $1.60\text{ m}$ pasa a ser $-0.15\text{ m}$ (debajo del promedio).
- Ninguna persona mide una estatura negativa; el número negativo indica que es más baja que la media.

#### 4. Nota Técnica sobre la Desviación Estándar (`1.010` vs `1.000`):
En la tabla de `df_scaled.describe()`, la fila `std` muestra `1.010` en lugar de `1.000`. Esto se debe a una diferencia estándar en Python:
- `StandardScaler` usa la **desviación estándar poblacional** ($N = 50$, grados de libertad `ddof=0`).
- `Pandas .describe()` calcula por defecto la **desviación estándar muestral** ($N-1 = 49$, grados de libertad `ddof=1`).
- Para 50 datos, $\sqrt{50/49} = 1.010$. ¡Es 100% normal y matemáticamente correcto!

### ⚙️ El Cómo (Desglose Minucioso Línea por Línea):

#### 📌 Línea 1: `scaler = StandardScaler()`
- **`StandardScaler`:** Clase importada de `sklearn.preprocessing`.
- **`scaler = ...`:** Instancia el objeto estandarizador. En este momento, `scaler` es una "máquina vacía" en memoria lista para aprender la media ($\mu$) y desviación estándar ($\sigma$) de los datos.

#### 📌 Línea 2: `X_scaled = scaler.fit_transform(df_pozos[features])`
- **`df_pozos[features]`:** Filtra el DataFrame original seleccionando únicamente las 5 columnas numéricas operativas.
- **`.fit_transform(...)`:** Combina dos acciones clave:
  - **`fit()`:** Lee las 50 filas de cada columna y calcula su media ($\mu$) y desviación estándar ($\sigma$).
  - **`transform()`:** Aplica la fórmula $Z = \frac{X - \mu}{\sigma}$ a cada casilla de la tabla.
- **`X_scaled` (Tipo de Dato):** Almacena el resultado. **Atención:** Scikit-Learn devuelve una **matriz numérica pura de NumPy (`ndarray`)** de $50 \times 5$, no un DataFrame.

#### 📌 Línea 3: `df_scaled = pd.DataFrame(X_scaled, columns=features)`
- **`pd.DataFrame(...)`:** Constructor de Pandas que convierte la matriz de NumPy de nuevo en un DataFrame estructurado.
- **`columns=features`:** Le devuelve a la matriz sus 5 encabezados de texto originales (`Qo_bpd`, `Qw_bpd`, `WHP_psi`, `BHP_psi`, `Water_Cut`).

#### 📌 Línea 4: `df_scaled.describe().round(3)`
- **`.describe()`:** Método de Pandas que calcula las 8 estadísticas descriptivas (`count`, `mean`, `std`, `min`, `25%`, `50%`, `75%`, `max`).
- **`.round(3)`:** Redondea la tabla resultante a 3 decimales para evitar saturación de flotantes en pantalla, permitiendo validar que las medias hayan quedado en $0.000$ y la desviación en $\approx 1.010$.

---

## 🛠️ Celda 5: Instalación de Yellowbrick

### 💻 Código de la Celda:
```python
!pip install -q yellowbrick
```

### 💡 El Porqué (Diferencia clave entre Yellowbrick y Matplotlib):
- **Matplotlib** es una librería general de dibujo (lienzo). Para hacer el método del codo con Matplotlib habría que escribir manualmente un bucle `for`, entrenar $K$ a $K$, guardar las inercias en un arreglo, hacer la gráfica de puntos y **tratar de adivinar el codo "a ojo"**.
- **Yellowbrick** es una librería de **diagnóstico visual para Machine Learning** que se apoya sobre Matplotlib. No solo grafica la curva del codo, sino que aplica algoritmos estadísticos internos para **detectar y marcar automáticamente el punto exacto del $K$ óptimo**.

---

## 📐 Celda 6: Selección del K Óptimo mediante Método del Codo (`KElbowVisualizer`)

### 💻 Código de la Celda:
```python
from yellowbrick.cluster import KElbowVisualizer
model = KMeans(random_state=42)
visualizer = KElbowVisualizer(model, k=(2, 6))
visualizer.fit(X_scaled)
visualizer.show()
```

### 🔎 Desglose Ultra-Detallado Línea por Línea (Sintaxis, Lógica y Parámetros)

#### 📌 Línea 1: `from yellowbrick.cluster import KElbowVisualizer`
- **¿Qué es `yellowbrick`?:** Es una suite de herramientas de diagnóstico visual para Machine Learning que envuelve los estimadores de Scikit-Learn y genera gráficos avanzados basados en Matplotlib.
- **`yellowbrick.cluster`:** Submódulo especializado exclusivamente en algoritmos de agrupamiento (K-Means, DBSCAN, Clustering Jerárquico, Gráficos de Silueta, Distancia Intercluster).
- **`KElbowVisualizer`:** Es la clase principal encargada de iterar sobre múltiples valores de $K$, entrenar los modelos automáticamente, calcular la métrica de distorsión y aplicar algoritmos de detección de curvatura para sugerir el $K$ óptimo.

---

#### 📌 Línea 2: `model = KMeans(random_state=42)`
- **`KMeans`:** Instancia el objeto del algoritmo de clustering de particionamiento.
- **`random_state=42` (Semilla Aleatoria de Inicialización):**
  - **Función Técnica (Reproducibilidad):** Los algoritmos en Machine Learning usan generadores de números pseudoaleatorios. Si no fijamos una semilla (`random_state=None`), Python usará el reloj del sistema y en cada corrida los resultados variarán ligeramente. Fijar cualquier entero (como 42) congela la aleatoriedad, garantizando que el modelo entregue **exactamente los mismos resultados a cualquier persona en cualquier computadora**.
  - **Origen Cultural del 42:** ¿Por qué 42 y no otro número? Es un célebre guiño cultural geek a la novela de ciencia ficción *"La Guía del Autoestopista Galáctico"* de Douglas Adams, donde la súper computadora *Pensador Profundo* concluye tras 7.5 millones de años que la respuesta al sentido de la vida, el universo y todo lo demás es el número **42**. La comunidad de Scikit-Learn lo adoptó por convención.

---

#### 📌 ¿Por qué no pasamos `n_clusters` aquí?:
Porque dejamos el objeto "abierto" para que `KElbowVisualizer` inyecte dinámicamente cada valor de $K$ durante el bucle de diagnóstico.

---

#### 📌 Línea 3: `visualizer = KElbowVisualizer(model, k=(2, 6))`
- **`model`:** Se pasa como primer argumento el estimador base `KMeans(random_state=42)` definido en la línea anterior.
- **`k=(2, 6)`:** Define el rango de valores de $K$ a probar. En sintaxis de rangos de Python, `(2, 6)` equivale a evaluarlo en el intervalo semIABIERTO $[2, 6)$, es decir, probará **$K = 2, 3, 4 \text{ y } 5$**.
- **Parámetros Ocultos Implícitos (Opciones avanzadas de Yellowbrick):**
  - `metric='distortion'` *(por defecto)*: Calcula la Inercia (suma de distancias al cuadrado al centroide). Se puede cambiar por `metric='silhouette'` o `metric='calinski_harabasz'`.
  - `locate_elbow=True` *(por defecto)*: Activa la detección algorítmica para marcar la línea vertical punteada en el codo.
  - `timings=True` *(por defecto)*: Activa la curva secundaria verde que mide el tiempo de ejecución en segundos.

---

#### 📌 Línea 4: `visualizer.fit(X_scaled)`
- **`X_scaled`:** Se pasa la matriz de características estandarizadas ($50 \text{ pozos} \times 5 \text{ variables}$).
- **¿Qué ejecuta `fit()` internamente?:**
  1. Ejecuta un bucle interno donde clona el `model`, le asigna $K=2$ y lo entrena con `fit(X_scaled)`. Mide la distorsión y el tiempo.
  2. Repite el proceso para $K=3$, $K=4$ y $K=5$.
  3. Almacena las inercias resultantes: $[68.1, 7.14, 6.95, 6.20]$.
  4. Ejecuta el **Algoritmo Kneedle (Satopaa et al.)** sobre la curva para hallar el punto de máxima curvatura.

---

#### 📌 Línea 5: `visualizer.show()`
- Renderiza y muestra en la pantalla del notebook el gráfico completo de Matplotlib con las dos curvas (Distorsión y Tiempo) y la línea vertical punteada en $K=3$.
- *Nota:* Si quisiéramos guardar el gráfico a disco en alta definición para un informe corporativo de SLB, podríamos ejecutar: `visualizer.show(outpath="metodo_codo.png", dpi=300)`.

---

### 📐 Fundamento Matemático & Algoritmo Kneedle

#### 1. Ecuación del Distortion Score (Inercia):
$$\text{Distorsion} = \sum_{k=1}^{K} \sum_{x_i \in C_k} \|x_i - \mu_k\|^2$$
Donde $x_i$ es el vector estandarizado del pozo $i$, $\mu_k$ es el centroide promedio del cluster $k$, y $\|x_i - \mu_k\|^2$ representa la distancia euclidiana al cuadrado entre el pozo y su centroide.

#### 2. Algoritmo Kneedle (Detección Automática del Codo):
Yellowbrick no "adivina visualmente". Utiliza el algoritmo científico **Kneedle**:
1. Traza una recta imaginaria desde el punto inicial $(K=2, \text{Score}=68.1)$ hasta el punto final $(K=5, \text{Score}=6.20)$.
2. Calcula la distancia perpendicular ortogonal desde cada punto de la curva real hacia esa recta imaginaria.
3. El punto $K$ que tenga la **distancia perpendicular máxima** hacia esa recta es identificado matemáticamente como el **Codo de Inflexión ($K=3$)**.

---

### 📊 Desglose Visual de la Gráfica Resultante

1. **Eje Horizontal X ($k$):** Rango de clusters evaluados ($2, 3, 4, 5$).
2. **Eje Izquierdo (`distortion score` - Línea Azul Continua):**
   - **$K=2 \rightarrow K=3$:** La inercia cae dramáticamente de $\approx 68.1$ a $7.140$. Reducción masiva de error.
   - **$K=3 \rightarrow K=4$:** La inercia baja solo de $7.140$ a $\approx 6.95$. Disminución insignificante.
3. **Línea Punteada Negra Vertical (`elbow at k = 3, score = 7.140`):** Marca el punto donde agregar más clusters genera rendimiento decreciente.
4. **Eje Derecho (`fit time` - Línea Verde Punteada):** Tiempo de cómputo ($<0.01\text{ s}$), confirmando alta eficiencia computacional.

---

## 🎯 Celda 7: Entrenamiento del Modelo K-Means ($K=3$) y Asignación de Clusters

### 💻 Código de la Celda:
```python
kmeans = KMeans(n_clusters=3, random_state=42)
df_pozos['Cluster'] = kmeans.fit_predict(X_scaled)
df_pozos.groupby('Cluster').size()
```

### 💡 El Porqué (Justificación Técnica):
- Una vez seleccionado $K=3$ a partir del diagnóstico de Yellowbrick, procedemos al entrenamiento final. El algoritmo itera recalculando la posición de los 3 centroides hasta convergencia.

### ⚙️ El Cómo (Lógica & Sintaxis):
- `kmeans = KMeans(n_clusters=3, random_state=42)`: Instancia el modelo final especificando 3 clusters.
- `df_pozos['Cluster'] = kmeans.fit_predict(X_scaled)`:
  - `fit_predict()`: Ajusta los centroides en la matriz estandarizada `X_scaled` y devuelve un vector de enteros con las etiquetas asignadas a cada pozo ($0, 1, 2$).
  - Guardamos el resultado directamente en el DataFrame original `df_pozos['Cluster']`.
- `df_pozos.groupby('Cluster').size()`: Muestra la distribución del número de pozos asignados a cada uno de los 3 grupos.

---

## 🔍 Celda 8: Caracterización y Perfilamiento Operativo de los Clusters

### 💻 Código de la Celda:
```python
df_pozos.groupby('Cluster')[features].mean().round(2)
```

### 💡 El Porqué (Interpretación Física & Valor para la Industria Petrolera):
- Las etiquetas numéricas ($0, 1, 2$) son **identificadores arbitrarios** asignados por el algoritmo K-Means. Para darles valor de negocio, debemos inspeccionar los promedios reales de la tabla de salida:
  - 🟡 **Cluster 0 (Pozos con Conificación de Agua / Acuífero Activo):** Caudal de agua desproporcionado (`Qw_bpd` $= 1,870.95 \text{ bpd}$) y corte de agua severo (`Water_Cut` $= 0.83$ o $83\%$). Producción de crudo mermada ($370.35 \text{ bpd}$). Requieren intervención inmediata de aislamiento de capas.
  - 🟢 **Cluster 1 (Pozos de Alto Rendimiento):** Producción masiva de crudo (`Qo_bpd` $= 2,084.64 \text{ bpd}$), muy bajo corte de agua (`Water_Cut` $= 0.14$ o $14\%$) y presiones de fondo muy saludables (`BHP_psi` $= 2,143.19 \text{ psi}$). Son los pozos "estrella" del campo.
  - 🔴 **Cluster 2 (Pozos Declinados / Baja Presión):** Caudales muy bajos (`Qo_bpd` $= 248.20 \text{ bpd}$) y presiones de cabeza y fondo agotadas (`WHP` $= 110.11 \text{ psi}$, `BHP` $= 1,305.83 \text{ psi}$). Candidatos prioritarios para optimización o instalación de Sistemas de Levantamiento Artificial (BES / Gas Lift).

### ⚙️ El Cómo (Lógica & Sintaxis):
- `df_pozos.groupby('Cluster')`: Agrupa los 50 pozos según su cluster asignado.
- `[features]`: Selecciona únicamente las columnas numéricas de interés operativo (`Qo_bpd`, `Qw_bpd`, `WHP_psi`, `BHP_psi`, `Water_Cut`).
- `.mean()`: Calcula la media aritmética real de cada parámetro en cada grupo.
- `.round(2)`: Redondea a 2 decimales para incluirlo limpiamente en reportes ejecutivos.

---

## 🌐 Herramientas Recomendadas para Presentar Datos 5D en Vivo

### 1. TensorFlow Embedding Projector (Online & Gratuito)
- **URL:** [https://projector.tensorflow.org](https://projector.tensorflow.org)
- **Descripción:** Desarrollado por Google. Te permite cargar un archivo CSV con tus 5 variables de los pozos y proyectarlos en un espacio 3D interactivo usando PCA o t-SNE. Los estudiantes pueden rotar la nube de datos en 3D y hacer clic en cada pozo.

### 2. Plotly 3D + Color + Tamaño (Gráfico 5D interactivo en Python)
Puedes mapear 5 dimensiones simultáneas en una sola figura de Plotly:
- **Eje X:** `Qo_bpd`
- **Eje Y:** `Water_Cut`
- **Eje Z:** `BHP_psi`
- **Color:** `Cluster` (Familia asignada)
- **Tamaño de Esfera:** `Qw_bpd` (Caudal de Agua)

```python
import plotly.express as px

fig = px.scatter_3d(
    df_pozos, 
    x='Qo_bpd', y='Water_Cut', z='BHP_psi',
    color='Cluster', size='Qw_bpd',
    hover_name='Well_ID',
    title='Visualización 5D de Pozos Petroleros'
)
fig.show()
```

### 3. Gráfico de Coordenadas Paralelas (Parallel Coordinates Plot)
Muestra 5 ejes verticales paralelos. Cada pozo se dibuja como una línea continua atravesando las 5 variables. Los clusters se aprecian como "mangueras" o haces de cables de colores bien separados.

---



