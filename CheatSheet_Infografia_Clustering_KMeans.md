# 🛢️ CHEATSHEET & INFOGRAFÍA: Clustering K-Means en la Industria Petrolera
**Capacitación SLB · Módulo 4: Aprendizaje No Supervisado (Día 1)**

---

## ⚖️ 1. StandardScaler (Normalización Z-Score)
> **"Tus ojos ven el gráfico 2D ajustado por la pantalla, pero K-Means calcula distancias numéricas puras."**

### 📐 La Fórmula Z-Score
$$Z = \frac{X - \mu}{\sigma}$$
- **$\mu = 0.00$** (Media del campo) | **$\sigma = 1.00$** (Desviación estándar)

### 💡 Significado Físico de los Valores
- 🟢 **$Z > 0$ (Positivo):** El pozo está **POR ENCIMA** del promedio del campo.
- ⚪ **$Z = 0$ (Cero):** El pozo es **IGUAL** al promedio del campo.
- 🔴 **$Z < 0$ (Negativo):** El pozo está **POR DEBAJO** del promedio del campo *(¡No significa producción ni presión negativa!)*.

---

## 🎯 2. El Algoritmo K-Means en 4 Pasos Iterativos

```
[1. Inicialización K-Means++] ➔ [2. Asignación a Centroide] ➔ [3. Recálculo de Media] ➔ [4. Convergencia (Stop)]
```

1. **Semilla Aleatoria (`random_state=42`):** Garantiza que la selección inicial de centroides sea **100% reproducible** en cualquier computadora.
2. **Distancia Euclidiana 5D:** $d(x, \mu_k) = \sqrt{\sum_{i=1}^5 (x_i - \mu_{k,i})^2}$ en el espacio (`Qo`, `Qw`, `WHP`, `BHP`, `Water Cut`).

---

## 📈 3. Diagnóstico del K Óptimo con Yellowbrick

### 🛠️ Código de Diagnóstico
```python
from yellowbrick.cluster import KElbowVisualizer
model = KMeans(random_state=42)
visualizer = KElbowVisualizer(model, k=(2, 6))
visualizer.fit(X_scaled)
visualizer.show()
```

### 🔍 ¿Cómo Leer la Gráfica del Codo?
- **Caída de Inercia:** Muestra la reducción del error intra-cluster.
- **Línea Negra Punteada Vertical (`Kneedle Algorithm`):** Marca el punto de **rendimiento decreciente ($K=3$)**. Después de $K=3$, crear más clusters solo añade complejidad sin reducir significativamente el error.

---

## 🛢️ 4. Matriz de Decisión de Yacimientos (`df.groupby('Cluster').mean()`)

| Cluster ID | Caudal Crudo (`Qo`) | Corte Agua (`Water Cut`) | Presión Fondo (`BHP`) | Diagnóstico Físico & Acción Operativa |
| :---: | :---: | :---: | :---: | :--- |
| **Cluster 0** | Bajo ($370 \text{ bpd}$) | ⚠️ **Alto ($83\%$)** | Media ($1526 \text{ psi}$) | 🟡 **Conificación de Agua / Acuífero Activo.** Requiere aislamiento de capas o gel de bloqueo. |
| **Cluster 1** | 🚀 **Alto ($2084 \text{ bpd}$)** | 🟢 **Bajo ($14\%$)** | 🚀 **Alta ($2143 \text{ psi}$)** | 🟢 **Pozos de Alto Rendimiento.** Pozos estrella del campo. Mantener régimen de flujo. |
| **Cluster 2** | Bajo ($248 \text{ bpd}$) | Medio ($42\%$) | 🔴 **Baja ($1305 \text{ psi}$)** | 🔴 **Pozos Declinados / Agotados.** Candidatos prioritarios a Levantamiento Artificial (BES / Gas Lift). |

---

## 🚨 REGLA DE ORO DEL INGENIERO DE DATOS
> ⚠️ **NUNCA asumas ciegamente que el "Cluster 0" es el de alto rendimiento.**  
> K-Means asigna los números de etiqueta ($0, 1, 2$) de forma **aleatoria** según la inicialización. Siempre debes ejecutar `df_pozos.groupby('Cluster')[features].mean()` para leer los valores físicos reales y diagnosticar cada grupo.

---
