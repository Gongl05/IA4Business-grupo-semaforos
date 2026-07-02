# Resumen Ejecutivo — SafeLight

> **Curso:** AD5018 — Inteligencia Artificial para Negocios · UTEC
> **Equipo:** Sebastian Sanchez · Gonzalo Gaviño · Giuseppe Del Negro
> **Entregable:** PC2 — Sustentación Final — Ciclo 2026-1

---

## 1. Identidad del MVP

**Nombre:** SafeLight

**MVP desplegado:** https://safelight-web.vercel.app

**Usuario principal:** Conductor adulto con daltonismo rojo-verde (deuteranopía o protanopía) que opera un vehículo privado en entornos urbanos de Lima Metropolitana y requiere asistencia autónoma para interpretar el estado de semáforos convencionales en tiempo real durante la conducción. **Mercado potencial estimado: ~200,000 conductores** en Lima Metropolitana.

---

## 2. Declaración del problema

> *"Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias, colisiones y pérdida de autonomía de conducción segura para los aproximadamente 200,000 conductores afectados en Lima Metropolitana."*

---

## 3. Tipo de IA y justificación

**Tipo:** ML Tradicional — Clasificación Supervisada (Visión Computacional)

El problema requiere detectar semáforos y clasificar su color desde frames de video en tiempo real. Se adoptó la estrategia **Integrate**: se usa COCO-SSD (TensorFlow.js, base MobileNetV2, pre-entrenado en COCO) como detector de objetos y se le agrega un algoritmo propio de análisis de brillo por tercios verticales para la clasificación del color. La alerta de voz se genera con Web Speech API (nativa en el navegador), sin dependencia de servidor ni API externa. Un LLM añadiría latencia, variabilidad y costo innecesarios cuando la salida siempre es una de tres frases predefinidas.

---

## 4. Arquitectura del sistema (versión final construida)

**Lo que planeamos (PC1):** CNN MobileNetV2 entrenada con transfer learning en Google Colab + pipeline local Python (OpenCV + TensorFlow Lite + pyttsx3 offline).

**Lo que construimos (PC2):** App web en Vercel que ejecuta COCO-SSD en el navegador + algoritmo de brillo por tercios + Web Speech API. Sin instalación, accesible por link, con modo evaluación integrado que exporta métricas de latencia (JSON) y falsas alertas (CSV).

**Flujo del MVP:**
```
Conductor abre safelight-web.vercel.app en su celular (soporte vehicular fijo)
       ↓
COCO-SSD detecta objeto "traffic light" en el frame
¿Confianza ≥ 0.35 y bbox ≥ 3.5% del frame? → NO: silencio (fail-safe)
       ↓ SÍ
Análisis de brillo en 3 tercios verticales del recorte del bbox:
  superior más brillante → ROJO
  medio más brillante    → AMARILLO
  inferior más brillante → VERDE
       ↓
¿6 frames consecutivos del mismo color? → NO: silencio (fail-safe)
       ↓ SÍ
Web Speech API emite alerta de voz: "Semáforo en ROJO / VERDE / AMARILLO"
```

---

## 5. Decisión tecnológica final — Integrate

| Componente | Herramienta | Costo |
|---|---|---|
| Detección de semáforos | COCO-SSD via TensorFlow.js (@tensorflow-models/coco-ssd) | 🟢 S/. 0 |
| Clasificación de color | Algoritmo propio (análisis de brillo por tercios) | 🟢 S/. 0 |
| Frontend / despliegue | App web (HTML/CSS/JS) en Vercel capa gratuita | 🟢 S/. 0 |
| Síntesis de voz | Web Speech API (nativa en navegador) | 🟢 S/. 0 |
| Base de datos de evaluación | Supabase (PostgreSQL) capa gratuita | 🟢 S/. 0 |

**Costo total del MVP: S/. 0 / mes.**

---

## 6. OKRs — resultados reales obtenidos

**Objetivo:** Permitir que conductores con daltonismo identifiquen el estado de semáforos urbanos de forma autónoma y en tiempo real, eliminando su dependencia de estrategias visuales compensatorias durante la conducción en Lima.

| # | Métrica | Meta comprometida | Resultado real | ¿Cumplió? |
|---|---|---|---|---|
| **KR1** | F1-score macro del sistema (3 clases) | ≥ 80% | **83.9%** (rojo 86.6% · verde 85.1% · amarillo 80.0%) | ✅ SÍ |
| **KR2** | Latencia total (captura → alerta de voz) | < 500 ms mediana | **470 ms** mediana (n=185, 4 sesiones) | ✅ SÍ |
| **KR3** | Tasa de alertas falsas en campo | < 10% | **12.3%** global (23 de 187 alertas) | ❌ NO |

**Conclusión:** El MVP cumplió parcialmente su objetivo. KR1 y KR2 se alcanzaron. KR3 no se cumplió: la tasa global de falsas alertas fue 12.3%, impulsada principalmente por la condición de alta luminosidad y reflejos (18.4%), frecuente en Lima. El sistema es confiable como asistencia complementaria en condiciones diurnas normales (tasa falsa 8.7%), pero requiere mejoras en robustez ante contraluz antes de operar sin supervisión activa del conductor.

---

## 7. Validación en campo

| # | Escenario | Conductores / dispositivo | Falsas alertas | Latencia mediana |
|---|---|---|---|---|
| 1 | Diurna normal (Av. Miguel Grau) | Conductor con daltonismo / Pixel 7 | 8.7% | 457 ms |
| 2 | Alta luminosidad / reflejos | Sin daltonismo / Samsung A54 | 18.4% | 481 ms |
| 3 | Atardecer | Sin daltonismo / iPhone 14 | 9.1% | 458 ms |
| 4 | Nocturna | Sin daltonismo / Xiaomi 13T | 12.5% | 473 ms |

---

*Resumen Ejecutivo — SafeLight · AD5018 UTEC · Ciclo 2026-1*
