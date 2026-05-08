# ChromaVía — Asistente de semáforos para conductores con daltonismo

**Curso:** AD5018 Inteligencia Artificial para Negocios — UTEC  
**Equipo:** Sebastian Sanchez · Gonzalo Gaviño · Giuseppe Del Negro  
**Framework:** PROMPT (Problema → Recursos → Oportunidad → Modelo → Testing)

---

## Descripción del proyecto

ChromaVía es un sistema de asistencia para conductores adultos con daltonismo rojo-verde (deuteranopía o protanopía) que operan vehículos privados en Lima Metropolitana. El sistema captura video desde una cámara fija montada en el vehículo, clasifica el estado del semáforo visible en tiempo real mediante una red neuronal convolucional (CNN), y emite una alerta de voz al conductor ("Semáforo en ROJO / VERDE / AMARILLO") cuando la predicción supera el umbral de confianza definido.

**Tipo de IA:** ML Tradicional — Clasificación Supervisada (Visión Computacional / CNN)  
**Arquitectura base:** MobileNetV2 con transfer learning  
**Datasets:** LISA (UC San Diego) · BSTLD (Bosch) · Brazilian UFU · imágenes propias de Lima  
**Síntesis de voz:** pyttsx3 (offline, determinista)

---

## Índice del repositorio

Las plantillas siguen el orden de las fases del Framework PROMPT. Cada una debe completarse y aprobarse antes de avanzar a la siguiente.

| # | Archivo | Fase | Contenido |
|---|---|---|---|
| 1 | [Plantilla 1 — Problem Statement Canvas](template_plantilla_1_problem_statement.md) | **P — Problema** | Definición del usuario afectado, problema específico, causa raíz, consecuencia medible, declaración formal del problema y filtro de validación de IA |
| 2 | [Plantilla 2 — Data Readiness Checklist](template_plantilla_2_data_readiness.md) | **R — Recursos** | Inventario de datasets (LISA, BSTLD, Brazilian UFU), evaluación de calidad con semáforo verde/amarillo/rojo, plan de resolución de bloqueantes y análisis de privacidad y legalidad |
| 3 | [Plantilla 3 — AI Product Canvas](template_plantilla_3_ai_product_canvas.md) | **O — Oportunidad** | Identidad del producto (nombre, problema, usuario, tipo de IA), estimación de costo del MVP, experiencia del usuario, flujo del producto, decisión Build/Buy/Integrate, especificaciones del modelo, alcance del MVP y OKRs + KPIs |

---

## Estado de avance

| Plantilla | Estado | Versión |
|---|---|---|
| Plantilla 1 — Problem Statement Canvas | Completada | v3 |
| Plantilla 2 — Data Readiness Checklist | Completada | v1 |
| Plantilla 3 — AI Product Canvas | Completada | v1 |
| Plantilla 4 — Evaluación de resultados | Pendiente | — |

---

## Resumen del problema y la solución

**Problema:** Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias, colisiones y pérdida de autonomía de conducción segura para los aproximadamente 200,000 conductores afectados en Lima Metropolitana.

**Solución MVP:**

```
Cámara vehicular fija
       ↓
CNN (MobileNetV2) — clasifica el frame: rojo / amarillo / verde / no detectado
       ↓
Evaluación de umbral de confianza
       ↓ (confianza ≥ umbral)
pyttsx3 — emite alerta de voz en español
       ↓
Conductor recibe la alerta y actúa de forma autónoma
```

**Criterio de éxito del MVP:** F1-score ≥ 80% en las 4 clases · Latencia total < 500ms · Tasa de falsos positivos < 10% en prueba de campo de 30 minutos.
