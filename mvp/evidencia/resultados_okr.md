# Resultados OKR — Datos Reales (PC2)

> Comparación de las metas comprometidas en PC1 (Plantilla 3, Sección 8) contra los resultados reales obtenidos en campo durante la validación del MVP.

**Fuente de datos:** Supabase (PostgreSQL) — exportado el 01/07/2026.
Los datos brutos se encuentran en los archivos CSV de esta carpeta:

| Archivo | Contenido |
|---|---|
| [sessions_s.csv](sessions_s.csv) | 4 sesiones formales de campo con métricas agregadas |
| [detections_s.csv](detections_s.csv) | 488 detecciones anotadas de las 4 sesiones formales (con `real_state` y `alerta_correcta`) |
| [sessions_rows.csv](sessions_rows.csv) | Export completo de la tabla `sessions` en Supabase (incluye sesiones de prueba) |
| [detections_rows.csv](detections_rows.csv) | Export completo de la tabla `detections` en Supabase (incluye frames en base64) |

---

## Objetivo general

Permitir que conductores con daltonismo identifiquen el estado de semáforos urbanos de forma autónoma y en tiempo real, eliminando su dependencia de estrategias visuales compensatorias durante la conducción en Lima.

---

## Comparación de resultados

| KR | Métrica | Meta (PC1) | Resultado real | ¿Cumplió? |
|---|---:|---:|---:|---:|
| KR1 — Calidad | F1-score macro (3 clases) | ≥ 80% | **83.9%** | ✅ SÍ |
| KR2 — Latencia | Latencia total mediana (captura → alerta de voz) | < 500 ms | **470 ms** | ✅ SÍ |
| KR3 — Uso seguro | Tasa de falsas alertas en campo | < 10% | **12.3%** | ❌ NO |

---

## KR1 — Calidad del modelo (F1-score)

**Fuente:** 488 detecciones en `detections_s.csv`, campo `alerta_correcta` + `predicted_state` + `real_state`.

| Clase | Precisión | Recall | F1 |
|---|---:|---:|---:|
| Rojo | 88.2% | 85.1% | **86.6%** |
| Verde | 86.9% | 83.5% | **85.1%** |
| Amarillo | 81.3% | 78.8% | **80.0%** |
| **Macro promedio** | | | **83.9%** |

**Interpretación:** La meta de F1 ≥ 80% se cumple. La clase amarillo tiene el desempeño más bajo (80.0%), consistente con su menor representación relativa en condiciones reales (los semáforos en Lima permanecen en amarillo muy brevemente).

---

## KR2 — Latencia total (captura → alerta de voz)

**Fuente:** Columna `latency_ms` en `detections_s.csv`. n = 185 alertas TTS emitidas en las 4 sesiones.

| Estadístico | Valor |
|---|---:|
| Mediana | **470 ms** |
| Promedio | 518 ms |
| P95 | 927 ms |

**Interpretación:** La mediana de 470 ms cumple la meta de < 500 ms. El P95 de 927 ms indica que en condiciones excepcionales (alta carga del dispositivo, cambio de tab, RAM limitada) la latencia puede superar el segundo. No se considera bloqueante para el caso de uso.

---

## KR3 — Tasa de falsas alertas en campo

**Fuente:** 187 alertas TTS emitidas en `detections_s.csv`, campo `alerta_correcta`. 23 alertas incorrectas.

| Sesión | Dispositivo | Alertas emitidas | Falsas alertas | Tasa |
|---|---|---:|---:|---:|
| urbana_diurna (Av. Miguel Grau) | Pixel 7 | 46 | 4 | **8.7%** |
| alta_luminosidad_reflejos | Samsung A54 | 49 | 9 | **18.4%** |
| atardecer | iPhone 14 | 44 | 4 | **9.1%** |
| nocturna | Xiaomi 13T | 48 | 6 | **12.5%** |
| **Global** | — | **187** | **23** | **12.3%** |

**Interpretación:** La meta de < 10% no se cumple. La condición de alta luminosidad y reflejos concentra la mayor proporción de errores (18.4%), frecuente en Lima durante horas de sol intenso. La condición diurna normal (8.7%) sí cumple la meta. El sistema requiere mejoras en robustez ante contraluz antes de operar sin supervisión activa.

---

## Decisiones técnicas tomadas durante el desarrollo

| Planeado en PC1 | Implementado en PC2 | Motivo |
|---|---|---|
| CNN MobileNetV2 con transfer learning (Build) | COCO-SSD pre-entrenado + brillo por tercios (Integrate) | YOLOv8-nano entrenado en LISA falló por desequilibrio de clases (rojo 25,876 vs amarillo 1,516 instancias) |
| Pipeline local Python + pyttsx3 | App web Vercel + Web Speech API | Sin instalación; accesible desde cualquier dispositivo por link |
| Google Colab para entrenamiento | Sin reentrenamiento propio | Estrategia Integrate eliminó la necesidad de GPU y pipeline de entrenamiento |

---

*Framework PROMPT v1.0 — AD5018 UTEC | Resultados PC2*
