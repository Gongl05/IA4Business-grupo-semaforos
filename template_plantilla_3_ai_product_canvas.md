# Plantilla 3 — AI Product Canvas
## Framework PROMPT | Fase O — Oportunidad de IA
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** 08/05/26
**Versión del canvas:** v1

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
Nombre: ChromaVía
```

### 1.2 Problema que resuelve

> *Copia exactamente la declaración del Problem Statement Canvas. No parafrasear.*

```
"Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar
de forma autónoma y en tiempo real el estado de los semáforos convencionales durante
la conducción porque la normativa de diseño semafórico peruana no exige criterios de
accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos
ante señales críticas, incrementando el riesgo de infracciones involuntarias, colisiones
y pérdida de autonomía de conducción segura para los aproximadamente 200,000 conductores
afectados en Lima Metropolitana."
```

### 1.3 Usuario principal

> *¿Quién usa el producto directamente? Sé específico — no "las empresas" sino el rol exacto.*

```
Conductor adulto con daltonismo rojo-verde (deuteranopía o protanopía) que opera un
vehículo privado en entornos urbanos de Lima Metropolitana y requiere asistencia autónoma
para interpretar el estado de semáforos convencionales en tiempo real durante la conducción.
```

### 1.4 Tipo de IA

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
| **Modelos de lenguaje (LLM)** — texto, conversación, generación | NO | La salida es una etiqueta de 4 estados fijos; un LLM añade latencia y variabilidad innecesaria |
| **Visión computacional** — imágenes, video, detección visual | SÍ | El núcleo del sistema es clasificar el estado del semáforo desde frames de video vehicular en tiempo real |
| **ML supervisado tabular** — predicción con datos históricos | NO | Los datos de entrada son imágenes, no variables numéricas o categóricas en tabla |
| **ML no supervisado** — segmentación, clustering | NO | Las 4 clases de salida están definidas de antemano; no se necesita agrupación sin etiquetas |
| **Automatización de flujos con IA** — orquestación de agentes | TAL VEZ | El pipeline frame → modelo → TTS requiere orquestación, implementable como script sin agente externo |
| **Clasificación de audio / voz** | NO | El sistema produce voz como output; no clasifica audio como input |

---

### 2.2 Estimación de costo rough del MVP

> *Nivel de precisión esperado: orden de magnitud, no presupuesto formal.*
> *Escala de referencia: 🟢 Gratuito o casi / 🟡 Freemium con límites / 🔴 Pago con costo significativo*

| Componente | Categoría de herramienta | Nivel de costo estimado | Comentario |
|---|---|---|---|
| Motor de IA principal | CNN con transfer learning — TensorFlow / PyTorch sobre Google Colab | 🟢 | Frameworks open source; entrenamiento en Colab capa gratuita o hardware local |
| Interfaz o frontend | Script Python con activación por botón | 🟢 | MVP académico sin app publicada; interfaz mínima de consola |
| Almacenamiento de datos | Google Drive / almacenamiento local | 🟢 | Datasets almacenados localmente durante entrenamiento; sin costo de nube |
| Orquestación / automatización | Pipeline Python (OpenCV + TensorFlow Lite + pyttsx3) | 🟢 | Librerías open source sin costo de licencia |
| Otros *(TTS, etiquetado)* | pyttsx3 offline / LabelImg o Roboflow capa gratuita | 🟢 | Síntesis de voz y etiquetado sin costo para uso académico |

**Costo total estimado del MVP (rango aproximado):**

```
Mínimo: S/. 0 / mes
Máximo: S/. 40 / mes

Supuestos clave de esta estimación:
- El entrenamiento del modelo es un proceso único, no recurrente en producción.
- El MVP opera en hardware local o dispositivo fijo del vehículo, sin servidor cloud.
- Si se requiere GPU para entrenamiento: Google Colab Pro (~$10/mes ≈ S/. 37).
- La captura de imágenes locales para fine-tuning se realiza con celular del equipo
  (sin costo adicional). No se contrata TTS de pago para el prototipo académico.
```

---

### 2.3 Complejidad de implementación percibida

> *Evaluación honesta del equipo sobre qué tan difícil sería construir con estas categorías.*

| Dimensión | Nivel | Comentario |
|---|---|---|
| Curva de aprendizaje de las herramientas | Media | TensorFlow/PyTorch tienen documentación extensa, pero transfer learning y fine-tuning requieren comprensión de visión computacional que el equipo debe desarrollar durante el curso |
| Disponibilidad de tutoriales y documentación | Alta | Existen tutoriales académicos y repositorios públicos de detección de semáforos con CNN; los tres datasets tienen papers de referencia con arquitecturas reproducibles |
| Dependencia de conocimiento técnico externo | Media | El pipeline completo puede construirse con librerías open source; el mayor riesgo técnico es el fine-tuning con imágenes propias capturadas en Lima |
| Viabilidad de construir el MVP en 7 semanas | Media | Alcanzable si se usa transfer learning sobre MobileNetV2 preentrenada y el MVP se delimita a prototipo funcional en entorno controlado, no a app publicada en tienda |

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
El conductor monta el dispositivo en soporte fijo del vehículo y activa el sistema
con un único toque antes de iniciar la marcha. A partir de ese momento, la cámara
captura frames de video de forma continua y automática sin ninguna acción adicional.
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
Cuando el modelo detecta un semáforo con confianza suficiente, emite una alerta de voz
("Semáforo en ROJO / VERDE / AMARILLO") por el audio del dispositivo o auricular.
Si la confianza es insuficiente o el semáforo no es visible, el sistema permanece en
silencio — nunca emite alerta falsa (principio de fallo seguro por omisión).
```

---

## SECCIÓN 4 — Flujo del producto

### 4.1 Diagrama de flujo

> *Dibuja o describe el flujo completo paso a paso. Usa flechas (→) para conectar los pasos.*

```
Paso 1: Conductor monta el dispositivo en soporte fijo y activa el sistema →

Paso 2: Cámara captura frames de video en tiempo real (perspectiva vehicular frontal) →

Paso 3: Modelo CNN procesa cada frame e intenta detectar y clasificar el semáforo visible →

Paso 4: Sistema evalúa el nivel de confianza de la predicción:
        - Confianza ≥ umbral definido → continúa al Paso 5
        - Confianza < umbral o semáforo no detectado → regresa al Paso 2 en silencio →

Paso 5: Módulo TTS convierte la etiqueta clasificada en alerta de voz →

Paso 6: → Conductor recibe: "Semáforo en ROJO / VERDE / AMARILLO" y actúa de forma
          autónoma y segura.
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
con el umbral de confianza — sin detección segura, no hay alerta.
```

### 4.3 Plan de contingencia

> *¿Qué pasa si la IA falla, no tiene respuesta o la API no está disponible?*

```
- Semáforo no detectado o confianza insuficiente: el sistema permanece en silencio.
  El conductor aplica sus estrategias habituales (posición de la luz, comportamiento
  del tráfico). No se emite alerta falsa bajo ninguna circunstancia.

- TTS sin conexión: el sistema usa pyttsx3, motor TTS offline embebido, sin dependencia
  de internet.

- Fallo o desconexión del dispositivo: el sistema emite un tono único de advertencia
  indicando que está desactivado. El conductor opera normalmente sin asistencia.
```

---

## SECCIÓN 5 — Decisión estratégica Build / Buy / Integrate

Marca con una X:

- [ ] **Buy** — usar herramienta existente sin modificar
- [ ] **Integrate** — conectar API de IA a flujo o interfaz propia
- [x] **Build** — entrenar modelo propio con datos del equipo
- [ ] **Combinación**

**Justificación (obligatoria):**

> *¿Por qué esta estrategia y no las otras dos? Argumenta en función del problema
> y el tiempo disponible.*

```
No existe herramienta comprable que clasifique semáforos en tiempo real calibrada para
el entorno visual de Lima. Las APIs de visión genérica (Google Vision, AWS Rekognition)
no ofrecen latencia <200ms para este caso de uso ni están calibradas para infraestructura
semafórica latinoamericana. Se entrena una CNN propia con transfer learning sobre
MobileNetV2 usando LISA + BSTLD + Brazilian UFU y fine-tuning con imágenes locales,
viable en 7 semanas con Google Colab. El módulo TTS (pyttsx3) es una biblioteca
open-source determinista — no constituye un componente de IA que justifique
una estrategia "Integrate" separada.
```

---

## SECCIÓN 6 — Diseño específico por tipo de IA

### Si es IA Generativa — System Prompt

> *No aplica a este proyecto. El componente de comunicación usa síntesis de voz (TTS)
> determinista sobre un conjunto cerrado de 4 etiquetas predefinidas (rojo / amarillo /
> verde / no detectado). No se emplea ningún modelo de lenguaje generativo. La salida
> es invariable dada una clasificación: predictibilidad es un requisito de seguridad.*

---

### Si es ML Tradicional — Especificaciones del modelo

| Campo | Detalle |
|---|---|
| Tipo de modelo | Clasificación supervisada de imagen — 4 clases: rojo / amarillo / verde / semáforo no detectado |
| Herramienta de entrenamiento | TensorFlow o PyTorch con arquitectura MobileNetV2 (transfer learning) — entrenamiento en Google Colab |
| Variable objetivo (target) | Estado del semáforo visible en el frame capturado por cámara vehicular frontal |
| Features principales | Píxeles del frame de video (región de interés centrada en el semáforo detectado); variaciones lumínicas implícitas en la imagen |
| Métrica de evaluación principal | F1-score por clase (más robusto que accuracy ante posible desbalance entre clases) |
| Criterio mínimo de aceptación | F1-score ≥ 80% en las 4 clases sobre el conjunto de test; latencia de inferencia < 200ms por frame en hardware del prototipo |

---

## SECCIÓN 7 — Alcance del MVP

### Lo que SÍ incluye el MVP

> *Lista las funcionalidades que estarán listas para la sustentación de la Semana 14.*

```
1. Modelo CNN entrenado con LISA + BSTLD + Brazilian UFU con fine-tuning sobre imágenes
   propias capturadas en Lima, capaz de clasificar el estado del semáforo en 4 categorías.

2. Pipeline de inferencia en tiempo real sobre stream de video desde cámara vehicular
   fija, con latencia de inferencia < 200ms por frame.

3. Módulo TTS que convierte la etiqueta clasificada en alerta de voz en español, emitida
   por el audio del dispositivo.

4. Umbral de confianza configurable: el sistema emite alerta solo cuando la predicción
   supera el umbral; en caso contrario, permanece en silencio (fail-safe por omisión).
```

### Lo que NO incluye el MVP *(pero podría incluir una versión futura)*

> *Declarar esto explícitamente demuestra madurez en la gestión del proyecto.*

```
1. Aplicación móvil publicada en tienda (App Store / Google Play) con UI completa.

2. Integración con GPS para alertas de proximidad a intersecciones semaforizadas.

3. Detección simultánea de múltiples semáforos en intersecciones complejas de varios
   carriles.
```

### Criterio de éxito del MVP

> *¿Cómo sabrá el equipo que el MVP está listo para ser sustentado?*

```
"El MVP está listo cuando un usuario externo al equipo puede montar el dispositivo en
su vehículo, activar el sistema con un único gesto y recibir alertas de voz correctas
del estado del semáforo en al menos 8 de cada 10 pruebas, en condiciones normales de
iluminación diurna, sin instrucciones adicionales del equipo."
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
| Métrica | F1-score del modelo CNN en el conjunto de test (promedio ponderado de las 4 clases) |
| Valor actual | 0% — no existe ningún sistema automatizado para este caso de uso en Lima |
| Meta con el MVP | F1-score ≥ 80% sobre el conjunto de test combinado (datasets públicos + imágenes locales de Lima) |
| Método de medición | Reporte automático del framework de entrenamiento (TensorFlow/PyTorch) sobre conjunto de test separado al final del ciclo de entrenamiento |
| Período de medición | Semana 12 — al finalizar el ciclo de entrenamiento y fine-tuning |

---

**Key Result 2 (KR2):**

| Campo | Detalle |
|---|---|
| Métrica | Latencia total del sistema: tiempo entre captura del frame y emisión de la alerta de voz |
| Valor actual | N/A — no existe sistema de referencia |
| Meta con el MVP | Latencia total (inferencia CNN + TTS) < 500ms por evento de detección en hardware del prototipo |
| Método de medición | Medición con timestamps en el pipeline Python durante sesión de prueba en entorno controlado |
| Período de medición | Semana 11 — al completar la integración del pipeline CNN + TTS |

---

**Key Result 3 (KR3) — opcional pero recomendado:**

| Campo | Detalle |
|---|---|
| Métrica | Tasa de alertas falsas (eventos donde el sistema emite alerta incorrecta o sin semáforo visible) |
| Valor actual | N/A |
| Meta con el MVP | Tasa de falsos positivos < 10% en sesión de prueba de 30 minutos en entorno urbano |
| Método de medición | Revisión manual del log de eventos de voz durante la sesión de prueba, contrastado con video grabado simultáneamente |
| Período de medición | Semana 13 — sesión de prueba en campo con usuario externo al equipo |

---

**KPI técnico del modelo:**

| Campo | Detalle |
|---|---|
| Métrica técnica | F1-score por clase en conjunto de test + tasa de falsos positivos en prueba en campo |
| Criterio mínimo aceptable | F1-score ≥ 80% en todas las clases; tasa de alertas falsas < 10% en prueba real |
| Método de medición | Reporte automático del framework de ML para evaluación offline + log de eventos en sesión de prueba en campo para evaluación en uso real |

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
| ¿El stack tecnológico fue verificado (cuentas creadas, accesos confirmados)? | SÍ — TensorFlow, OpenCV y pyttsx3 son open source; Google Colab es de acceso libre |
| ¿El alcance del MVP es realista para construir en 7 semanas? | SÍ — se delimita a prototipo funcional con transfer learning, no a app publicada |
| ¿El system prompt fue probado al menos una vez antes de entregar? | N/A — el sistema no usa IA Generativa ni system prompt |
| ¿El Objetivo del OKR refleja el problema definido en Fase P? | SÍ |
| ¿Los KRs tienen valores actuales concretos (no estimados)? | SÍ — valor actual documentado como 0/N/A con justificación explícita |
| ¿Todos los integrantes entienden cada sección de este canvas? | SÍ |

> **Si alguna respuesta es NO → el canvas no está listo para entregar.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 3 de 4*