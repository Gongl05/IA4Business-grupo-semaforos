# Plantilla 2 — Data Readiness Checklist
## Framework PROMPT | Fase R — Recursos de Datos
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** 30 de junio de 2026
**Tipo de IA del proyecto:** ML Tradicional — Clasificación Supervisada (Visión Computacional / CNN)

---

> **Nota de contexto del proyecto:** El usuario objetivo es un **conductor con daltonismo**.
> Por restricciones legales (prohibición de uso de celular al volante), el sistema opera
> desde un dispositivo fijo montado en el parabrisas o tablero (dashcam o soporte vehicular).
> Esto determina el ángulo de captura, la distancia al semáforo y los datasets relevantes.
> **Hallazgo crítico de investigación:** No existe ningún dataset público de semáforos
> capturado en Perú o Lima. El corpus peruano más cercano (UNMSM / Callao) fue recolectado
> para investigación interna y nunca fue publicado abiertamente. Esto representa un
> bloqueante real que se documenta en la Sección 3.
>
> **Nota de implementación final:** El sistema MVP final adoptó la estrategia Integrate
> en lugar de Build: se utiliza COCO-SSD (TensorFlow.js, base MobileNetV2) como modelo
> pre-entrenado en COCO sin fine-tuning propio. El dataset LISA fue usado únicamente
> para entrenar el modelo YOLOv8-nano (descartado). Los datasets BSTLD, Brazilian UFU
> e imágenes propias de Lima fueron evaluados en fase de planificación pero no utilizados
> en el producto final.

---

## SECCIÓN 1 — Inventario de datos

### Para ML Tradicional — Inventario de datos históricos

| # | Dataset | Fuente | Formato | N° de registros aprox. | ¿Tiene etiquetas? |
|---|---|---|---|---|---|
| 1 | LISA Traffic Light Dataset | UC San Diego — público en Kaggle | Imágenes + video / CSV de anotaciones | ~17,808 imágenes (50,542 bboxes) | SÍ (Go / Warning / Stop + variantes direccionales) — usado para entrenamiento de YOLOv8-nano (modelo descartado) |
| 2 | COCO (implícito) | Microsoft COCO — pesos del modelo COCO-SSD | Pesos pre-entrenados TF.js | >200,000 imágenes con 80 clases incluyendo "traffic light" | SÍ — pesos del modelo final en producción; no curado por el equipo |
| 3 | Detecciones propias en Supabase | Recopilación automática del sistema en producción | PostgreSQL / JPEG 200×120px | 488 detecciones anotadas en 4 sesiones formales | SÍ — anotación manual posterior (real_state, alerta_correcta) |

**Justificación de la selección:** El modelo final COCO-SSD aprovecha los pesos pre-entrenados
sobre COCO, que incluye la clase "traffic light" con suficiente diversidad de condiciones
lumínicas globales. El dataset LISA fue usado en la fase experimental para entrenar YOLOv8-nano,
que fue descartado por desequilibrio de clases (rojo: 25,876 bboxes vs amarillo: 1,516 bboxes),
lo que causaba clasificación sistemática como "verde". Las detecciones propias recopiladas
en Supabase permiten evaluación posterior con datos reales del entorno limeño.

---

**Variable objetivo (target):**

Estado actual del semáforo capturado por la cámara del dispositivo, clasificado en una de las
siguientes categorías: **rojo / amarillo / verde**. La ausencia de semáforo detectado no es
una clase del modelo sino la ausencia de respuesta (silencio del sistema). La clasificación
se realiza en tiempo real con latencia total mediana de 470ms desde captura hasta inicio de audio.

**Tipo de problema confirmado:**

- [x] Clasificación — predice una categoría (rojo / amarillo / verde)
- [ ] Regresión — predice un número continuo
- [ ] Clustering — agrupa sin etiquetas previas

---

## SECCIÓN 2 — Evaluación de calidad con semáforo

### Dataset / Fuente principal: LISA Traffic Light Dataset (UC San Diego) — usado en modelo experimental

| Dimensión | Semáforo | Evidencia que respalda la evaluación | Plan de acción (si es 🟡 o 🔴) |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Dataset público disponible en Kaggle. Descarga directa sin restricción de acceso. | No aplica |
| **Volumen** | 🟢 | ~17,808 imágenes con 50,542 bboxes. Desglose real: rojo 25,876 / amarillo 1,516 / verde resto. Desequilibrio severo entre clases. | No aplica — dataset descartado del modelo final |
| **Calidad** | 🔴 | Desequilibrio crítico de clases: amarillo con solo 1,516 instancias vs rojo 25,876. El modelo YOLOv8-nano entrenado con este dataset clasificaba sistemáticamente como "verde" — causa directa del descarte. | Descartado. El modelo final usa COCO-SSD pre-entrenado sin fine-tuning. |
| **Relevancia** | 🟡 | Perspectiva vehicular frontal coherente con el caso de uso. El contexto urbano difiere del latinoamericano. | No aplica — dataset descartado del modelo final |
| **Legalidad** | 🟢 | Licencia CC BY-NC-SA 4.0. Uso académico y de investigación sin restricciones adicionales. | No aplica |

---

### Dataset / Fuente principal del modelo final: COCO (pesos pre-entrenados COCO-SSD)

| Dimensión | Semáforo | Evidencia | Plan de acción |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Modelo COCO-SSD disponible como paquete TensorFlow.js (@tensorflow-models/coco-ssd). Integración directa sin descarga manual del dataset. | No aplica |
| **Volumen** | 🟢 | COCO contiene más de 200,000 imágenes etiquetadas con 80 clases, incluyendo "traffic light" con representación suficiente para detección robusta. | No aplica |
| **Calidad** | 🟢 | Modelo pre-entrenado ampliamente validado. Confianza promedio del detector en producción: 0.81. mAP reconocido en benchmarks académicos. | No aplica |
| **Relevancia** | 🟡 | Modelo entrenado sobre imágenes globales; no específico para Lima. La determinación del color se realiza mediante análisis de brillo por tercios (lógica de aplicación), no por el modelo CNN. | Mitigado con filtro de bounding box (altura ≥ 3.5% del frame) y estabilidad de 6 frames consecutivos. |
| **Legalidad** | 🟢 | Apache License 2.0. Uso comercial y académico permitido. | No aplica |

---

### Dataset / Fuente de evaluación: Detecciones propias recopiladas en Supabase

| Dimensión | Semáforo | Evidencia | Plan de acción |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Recopilación automática en PostgreSQL (Supabase). 488 detecciones en 4 sesiones formales documentadas. | No aplica |
| **Volumen** | 🟡 | 488 detecciones y 187 alertas TTS. Suficiente para evaluación de KRs pero limitado para reentrenamiento. | Escalar con más sesiones si el proyecto continúa. |
| **Calidad** | 🟢 | Cada detección incluye: predicted_state, confidence, bbox, tts_emitted, detection_time_ms, analysis_time_ms, latency_ms, frame_b64 (200×120px). Anotación manual posterior con real_state y alerta_correcta. | No aplica |
| **Relevancia** | 🟢 | Capturado en entorno urbano real de Lima en 4 condiciones: diurna, alta luminosidad/reflejos, atardecer, nocturna. Representa el contexto de uso exacto del producto. | No aplica |
| **Legalidad** | 🟢 | Datos propios del equipo. Frame JPEG en resolución reducida (200×120px) minimiza exposición de datos personales. | No aplica |

---

## SECCIÓN 3 — Plan de resolución de bloqueantes

### Bloqueante 1 — CRÍTICO 🔴 (identificado en fase de planificación)

```
Dimensión afectada:
Relevancia (los tres datasets evaluados inicialmente)

Descripción del problema:
No existe ningún dataset público de semáforos capturado en Lima o Perú.
El único corpus peruano conocido (desarrollado en UNMSM para el Callao)
nunca fue publicado abiertamente. Entrenar sin datos locales expone al
modelo a fallas específicas del entorno limeño:

- "Panza de burro": nubosidad costera difusa que aplana el contraste
  cromático y dificulta distinguir LEDs del fondo gris.
- Oclusión masiva: buses y cústers bloquean frecuentemente la línea
  de visión hacia el semáforo.
- Infraestructura híbrida: semáforos LED modernos conviven con equipos
  incandescentes obsoletos y contadores numéricos que el modelo puede
  confundir con semáforos independientes.

Resolución adoptada (Plan B ejecutado):
En lugar de fine-tuning con datos locales, se adoptó el modelo COCO-SSD
pre-entrenado (sin fine-tuning propio) como detector de objetos clase
"traffic light", complementado con lógica de análisis de color por brillo
en tercios verticales del bounding box. Esta arquitectura demostró
robustez ante variaciones lumínicas del entorno limeño, con F1 macro
de 83.9% en 488 detecciones reales.

El desequilibrio de clases en LISA (rojo 25,876 vs amarillo 1,516 bboxes)
fue la causa directa del descarte del modelo YOLOv8-nano entrenado.

Responsable dentro del equipo:
Gonzalo Gaviño (coordinación técnica)
Equipo completo (sesiones de prueba en campo)

Estado final:
RESUELTO mediante estrategia alternativa (COCO-SSD pre-entrenado).
```

---

## SECCIÓN 4 — Privacidad y legalidad de los datos

| Pregunta | Respuesta | Detalle |
|---|---|---|
| ¿Los datos contienen información personal de usuarios? | SÍ | Las imágenes de tráfico capturadas por el sistema pueden contener rostros de personas y placas vehiculares. Los frames almacenados en Supabase son en resolución reducida (200×120px JPEG), lo que limita la identificabilidad. |
| ¿Se cuenta con consentimiento explícito para usar esos datos? | N/A | Las sesiones de prueba fueron realizadas por miembros del equipo en vía pública. Las imágenes propias en vía pública no requieren consentimiento individual en espacio público bajo normativa peruana, pero deben minimizarse para protección de datos. |
| ¿Los datos serán anonimizados antes de usarlos en el proyecto? | SÍ | Los frames almacenados en Supabase tienen resolución reducida (200×120px) que dificulta la identificación de personas. El frame completo no se almacena. |
| ¿Aplica la Ley N° 29733 de Protección de Datos Personales del Perú? | SÍ | Si se capturan imágenes en vía pública con personas identificables, aplica la normativa peruana. Se mitiga mediante resolución reducida de los frames almacenados. |
| ¿Hay alguna restricción contractual o de confidencialidad? | NO | Los datasets públicos usados tienen licencias abiertas para uso académico. Los datos propios son del equipo. No hay datos corporativos ni confidenciales involucrados. |

---

## SECCIÓN 5 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Cada semáforo tiene evidencia concreta que lo respalda? | SÍ |
| ¿Todos los 🔴 tienen un plan de acción con fecha y responsable? | SÍ — El bloqueante crítico de datos locales fue resuelto mediante estrategia alternativa (COCO-SSD pre-entrenado). |
| ¿El equipo verificó el acceso real a los datos antes de completar este checklist? | SÍ — COCO-SSD disponible via npm (@tensorflow-models/coco-ssd); datos propios en Supabase con acceso confirmado. |
| ¿La estrategia de contexto o tipo de ML es coherente con los datos disponibles? | SÍ — Uso de modelo pre-entrenado COCO-SSD + análisis de color por brillo es coherente con la ausencia de dataset local etiquetado. |

> **Si alguna respuesta es NO → el checklist no está listo para entregar.**

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 2 de 4*
