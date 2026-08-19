# Actividad Práctica: Teachable Machine

## Módulo 6 · 5-10 minutos de exploración guiada

**Herramienta:** teachablemachine.withgoogle.com (gratis, en el navegador, sin código)
**Objetivo:** que los estudiantes EXPERIMENTEN el proceso completo de entrenar un
modelo, usando su propia cámara web.

> Importante: Teachable Machine es CLASIFICACIÓN (categorías), no regresión como
> nuestro ejemplo de porosidad. Pero el FLUJO de trabajo es idéntico: recolectar
> datos, entrenar, probar. Aclara esto al inicio.

---

## Paso a paso (5-10 minutos)

### Paso 1: Abrir y elegir proyecto (1 min)

1. Entra a teachablemachine.withgoogle.com
2. Haz clic en "Get Started" y luego en "Image Project".

**Frase:** "Vamos a construir un modelo que aprende a distinguir entre dos cosas,
igual que nuestra red aprendió a distinguir porosidad."

### Paso 2: Recolectar datos (3 min)

1. Clase 1: apunta la cámara a tu mano ABIERTA y mantén presionado "Record" mientras
   la mueves lentamente (para capturar distintos ángulos).
2. Clase 2: haz lo mismo con tu mano en PUÑO.
3. Recolecta al menos 50-100 imágenes por clase.

**Frase clave (conexión con el curso):** "Esto que están haciendo es EXACTAMENTE el
conjunto de entrenamiento. Nuestra red de porosidad usó 5,600 registros; ustedes
están recolectando sus propios datos ahora mismo."

### Paso 3: Entrenar (1 min)

1. Haz clic en "Train Model".
2. Observa cómo entrena en segundos.

**Frase:** "Este botón es el model.fit() de nuestro notebook. Aquí el modelo ajusta
sus pesos para aprender a distinguir mano abierta de puño."

### Paso 4: Probar (2 min)

1. Apunta la cámara a tu mano abierta y mira la predicción en vivo.
2. Cambia a puño. Observa cómo cambia la probabilidad.

**Frase:** "Esto es el model.predict() de nuestro notebook. La red da una probabilidad
para cada clase. Si falla, es porque necesita más datos — igual que nuestro modelo."

### Paso 5: Romperlo a propósito (2 min) — la parte más didáctica

1. Apunta la cámara a algo que NUNCA vio (una taza, otra persona, la pared).
2. Observa cómo el modelo "adivina" con baja confianza.

**Frase clave (anticipo del overfitting):** "Miren esto: el modelo nunca vio una taza,
pero tiene que responder. Esto es el equivalente a nuestro 'tramo ciego' del Día 2.
Cuando el modelo ve datos nuevos que no conoce, se nota si GENERALIZÓ o solo memorizó
sus ejemplos de entrenamiento."

---

## Conexión con los conceptos del curso

| Paso de Teachable Machine | Concepto del curso |
|---------------------------|-------------------|
| Recolectar imágenes | Crear el dataset de entrenamiento |
| Train Model | model.fit() (ajustar pesos) |
| Predecir en vivo | model.predict() |
| Probar con algo nuevo | El tramo ciego (validación) |
| Baja confianza con datos nuevos | Generalización vs memorización |

---

## Variante petrolera (si hay tiempo)

En vez de manos, usa OBJETOS relacionados con la clase:
- Clase 1: una foto de un núcleo de roca (o una piedra porosa)
- Clase 2: una foto de una roca compacta (o un ladrillo)

**Frase:** "Esto es clasificación de litología: arena vs arcilla. Es el mismo
concepto del PCA del Módulo 4, donde clasificamos litofacies."

---

## Resumen de frases clave (para memorizar)

1. "Recolectar datos = construir el dataset de entrenamiento."
2. "Train Model = model.fit(): ajustar pesos."
3. "Predecir en vivo = model.predict()."
4. "Probar algo nuevo = el tramo ciego: ¿generalizó o memorizó?"
