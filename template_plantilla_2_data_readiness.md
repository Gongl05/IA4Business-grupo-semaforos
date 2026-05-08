# Plantilla 2 — Data Readiness Checklist
## Framework PROMPT | Fase R — Recursos de Datos
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** _______________
**Tipo de IA del proyecto:** ML Tradicional — Clasificación supervisada (Visión Computacional / CNN)

---

> **Nota de contexto del proyecto:** El usuario objetivo es un **conductor con daltonismo**.
> Por restricciones legales (prohibición de uso de celular al volante), el sistema opera
> desde un dispositivo fijo montado en el parabrisas o tablero (dashcam o soporte vehicular).
> Esto determina el ángulo de captura, la distancia al semáforo y los datasets relevantes.
> **Hallazgo crítico de investigación:** No existe ningún dataset público de semáforos
> capturado en Perú o Lima. El corpus peruano más cercano (UNMSM / Callao) fue recolectado
> para investigación interna y nunca fue publicado abiertamente. Esto representa un
> bloqueante real que se documenta en la Sección 3.

---

## SECCIÓN 1 — Inventario de datos

### Para ML Tradicional — Inventario de datos históricos

| # | Dataset | Fuente | Formato | N° de registros aprox. | ¿Tiene etiquetas? |
|---|---|---|---|---|---|
| 1 | LISA Traffic Light Dataset | UC San Diego — público en Kaggle | Imágenes + video / CSV de anotaciones | ~43,000 frames | SÍ (Go / Warning / Stop + variantes direccionales) |
| 2 | Bosch Small Traffic Lights Dataset (BSTLD) | Bosch — público en Zenodo | Imágenes HDR convertidas a RGB / YAML de anotaciones | ~13,427 imágenes (~24,000 instancias) | SÍ (Red / Yellow / Green / Off) |
| 3 | Brazilian Vertical Traffic Signs and Lights Dataset | Univ. Federal de Uberlândia, Brasil — público en Mendeley Data | Imágenes de alta resolución / XML (PASCAL VOC) | Múltiples frames curados, 16 clases anotadas | SÍ (Red light / Yellow light / Green light) |

**Justificación de la selección:** LISA y Bosch son los estándares académicos globales para
esta tarea. El dataset brasileño fue seleccionado como proxy latinoamericano porque comparte
condiciones visuales más cercanas al entorno limeño: infraestructura vial heterogénea,
exposición solar extrema, oclusión por transporte de gran formato y coexistencia de
semáforos LED modernos con equipos incandescentes obsoletos.

---

**Variable objetivo (target):**

Estado actual del semáforo capturado por la cámara del vehículo, clasificado en una de las
siguientes categorías: **rojo / amarillo / verde / semáforo no detectado**. La clasificación
debe realizarse en tiempo real (inferencia por frame) con latencia menor a 200ms para ser
accionable durante la conducción.

**Tipo de problema confirmado:**

- [x] Clasificación — predice una categoría (rojo / amarillo / verde / no detectado)
- [ ] Regresión — predice un número continuo
- [ ] Clustering — agrupa sin etiquetas previas

---

## SECCIÓN 2 — Evaluación de calidad con semáforo

### Dataset / Fuente principal: LISA Traffic Light Dataset (UC San Diego)

| Dimensión | Semáforo | Evidencia que respalda la evaluación | Plan de acción (si es 🟡 o 🔴) |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Dataset público disponible en Kaggle. Descarga directa sin restricción de acceso. | No aplica |
| **Volumen** | 🟢 | ~43,000 frames etiquetados, incluyendo 24,988 diurnos y 18,028 nocturnos. Volumen suficiente para entrenar una CNN de clasificación de 4 clases. | No aplica |
| **Calidad** | 🟡 | Anotaciones completas y consistentes. Sin embargo, fue capturado en San Diego y Cincinnati (EE.UU.). No representa la infraestructura semafórica limeña ni las condiciones de la "panza de burro" (nubosidad costera difusa que aplana el contraste cromático). | Complementar con dataset brasileño y con imágenes propias del entorno local para ajuste fino. |
| **Relevancia** | 🟡 | Perspectiva vehicular frontal coherente con el caso de uso. El contexto urbano difiere del latinoamericano: sin transporte informal de gran formato, sin contadores numéricos prominentes que el modelo pueda confundir como semáforos adicionales. | Usar como base de preentrenamiento. Validar el modelo con imágenes locales antes de declararlo listo. |
| **Legalidad** | 🟢 | Licencia CC BY-NC-SA 4.0. Uso académico y de investigación sin restricciones adicionales. | No aplica |

---

### Dataset / Fuente secundaria: Bosch Small Traffic Lights Dataset (BSTLD)

| Dimensión | Semáforo | Evidencia | Plan de acción |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Disponible públicamente en Zenodo. Acceso confirmado para uso académico. | No aplica |
| **Volumen** | 🟢 | ~13,427 imágenes con ~24,000 instancias anotadas. Mediana de ancho de semáforo: 8.6 píxeles — ideal para entrenar detección de objetos pequeños a distancia. | No aplica |
| **Calidad** | 🟡 | Imágenes capturadas en HDR y convertidas a RGB, lo que fuerza al modelo a aprender patrones morfológicos en lugar de sobreajustarse a valores de color absolutos. Capturado en California — contexto visual diferente al local. | Usar principalmente para mejorar robustez ante variaciones lumínicas (contraluz, lluvia, noche). No usar como fuente representativa de infraestructura latinoamericana. |
| **Relevancia** | 🟡 | Perspectiva vehicular consistente con el caso de uso. Diseño de semáforos estadounidense, no latinoamericano. | Usar como dataset de augmentación junto con LISA. |
| **Legalidad** | 🟢 | Licencia de uso no comercial. Permitido para proyectos académicos. | No aplica |

---

### Dataset / Fuente terciaria: Brazilian Vertical Traffic Signs and Lights Dataset (UFU)

| Dimensión | Semáforo | Evidencia | Plan de acción |
|---|---|---|---|
| **Disponibilidad** | 🟢 | Disponible públicamente en Mendeley Data. Accesible también a través de Roboflow. | No aplica |
| **Volumen** | 🟡 | Volumen más acotado que LISA o Bosch. Cubre 16 clases de señalética vial, con semáforos como subconjunto. | Combinar con LISA y Bosch para aumentar volumen total. Priorizar para el ajuste fino final del modelo. |
| **Calidad** | 🟢 | Anotaciones en formato XML (PASCAL VOC). Validado en literatura académica: SSD + MobileNet supera 80% mAP sobre este dataset. | No aplica |
| **Relevancia** | 🟢 | Es el dataset latinoamericano con mayor transferibilidad al contexto limeño: infraestructura heterogénea, exposición solar extrema, coexistencia de equipos modernos y obsoletos, oclusión por vehículos de gran formato. | No aplica |
| **Legalidad** | 🟢 | Uso académico y de investigación. Publicado bajo acceso abierto en Mendeley Data. | No aplica |

---

## SECCIÓN 3 — Plan de resolución de bloqueantes

### Bloqueante 1 — CRÍTICO 🔴

```
Dimensión afectada:
Relevancia (los tres datasets disponibles)

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

Acción concreta:
Capturar entre 500 y 1,000 imágenes propias desde un vehículo en
intersecciones urbanas de la ciudad objetivo, en distintas condiciones:
mañana nublada, mediodía con sol cenital, noche urbana. Etiquetar con
LabelImg (offline, gratuito) o Roboflow (online, capa gratuita). Usar
estas imágenes exclusivamente para ajuste fino (fine-tuning) sobre el
modelo preentrenado con LISA + Bosch + dataset brasileño.

Responsable dentro del equipo:
Gonzalo Gaviño (coordinación de captura en campo)
Equipo completo (etiquetado manual)

Fecha límite de resolución:
Definir según cronograma del curso — completar antes de la Fase M

¿Qué pasa si no se resuelve? (Plan B):
Aplicar Data Augmentation agresiva sobre el dataset brasileño:
reducción artificial de contraste (simula "panza de burro"), oclusión
aleatoria de zonas del semáforo (simula bloqueo por vehículos), y
variación de brillo extremo (simula contraluz y noche). Solución menos
robusta pero funcional para un prototipo académico.
```

---

## SECCIÓN 4 — Privacidad y legalidad de los datos

| Pregunta | Respuesta | Detalle |
|---|---|---|
| ¿Los datos contienen información personal de usuarios? | SÍ | Las imágenes de tráfico pueden contener rostros de personas y placas vehiculares. Los datasets públicos (LISA, Bosch, brasileño) fueron anonimizados por sus instituciones. Las imágenes propias a capturar requieren tratamiento previo. |
| ¿Se cuenta con consentimiento explícito para usar esos datos? | N/A | Los datasets públicos fueron recolectados bajo las políticas de sus instituciones. Las imágenes propias en vía pública no requieren consentimiento individual en espacio público, pero deben ser anonimizadas antes del entrenamiento. |
| ¿Los datos serán anonimizados antes de usarlos en el proyecto? | SÍ | Las imágenes propias capturadas en campo tendrán rostros y placas difuminados (blur) antes de incorporarlas al dataset de entrenamiento. |
| ¿Aplica la Ley N° 29733 de Protección de Datos Personales del Perú? | SÍ | Si se capturan imágenes en vía pública con personas identificables, aplica la normativa peruana. Se mitigará mediante anonimización previa al entrenamiento. |
| ¿Hay alguna restricción contractual o de confidencialidad? | NO | Los tres datasets públicos tienen licencias abiertas para uso académico. No hay datos corporativos ni confidenciales involucrados. |

---

## SECCIÓN 5 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Cada semáforo tiene evidencia concreta que lo respalda? | SÍ |
| ¿Todos los 🔴 tienen un plan de acción con fecha y responsable? | SÍ — El bloqueante crítico de datos locales tiene plan definido con Plan B incluido. |
| ¿El equipo verificó el acceso real a los datos antes de completar este checklist? | SÍ — LISA (Kaggle), Bosch (Zenodo) y dataset brasileño (Mendeley Data) tienen acceso público confirmado. |
| ¿La estrategia de contexto o tipo de ML es coherente con los datos disponibles? | SÍ — Clasificación supervisada con CNN, usando preentrenamiento global + fine-tuning con datos locales, es coherente con los recursos disponibles. |

> **Si alguna respuesta es NO → el checklist no está listo para entregar.**

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 2 de 4*