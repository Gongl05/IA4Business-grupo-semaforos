# Plantilla 3 — AI Product Canvas
## Framework PROMPT | Fase O — Oportunidad de IA
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** 30 de junio de 2026
**Versión del canvas:** v2

---

> **Instrucción general:**
> Este canvas se completa DESPUÉS de tener aprobados el Problem Statement Canvas y el Data
> Readiness Checklist. Si el problema o los datos cambian, este canvas debe actualizarse.
> Las secciones deben ser consistentes entre sí — si hay contradicciones internas, el canvas
> no está listo.

---

## SECCIÓN 1 — Identidad del producto

### 1.1 Nombre del producto / MVP

```
Nombre: SafeLight
```

### 1.2 Problema que resuelve

> *Copia exactamente la declaración del Problem Statement Canvas. No parafrasear.*

```
"Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias, colisiones y pérdida de autonomía de conducción segura para los aproximadamente 200,000 conductores afectados en Lima Metropolitana."
```

### 1.3 Usuario principal

> *¿Quién usa el producto directamente? Sé específico — no "las empresas" sino el rol exacto.*

```
Conductor adulto con daltonismo rojo-verde (deuteranopía o protanopía) que opera un vehículo privado en entornos urbanos de Lima Metropolitana y requiere asistencia autónoma para interpretar el estado de semáforos convencionales en tiempo real durante la conducción.
```

- [ ] IA Generativa
- [x] ML Tradicional — Clasificación
- [ ] ML Tradicional — Regresión
- [ ] ML Tradicional — Clustering
- [ ] Combinación

---

## SECCIÓN 2 — Tech & Cost Overview *(visión de temperatura)*

> **Instrucción:**
> Esta sección NO es la selección definitiva del stack tecnológico — esa decisión se toma
> en la **Fase M1** con criterios técnicos y financieros más detallados.
> El objetivo aquí es tener una primera lectura de qué categorías de herramientas podrían
> aplicar y cuánto podría costar el MVP a nivel general.
> Sé honesto sobre lo que no sabes aún — una estimación consciente vale más que un número
> inventado.

---

### 2.1 Categorías de IA que podrían aplicar al problema

> *Marca todas las que el equipo considera relevantes para el problema definido.
> No es un compromiso — es una exploración.*

| Categoría | ¿Podría aplicar? | Razonamiento en 1 línea |
|---|---|---|
| **Modelos de lenguaje (LLM)** — texto, conversación, generación | NO | La salida es una etiqueta de 3 estados fijos; un LLM añade latencia y variabilidad innecesaria |
| **Visión computacional** — imágenes, video, detección visual | SÍ | El núcleo del sistema es detectar semáforos y determinar su color desde frames de video vehicular en tiempo real |
| **ML supervisado tabular** — predicción con datos históricos | NO | Los datos de entrada son imágenes, no variables numéricas o categóricas en tabla |
| **ML no supervisado** — segmentación, clustering | NO | Las 3 clases de salida están definidas de antemano; no se necesita agrupación sin etiquetas |
| **Automatización de flujos con IA** — orquestación de agentes | TAL VEZ | El pipeline frame → detección → análisis color → TTS requiere orquestación, implementable como lógica de aplicación sin agente externo |
| **Clasificación de audio / voz** | NO | El sistema produce voz como output; no clasifica audio como input |

---

### 2.2 Estimación de costo rough del MVP

> *Nivel de precisión esperado: orden de magnitud, no presupuesto formal.*
> *Escala de referencia: 🟢 Gratuito o casi / 🟡 Freemium con límites / 🔴 Pago con costo significativo*

| Componente | Categoría de herramienta | Nivel de costo estimado | Comentario |
|---|---|---|---|
| Motor de IA principal | COCO-SSD pre-entrenado via TensorFlow.js (@tensorflow-models/coco-ssd) | 🟢 | Modelo open source; inferencia en navegador, sin servidor de ML |
| Interfaz o frontend | App web (HTML/CSS/JS) desplegada en Vercel | 🟢 | Vercel capa gratuita; accesible desde cualquier navegador móvil |
| Almacenamiento de datos | Supabase (PostgreSQL) — recopilación de detecciones | 🟢 | Capa gratuita de Supabase para proyecto académico |
| Orquestación / automatización | Pipeline JavaScript en navegador (TF.js + canvas + Web Speech API) | 🟢 | Librerías open source sin costo de licencia |
| Otros *(TTS, etiquetado)* | Web Speech API (nativa en navegador) | 🟢 | Síntesis de voz sin costo, offline en dispositivo |

**Costo total estimado del MVP (rango aproximado):**

```
Mínimo: S/. 0 / mes
Máximo: S/. 0 / mes

Supuestos clave de esta estimación:
- El modelo COCO-SSD es pre-entrenado y se ejecuta en el navegador sin servidor propio.
- El MVP está desplegado en Vercel (capa gratuita): safelight-web.vercel.app
- La recopilación de datos de detección usa Supabase capa gratuita.
- Web Speech API es nativa en el navegador — sin costo de TTS.
- No se require GPU propia ni servidor cloud de inferencia.
```

---

### 2.3 Complejidad de implementación percibida

> *Evaluación honesta del equipo sobre qué tan difícil sería construir con estas categorías.*

| Dimensión | Nivel | Comentario |
|---|---|---|
| Curva de aprendizaje de las herramientas | Media | TensorFlow.js tiene documentación extensa; la integración de COCO-SSD en navegador es directa pero la lógica de análisis de color por tercios y el filtro de estabilidad requirieron iteración |
| Disponibilidad de tutoriales y documentación | Alta | Existen tutoriales académicos y repositorios públicos de detección con COCO-SSD; la arquitectura de análisis de color es propia del equipo |
| Dependencia de conocimiento técnico externo | Baja | El pipeline completo se construyó con librerías open source en JavaScript; sin dependencias de APIs externas de pago |
| Viabilidad de construir el MVP en 7 semanas | Alta | La estrategia Integrate (COCO-SSD pre-entrenado) eliminó la necesidad de entrenamiento propio, acelerando significativamente el desarrollo |

---

> **\* Análisis financiero detallado:** Esta sección será complementada con la
> **Plantilla 5 — Financial & Tech Feasibility**, que desarrollaremos más adelante en el
> curso. Incluirá costeo por tokens, comparación de APIs, ROI estimado del MVP y análisis
> de viabilidad financiera del producto a escala.

---

## SECCIÓN 3 — Experiencia del usuario

### 3.1 Input del usuario

> *¿Qué hace o ingresa el usuario para activar la IA?*

- [ ] Escribe texto en un chat o formulario
- [ ] Sube un archivo (imagen, PDF, CSV, audio)
- [x] Hace clic en un botón o selecciona una opción *(activación inicial del sistema)*
- [ ] Habla o graba un audio
- [x] El sistema se activa automáticamente (sin acción del usuario) *(luego del arranque)*
- [ ] Otro: _______________

**Descripción detallada del input:**

```
El conductor abre la app web (safelight-web.vercel.app) desde su dispositivo móvil
montado en soporte fijo del vehículo y activa la cámara con un único toque antes de
iniciar la marcha. A partir de ese momento, la cámara captura frames de video de forma
continua y automática sin ninguna acción adicional.
```

### 3.2 Output de la IA

> *¿Qué recibe el usuario como resultado de la interacción con la IA?*

- [ ] Texto / respuesta en lenguaje natural
- [ ] Número o predicción
- [ ] Clasificación o categoría
- [x] Alerta o notificación
- [ ] Recomendación con opciones
- [ ] Imagen generada
- [ ] Acción ejecutada automáticamente (email enviado, registro guardado, etc.)
- [ ] Otro: _______________

**Descripción detallada del output:**

```
Cuando el sistema detecta un semáforo con confianza ≥ 0.35, determina su color
mediante análisis de brillo en 3 tercios verticales del bounding box, y tras confirmar
el color en 6 frames consecutivos emite una alerta de voz ("Semáforo en ROJO / VERDE /
AMARILLO") via Web Speech API. Si el color confirmado cambia respecto al anterior,
emite nueva alerta con intervalo mínimo de 4000ms entre alertas del mismo color.
Si la confianza es insuficiente o el semáforo no es visible, el sistema permanece en
silencio — nunca emite alerta falsa (principio de fallo seguro por omisión).
```

---

## SECCIÓN 4 — Flujo del producto

### 4.1 Diagrama de flujo

> *Dibuja o describe el flujo completo paso a paso. Usa flechas (→) para conectar los pasos.*

```
Paso 1: Conductor abre safelight-web.vercel.app en navegador móvil y activa cámara →

Paso 2: Cámara captura frames de video en tiempo real (perspectiva vehicular frontal) →

Paso 3: COCO-SSD (TF.js, base MobileNetV2 pre-entrenado) detecta objetos clase
        "traffic light" en cada frame →

Paso 4: Sistema evalúa confianza y tamaño del bounding box:
        - Confianza ≥ 0.35 Y altura bbox ≥ 3.5% del alto del video → continúa al Paso 5
        - No cumple criterios → regresa al Paso 2 en silencio →

Paso 5: Análisis de brillo promedio en 3 tercios verticales del recorte:
        Superior más brillante → rojo / Medio más brillante → amarillo /
        Inferior más brillante → verde →

Paso 6: Sistema acumula frames: ¿6 frames consecutivos del mismo color?
        - SÍ y color distinto al anterior → continúa al Paso 7
        - NO → regresa al Paso 2 en silencio →

Paso 7: Web Speech API emite alerta de voz: "Semáforo en ROJO / VERDE / AMARILLO" →

Paso 8: Conductor recibe alerta y actúa de forma autónoma y segura.
        Sistema registra detección en Supabase (predicted_state, confidence, latency_ms, etc.)
```

**Versión visual (opcional pero recomendada):**

```
https://drive.google.com/file/d/1eHzOX-db_tPfh8beo718tSK1rh9xOPrK/view?usp=sharing
```

### 4.2 Humano en el circuito

> *¿En qué punto del flujo interviene un humano para validar o corregir antes de que la IA actúe?*

- [x] No hay intervención humana — la IA actúa de forma autónoma
- [ ] El humano revisa el output antes de que llegue al usuario final
- [ ] El humano puede aprobar o rechazar la recomendación de la IA
- [ ] El humano interviene solo cuando la IA no tiene respuesta
- [ ] Otro: _______________

**Justificación de la decisión:**

```
El contexto de conducción activa hace inviable cualquier intervención humana: el conductor
no puede validar una predicción mientras opera el vehículo. El riesgo de error se gestiona
con el umbral de confianza (0.35), el filtro de tamaño mínimo de bbox, y el requisito de
6 frames consecutivos del mismo color — sin detección estable y suficientemente confiable,
no hay alerta.
```

### 4.3 Plan de contingencia

> *¿Qué pasa si la IA falla, no tiene respuesta o la API no está disponible?*

```
- Semáforo no detectado o confianza insuficiente: el sistema permanece en silencio.
  El conductor aplica sus estrategias habituales (posición de la luz, comportamiento
  del tráfico). No se emite alerta falsa bajo ninguna circunstancia.

- TTS: el sistema usa Web Speech API, síntesis de voz nativa del navegador sin
  dependencia de servidor externo. Disponible offline en la mayoría de dispositivos
  móviles modernos.

- Fallo del navegador o desconexión: el conductor opera normalmente sin asistencia.
  No existe mecanismo de fallo catastrófico — la ausencia de alerta es el estado seguro.
```

---

## SECCIÓN 5 — Decisión estratégica Build / Buy / Integrate

Marca con una X:

- [ ] **Buy** — usar herramienta existente sin modificar
- [x] **Integrate** — conectar API de IA a flujo o interfaz propia
- [ ] **Build** — entrenar modelo propio con datos del equipo
- [ ] **Combinación**

**Justificación (obligatoria):**

> *¿Por qué esta estrategia y no las otras dos? Argumenta en función del problema
> y el tiempo disponible.*

```
Se adoptó la estrategia Integrate usando COCO-SSD (TensorFlow.js, base MobileNetV2)
pre-entrenado en COCO, que incluye la clase "traffic light". No existe herramienta
comprable que clasifique el COLOR del semáforo; la determinación de color se
implementa como lógica propia de análisis de brillo por tercios verticales del
bounding box — componente propio del equipo integrado sobre el detector COCO.

Se descartó Build (entrenamiento propio) por el fallo del modelo YOLOv8-nano
entrenado con LISA Dataset: el desequilibrio severo de clases (rojo 25,876 vs
amarillo 1,516 bboxes) causó clasificación sistemática como "verde", haciendo
el modelo inutilizable. COCO-SSD pre-entrenado demostró confianza promedio de 0.81
en producción y F1 macro de 83.9% sin fine-tuning adicional.

El módulo TTS usa Web Speech API nativa del navegador — sin costo, sin latencia
de red, sin dependencia de proveedor externo.
```

---

## SECCIÓN 6 — Diseño específico por tipo de IA

### Si es IA Generativa — System Prompt

> *No aplica a este proyecto. El componente de comunicación usa síntesis de voz (Web Speech API)
> determinista sobre un conjunto cerrado de 3 etiquetas predefinidas (rojo / amarillo / verde).
> No se emplea ningún modelo de lenguaje generativo. La salida es invariable dada una
> clasificación: predictibilidad es un requisito de seguridad.*

---

### Si es ML Tradicional — Especificaciones del modelo

| Campo | Detalle |
|---|---|
| Tipo de modelo | Detección de objetos + clasificación de color — 3 clases de estado: rojo / amarillo / verde. El modelo COCO-SSD detecta "traffic light"; el color se determina por análisis de brillo en tercios verticales del bounding box |
| Herramienta de inferencia | TensorFlow.js (@tensorflow-models/coco-ssd) en navegador web — modelo pre-entrenado en COCO, base MobileNetV2, SIN fine-tuning propio |
| Variable objetivo (target) | Estado del semáforo visible en el frame capturado por cámara vehicular frontal: rojo / amarillo / verde |
| Features principales | Píxeles del frame de video procesados por COCO-SSD; brillo promedio en 3 tercios verticales del recorte del bounding box para determinación de color |
| Métrica de evaluación principal | F1-score por clase (más robusto que accuracy ante posible desbalance entre clases en datos de evaluación) |
| Criterio mínimo de aceptación | F1-score macro ≥ 80% en las 3 clases sobre conjunto de prueba en campo; latencia total mediana < 500ms desde captura hasta inicio de audio; tasa de alertas falsas < 10% |

---

## SECCIÓN 7 — Alcance del MVP

### Lo que SÍ incluye el MVP

> *Lista las funcionalidades que estarán listas para la sustentación de la Semana 14.*

```
1. Modelo COCO-SSD pre-entrenado (TF.js, base MobileNetV2) ejecutado en navegador web,
   capaz de detectar semáforos en tiempo real con confianza promedio 0.81.

2. Pipeline de análisis de color por brillo en 3 tercios verticales del bounding box,
   con filtro de tamaño mínimo (altura ≥ 3.5% del frame) y estabilidad de 6 frames
   consecutivos antes de confirmar un color.

3. Módulo TTS (Web Speech API) que convierte la etiqueta clasificada en alerta de voz
   en español, emitida por el audio del dispositivo. Intervalo mínimo 4000ms entre
   alertas del mismo color.

4. App web desplegada en Vercel (safelight-web.vercel.app), accesible desde cualquier
   navegador móvil moderno sin instalación.

5. Sistema de recopilación automática de detecciones en Supabase (PostgreSQL) con
   métricas de latencia, confianza y estado para evaluación posterior.
```

### Lo que NO incluye el MVP *(pero podría incluir una versión futura)*

> *Declarar esto explícitamente demuestra madurez en la gestión del proyecto.*

```
1. Aplicación móvil nativa publicada en tienda (App Store / Google Play) con UI completa.

2. Integración con GPS para alertas de proximidad a intersecciones semaforizadas.

3. Detección simultánea de múltiples semáforos en intersecciones complejas de varios
   carriles.

4. Fine-tuning del modelo para condiciones específicas del entorno limeño (requeriría
   dataset local etiquetado que no existe públicamente).
```

### Criterio de éxito del MVP

> *¿Cómo sabrá el equipo que el MVP está listo para ser sustentado?*

```
"El MVP está listo cuando un usuario externo al equipo puede abrir la app web desde
su dispositivo móvil, activar la cámara con un único gesto y recibir alertas de voz
correctas del estado del semáforo con F1 macro ≥ 80% en condiciones normales de
iluminación diurna, con latencia total mediana < 500ms, sin instrucciones adicionales
del equipo."
```

---

## SECCIÓN 8 — OKRs y KPIs del producto

> **Instrucción:**
> Esta sección define los compromisos de impacto del producto **antes de construir**.
> La Plantilla 4 usará estos valores como línea base para evaluar resultados reales.
> Un equipo que no puede definir esto antes de construir no sabe qué está construyendo.

---

### Estructura obligatoria: O + KR + KPI

```
OBJETIVO (O)          → La mejora de negocio que persigue el producto (1 frase aspiracional)
    Key Result 1 (KR) → Métrica de negocio con valor actual y meta específica
    Key Result 2 (KR) → Métrica de negocio con valor actual y meta específica
    KPI técnico       → Métrica del modelo que indica que la IA funciona correctamente
```

**Ejemplo de referencia (NO copiar — solo para entender la estructura):**

```
O:   Reducir la carga operativa del equipo de atención al cliente
KR1: Tasa de resolución autónoma por IA > 75% (valor actual: 0% — todo es manual)
KR2: Tiempo promedio de respuesta < 2 min (valor actual: 45 min)
KPI: Tasa de alucinación del modelo < 5%
```

---

### OKR + KPI del proyecto

**Objetivo (O):**

> *Una frase que describe la mejora de negocio que el producto persigue.
> Debe ser ambiciosa pero alcanzable en el semestre.*

```
O: Permitir que conductores con daltonismo identifiquen el estado de semáforos
   urbanos de forma autónoma y en tiempo real, eliminando su dependencia de
   estrategias visuales compensatorias durante la conducción en Lima.
```

---

**Key Result 1 (KR1):**

| Campo | Detalle |
|---|---|
| Métrica | F1-score macro del sistema en evaluación de campo (promedio de las 3 clases: rojo / amarillo / verde) |
| Valor actual | 0% — no existe ningún sistema automatizado para este caso de uso en Lima |
| Meta con el MVP | F1-score macro ≥ 80% sobre detecciones reales anotadas en sesión de prueba en campo |
| Método de medición | Anotación manual posterior de predicted_state vs real_state en datos recopilados en Supabase; cálculo de F1 por clase y macro |
| Período de medición | Semanas 12-13 — al finalizar sesiones de prueba en campo |

---

**Key Result 2 (KR2):**

| Campo | Detalle |
|---|---|
| Métrica | Latencia total del sistema: tiempo desde captura del frame (T0) hasta inicio de la alerta de voz (T5) |
| Valor actual | N/A — no existe sistema de referencia |
| Meta con el MVP | Latencia total mediana < 500ms por evento de detección en hardware del prototipo |
| Método de medición | Medición automática con timestamps en el pipeline (latency_ms) almacenados en Supabase durante sesión de prueba |
| Período de medición | Semanas 11-13 — durante integración y sesiones de prueba |

---

**Key Result 3 (KR3) — opcional pero recomendado:**

| Campo | Detalle |
|---|---|
| Métrica | Tasa de alertas falsas (eventos donde el sistema emite alerta incorrecta o sin semáforo visible) |
| Valor actual | N/A |
| Meta con el MVP | Tasa de alertas falsas < 10% en sesión de prueba en entorno urbano real |
| Método de medición | Revisión manual del campo alerta_correcta en Supabase, contrastado con real_state anotado en cada sesión de prueba |
| Período de medición | Semana 13 — sesión de prueba en campo con usuario externo al equipo |

---

**KPI técnico del modelo:**

| Campo | Detalle |
|---|---|
| Métrica técnica | F1-score macro en evaluación de campo + tasa de alertas falsas en prueba real |
| Criterio mínimo aceptable | F1-score macro ≥ 80% en las 3 clases Y tasa de alertas falsas < 10% en prueba real (ambas condiciones deben cumplirse simultáneamente) |
| Método de medición | Cálculo sobre datos de Supabase: F1 desde predicted_state vs real_state; tasa de falsas desde campo alerta_correcta |

---

### Verificación de coherencia interna

> *Antes de continuar, confirma que el OKR es coherente con el resto del canvas.*

| Pregunta | Respuesta |
|---|---|
| ¿El Objetivo refleja directamente el problema de la Sección 1.2? | SÍ |
| ¿Los KRs son medibles con números concretos (no "mejorar" o "aumentar")? | SÍ |
| ¿El KPI técnico está conectado al tipo de IA elegido en la Sección 1.4? | SÍ |
| ¿El equipo puede obtener el valor actual de los KRs antes de la Semana 6? | SÍ — los valores actuales son 0/N/A porque el sistema no existe; esto es verificable y documentable |

> **Si algún KR dice "mejorar", "aumentar" o "reducir" sin número → no es un KR válido.**

---

## SECCIÓN 9 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema en la Sección 1.2 es copia exacta del Problem Statement Canvas? | SÍ |
| ¿El flujo de la Sección 4 es consistente con el tipo de IA elegido? | SÍ |
| ¿El stack tecnológico fue verificado (cuentas creadas, accesos confirmados)? | SÍ — TF.js, COCO-SSD, Web Speech API y Supabase están en producción en safelight-web.vercel.app |
| ¿El alcance del MVP es realista para construir en 7 semanas? | SÍ — estrategia Integrate con COCO-SSD pre-entrenado elimina la necesidad de entrenamiento propio |
| ¿El system prompt fue probado al menos una vez antes de entregar? | N/A — el sistema no usa IA Generativa ni system prompt |
| ¿El Objetivo del OKR refleja el problema definido en Fase P? | SÍ |
| ¿Los KRs tienen valores actuales concretos (no estimados)? | SÍ — valor actual documentado como 0/N/A con justificación explícita |
| ¿Todos los integrantes entienden cada sección de este canvas? | SÍ |

> **Si alguna respuesta es NO → el canvas no está listo para entregar.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 3 de 4*
