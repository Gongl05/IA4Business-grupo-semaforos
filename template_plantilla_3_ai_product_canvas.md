# Plantilla 3 — AI Product Canvas
## Framework PROMPT | Fase O — Oportunidad de IA
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: _______________________________________________
- Integrante 2: _______________________________________________
- Integrante 3: _______________________________________________

**Fecha de entrega:** _______________
**Versión del canvas:** _______________

---

> **Instrucción general:**
> Este canvas se completa DESPUÉS de tener aprobados el Problem Statement Canvas y el Data Readiness Checklist. Si el problema o los datos cambian, este canvas debe actualizarse. Las secciones deben ser consistentes entre sí — si hay contradicciones internas, el canvas no está listo.

---

## SECCIÓN 1 — Identidad del producto

### 1.1 Nombre del producto / MVP
```
Nombre:
```

### 1.2 Problema que resuelve
> *Copia exactamente la declaración del Problem Statement Canvas. No parafrasear.*

```
"[Usuario] tiene dificultad para [acción] porque [causa], lo que genera [consecuencia]."


```

### 1.3 Usuario principal
> *¿Quién usa el producto directamente? Sé específico — no "las empresas" sino el rol exacto.*

```
Escribe aquí:


```

### 1.4 Tipo de IA
- [ ] IA Generativa
- [ ] ML Tradicional — Clasificación
- [ ] ML Tradicional — Regresión
- [ ] ML Tradicional — Clustering
- [ ] Combinación GenAI + ML

---

## SECCIÓN 2 — Tech & Cost Overview *(visión de temperatura)*

> **Instrucción:**
> Esta sección NO es la selección definitiva del stack tecnológico — esa decisión se toma en la **Fase M1** con criterios técnicos y financieros más detallados.
> El objetivo aquí es tener una primera lectura de qué categorías de herramientas podrían aplicar y cuánto podría costar el MVP a nivel general.
> Sé honesto sobre lo que no sabes aún — una estimación consciente vale más que un número inventado.

---

### 2.1 Categorías de IA que podrían aplicar al problema

> *Marca todas las que el equipo considera relevantes para el problema definido. No es un compromiso — es una exploración.*

| Categoría | ¿Podría aplicar? | Razonamiento en 1 línea |
|---|---|---|
| **Modelos de lenguaje (LLM)** — texto, conversación, generación | SÍ / NO / TAL VEZ | |
| **Visión computacional** — imágenes, video, detección visual | SÍ / NO / TAL VEZ | |
| **ML supervisado tabular** — predicción con datos históricos | SÍ / NO / TAL VEZ | |
| **ML no supervisado** — segmentación, clustering | SÍ / NO / TAL VEZ | |
| **Automatización de flujos con IA** — orquestación de agentes | SÍ / NO / TAL VEZ | |
| **Clasificación de audio / voz** | SÍ / NO / TAL VEZ | |

---

### 2.2 Estimación de costo rough del MVP

> *Nivel de precisión esperado: orden de magnitud, no presupuesto formal.*
> *Escala de referencia: 🟢 Gratuito o casi / 🟡 Freemium con límites / 🔴 Pago con costo significativo*

| Componente | Categoría de herramienta | Nivel de costo estimado | Comentario |
|---|---|---|---|
| Motor de IA principal | | 🟢 / 🟡 / 🔴 | |
| Interfaz o frontend | | 🟢 / 🟡 / 🔴 | |
| Almacenamiento de datos | | 🟢 / 🟡 / 🔴 | |
| Orquestación / automatización | | 🟢 / 🟡 / 🔴 | |
| Otros *(APIs, integraciones)* | | 🟢 / 🟡 / 🔴 | |

**Costo total estimado del MVP (rango aproximado):**
```
Mínimo: S/. _____ / mes      Máximo: S/. _____ / mes
Supuestos clave de esta estimación:


```

---

### 2.3 Complejidad de implementación percibida

> *Evaluación honesta del equipo sobre qué tan difícil sería construir con estas categorías.*

| Dimensión | Nivel | Comentario |
|---|---|---|
| Curva de aprendizaje de las herramientas | Alta / Media / Baja | |
| Disponibilidad de tutoriales y documentación | Alta / Media / Baja | |
| Dependencia de conocimiento técnico externo | Alta / Media / Baja | |
| Viabilidad de construir el MVP en 7 semanas | Alta / Media / Baja | |

---

> **\* Análisis financiero detallado:** Esta sección será complementada con la **Plantilla 5 — Financial & Tech Feasibility**, que desarrollaremos más adelante en el curso. Incluirá costeo por tokens, comparación de APIs, ROI estimado del MVP y análisis de viabilidad financiera del producto a escala.

---

## SECCIÓN 3 — Experiencia del usuario

### 3.1 Input del usuario
> *¿Qué hace o ingresa el usuario para activar la IA?*

- [ ] Escribe texto en un chat o formulario
- [ ] Sube un archivo (imagen, PDF, CSV, audio)
- [ ] Hace clic en un botón o selecciona una opción
- [ ] Habla o graba un audio
- [ ] El sistema se activa automáticamente (sin acción del usuario)
- [ ] Otro: _______________

**Descripción detallada del input:**
```
Escribe aquí (2-3 líneas):


```

### 3.2 Output de la IA
> *¿Qué recibe el usuario como resultado de la interacción con la IA?*

- [ ] Texto / respuesta en lenguaje natural
- [ ] Número o predicción
- [ ] Clasificación o categoría
- [ ] Alerta o notificación
- [ ] Recomendación con opciones
- [ ] Imagen generada
- [ ] Acción ejecutada automáticamente (email enviado, registro guardado, etc.)
- [ ] Otro: _______________

**Descripción detallada del output:**
```
Escribe aquí (2-3 líneas):


```

---

## SECCIÓN 4 — Flujo del producto

### 4.1 Diagrama de flujo
> *Dibuja o describe el flujo completo paso a paso. Usa flechas (→) para conectar los pasos.*

```
Paso 1: Usuario →
Paso 2: →
Paso 3: →
Paso 4: →
Paso 5: → Usuario recibe:
```

**Versión visual (opcional pero recomendada):**
> *Pega aquí una imagen del diagrama o el link a la herramienta donde lo dibujaste (Miro, Lucidchart, draw.io, etc.)*

```
Link o imagen:
```

### 4.2 Humano en el circuito
> *¿En qué punto del flujo interviene un humano para validar o corregir antes de que la IA actúe?*

- [ ] No hay intervención humana — la IA actúa de forma autónoma
- [ ] El humano revisa el output antes de que llegue al usuario final
- [ ] El humano puede aprobar o rechazar la recomendación de la IA
- [ ] El humano interviene solo cuando la IA no tiene respuesta
- [ ] Otro: _______________

**Justificación de la decisión:**
```
Escribe aquí (2-3 líneas):


```

### 4.3 Plan de contingencia
> *¿Qué pasa si la IA falla, no tiene respuesta o la API no está disponible?*

```
Escribe aquí:


```

---

## SECCIÓN 5 — Decisión estratégica Build / Buy / Integrate

Marca con una X:

- [ ] **Buy** — usar herramienta existente sin modificar
- [ ] **Integrate** — conectar API de IA a flujo o interfaz propia
- [ ] **Build** — entrenar modelo propio con datos del equipo
- [ ] **Combinación** — especificar: _______________

**Justificación (obligatoria):**
> *¿Por qué esta estrategia y no las otras dos? Argumenta en función del problema y el tiempo disponible.*

```
Escribe aquí (4-5 líneas):


```

---

## SECCIÓN 6 — Diseño específico por tipo de IA

### Si es IA Generativa — System Prompt

> *Escribe el system prompt completo que usará el MVP. Debe seguir la estructura mínima obligatoria.*

```
ROL:
[Quién es la IA en este producto]

CONTEXTO:
[Qué sabe sobre la empresa, el dominio o el usuario]

RESTRICCIONES:
[Qué NO debe hacer nunca]

FORMATO DE RESPUESTA:
[Cómo debe estructurar sus respuestas]

TONO:
[Formal / cercano / técnico / empático]

IDIOMA:
[Idioma en el que responde siempre]
```

**Plan de gestión de alucinaciones:**
```
¿Qué pasa si la IA inventa información?

¿Hay validación humana en decisiones críticas?

¿El usuario puede reportar respuestas incorrectas?
```

---

### Si es ML Tradicional — Especificaciones del modelo

| Campo | Detalle |
|---|---|
| Tipo de modelo | *(clasificación / regresión / clustering)* |
| Herramienta de entrenamiento | *(Teachable Machine / BigML / AutoML / otra)* |
| Variable objetivo (target) | |
| Features principales | *(variables de entrada más importantes)* |
| Métrica de evaluación principal | *(accuracy / precision / recall / F1 / RMSE)* |
| Criterio mínimo de aceptación | *(ej: accuracy > 80% para considerar el modelo útil)* |

---

## SECCIÓN 7 — Alcance del MVP

### Lo que SÍ incluye el MVP
> *Lista las funcionalidades que estarán listas para la sustentación de la Semana 14.*

```
1.
2.
3.
4.
```

### Lo que NO incluye el MVP *(pero podría incluir una versión futura)*
> *Declarar esto explícitamente demuestra madurez en la gestión del proyecto.*

```
1.
2.
3.
```

### Criterio de éxito del MVP
> *¿Cómo sabrá el equipo que el MVP está listo para ser sustentado?*

```
"El MVP está listo cuando un usuario externo al equipo puede [acción específica]
sin instrucciones adicionales y obtiene [resultado específico]."


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
> *Una frase que describe la mejora de negocio que el producto persigue. Debe ser ambiciosa pero alcanzable en el semestre.*

```
O:


```

---

**Key Result 1 (KR1):**

| Campo | Detalle |
|---|---|
| Métrica | ¿Qué se mide exactamente? |
| Valor actual | ¿Cuánto es hoy, sin el MVP? |
| Meta con el MVP | ¿Cuánto debería ser con el MVP funcionando? |
| Método de medición | ¿Cómo y con qué herramienta se medirá? |
| Período de medición | ¿En qué semana se medirá para la sustentación? |

---

**Key Result 2 (KR2):**

| Campo | Detalle |
|---|---|
| Métrica | ¿Qué se mide exactamente? |
| Valor actual | ¿Cuánto es hoy, sin el MVP? |
| Meta con el MVP | ¿Cuánto debería ser con el MVP funcionando? |
| Método de medición | ¿Cómo y con qué herramienta se medirá? |
| Período de medición | ¿En qué semana se medirá para la sustentación? |

---

**Key Result 3 (KR3) — opcional pero recomendado:**

| Campo | Detalle |
|---|---|
| Métrica | |
| Valor actual | |
| Meta con el MVP | |
| Método de medición | |
| Período de medición | |

---

**KPI técnico del modelo:**

| Campo | Detalle |
|---|---|
| Métrica técnica | *(ej: accuracy, tasa de alucinación, tasa de rechazo, F1 score)* |
| Criterio mínimo aceptable | *(ej: accuracy > 80%, alucinación < 5%)* |
| Método de medición | *(evaluación manual, log de conversaciones, reporte de herramienta)* |

---

### Verificación de coherencia interna

> *Antes de continuar, confirma que el OKR es coherente con el resto del canvas.*

| Pregunta | Respuesta |
|---|---|
| ¿El Objetivo refleja directamente el problema de la Sección 1.2? | SÍ / NO |
| ¿Los KRs son medibles con números concretos (no "mejorar" o "aumentar")? | SÍ / NO |
| ¿El KPI técnico está conectado al tipo de IA elegido en la Sección 1.4? | SÍ / NO |
| ¿El equipo puede obtener el valor actual de los KRs antes de la Semana 6? | SÍ / NO |

> **Si algún KR dice "mejorar", "aumentar" o "reducir" sin número → no es un KR válido.**

---

## SECCIÓN 9 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema en la Sección 1.2 es copia exacta del Problem Statement Canvas? | SÍ / NO |
| ¿El flujo de la Sección 4 es consistente con el tipo de IA elegido? | SÍ / NO |
| ¿El stack tecnológico fue verificado (cuentas creadas, accesos confirmados)? | SÍ / NO |
| ¿El alcance del MVP es realista para construir en 7 semanas? | SÍ / NO |
| ¿El system prompt fue probado al menos una vez antes de entregar? | SÍ / NO |
| ¿El Objetivo del OKR refleja el problema definido en Fase P? | SÍ / NO |
| ¿Los KRs tienen valores actuales concretos (no estimados)? | SÍ / NO |
| ¿Todos los integrantes entienden cada sección de este canvas? | SÍ / NO |

> **Si alguna respuesta es NO → el canvas no está listo para entregar.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 3 de 4*
