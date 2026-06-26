# SafeLight — Asistente de semáforos para conductores con daltonismo

**Curso:** AD5018 Inteligencia Artificial para Negocios — UTEC
**Equipo:** Sebastian Sanchez · Gonzalo Gaviño · Giuseppe Del Negro
**Framework:** PROMPT (Problema → Recursos → Oportunidad → Modelo → Performance → Transparencia)

---

## Descripción del proyecto

SafeLight es un sistema de asistencia para conductores adultos con daltonismo rojo-verde (deuteranopía o protanopía) que operan vehículos privados en Lima Metropolitana. El sistema clasifica el estado del semáforo visible en tiempo real mediante visión computacional y emite una alerta de voz al conductor ("Semáforo en ROJO / VERDE / AMARILLO") cuando la predicción supera el umbral de confianza definido. Si la confianza es insuficiente, el sistema permanece en silencio (fail-safe por omisión).

**Tipo de IA:** ML Tradicional — Clasificación Supervisada (Visión Computacional)
**Arquitectura:** Clasificador híbrido (detección de círculos Hough Transform + centroides HSV calibrados) con CNN como segunda opinión
**Dataset:** Udacity Traffic Light Classifier
**Síntesis de voz:** gTTS
**MVP desplegado:** https://huggingface.co/spaces/Gonzagl5/safelight

---

## Índice del repositorio

### Plantillas (Framework PROMPT)

| # | Archivo | Fase | Contenido |
|---|---|---|---|
| 1 | [plantilla_1_problem_statement.md](plantillas/plantilla_1_problem_statement.md) | **P — Problema** | Usuario afectado, problema específico, causa raíz, consecuencia medible y declaración formal del problema |
| 2 | [plantilla_2_data_readiness.md](plantillas/plantilla_2_data_readiness.md) | **R — Recursos** | Inventario de datasets, evaluación de calidad, plan de resolución de bloqueantes y análisis de privacidad |
| 3 | [plantilla_3_ai_product_canvas.md](plantillas/plantilla_3_ai_product_canvas.md) | **O — Oportunidad** | Identidad del producto, flujo, decisión Build/Buy/Integrate, alcance del MVP y OKRs |
| 4 | [plantilla_4_scorecard_risk.md](plantillas/plantilla_4_scorecard_risk.md) | **P2 + T — Performance y Transparencia** | Scorecard de OKRs con resultados reales, validación iterativa y matriz de riesgos |

### MVP

| Archivo | Contenido |
|---|---|
| [mvp/README_mvp.md](mvp/README_mvp.md) | URL de despliegue e instrucciones de uso para el evaluador |
| [mvp/evidencia/resultados_okr.md](mvp/evidencia/resultados_okr.md) | Métricas reales obtenidas vs. metas comprometidas en PC1 |
| [mvp/evidencia/pruebas_usuario.md](mvp/evidencia/pruebas_usuario.md) | Registro de validación iterativa: hallazgos y cambios implementados |

### Otros archivos

| Archivo | Contenido |
|---|---|
| [resumen_ejecutivo.md](resumen_ejecutivo.md) | Síntesis del proyecto con problema, solución y OKRs |
| [SafeLight_Sustentacion_AD5018.pdf](SafeLight_Sustentacion_AD5018.pdf) | Deck de sustentación PC2 |

---

## Estado del proyecto

| Entregable | Estado |
|---|---|
| Plantilla 1 — Problem Statement Canvas | ✅ Completada (v3) |
| Plantilla 2 — Data Readiness Checklist | ✅ Completada (v1) |
| Plantilla 3 — AI Product Canvas | ✅ Completada (v1) |
| Plantilla 4 — Scorecard & Risk Matrix | ✅ Completada con datos reales |
| MVP desplegado y accesible por URL | ✅ https://huggingface.co/spaces/Gonzagl5/safelight |
| Evidencia de validación | ✅ 4 iteraciones documentadas en pruebas_usuario.md |

---

## Resultados del MVP (PC2)

| KR | Meta | Resultado |
|---|---|---|
| KR1 — F1-score macro | ≥ 80% | **86.4%** ✅ (amarillo: 62.1% ⚠️) |
| KR2 — Latencia de inferencia | < 500 ms | **380 ms** ✅ |
| KR3 — Tasa de falsas alertas | < 10% | **0%** tras calibración de umbral ✅ |

---

## Resumen del problema y la solución

**Problema:** Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias y colisiones para los aproximadamente 200,000 conductores afectados en Lima Metropolitana.

**Flujo del MVP:**
```
Cámara apunta al semáforo
       ↓
Detección de círculos (Hough Transform) → aísla el foco encendido
       ↓
Clasificación de color por centroides HSV calibrados
       ↓
¿Confianza ≥ 0.5?
       ↓ SÍ                    ↓ NO
gTTS emite alerta de voz     Silencio (fail-safe)
       ↓
Conductor recibe la alerta y actúa de forma autónoma
```
