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
| KR3 — Uso seguro | Tasa de falsas alertas durante validación iterativa | <10% | **0 falsas alertas tras calibración del umbral** | ✅ Cumple* | pruebas_usuario.md | *Validación realizada por el equipo durante el desarrollo; no es una sesión de campo externa. El umbral de confianza calibrado (0.5) elimina las alertas en casos ambiguos mediante fail-safe. |

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

# KR3 — Tasa de falsas alertas

| Campo | Resultado |
|---|---|
| Meta | Tasa de falsas alertas < 10% |
| Resultado | **0 falsas alertas** tras calibración del umbral de confianza a 0.5 |
| Método | Validación iterativa por el equipo durante el desarrollo — ver pruebas_usuario.md |
| Nota | Objetos ambiguos (ropa, personas) que antes generaban falsas alertas ahora devuelven silencio (confianza < 0.5). El sistema nunca emite una alerta incorrecta con alta confianza — o acierta, o se calla. |

---

# Resumen ejecutivo

## Objetivos alcanzados

- ✅ KR1: Calidad del modelo superó la meta (86.4% vs 80%).
- ✅ KR2: Latencia ampliamente mejor que el objetivo (<500 ms).
- ✅ KR3: 0 falsas alertas tras calibración del umbral — validación iterativa por el equipo.

## Principales aprendizajes

La validación técnica del modelo confirmó que la arquitectura (CNN + fail-safe) es viable para tiempo real. El mecanismo fail-safe funciona como fue diseñado: ante incertidumbre, el sistema prefiere no emitir una alerta antes que entregar una indicación potencialmente incorrecta. La limitación principal es el desempeño en la clase amarillo (F1 62.1%), vinculada directamente a la ausencia de datos locales de Lima.

## Próximas mejoras

- Entrenar el modelo con imágenes reales de semáforos de Lima.
- Aumentar la cantidad de ejemplos de la clase amarillo.
- Completar la sesión de validación en campo (KR3).

---

*Framework PROMPT v1.1 — AD5018 UTEC*
