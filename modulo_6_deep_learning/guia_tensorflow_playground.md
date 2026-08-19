# Guía de Uso: TensorFlow Playground en Clase

## Módulo 6 · Día 1 · 10 minutos de demo interactiva

**Herramienta:** playground.tensorflow.org (gratis, en el navegador, sin código)
**Objetivo:** hacer tangible la intuición de "por qué capas y neuronas" que ya vimos en el notebook.

> Nota importante: el Playground es de CLASIFICACIÓN (regiones azul/naranja), no de
> regresión como nuestro ejemplo de porosidad. Pero el PRINCIPIO es idéntico: las
> capas ocultas con ReLU son las que permiten aprender relaciones no-lineales.
> Aclara esto a los estudiantes en un minuto.

---

## Paso a paso (10 minutos)

### Minuto 0-1: Preparar y contextualizar

- Abre playground.tensorflow.org y proyéctalo.
- Frase de apertura: "Esto es un simulador de Google donde ven las neuronas aprender EN VIVO. Aquí es clasificación, pero el principio es el mismo que en nuestra regresión de porosidad: las capas ocultas aprenden patrones no-lineales."

### Minuto 1-3: El dataset "Spiral" sin capas ocultas (fracaso)

1. En "DATOS", elige el dataset **Spiral** (espiral de dos colores).
2. En "CAPAS OCULTAS", deja **0 capas ocultas** (borra cualquier capa).
3. En "NEURONAS", deja 1 neurona (la de salida).
4. Presiona el botón de **play** (triángulo).

**Qué pasa:** la línea de decisión es recta y no logra separar la espiral. El error (loss) se queda alto.

**Frase:** "Sin capas ocultas, la red solo puede dibujar una LÍNEA RECTA. La espiral es no-lineal: imposible separarla con una recta. Por eso nuestro MLP necesita capas ocultas."

### Minuto 3-5: Añadir capas ocultas (éxito)

1. Añade **2 capas ocultas** (botón "+").
2. Pon **4 neuronas** en cada capa.
3. Activa **ReLU** en ambas (selector de activación).
4. Reinicia y presiona play.

**Qué pasa:** ahora la frontera se curva y empieza a separar la espiral.

**Frase:** "Acabamos de ver la magia: con capas ocultas y ReLU, la red aprende CURVAS, no solo rectas. Esto es exactamente por qué usamos ReLU en las capas ocultas de nuestro notebook."

### Minuto 5-7: Subir neuronas (más capacidad)

1. Sube a **8 neuronas** por capa.
2. Reinicia y play.

**Qué pasa:** separa mejor y más rápido.

**Frase:** "Más neuronas = más capacidad. Pero recuerden: no siempre más es mejor. Demasiadas neuronas y empezará a memorizar. Eso lo veremos mañana con el overfitting."

### Minuto 7-9: Ver el overfitting (anticipo del Día 2)

1. Pon **8 neuronas** en la primera capa, **8 en la segunda**, y añade más capas si quieres.
2. Deja entrenar MUCHO tiempo (muchas épocas).
3. Señala que la pérdida de entrenamiento baja a casi cero, pero la de validación (test) ya no mejora.

**Frase:** "Miren esto: el error de entrenamiento baja y baja, pero el de prueba (test) se estanca. La red está MEMORIZANDO los datos de entrenamiento, no aprendiendo el patrón. Ese es el enemigo del Día 2."

### Minuto 9-10: Cierre y conexión

**Frase de cierre:** "Todo lo que vieron aquí — capas, neuronas, ReLU, overfitting — es lo mismo que haremos con nuestro pozo. El Playground es la versión de juguete; el notebook es la versión real."

---

## Los 3 datasets que puedes usar

| Dataset | Qué demuestra | Cuándo usarlo |
|---------|---------------|---------------|
| **Spiral** | La no-linealidad requiere capas ocultas | Para mostrar el poder de las capas (principal) |
| **Circle** | Una red lineal no puede separar el círculo | Alternativa al spiral, más simple |
| **XOR** | El problema clásico no-lineal | Si el público es más técnico |

---

## Resumen de frases clave (para memorizar)

1. "Sin capas ocultas, solo rectas. Con capas y ReLU, curvas."
2. "Más neuronas = más capacidad, pero cuidado con memorizar."
3. "El error de entrenamiento baja y baja; el de prueba se estanca: overfitting."
4. "Todo esto es lo mismo que haremos con el pozo, pero de juguete."
