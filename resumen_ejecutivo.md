# Resumen Ejecutivo — SafeLight

> **Curso:** AD5018 — Inteligencia Artificial para Negocios · UTEC
> **Equipo:** Sebastian Sanchez · Gonzalo Gaviño · Giuseppe Del Negro
> **Entregable:** PC1 — Semana 7 — Ciclo 2026-1

---

## 1. Identidad del MVP

**Nombre:** SafeLight

**Usuario principal:** Conductor adulto con daltonismo rojo-verde (deuteranopía o protanopía) que opera un vehículo privado en entornos urbanos de Lima Metropolitana y requiere asistencia autónoma para interpretar el estado de semáforos convencionales en tiempo real durante la conducción. **Mercado potencial estimado: ~200,000 conductores** en Lima Metropolitana.

---

## 2. Declaración del problema

> *"Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias, colisiones y pérdida de autonomía de conducción segura para los aproximadamente 200,000 conductores afectados en Lima Metropolitana."*

---

## 3. Tipo de IA y justificación

**Tipo:** ML Tradicional — Clasificación Supervisada (Visión Computacional / CNN)

El problema requiere clasificar el estado del semáforo desde frames de video en tiempo real: tarea nativa de una CNN entrenada de forma supervisada. La alerta de voz se genera con `pyttsx3`, una biblioteca TTS determinista que convierte una de cuatro etiquetas fijas en audio — no es un componente de IA Generativa. Un LLM añadiría latencia, variabilidad y costo innecesarios cuando la salida siempre es una de cuatro frases predefinidas; la IA Generativa no aplica porque el problema no involucra lenguaje natural abierto ni generación de contenido.

---

## 4. Diagnóstico de datos

| Fuente | Tipo | Volumen | Acceso verificado | Semáforo |
|---|---|---|---|---|
| LISA Traffic Light Dataset (UCSD) | Público académico | ~43,000 frames anotados | ✅ Kaggle — acceso directo confirmado | 🟢 |
| Bosch Small Traffic Lights (BSTLD) | Público académico | ~13,427 imágenes / ~24,000 instancias | ✅ Zenodo — acceso académico confirmado | 🟢 |
| Brazilian Traffic Lights UFU | Público académico | Múltiples frames, 16 clases anotadas | ✅ Mendeley Data — acceso confirmado | 🟢 |
| Dataset propio — Lima | Captura del equipo | Meta: 500–1,000 imágenes | ⏳ Plan Semanas 7–9. Responsable: Gonzalo Gaviño | 🟡 |

**Diagnóstico general del equipo: 🟡** — tres fuentes públicas en verde con acceso verificado; bloqueante identificado en datos locales con plan documentado y plan B (data augmentation sobre dataset brasileño si la captura no es viable). Todos los datasets públicos tienen licencias abiertas para uso académico. Las imágenes propias se anonimizarán (blur de rostros y placas) antes del entrenamiento en cumplimiento de la Ley N° 29733.

---

## 5. Descripción del producto y flujo del MVP

**Qué hace:** el conductor monta el dispositivo en soporte fijo del vehículo y lo activa con un único toque. La cámara captura frames de video de forma continua y automática. Un modelo CNN procesa cada frame, clasifica el estado del semáforo (rojo / amarillo / verde / no detectado) y, cuando la predicción supera el umbral de confianza configurado, emite una alerta de voz breve en español. Si la confianza es insuficiente, el sistema permanece en silencio — nunca emite una alerta falsa.

**Flujo:**
```
Activación (1 toque) → Cámara captura frames →
CNN clasifica estado del semáforo →
¿Confianza ≥ umbral? → SÍ: pyttsx3 emite alerta de voz
                      → NO: silencio (fail-safe por omisión)
```

**Alcance del MVP (Semana 14):** modelo CNN entrenado con los 4 datasets · pipeline de inferencia en tiempo real (<200ms/frame) · módulo TTS integrado · umbral de confianza configurable · prototipo funcional demostrable en entorno controlado.

**Fuera del MVP:** app publicada en tienda, integración GPS, detección de múltiples semáforos simultáneos.

**Criterio de éxito:** *"Un usuario externo al equipo puede montar el dispositivo, activarlo con un único gesto y recibir alertas de voz correctas del estado del semáforo en al menos 8 de cada 10 pruebas, en condiciones normales de iluminación diurna, sin instrucciones adicionales del equipo."*

---

## 6. Decisión tecnológica — Build

**Estrategia: Build.** Transfer learning sobre MobileNetV2 preentrenada (LISA + BSTLD + Brazilian UFU) con fine-tuning sobre imágenes propias de Lima. No existe API comercial calibrada para infraestructura semafórica latinoamericana con latencia <200ms; Buy no aplica. El módulo TTS (`pyttsx3`) es una biblioteca open-source offline, no un componente de IA que justifique una estrategia Integrate separada.

| Componente | Herramienta | Costo estimado |
|---|---|---|
| Modelo de clasificación | TensorFlow / PyTorch — MobileNetV2 | 🟢 S/. 0 (open source) |
| Entrenamiento | Google Colab (GPU T4) | 🟢 S/. 0 (capa gratuita) |
| Inferencia en tiempo real | OpenCV + TensorFlow Lite | 🟢 S/. 0 (open source) |
| Síntesis de voz | pyttsx3 (offline) | 🟢 S/. 0 (open source) |
| Etiquetado de imágenes propias | LabelImg / Roboflow Free | 🟢 S/. 0 (capa gratuita) |
| GPU adicional si se requiere | Google Colab Pro (1 mes) | 🟡 ~S/. 37 |

**Costo total estimado del MVP: S/. 0 – S/. 37.** Supuesto clave: entrenamiento es un proceso único no recurrente; el MVP opera en hardware local sin servidor cloud.

---

## 7. OKRs y KPI técnico

**Objetivo:** Permitir que conductores con daltonismo identifiquen el estado de semáforos urbanos de forma autónoma y en tiempo real, eliminando su dependencia de estrategias visuales compensatorias durante la conducción en Lima.

| # | Métrica | Valor actual | Meta MVP | Medición |
|---|---|---|---|---|
| **KR1** | F1-score del modelo CNN (promedio ponderado, 4 clases) | 0% — no existe sistema previo | ≥ 80% sobre conjunto de test | Semana 12 — reporte automático del framework |
| **KR2** | Latencia total del sistema (captura → alerta de voz) | N/A — sistema no existe | < 500ms por evento de detección | Semana 11 — timestamps en el pipeline Python |
| **KR3** | Tasa de alertas falsas en sesión de prueba de campo (30 min) | N/A | < 10% de eventos | Semana 13 — log de voz vs. video grabado simultáneamente |

**KPI técnico del modelo:**

| Métrica | Criterio mínimo aceptable |
|---|---|
| F1-score por clase (rojo / amarillo / verde / no detectado) | ≥ 80% en **todas** las clases individualmente |
| Latencia de inferencia CNN por frame | < 200ms en hardware del prototipo |
| Tasa de falsos positivos en prueba real | < 10% |

---

*Resumen Ejecutivo — ChromaVía · AD5018 UTEC · Ciclo 2026-1*
