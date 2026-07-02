# Plantilla 4 — AI Impact Scorecard + AI Risk Matrix
## Framework PROMPT | Fases P2 + T — Performance y Transparencia
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** 30 de junio de 2026
**Nombre del MVP:** SafeLight

---

> **Instrucción general:**
> Esta plantilla se completa en dos momentos distintos:
> - **Semanas 9-10:** Define las métricas y el plan de evaluación (Secciones 1, 2 y 4)
> - **Semanas 12-13:** Completa con datos reales obtenidos de las pruebas (Sección 3)
> No inventes datos. Un scorecard honesto con resultados bajos y buena reflexión vale más que uno inflado sin evidencia.

---

# PARTE A — AI IMPACT SCORECARD
## Fase P2 — Performance y Métricas

---

## SECCIÓN 1 — Evaluación de OKRs declarados en el AI Product Canvas

> **Instrucción:**
> Esta sección NO define métricas nuevas — evalúa los compromisos que el equipo declaró en la Sección 8 del AI Product Canvas (Plantilla 3).
> Copia el Objetivo y cada KR exactamente como los escribiste allá, y completa los resultados reales obtenidos.
> Un equipo que cambia sus KRs después de construir está haciendo trampa — los compromisos son inamovibles desde la Semana 6.

---

### Objetivo (O) del proyecto
> *Copia exactamente el Objetivo declarado en la Plantilla 3, Sección 8.*

```
O: Permitir que conductores con daltonismo identifiquen el estado de semáforos
   urbanos de forma autónoma y en tiempo real, eliminando su dependencia de
   estrategias visuales compensatorias durante la conducción en Lima.

```

---

### Evaluación de Key Results

**KR1:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | F1-score macro del sistema en evaluación de campo (promedio de las 3 clases: rojo / amarillo / verde) | F1-score macro medido sobre 488 detecciones anotadas en Supabase |
| Valor actual (antes del MVP) | 0% — no existe ningún sistema automatizado para este caso de uso en Lima | — *(no cambia)* |
| Meta comprometida | F1-score macro ≥ 80% sobre detecciones reales anotadas en sesión de prueba en campo | — *(no cambia)* |
| Resultado real | — | F1 macro: **83.9%** (rojo: 86.6% / verde: 85.1% / amarillo: 80.0%) |
| Método usado para medir | Anotación manual posterior de predicted_state vs real_state en datos recopilados en Supabase; cálculo de F1 por clase y macro | Anotación manual de campo real_state y alerta_correcta en Supabase; cálculo de F1 por clase sobre 488 detecciones en 4 sesiones formales |
| **¿Se alcanzó la meta?** | — | **SÍ** |

**Análisis del KR1:**
```
El sistema superó la meta de F1 macro ≥ 80% con un resultado de 83.9%.
El color rojo obtuvo el mejor desempeño (86.6%), seguido de verde (85.1%).
El amarillo fue el más desafiante (80.0%), apenas en el límite de la meta,
lo que es esperable dado que el brillo del tercio medio del semáforo en
luz amarilla es más difícil de distinguir del rojo o verde en condiciones
de alta luminosidad. El factor determinante fue la combinación de COCO-SSD
(confianza promedio 0.81) con el filtro de 6 frames consecutivos, que
eliminó detecciones inestables y elevó la precisión general.
```

---

**KR2:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | Latencia total del sistema: tiempo desde captura del frame (T0) hasta inicio de la alerta de voz (T5) | Latencia medida automáticamente (campo latency_ms) en cada detección almacenada en Supabase |
| Valor actual (antes del MVP) | N/A — no existe sistema de referencia | — *(no cambia)* |
| Meta comprometida | Latencia total mediana < 500ms por evento de detección en hardware del prototipo | — *(no cambia)* |
| Resultado real | — | Mediana: **470ms** / Promedio: 518ms / P95: 927ms |
| Método usado para medir | Medición automática con timestamps en el pipeline (latency_ms) almacenados en Supabase durante sesión de prueba | Timestamps automáticos en pipeline JavaScript: T0 (captura frame) → T5 (inicio audio TTS). Desglose: detección COCO-SSD avg 138ms, análisis color avg 19ms, solicitud TTS avg 195ms |
| **¿Se alcanzó la meta?** | — | **SÍ (marginalmente)** |

**Análisis del KR2:**
```
La mediana de 470ms cumple la meta de <500ms, pero de forma ajustada.
El promedio de 518ms supera la meta, y el percentil P95 de 927ms revela
que en eventos extremos la latencia supera ampliamente el objetivo.
El factor determinante fue la variabilidad inherente a la Web Speech API
(avg 195ms) que representa el componente de mayor latencia del sistema.
La inferencia COCO-SSD (avg 138ms) y el análisis de color (avg 19ms)
son rápidos y consistentes. Se considera KR2 CUMPLIDO al evaluar por
mediana según lo comprometido, aunque la latencia en casos extremos
es un riesgo documentado.
```

---

**KR3 *(si fue declarado)*:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | Tasa de alertas falsas (eventos donde el sistema emite alerta incorrecta o sin semáforo visible) | Tasa medida sobre 187 alertas TTS emitidas, campo alerta_correcta anotado manualmente |
| Valor actual (antes del MVP) | N/A | — |
| Meta comprometida | Tasa de alertas falsas < 10% en sesión de prueba en entorno urbano real | — |
| Resultado real | — | **12.3% global** (diurna: 8.7% / alta luminosidad: 18.4% / atardecer: 9.1% / nocturna: 12.5%) |
| Método usado para medir | Revisión manual del campo alerta_correcta en Supabase, contrastado con real_state anotado en cada sesión de prueba | Anotación manual posterior de alerta_correcta para cada una de las 187 alertas TTS emitidas en 4 sesiones formales |
| **¿Se alcanzó la meta?** | — | **NO** |

---

### Evaluación del KPI técnico

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica técnica | F1-score macro en evaluación de campo + tasa de alertas falsas en prueba real | F1 macro: 83.9% / Tasa alertas falsas global: 12.3% |
| Criterio mínimo aceptable | F1-score macro ≥ 80% en las 3 clases Y tasa de alertas falsas < 10% en prueba real (ambas condiciones simultáneas) | — *(no cambia)* |
| Resultado real | — | F1 macro 83.9% ✓ (cumple) / Tasa falsas 12.3% ✗ (no cumple) |
| Método de medición | Cálculo sobre datos de Supabase: F1 desde predicted_state vs real_state; tasa de falsas desde campo alerta_correcta | 488 detecciones anotadas / 187 alertas TTS evaluadas en Supabase |
| **¿Es aceptable?** | — | **NO** (KR3 no cumple — condición conjunta falla) |

---

### Resumen ejecutivo del OKR

| | KR1 | KR2 | KR3 | KPI técnico |
|---|---|---|---|---|
| **¿Alcanzado?** | SÍ (83.9% ≥ 80%) | SÍ / PARCIAL (mediana 470ms ✓ / P95 927ms ✗) | NO (12.3% > 10%) | NO (KR3 falla condición conjunta) |

**Conclusión del equipo:**
> *¿El MVP cumplió su objetivo de negocio? Argumenta con los datos anteriores.*

```
El MVP cumplió parcialmente su objetivo de negocio. El sistema demostró capacidad
real de detección y clasificación de semáforos en entorno urbano limeño con F1 macro
de 83.9%, superando la meta de KR1, y con latencia mediana de 470ms que cumple
marginalmente KR2. Sin embargo, la tasa de alertas falsas de 12.3% supera el umbral
comprometido del 10%, impidiendo declarar el KPI técnico como cumplido.

El análisis por sesión revela que el sistema es confiable en condiciones diurnas
normales (8.7% tasa falsa) y al atardecer (9.1%), pero degrada significativamente
ante alta luminosidad y reflejos (18.4%), condición frecuente en Lima. El MVP
es funcional como prototipo académico y prueba de concepto viable, pero requiere
mejoras en robustez ante condiciones lumínicas extremas antes de ser desplegado
para uso real de conductores con daltonismo.
```

---

## SECCIÓN 2 — Evaluación técnica del modelo

> **Instrucción:**
> El criterio mínimo aceptable fue declarado en la Plantilla 3, Sección 8 (KPI técnico).
> Aquí solo se reportan los resultados reales y se interpretan en contexto de negocio.
> Completa solo las métricas aplicables al tipo de IA del proyecto.

### Para IA Generativa

> *No aplica — SafeLight usa ML Tradicional (COCO-SSD pre-entrenado + análisis de color
> por brillo). No se emplea ningún modelo de lenguaje generativo.*

---

### Para ML Tradicional

| Métrica | Criterio mínimo aceptable | Resultado real | ¿Aceptable? |
|---|---|---|---|
| **F1-score macro** | ≥ 80% | 83.9% | SÍ |
| **F1-score Rojo** | ≥ 80% | 86.6% | SÍ |
| **F1-score Verde** | ≥ 80% | 85.1% | SÍ |
| **F1-score Amarillo** | ≥ 80% | 80.0% | SÍ (en el límite) |
| **Tasa de alertas falsas** | < 10% | 12.3% global | NO |
| **Confianza promedio del detector** | — | 0.81 | — |
| **Baseline** *(estrategia aleatoria entre 3 clases)* | 33.3% F1 | — | — |

**Interpretación del equipo:**
> *¿Qué significan estos números en términos de negocio? ¿El modelo es confiable para el problema definido?*

```
El sistema supera ampliamente el baseline (33.3% F1 aleatoria) con un F1 macro de
83.9%, lo que demuestra que la arquitectura COCO-SSD + análisis de brillo por tercios
es técnicamente viable para el problema. En términos de negocio, el sistema puede
alertar correctamente al conductor en aproximadamente 8 de cada 10 eventos de detección
en condiciones normales, lo que reduce significativamente el estrés cognitivo del
conductor con daltonismo.

Sin embargo, el 12.3% de falsas alertas implica que aproximadamente 1 de cada 8 alertas
emitidas es incorrecta. En un contexto de seguridad vial, este nivel puede generar
desconfianza progresiva en el usuario. El riesgo es diferenciado: la sesión de alta
luminosidad (18.4%) es la más preocupante porque Lima tiene alta incidencia de sol
directo durante el día. El sistema es confiable como asistencia complementaria, no
como sustituto único de la percepción del conductor.
```

---

## SECCIÓN 3 — Evaluación con usuarios reales

> *El MVP debe ser probado por al menos 3 personas externas al equipo antes de la sustentación.*

### Registro de pruebas de usuario

| # | Perfil del usuario | Tarea asignada | ¿Completó la tarea? | Observaciones clave |
|---|---|---|---|---|
| 1 | Conductor adulto, condición diurna normal | Activar el sistema y circular por 2 intersecciones semaforizadas en Lima | SÍ | Sistema emitió alertas correctas en ambas intersecciones; latencia percibida como aceptable |
| 2 | Conductor adulto, condición de alta luminosidad / reflejos | Activar el sistema en hora pico con sol directo sobre el parabrisas | PARCIAL | Sistema tuvo 2 falsas alertas (amarillo detectado como rojo) en condición de contraluz extremo |
| 3 | Conductor adulto, condición nocturna | Activar el sistema en recorrido nocturno urbano | SÍ | Sistema detectó semáforos LED correctamente; usuarios reportaron confianza en las alertas emitidas |

### Hallazgos principales de las pruebas

**Lo que funcionó bien:**
```
1. Activación del sistema con un único toque — flujo intuitivo sin instrucciones adicionales.
2. Alertas de voz claras y en español ("Semáforo en ROJO / VERDE / AMARILLO") — comprensibles
   sin entrenamiento previo.
3. Silencio cuando el semáforo no es visible — los usuarios valoraron que el sistema
   no los "bombardeara" con alertas innecesarias.
```

**Lo que no funcionó o confundió al usuario:**
```
1. Falsas alertas en condición de sol directo sobre el parabrisas — usuarios reportaron
   confusión cuando la alerta no correspondía al semáforo visible.
2. Latencia en eventos extremos (hasta ~900ms) percibida como "lenta" por uno de los usuarios
   en una intersección con ciclo semafórico corto.
3. El sistema no distingue semáforos peatonales de vehiculares cuando ambos son visibles
   en el frame — puede emitir alerta del semáforo equivocado.
```

**Cambios realizados al MVP como resultado de las pruebas:**
```
1. Ajuste del umbral de tamaño mínimo de bounding box (altura ≥ 3.5% del frame) para
   reducir detecciones de semáforos lejanos o pequeños que aumentaban falsas alertas.
2. Incremento del filtro de estabilidad a 6 frames consecutivos (vs 3 frames anterior)
   para reducir parpadeo rojo↔amarillo en condiciones de alta luminosidad.
```

---

## SECCIÓN 4 — Reflexión de resultados

> *Completa esta sección después de tener todos los datos reales. Es la sección más importante para la sustentación.*

### ¿El MVP resuelve el problema definido en la Fase P?

- [ ] **SÍ completamente** — el MVP resuelve el problema y las métricas lo demuestran
- [x] **SÍ parcialmente** — el MVP resuelve una parte del problema con limitaciones claras
- [ ] **NO todavía** — el MVP tiene el enfoque correcto pero necesita más iteraciones

**Argumento del equipo (obligatorio):**
```
El MVP demuestra que la arquitectura COCO-SSD + análisis de brillo por tercios es
técnicamente viable: F1 macro de 83.9% y latencia mediana de 470ms son resultados
reales que superan los compromisos de KR1 y KR2. En condiciones diurnas normales
(tasa falsa 8.7%) y al atardecer (9.1%), el sistema cumple todos los criterios.

Sin embargo, la tasa de alertas falsas global de 12.3% (especialmente 18.4% en alta
luminosidad) impide declarar el problema completamente resuelto. El problema del
conductor con daltonismo en Lima incluye condiciones de sol extremo que son frecuentes
e impredecibles. Un sistema de asistencia vial con 1 de cada 8 alertas incorrectas
puede generar desconfianza progresiva, reduciendo su adopción real. El MVP es un
prototipo funcional y académicamente válido, pero requiere mejoras específicas en
robustez ante contraluz para ser desplegado como producto de uso cotidiano.
```

### Lecciones aprendidas por fase PROMPT

| Fase | ¿Qué cambiarías si empezaras de nuevo? |
|---|---|
| **P** — Problema | Delimitar el caso de uso a condiciones lumínicas específicas desde el inicio (no prometer solución universal para Lima sin evidencia del desempeño en contraluz). |
| **R** — Datos | Evaluar el desequilibrio de clases de LISA antes de invertir tiempo en entrenamiento de YOLOv8. El desequilibrio rojo:25,876 vs amarillo:1,516 era un red flag evidente que habría dirigido la decisión hacia COCO-SSD pre-entrenado desde el inicio. |
| **O** — Diseño | Incluir en el canvas la posibilidad de estrategia Integrate desde la semana 6, en lugar de comprometerse con Build sin haber evaluado el modelo pre-entrenado disponible. COCO-SSD demostró que no siempre es necesario entrenar desde cero. |
| **M** — Construcción | Implementar la recopilación de datos en Supabase desde el primer día de pruebas, no como añadido posterior. La disponibilidad de 488 detecciones anotadas fue crítica para la evaluación objetiva de KRs. |
| **P2** — Métricas | Definir desde el inicio si la meta del KR3 (tasa falsa < 10%) se evalúa por sesión o en promedio global. La varianza entre sesiones (8.7% a 18.4%) habría requerido una meta diferenciada por condición lumínica. |
| **T** — Ética | Involucrar a usuarios con daltonismo real en las pruebas de usuario, no solo conductores sin discapacidad visual. El problema está definido para ese usuario específico y la evaluación debería reflejarlo. |

### Próximos pasos si el proyecto continuara

```
1. (corto plazo — próximas 2 semanas)
   Implementar corrección de color adaptativa (CLAHE o ecualización de histograma)
   sobre el recorte del bounding box antes del análisis de brillo, para reducir el
   impacto del contraluz. Meta: bajar tasa falsa en sesión de alta luminosidad de
   18.4% a < 12%.

2. (mediano plazo — próximos 2 meses)
   Capturar dataset propio de semáforos en Lima (500-1,000 imágenes etiquetadas)
   para fine-tuning específico del modelo de clasificación de color. Esto permitiría
   superar la dependencia del análisis de brillo por tercios y mejorar el F1 del
   amarillo (actualmente en el límite: 80.0%).

3. (largo plazo — versión 2.0 del producto)
   Desarrollar app nativa iOS/Android con WebRTC optimizado para reducir latencia P95
   (actualmente 927ms). Integrar GPS para pre-alertas de proximidad a intersecciones
   y modo de detección selectiva solo cuando el vehículo se aproxima a un semáforo.
   Publicar en tiendas con validación de usuarios reales con daltonismo en Lima.
```

---

---

# PARTE B — AI RISK MATRIX
## Fase T — Transparencia y Ética

---

## SECCIÓN 5 — Identificación de riesgos

> *El equipo debe identificar mínimo 3 riesgos: al menos 1 técnico, 1 ético y 1 legal o de privacidad.*
> *Nivel de riesgo: Alto (puede causar daño significativo al usuario o al negocio), Medio (impacto manejable con acción), Bajo (impacto menor o muy improbable).*

### Riesgo 1

| Campo | Detalle |
|---|---|
| **Tipo** | Técnico |
| **Nombre del riesgo** | Falsas alertas en condiciones de alta luminosidad y reflejos |
| **Descripción** | El sistema clasifica incorrectamente el color del semáforo cuando hay sol directo sobre el parabrisas, reflejos en el vidrio o contraluz extremo. El brillo del recorte del bounding box es distorsionado por la luz ambiente, causando que el tercio "incorrecto" aparezca más brillante. Tasa real medida: 18.4% en sesión de alta luminosidad. |
| **Nivel** | Alto |
| **Probabilidad** | Alta — Lima tiene alta incidencia de sol directo en horario diurno |
| **Impacto si ocurre** | El conductor con daltonismo recibe una alerta incorrecta (ej. "verde" cuando el semáforo está en rojo) y podría cruzar en rojo basándose en la alerta falsa, generando riesgo de colisión o infracción. |
| **Mitigación** | Filtro de 6 frames consecutivos reduce el impacto de detecciones aisladas. Umbral de confianza de 0.35 filtra detecciones poco confiables. Filtro de tamaño mínimo (altura ≥ 3.5%) reduce semáforos mal iluminados lejanos. Próximo paso: corrección adaptativa de contraste (CLAHE). |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE — la tasa bajó de niveles sin filtro pero 18.4% en alta luminosidad aún supera la meta del 10% |

---

### Riesgo 2

| Campo | Detalle |
|---|---|
| **Tipo** | Ético |
| **Nombre del riesgo** | Dependencia excesiva del usuario en el sistema con conciencia insuficiente de sus limitaciones |
| **Descripción** | Un conductor con daltonismo podría adoptar SafeLight como fuente única de información sobre semáforos, delegando completamente la decisión al sistema. Si el usuario no es consciente de las limitaciones (condiciones de fallo documentadas), una falla inesperada del sistema en situación crítica podría tener consecuencias graves. |
| **Nivel** | Alto |
| **Probabilidad** | Media — depende del diseño de comunicación al usuario |
| **Impacto si ocurre** | Accidente de tránsito o infracción por confianza ciega en una alerta incorrecta o ausencia de alerta. Responsabilidad moral del equipo/producto. |
| **Mitigación** | La interfaz muestra visualmente el estado detectado en tiempo real (retroalimentación visual + auditiva). El sistema nunca suprime la vista de cámara — el usuario mantiene acceso visual a la escena. Documentar claramente en la app que SafeLight es asistencia complementaria, no sustituto de la atención del conductor. |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE — la retroalimentación visual existe pero no hay aviso explícito de limitaciones en la interfaz actual |

---

### Riesgo 3

| Campo | Detalle |
|---|---|
| **Tipo** | Legal / Privacidad |
| **Nombre del riesgo** | Recopilación de imágenes en vía pública con personas identificables |
| **Descripción** | El sistema almacena en Supabase frames JPEG (200×120px) de cada detección. Estos frames pueden contener imágenes de personas, peatones o placas vehiculares captadas en vía pública. Bajo la Ley N° 29733 de Protección de Datos Personales del Perú, las imágenes de personas identificables constituyen datos personales. |
| **Nivel** | Medio |
| **Probabilidad** | Alta — toda sesión de prueba en vía pública captura personas en el frame |
| **Impacto si ocurre** | Sanción por ANPD (Autoridad Nacional de Protección de Datos) por recopilación de datos personales sin consentimiento explícito o sin cumplir requisitos de anonimización. |
| **Mitigación** | Los frames almacenados son en resolución reducida (200×120px) que dificulta la identificación de individuos. El frame completo no se almacena. Para una versión de producción, implementar blur automático de rostros y placas antes del almacenamiento, o eliminar el almacenamiento de frames completamente. |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE — resolución reducida mitiga parcialmente pero no garantiza anonimización completa |

---

### Riesgo 4 *(opcional — recomendado)*

| Campo | Detalle |
|---|---|
| **Tipo** | Técnico |
| **Nombre del riesgo** | Latencia elevada en eventos extremos (P95: 927ms) en intersecciones con ciclo corto |
| **Descripción** | El percentil 95 de latencia es 927ms, casi el doble de la meta de 500ms. En intersecciones con ciclos semafóricos cortos (especialmente amarillo, que dura 3-5 segundos), una latencia de ~900ms puede implicar que la alerta llega cuando el semáforo ya cambió de estado, generando confusión o alerta tardía. |
| **Nivel** | Medio |
| **Probabilidad** | Baja — ocurre solo en el 5% de los eventos (P95) |
| **Impacto si ocurre** | Conductor recibe alerta de "amarillo" cuando el semáforo ya está en rojo, pudiendo continuar circulando indebidamente. |
| **Mitigación** | El filtro de 6 frames consecutivos ya reduce la probabilidad de alertas en transición de color. El intervalo mínimo de 4000ms entre alertas del mismo color evita alertas repetidas. La variabilidad de la Web Speech API (avg 195ms) es el componente principal de latencia alta — alternativas offline más deterministas reducirían el P95. |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE — filtros de estabilidad mitigan el impacto pero no eliminan el riesgo en eventos extremos |

---

## SECCIÓN 6 — Marco regulatorio

### Regulación aplicable al proyecto

> *Marca todas las que apliquen y completa el detalle.*

| Regulación | ¿Aplica? | ¿Cómo cumple el MVP con ella? |
|---|---|---|
| **Ley N° 29733** — Protección de Datos Personales (Perú) | SÍ | Los frames almacenados en Supabase son en resolución reducida (200×120px JPEG) para minimizar la identificabilidad de personas. El sistema no almacena datos personales del usuario (nombre, DNI, etc.). Para versión de producción: implementar blur de rostros y placas o eliminar almacenamiento de frames. |
| **D.S. 003-2013-JUS** — Reglamento de la Ley 29733 | SÍ | El MVP es un prototipo académico de uso interno del equipo. Las sesiones de prueba fueron realizadas por miembros del equipo en su propia condición de conductores voluntarios. Para uso con terceros: requerir consentimiento informado explícito. |
| **Regulación sectorial específica** *(salud, finanzas, educación)* | NO | El producto es un asistente de movilidad, no un dispositivo médico. No aplica regulación de salud (no diagnostica ni trata condiciones médicas). La discapacidad visual es el contexto de usuario, no el objeto de regulación médica. |
| **EU AI Act** *(referencial)* | SÍ (referencial) | Bajo el EU AI Act, un sistema de asistencia a la conducción que afecta la seguridad vial podría clasificarse como "alto riesgo". Como referencia: el MVP debería documentar sus limitaciones de desempeño, mantener logs de auditoría y comunicar claramente al usuario que es un sistema complementario, no autónomo. |

**¿El proyecto procesa datos personales de usuarios?**
- [ ] No — el proyecto no maneja datos personales
- [x] Sí — los datos están anonimizados antes de ser usados
- [ ] Sí — contamos con consentimiento explícito de los usuarios
- [ ] Sí — estamos en proceso de obtener autorización

---

## SECCIÓN 7 — Preguntas éticas clave

> *El equipo debe poder responder estas preguntas en la sustentación. Prepara la respuesta aquí.*

**¿El usuario sabe que está interactuando con IA?**
```
SÍ — La interfaz de SafeLight muestra visualmente el bounding box del semáforo
detectado y el color clasificado en tiempo real sobre el feed de cámara. El usuario
observa directamente la inferencia del modelo. Adicionalmente, la naturaleza de la
herramienta (app que "detecta" semáforos) hace implícita la presencia de IA.
Para mayor transparencia, la versión de producción debería incluir un aviso explícito
al primer uso: "SafeLight usa inteligencia artificial para detectar semáforos.
Es un asistente complementario, no un sustituto de tu atención al conducir."
```

**¿Qué pasa si la IA comete un error que afecta al usuario?**
```
Responsabilidad: Como proyecto académico, la responsabilidad recae en el equipo
desarrollador. Para un producto comercial, el fabricante debería asumir responsabilidad
civil dentro de los límites legales aplicables a sistemas de asistencia al conductor.

Mecanismo actual: El sistema registra cada detección en Supabase con predicted_state,
real_state (anotación posterior) y alerta_correcta, lo que permite auditoría completa
de errores. No existe aún un canal de reporte de errores para el usuario.

Salvaguarda técnica: El sistema falla silenciosamente (sin alerta) ante detecciones
inseguras — nunca fuerza una alerta si la confianza es insuficiente. Esto reduce el
riesgo de errores activos, aunque no los elimina completamente.
```

**¿El producto funciona igual de bien para todos los segmentos de usuarios?**
```
No completamente. El desempeño varía significativamente según la condición lumínica:
- Condición diurna normal: tasa falsa 8.7% (aceptable)
- Condición de alta luminosidad / reflejos: tasa falsa 18.4% (problemática)
- Condición nocturna: tasa falsa 12.5% (supera meta)

Esto implica que usuarios que conducen frecuentemente en condiciones de contraluz
extremo (común en Lima en horario de mediodía) tienen una experiencia significativamente
peor. El sistema no discrimina por perfil de usuario sino por condición ambiental,
pero dado que no todos los conductores enfrentan las mismas condiciones con la misma
frecuencia, existe una desigualdad de servicio según zona, horario y orientación vial.
```

**¿El usuario tiene control sobre sus datos y puede solicitar su eliminación?**
```
En el MVP actual: No existe un mecanismo de control de datos para el usuario final.
Los frames y metadatos de detección se almacenan automáticamente en Supabase sin
notificación explícita al usuario durante la sesión de prueba.

Para cumplimiento de Ley 29733: Una versión de producción debe implementar:
1. Aviso informativo al inicio de sesión sobre qué datos se recopilan y por qué.
2. Opción de deshabilitar el almacenamiento de frames (modo privado).
3. Canal de solicitud de eliminación de datos (email del equipo o formulario en la app).
4. Política de retención: los frames de evaluación deben eliminarse tras la evaluación.
```

---

## SECCIÓN 8 — Declaración de uso de IA en el proyecto

> *El curso alienta el uso de IA en el desarrollo. Documenta cómo la usaste.*

| Herramienta de IA usada | Para qué tarea del proyecto | Fase PROMPT en la que se usó |
|---|---|---|
| Claude (Anthropic) | Redacción y revisión de plantillas del proyecto; depuración de lógica del pipeline JavaScript; análisis de inconsistencias entre documentos | P, R, O, M, P2, T |
| ChatGPT (OpenAI) | Consultas de arquitectura técnica (COCO-SSD vs YOLOv8); revisión de código JavaScript para análisis de brillo por tercios; interpretación de métricas F1 | O, M, P2 |
| GitHub Copilot | Autocompletado de código JavaScript en el pipeline de detección y en la integración con Supabase | M |

---

## SECCIÓN 9 — Autoevaluación final del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Los KRs evaluados son exactamente los declarados en la Plantilla 3? | SÍ |
| ¿Los resultados reales tienen evidencia concreta que los respalda? | SÍ — 488 detecciones anotadas en Supabase, 187 alertas TTS evaluadas en 4 sesiones formales |
| ¿El equipo no cambió ningún KR después de la Semana 6? | SÍ |
| ¿Hay al menos 1 riesgo técnico, 1 ético y 1 legal identificados? | SÍ — Riesgo 1 (técnico), Riesgo 2 (ético), Riesgo 3 (legal/privacidad) |
| ¿Cada riesgo tiene una mitigación concreta y no genérica? | SÍ |
| ¿El equipo conoce la regulación aplicable a su sector? | SÍ — Ley 29733 y D.S. 003-2013-JUS (Perú); EU AI Act como referencia |
| ¿El MVP informa visiblemente al usuario que interactúa con IA? | SÍ (visualización de bounding box en tiempo real) — aviso explícito pendiente para versión producción |
| ¿Todos los integrantes pueden responder las preguntas éticas de la Sección 7? | SÍ |

> **Si alguna respuesta es NO → la plantilla no está lista para la sustentación.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 4 de 4*
