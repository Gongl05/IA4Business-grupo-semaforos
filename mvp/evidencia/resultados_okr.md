# Resultados OKR — Datos Reales (PC2)

> Comparación de las metas comprometidas en la PC1 (Plantilla 3, Sección 8) contra los resultados reales obtenidos durante el desarrollo, entrenamiento y validación del MVP.

---

# Objetivo general

Permitir que conductores con daltonismo identifiquen el estado de un semáforo mediante clasificación automática de imágenes con alerta de voz, priorizando la seguridad mediante un mecanismo **fail-safe**: cuando el sistema no alcanza un nivel mínimo de confianza, permanece en silencio antes que emitir una indicación potencialmente incorrecta.

---

# Comparación de resultados

| Objetivo / KR | Métrica | Meta comprometida (PC1) | Resultado obtenido | Cumplimiento | Fuente de evidencia | Observaciones |
|---|---:|---:|---:|---:|---|---|
| KR1 — Calidad | F1-score macro | ≥80% | **86.4%** | ✅ 108% | training_results.json | La meta se cumple. La clase amarillo continúa siendo la más difícil debido a la poca cantidad de ejemplos disponibles. |
| KR2 — Latencia | Tiempo de inferencia | <500 ms | **60–400 ms** | ✅ Cumple | Medición directa del pipeline | El tiempo corresponde únicamente a la inferencia del modelo; no incluye la generación del audio. |
| KR3 — Uso seguro | Tasa de falsas alertas durante pruebas de usuario | <10% | **0 falsas alertas observadas en 4 sesiones (0%)** | ✅ Cumple* | pruebas_usuario.md | Muestra pequeña; el resultado sirve como evidencia inicial, no como validación estadística definitiva. |

> **\*Nota:** El KR3 se considera cumplido para el alcance del MVP. Sin embargo, la cantidad de participantes aún es insuficiente para estimar el desempeño real del sistema en condiciones de uso masivo.

---

# Nota de transparencia sobre desviaciones respecto a la PC1

Durante el desarrollo se realizaron algunos cambios respecto al planteamiento inicial debido a restricciones técnicas y de tiempo.

| Planeado en PC1 | Implementado en el MVP | Motivo |
|---|---|---|
| Dataset LISA + BSTLD + UFU | Dataset Udacity Traffic Light Classifier | Permitía entrenar rápidamente porque las imágenes ya estaban recortadas y etiquetadas. |
| MobileNetV2 con transfer learning | CNN propia entrenada desde cero | El entorno de desarrollo impedía descargar los pesos preentrenados. |
| Voz offline con pyttsx3 | gTTS | Mayor facilidad de despliegue en la aplicación web. |
| Clasificación directa | Preprocesamiento de localización + clasificación | Se detectó durante las pruebas que la carcasa del semáforo influía negativamente en la predicción. |

Estas decisiones permitieron terminar un MVP funcional, aunque también dejaron limitaciones abiertas que se documentan para futuras iteraciones.

---

# KR1 — Calidad del modelo

| Campo | Resultado |
|---|---:|
| Meta | F1-score ≥80% |
| Resultado | **86.4%** |
| Accuracy | **94.9%** |

## Desempeño por clase

| Clase | Precisión | Recall | F1 |
|---|---:|---:|---:|
| Verde | 100% | 93.5% | 96.6% |
| Rojo | 97.7% | 95.6% | 96.6% |
| Amarillo | 45.0% | 100% | 62.1% |

### Interpretación

El modelo cumple ampliamente la meta global. Sin embargo, el desempeño sobre la clase **amarillo** continúa siendo inferior debido a la escasa cantidad de ejemplos disponibles durante el entrenamiento. Esta limitación permanece abierta y representa una oportunidad clara de mejora.

## Incidente detectado durante el desarrollo

En pruebas previas a la validación con usuarios se observó que la CNN confundía el color de la carcasa del semáforo con el color de la luz encendida.

Como respuesta, el equipo incorporó un paso previo de localización del foco mediante procesamiento de imágenes antes de realizar la clasificación del color.

Esta modificación permitió mejorar considerablemente la robustez del sistema frente a fotografías reales.

---

# KR2 — Latencia

| Campo | Resultado |
|---|---:|
| Meta | <500 ms |
| Resultado | **60–400 ms** |

El sistema responde muy por debajo del límite establecido, permitiendo una interacción prácticamente inmediata para el usuario.

---

# KR3 — Validación con usuarios

Se realizaron pruebas funcionales con **4 participantes**, utilizando tanto el modo cámara como el modo fotografía.

## Resultados principales

- 4 de 4 participantes lograron completar la tarea.
- No se registraron falsas alertas durante las pruebas.
- Todos recibieron correctamente las alertas cuando el sistema detectó el estado del semáforo.
- En escenarios de baja confianza el sistema permaneció en silencio, cumpliendo el comportamiento esperado del mecanismo fail-safe.

## Problemas detectados

- El silencio del sistema puede interpretarse como que la aplicación dejó de funcionar.
- El permiso inicial de cámara no resulta evidente para algunos usuarios.
- El movimiento del celular dificulta una detección rápida.
- El modo Foto terminó siendo una alternativa útil cuando no existía un semáforo disponible para probar.

Estas observaciones permitieron identificar mejoras concretas para una siguiente iteración del producto.

---

# Resumen ejecutivo

## Objetivos alcanzados

- ✅ KR1: Calidad del modelo superó la meta (86.4% vs 80%).
- ✅ KR2: Latencia ampliamente mejor que el objetivo (<500 ms).
- ✅ KR3: No se observaron falsas alertas durante las pruebas con usuarios.

## Principales aprendizajes

Las pruebas con usuarios demostraron que el principal reto ya no es el algoritmo de clasificación, sino la experiencia de uso. La necesidad de comunicar claramente cuándo el sistema está escaneando, facilitar el permiso de cámara y orientar mejor al usuario fueron hallazgos que no habían aparecido durante el desarrollo técnico.

Asimismo, la validación confirmó que el mecanismo fail-safe funciona como fue diseñado: ante incertidumbre, el sistema prefiere no emitir una alerta antes que entregar una indicación potencialmente incorrecta.

## Próximas mejoras

- Incorporar un indicador permanente de "Escaneando...".
- Mejorar el flujo inicial de permisos de cámara.
- Entrenar el modelo con imágenes reales de semáforos de Lima.
- Aumentar la cantidad de ejemplos de la clase amarillo.
- Realizar una validación con una muestra significativamente mayor de usuarios.

---

*Framework PROMPT v1.1 — AD5018 UTEC*
