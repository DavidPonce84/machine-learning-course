# Ejemplos de Vanguardia: Machine Learning en Petróleo y Gas

## Módulo 6 · Material complementario para los estudiantes

**Objetivo:** que los estudiantes se lleven una visión de DÓNDE se está usando hoy
el deep learning en la industria, y vean que lo que aprendieron (redes neuronales,
regresión, overfitting) es la base de las tecnologías más avanzadas.

Cada ejemplo conecta con un concepto que YA vimos en el curso.

---

## 1. PINNs — Redes Neuronales Informadas por Física

**Qué es:** Physics-Informed Neural Networks. Redes neuronales que no solo aprenden
de los DATOS, sino que además incorporan las ECUACIONES FÍSICAS (Ley de Darcy,
ecuaciones de flujo) como restricción durante el entrenamiento.

**Aplicación en petróleo:** simulación de yacimientos mucho más rápida que los
simuladores numéricos tradicionales (Eclipse, CMG). Pueden predecir presión y
saturación en todo el yacimiento en segundos.

**Conexión con el curso:** "la red aprende la física" — lo vimos al final del Día 1.
Una PINN lleva esa idea al extremo: le DECIMOS las ecuaciones, no solo le damos datos.

**Frase para clase:** "Nuestra red predijo porosidad de los datos. Una PINN predice
todo el yacimiento usando datos MÁS las leyes de la física."

---

## 2. CNNs — Interpretación Sísmica Automática

**Qué es:** redes convolucionales (CNN) que procesan IMÁGENES sísmicas para
detectar fallas, horizontes y estructuras geológicas automáticamente.

**Aplicación:** un geofísico tardaba semanas interpretando un volumen sísmico 3D.
Una CNN lo hace en horas, detectando fallas que a veces el ojo humano pasa por alto.

**Conexión con el curso:** es el mismo concepto de "capas que extraen características"
que vimos en el MLP, pero especializado en imágenes. La capa de entrada ya no son 3
logs, son millones de píxeles.

**Frase para clase:** "Nuestro MLP extrae patrones de 3 logs. Una CNN extrae patrones
de una imagen sísmica completa. La idea es la misma, a mayor escala."

---

## 3. Digital Twins — Gemelos Digitales para Mantenimiento Predictivo

**Qué es:** una réplica digital de un equipo (bomba, compresor, pozo) que predice
fallas ANTES de que ocurran, analizando datos de sensores en tiempo real.

**Aplicación:** evitar paradas no planificadas. El modelo aprende el "comportamiento
normal" del equipo y alerta cuando algo se desvía.

**Conexión con el curso:** es EXACTAMENTE lo que vimos con DBSCAN (Módulo 4) y la
detección de anomalías. Pero ahora con redes neuronales y datos en tiempo real.

**Frase para clase:** "Recuerdan las anomalías de telemetría del Módulo 4. Un gemelo
digital es eso mismo, pero corriendo en vivo las 24 horas."

---

## 4. Perforación Autónoma

**Qué es:** sistemas de ML que ajustan los parámetros de perforación (peso sobre
broca, RPM, caudal de lodo) en tiempo real para optimizar la velocidad de
penetración (ROP) y evitar atascamientos.

**Aplicación:** plataformas que "aprenden" de cada metro perforado y ajustan el
siguiente. Schlumberger y otras compañías ya lo usan en perforación direccional.

**Conexión con el curso:** es una red neuronal de REGRESIÓN (predecir el ROP óptimo),
exactamente como nuestra red de porosidad. Solo cambian las entradas y la salida.

**Frase para clase:** "Es nuestro mismo MLP, pero en vez de predecir porosidad,
predice la velocidad de perforación óptima. Mismo cerebro, otro problema."

---

## 5. Forecasting de Producción con Series Temporales

**Qué es:** modelos que predicen la producción futura de un pozo (declinación,
water cut) usando el historial de producción. Los modelos modernos usan
Transformers (la arquitectura detrás de ChatGPT).

**Aplicación:** planificación de producción, estimación de reservas, optimización
de levantamiento artificial.

**Conexión con el curso:** es regresión sobre datos en el tiempo. La red aprende la
curva de declinación sin que le den la fórmula de Arps.

**Frase para clase:** "La curva de declinación de Arps existe desde 1945. Hoy una red
neuronal la aprende sola de los datos, adaptándose a cada pozo."

---

## Resumen: de lo básico a lo avanzado

| Lo que vimos | Vanguardia equivalente |
|--------------|------------------------|
| MLP de regresión (porosidad) | PINNs (yacimiento completo) |
| Capas que extraen patrones | CNNs (sísmica) |
| DBSCAN anomalías (Módulo 4) | Digital twins (mantenimiento) |
| Regresión | Perforación autónoma, forecasting |

**Mensaje final:** "No aprendieron una herramienta de juguete. Aprendieron los
cimientos de la misma tecnología que hoy mueve la industria."
