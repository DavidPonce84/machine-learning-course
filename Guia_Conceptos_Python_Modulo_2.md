# Guía de Sintaxis y Conceptos de Python - Módulo 2
### Diseñada para: David Ponce (Tu Soporte para Preguntas de Código)

Esta guía explica detalladamente la sintaxis y funciones de Python utilizadas en los notebooks de la clase. Te ayudará a responder con solvencia si algún alumno te pregunta el "por qué" de una línea de código.

---

## 1. Biblioteca: Pandas & Numpy

### 📌 `pd.to_datetime(df['Fecha'])`
* **¿Qué hace?**: Convierte una columna de texto (strings) que representa fechas en objetos tipo `datetime64` entendibles por Python.
* **¿Por qué se usa?**: Al cargar un CSV, Pandas lee las fechas como texto simple. Si no se convierten a datetime, no podremos ordenarlas cronológicamente de forma correcta, ni graficar series temporales, ni agrupar por meses o años.

### 📌 `.loc[condicion, columna] = valor`
* **¿Qué hace?**: Accede a un grupo de filas y columnas mediante etiquetas o una condición booleana.
* **¿Por qué se usa en lugar de `df[df['columna'] == x]['BHP'] = y`?**: 
  * Si usas la indexación doble `df[][]`, Python genera una copia temporal del DataFrame y puedes recibir el error `SettingWithCopyWarning` (el cambio no se guarda en el DataFrame original).
  * Usar `.loc` garantiza que estás modificando el espacio de memoria del DataFrame original de forma directa y segura.

### 📌 `df.groupby('Pozo')['BHP_psi'].transform(lambda x: x.interpolate(method='linear'))`
Esta es la línea más avanzada del Día 1. Desglosémosla:
* **`groupby('Pozo')`**: Separa los datos por pozo temporalmente.
* **`.transform(...)`**: Ejecuta la función interna sobre cada pozo y **devuelve una serie con la misma longitud que el DataFrame original**. Esto nos permite reasignar directamente el resultado a la columna original `df['BHP_psi']`. Si usáramos `.apply()`, la estructura del DataFrame cambiaría y no podríamos sobreescribir la columna fácilmente.
* **`lambda x: x.interpolate(...)`**:
  * `lambda x` es una función anónima (al vuelo) donde `x` representa la serie temporal de BHP de un solo pozo a la vez.
  * `.interpolate(method='linear')` dibuja una línea recta imaginaria entre el último valor bueno antes del `NaN` y el primer valor bueno después del `NaN`, rellenando los vacíos secuencialmente.

### 📌 `.fillna(0.0)` y `.replace([np.inf, -np.inf], np.nan)`
* **¿Por qué se usan juntos en KPIs?**:
  * Al calcular la GOR (`Qg / Qo`), si un pozo está cerrado, `Qo` es cero. En matemáticas, dividir un número positivo por cero tiende a **infinito**. Python representa esto como `inf` o `-inf`.
  * `.replace([np.inf, -np.inf], np.nan)` convierte esos infinitos matemáticos en valores nulos (`NaN`).
  * Posteriormente, `.fillna(0.0)` reemplaza cualquier `NaN` por `0.0` para que el pozo cerrado marque simplemente una tasa de cero y no rompa los cálculos estadísticos (como promedios).

---

## 2. Biblioteca: Matplotlib & Seaborn

### 📌 `sns.heatmap(corr, annot=True, cmap='coolwarm', fmt=".2f")`
* **`corr`**: La matriz de coeficientes de correlación generada con `df.corr()`.
* **`annot=True`**: Escribe el número del coeficiente dentro de cada cuadrado del mapa de calor. Si se omite, solo se mostrarán colores.
* **`cmap='coolwarm'`**: Paleta de colores cromática ideal para correlaciones. Los valores cercanos a +1 (relación directa) se pintan de rojo cálido; los cercanos a -1 (inversa) de azul frío; y los cercanos a 0 en color neutro.
* **`fmt=".2f"`**: Formatea los números para mostrar exactamente **2 decimales**.

### 📌 `plt.gca().invert_yaxis()`
* **`gca()`**: Significa "Get Current Axes" (Obtener los ejes actuales).
* **`invert_yaxis()`**: Invierte el eje vertical.
* **¿Por qué se usa?**: Por defecto, los gráficos empiezan en 0 en la parte inferior y suben. En geología y perforación, a mayor profundidad, mayor distancia hacia abajo. Invertir el eje hace que el gráfico muestre el pozo de arriba hacia abajo, alineándose con la física del subsuelo.

### 📌 `ax[1].set_xscale('log')`
* **¿Qué hace?**: Cambia la escala del eje X a escala logarítmica (base 10).
* **¿Por qué se usa en Resistividad (ILD)?**: Las lecturas de resistividad en formaciones limpias con crudo pueden subir a más de 1000 ohm-m, mientras que en zonas de agua salada marcan 0.5 ohm-m. Si usamos una escala lineal normal, la variación fina en valores bajos (de 0.5 a 10) sería invisible en el gráfico. La escala logarítmica permite ver con claridad la variación en todo el espectro de magnitudes.

### 📌 `ax[0].fill_betweenx(df_logs['Profundidad_m'], 0, 150, where=mask, color='yellow', alpha=0.3)`
* **`fill_betweenx`**: Rellena de color el espacio horizontal entre dos límites para un rango vertical del eje Y (`Profundidad_m`).
* **`0, 150`**: El límite de inicio y fin en el eje X de la curva Gamma Ray.
* **`where=mask`**: Condición lógica (en este caso, donde GR < 40 e ILD > 15). Solo rellenará de color las profundidades que cumplan con esta condición.
* **`alpha=0.3`**: Transparencia del sombreado (30%) para poder seguir leyendo la línea del registro detrás del color.
