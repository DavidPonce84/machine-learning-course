# 🐍 Pandas CheatSheet for Petroleum Engineers
### Guía Rápida de Sintaxis y Comandos de Analítica Aplicada

Esta infografía/guía rápida resume las **6 secciones clave de comandos de Pandas y Python** utilizadas en la capacitación de Machine Learning para **Schlumberger (SLB)**.

---

## 1. Importación e Inspección Inicial de Datos
*Cargar archivos de telemetría y auditar las dimensiones del conjunto de datos.*

```python
import pandas as pd
import numpy as np

# 1. Cargar archivo CSV de producción
df = pd.read_csv('produccion_pozos.csv')

# 2. Ver las primeras N filas
df.head(5)

# 3. Consultar la estructura (filas, columnas)
df.shape  # Retorna una tupla: (5475, 7)

# 4. Verificar tipos de datos asignados
df.dtypes

# 5. Convertir columna de texto a formato Datetime
df['Fecha'] = pd.to_datetime(df['Fecha'])
```

---

## 2. Diagnóstico de Nulos y Filtros Operativos
*Identificar celdas vacías y aislar errores numéricos de calibración SCADA.*

```python
# 1. Conteo de nulos por variable
df.isnull().sum()

# 2. Porcentaje de nulos por columna
df.isnull().mean() * 100

# 3. Filtrar registros por condición (ej. errores de calibración BHP = -999.0)
df[df['BHP_psi'] == -999.0]

# 4. Reemplazar valores de error numérico por nulos reales (NaN) usando .loc
df.loc[df['BHP_psi'] == -999.0, 'BHP_psi'] = np.nan

# 5. Reemplazar infinitos por NaN (útil tras divisiones para cero)
df['GOR'] = df['GOR'].replace([np.inf, -np.inf], np.nan)
```

---

## 3. Interpolación Lineal Agrupada por Pozo
*Imputar datos faltantes de forma limpia sin mezclar información entre pozos.*

```python
# Regla de oro: NUNCA interpolar sin agrupar por pozo (groupby)

# Interpolación lineal aislada por activo
df['BHP_psi'] = df.groupby('Pozo')['BHP_psi'].transform(lambda x: x.interpolate(method='linear'))

# Rellenar nulos residuales en pozos cerrados con valor cero
df['Water_Cut'] = df['Water_Cut'].fillna(0.0)
```

---

## 4. Ingeniería de Características (KPIs Petroleros)
*Transformación analítica de datos crudos en variables con significado físico.*

```python
# 1. Water-Cut (Corte de Agua: Qw / (Qo + Qw))
df['Water_Cut'] = df['Qw_bpd'] / (df['Qo_bpd'] + df['Qw_bpd'])
df['Water_Cut'] = df['Water_Cut'].fillna(0.0)

# 2. GOR (Gas-Oil Ratio: (Qg * 1000) / Qo)
df['GOR'] = (df['Qg_mscfd'] * 1000) / df['Qo_bpd']
df['GOR'] = df['GOR'].replace([np.inf, -np.inf], np.nan).fillna(0.0)

# 3. PI (Índice de Productividad: Qo / (P_estática - BHP))
df['PI'] = df['Qo_bpd'] / (3300.0 - df['BHP_psi'])
df['PI'] = df['PI'].replace([np.inf, -np.inf], np.nan).fillna(0.0)
```

---

## 5. Visualización Estadística de Subsuelo (Seaborn & Matplotlib)
*Generar gráficos estéticos para auditar distribuciones y correlaciones.*

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.set_theme(style="whitegrid")

# 1. Boxplot comparativo por pozo (Detección de outliers)
sns.boxplot(data=df, x='Pozo', y='Qo_bpd', hue='Pozo', palette='Set2')
plt.title('Distribución de Producción por Pozo')
plt.show()

# 2. Matriz de Correlación de Pearson (Heatmap)
corr = df.select_dtypes(include=[np.number]).corr()
sns.heatmap(corr, annot=True, cmap='coolwarm', fmt=".2f")
plt.title('Matriz de Correlación Operativa')
plt.show()

# 3. Gráficos verticales de subsuelo (Invertir eje Y)
plt.plot(df['GR_API'], df['Profundidad_m'])
plt.gca().invert_yaxis()  # Invertir eje Y para representar profundidad real
plt.show()
```

---

## 6. Diagnóstico Automatizado con AutoEDA (`ydata-profiling`)
*Generación instantánea de reportes ejecutivos en formato HTML.*

```python
from ydata_profiling import ProfileReport

# 1. Crear el objeto de diagnóstico completo
profile = ProfileReport(df, title="Reporte EDA de Producción - SLB", explorative=True)

# 2. Exportar a un archivo web interactivo en HTML
profile.to_file("reporte_produccion.html")
```
