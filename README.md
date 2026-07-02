# SafeLight — Asistente de semáforos para conductores con daltonismo

**Curso:** AD5018 Inteligencia Artificial para Negocios — UTEC
**Equipo:** Sebastian Sanchez · Gonzalo Gaviño · Giuseppe Del Negro
**Framework:** PROMPT (Problema → Recursos → Oportunidad → Modelo → Performance → Transparencia)

---

## Descripción del proyecto

SafeLight es un sistema de asistencia para conductores adultos con daltonismo rojo-verde (deuteranopía o protanopía) que operan vehículos privados en Lima Metropolitana. El sistema detecta semáforos en tiempo real mediante visión computacional y emite una alerta de voz al conductor ("Semáforo en ROJO / VERDE / AMARILLO") cuando la predicción es estable y supera el umbral de confianza. Si la confianza es insuficiente, el sistema permanece en silencio (fail-safe por omisión).

**Tipo de IA:** ML Tradicional — Clasificación Supervisada (Visión Computacional)
**Arquitectura:** COCO-SSD (TensorFlow.js, pre-entrenado en COCO) para detección + algoritmo propio de análisis de brillo por tercios verticales para clasificación de color
**Dataset de evaluación:** 488 detecciones anotadas en Supabase (4 sesiones de prueba en campo en Lima)
**Síntesis de voz:** Web Speech API (nativa en navegador)
**MVP desplegado:** https://safelight-web.vercel.app

---

## Índice del repositorio

### Plantillas (Framework PROMPT)

| # | Archivo | Fase | Contenido |
|---|---|---|---|
| 1 | [plantilla_1_problem_statement.md](plantillas/plantilla_1_problem_statement.md) | **P — Problema** | Usuario afectado, problema específico, causa raíz, consecuencia medible y declaración formal del problema |
| 2 | [plantilla_2_data_readiness.md](plantillas/plantilla_2_data_readiness.md) | **R — Recursos** | Inventario de datasets, evaluación de calidad, plan de resolución de bloqueantes y análisis de privacidad |
| 3 | [plantilla_3_ai_product_canvas.md](plantillas/plantilla_3_ai_product_canvas.md) | **O — Oportunidad** | Identidad del producto, flujo, decisión Build/Buy/Integrate, alcance del MVP y OKRs |
| 4 | [plantilla_4_scorecard_risk.md](plantillas/plantilla_4_scorecard_risk.md) | **P2 + T — Performance y Transparencia** | Scorecard de OKRs con resultados reales, validación en campo y matriz de riesgos |

### MVP

| Archivo | Contenido |
|---|---|
| [mvp/README_mvp.md](mvp/README_mvp.md) | URL de despliegue e instrucciones de uso para el evaluador |
| [mvp/evidencia/resultados_okr.md](mvp/evidencia/resultados_okr.md) | Métricas reales obtenidas vs. metas comprometidas en PC1 |
| [mvp/evidencia/pruebas_usuario.md](mvp/evidencia/pruebas_usuario.md) | Registro de pruebas en campo con 4 conductores: hallazgos y cambios implementados |
| [mvp/evidencia/sessions_s.csv](mvp/evidencia/sessions_s.csv) | Export Supabase — 4 sesiones formales con métricas agregadas |
| [mvp/evidencia/detections_s.csv](mvp/evidencia/detections_s.csv) | Export Supabase — 488 detecciones anotadas con `real_state` y `alerta_correcta` |
| [mvp/evidencia/sessions_rows.csv](mvp/evidencia/sessions_rows.csv) | Export completo tabla `sessions` (incluye sesiones de prueba) |
| [mvp/evidencia/detections_rows.csv](mvp/evidencia/detections_rows.csv) | Export completo tabla `detections` (incluye frames en base64) |

### Otros archivos

| Archivo | Contenido |
|---|---|
| [resumen_ejecutivo.md](resumen_ejecutivo.md) | Síntesis del proyecto con problema, solución y OKRs |
| [presentacion_pc2.pdf](presentacion_pc2.pdf) | Deck de sustentación PC2 (versión final) |

---

## Estado del proyecto

| Entregable | Estado |
|---|---|
| Plantilla 1 — Problem Statement Canvas | ✅ Completada (v3) |
| Plantilla 2 — Data Readiness Checklist | ✅ Completada (versión final) |
| Plantilla 3 — AI Product Canvas | ✅ Completada (v2) |
| Plantilla 4 — Scorecard & Risk Matrix | ✅ Completada con datos reales de campo |
| MVP desplegado y accesible por URL | ✅ https://safelight-web.vercel.app |
| Evidencia de validación en campo | ✅ 4 sesiones reales documentadas (187 alertas anotadas) |

---

## Resultados del MVP (PC2)

| KR | Meta | Resultado | ¿Cumplió? |
|---|---|---|---|
| KR1 — F1-score macro | ≥ 80% | **83.9%** (rojo: 86.6% · verde: 85.1% · amarillo: 80.0%) | ✅ SÍ |
| KR2 — Latencia total (captura → alerta de voz) | < 500 ms mediana | **470 ms** mediana (n=185, 4 sesiones) | ✅ SÍ |
| KR3 — Tasa de falsas alertas | < 10% | **12.3%** global (23 de 187 alertas) | ❌ NO |

**Interpretación:** El sistema acierta el color del semáforo en el 87.7% de las alertas emitidas y cumple la meta de latencia. Sin embargo, la tasa de alertas falsas del 12.3% superó el umbral comprometido del 10%, impulsada principalmente por la condición de alta luminosidad y reflejos (18.4%), frecuente en Lima. El MVP es funcional como prototipo académico pero requiere mejoras en robustez ante contraluz antes de operar sin supervisión activa del conductor.

---

## Resumen del problema y la solución

**Problema:** Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias y colisiones para los aproximadamente 200,000 conductores afectados en Lima Metropolitana.

**Flujo del MVP:**
```
Cámara captura frames en tiempo real (app web en el celular del conductor)
       ↓
COCO-SSD detecta objeto clase "traffic light" en el frame
       ↓
¿Confianza ≥ 0.35 y tamaño del bbox ≥ 3.5% del frame?
       ↓ SÍ                              ↓ NO
Análisis de brillo en 3 tercios      Silencio (fail-safe)
verticales del recorte del bbox
(superior → ROJO / medio → AMARILLO / inferior → VERDE)
       ↓
¿6 frames consecutivos del mismo color?
       ↓ SÍ                              ↓ NO
Web Speech API emite alerta de voz   Silencio (fail-safe)
("Semáforo en ROJO / VERDE / AMARILLO")
       ↓
Conductor recibe la alerta y actúa de forma autónoma
```
