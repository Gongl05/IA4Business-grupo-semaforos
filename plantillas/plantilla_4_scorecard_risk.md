# Plantilla 4 — AI Impact Scorecard + AI Risk Matrix
## Framework PROMPT | Fases P2 + T — Performance y Transparencia
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** Semana 14 — sustentación PC2
**Nombre del MVP:** SafeLight

---

# PARTE A — AI IMPACT SCORECARD
## Fase P2 — Performance y Métricas

---

## SECCIÓN 1 — Evaluación de OKRs declarados en el AI Product Canvas

> Los compromisos se tomaron de la Sección 8 de la Plantilla 3. No fueron modificados.

---

### Objetivo (O) del proyecto

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
| Métrica | F1-score macro del modelo CNN en conjunto de test (promedio ponderado 4 clases) | F1-score macro: 83.7% |
| Valor actual (antes del MVP) | 0% — sistema no existía | — *(no cambia)* |
| Meta comprometida | F1-score ≥ 80% | — *(no cambia)* |
| Resultado real | — | **86.4% macro** — Accuracy: 94.9% (con advertencia: clase amarillo en F1 62.1%) |
| Método usado para medir | Reporte automático del framework de ML sobre conjunto de test separado | classification_report del framework de entrenamiento; resultados en training_results.json |
| **¿Se alcanzó la meta?** | — | **PARCIAL** — la meta agregada se cumple (108% de la meta), pero el amarillo no alcanza nivel aceptable para uso en seguridad vial |

**Análisis del KR1:**
```
El F1-score macro de 86.4% supera la meta del 80% con margen. Sin embargo, el
desagregado por clase revela una limitación real: la clase amarillo obtiene F1 de 62.1%
(precisión 45%, recall 100%), significativamente por debajo del umbral mínimo.
El factor determinante fue la subrepresentación del amarillo en el dataset Udacity y la
ausencia de fine-tuning con imágenes locales de Lima.

Adicionalmente, durante el desarrollo se detectó que la CNN confundía el color de la
carcasa del semáforo con la luz encendida; se incorporó un paso previo de localización
del foco para corregirlo.
```

---

**KR2:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | Latencia total del sistema: captura del frame → emisión de alerta de voz | Latencia de inferencia medida |
| Valor actual (antes del MVP) | N/A — sistema no existía | — *(no cambia)* |
| Meta comprometida | Latencia total (inferencia + TTS) < 500ms | — *(no cambia)* |
| Resultado real | — | **60.2 ms** (solo inferencia CNN; TTS no integrado en entorno de demo web) |
| Método usado para medir | Timestamps en el pipeline Python durante sesión de prueba controlada | time.perf_counter() antes y después de model.predict() sobre frame individual |
| **¿Se alcanzó la meta?** | — | **SÍ** — 60.2 ms supera ampliamente la meta, con nota: medición corresponde solo a inferencia |

**Análisis del KR2:**
```
La latencia de 60.2 ms es un resultado sólido que confirma la viabilidad de usar
MobileNetV2 para inferencia en tiempo real. La meta de 500 ms incluía también el módulo
TTS (pyttsx3), que no fue integrado en la demo web final. En un dispositivo fijo real,
la latencia total estimada estaría entre 80–200 ms (inferencia + síntesis offline),
aún dentro del umbral comprometido.
```

---

**KR3:**

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica | Tasa de alertas falsas en sesión de prueba de 30 min en entorno urbano | Pendiente de prueba en campo |
| Valor actual (antes del MVP) | N/A | — |
| Meta comprometida | Tasa de falsos positivos < 10% en sesión real | — |
| Resultado real | — | **0 falsas alertas** tras calibración del umbral de confianza (0.5) |
| Método usado para medir | Revisión manual de resultados durante validación iterativa del equipo | Objetos ambiguos probados en vivo: ropa, personas — todos resueltos con silencio (fail-safe) |
| **¿Se alcanzó la meta?** | — | **SÍ*** — validación iterativa por el equipo; no es sesión de campo externa |

---

### Evaluación del KPI técnico

| Campo | Declarado en Plantilla 3 | Resultado real obtenido |
|---|---|---|
| Métrica técnica | F1-score por clase + tasa de falsos positivos en campo | F1-score por clase medido en test offline |
| Criterio mínimo aceptable | F1-score ≥ 80% en todas las clases; tasa de alertas falsas < 10% | — *(no cambia)* |
| Resultado real | — | F1 rojo: 96.6% ✅ / F1 verde: 96.6% ✅ / F1 amarillo: 62.1% ❌ / Tasa alertas falsas: pendiente ⏳ |
| Método de medición | Reporte del framework de ML + log de sesión en campo | classification_report sobre conjunto de test |
| **¿Es aceptable?** | — | **PARCIAL** — rojo y verde dentro del criterio; amarillo fuera del mínimo |

---

### Resumen ejecutivo del OKR

| | KR1 | KR2 | KR3 | KPI técnico |
|---|---|---|---|---|
| **¿Alcanzado?** | PARCIAL | SÍ | SÍ* | PARCIAL |

**Conclusión del equipo:**
```
El MVP cumple su objetivo de negocio de forma parcial. Valida que la arquitectura
(captura → CNN → alerta de voz → fail-safe) es técnicamente viable: la inferencia
es rápida (60.2 ms) y el desempeño agregado supera el 80%. Sin embargo, el
desempeño desagregado revela que el amarillo no es confiable (62.1%), lo que
impide declarar el sistema "listo para producción". Este es un MVP académico que
demuestra la arquitectura y la dirección correcta del producto, no un sistema
certificado para uso vial real sin las validaciones de campo pendientes.
```

---

## SECCIÓN 2 — Evaluación técnica del modelo

### Para ML Tradicional

| Métrica | Criterio mínimo aceptable | Resultado real | ¿Aceptable? |
|---|---|---|---|
| **Accuracy** | > 80% | **94.9%** | SÍ |
| **Precision** | > 80% | ~81% macro (45% en amarillo) | PARCIAL |
| **Recall** | > 80% | ~96% macro | SÍ |
| **F1 Score** | > 0.80 macro | **0.864 macro** | SÍ (con advertencia en clase amarillo: 0.621) |
| **Baseline** *(modelo trivial)* | Clasificador por clase mayoritaria (rojo) | ~33% accuracy (3 clases) | — |

**Interpretación del equipo:**
```
Un F1 macro de 83.7% sobre el baseline del 33% (clasificador trivial) confirma que el
modelo aprende patrones reales y no memoriza la clase mayoritaria. Para un sistema de
seguridad vial, el desempeño por clase importa más que el agregado: rojo (~90%) y verde
(~88%) son los estados críticos para la conducción y ambos superan el umbral. El
amarillo (62.1%) representa la limitación principal — en contexto de seguridad vial,
un F1 bajo en amarillo significa que el sistema frecuentemente omite la advertencia de
"atención, cambio inminente", lo que puede generar exceso de confianza. El umbral de
confianza del 60% mitiga parcialmente esto al silenciar predicciones inseguras.
```

---

## SECCIÓN 3 — Evaluación con usuarios reales

> *Validación iterativa realizada por el equipo de desarrollo. Los tres integrantes probaron todas las funcionalidades durante el ciclo de construcción del MVP — el equipo actuó como primer usuario del sistema antes de cualquier despliegue externo.*

### Registro de pruebas

| # | Escenario probado | Modo | ¿Funcionó correctamente? | Observaciones clave |
|---|---|---|---|---|
| 1 | Apuntar cámara a foto de semáforo en verde con carcasa amarilla visible | Cámara en vivo | NO | El sistema devolvió "AMARILLO" — confundía el color de la carcasa con el de la luz encendida |
| 2 | Apuntar cámara a una persona con polera de color | Cámara en vivo | NO | El sistema emitió alerta de color — confundía la variación visual de la ropa con un semáforo |
| 3 | Cambiar entre cámara frontal y trasera en la interfaz | Cámara en vivo | NO | Bug conocido de Gradio (#7493): el cambio de cámara no se aplicaba de forma confiable |
| 4 | Probar fotos del dataset para validación rápida del modelo | Foto | SÍ | Modo foto permitió iterar sin depender de un semáforo físico disponible |

### Hallazgos principales de las pruebas

**Lo que funcionó bien:**
```
1. El modo foto permitió validar el modelo rápidamente durante el desarrollo.
2. El mecanismo fail-safe (silencio ante baja confianza) funcionó como fue diseñado:
   objetos ambiguos devuelven confianza < 0.5 y el sistema permanece en silencio.
3. La latencia de 380 ms resultó aceptable para el intervalo de actualización de 1.2 s.
```

**Lo que no funcionó o confundió al usuario:**
```
1. El sistema confundía la carcasa amarilla del semáforo con la luz encendida —
   causa: el dataset de entrenamiento solo contiene focos recortados, sin carcasa.
2. Objetos con variación visual (ropa, personas) activaban falsas alertas —
   causa: el modelo no tiene noción de "esto es un semáforo", solo detecta
   forma circular con contraste y color compatible.
3. El selector de cámara de Gradio no cambiaba de frontal a trasera de forma confiable.
```

**Cambios realizados al MVP como resultado de las pruebas:**
```
1. Se reemplazó el clasificador CNN puro por un sistema híbrido: detección de círculos
   (Hough Transform) para aislar el foco + clasificación por distancia a centroides HSV
   calibrados. La CNN se mantiene como segunda opinión en el panel técnico.
2. Se calibró el umbral de confianza a 0.5 con datos reales (no a ojo): predicciones
   por debajo quedan en silencio — fail-safe por omisión.
3. Se forzó la cámara trasera vía JavaScript como workaround al bug de Gradio.
```

---

## SECCIÓN 4 — Reflexión de resultados

### ¿El MVP resuelve el problema definido en la Fase P?

- [ ] **SÍ completamente** — el MVP resuelve el problema y las métricas lo demuestran
- [x] **SÍ parcialmente** — el MVP resuelve una parte del problema con limitaciones claras
- [ ] **NO todavía** — el MVP tiene el enfoque correcto pero necesita más iteraciones

**Argumento del equipo (obligatorio):**
```
El MVP demuestra que la arquitectura planteada (CNN + TTS + fail-safe) es técnicamente
viable para asistir a conductores con daltonismo en la identificación de semáforos. Los
estados críticos rojo y verde —que son la principal necesidad del usuario objetivo—
alcanzan F1 por encima del 88%, y la latencia de inferencia (60.2 ms) es adecuada para
tiempo real. La limitación principal es el amarillo (F1 = 62.1%), que no alcanza el nivel
de confiabilidad necesario para un sistema de seguridad vial. Esto se debe directamente
al bloqueante de datos locales documentado en la Plantilla 2 y no resuelto antes del
entrenamiento final. El proyecto no está listo para producción, pero sí valida la
dirección correcta del producto con evidencia cuantitativa real.
```

### Lecciones aprendidas por fase PROMPT

| Fase | ¿Qué cambiarías si empezaras de nuevo? |
|---|---|
| **P** — Problema | Acotar el MVP desde el inicio a solo 2 clases (rojo/verde) y agregar el amarillo en v2, para no asumir que los 3 estados son igualmente alcanzables con datos públicos |
| **R** — Datos | Resolver el bloqueante de datos locales en las primeras semanas, no al final. Sin imágenes de Lima, el modelo no puede generalizarse al entorno real del usuario |
| **O** — Diseño | Definir desde el canvas un criterio de éxito por clase, no solo el promedio macro. F1 macro ≥ 80% oculta que el amarillo puede estar muy por debajo |
| **M** — Construcción | Comenzar con un dataset más pequeño y bien curado antes de escalar. Dedicamos tiempo a integrar datasets que no mejoraron el desempeño del amarillo |
| **P2** — Métricas | Completar KR3 (prueba en campo) antes de la sustentación para tener el cuadro completo. Dejarlo pendiente reduce la solidez del argumento de impacto |
| **T** — Ética | Incorporar desde el diseño inicial un protocolo de comunicación al usuario sobre las limitaciones del sistema, no solo al final como ajuste post-pruebas |

### Próximos pasos si el proyecto continuara

```
1. (corto plazo — próximas 2 semanas) Capturar 300–500 imágenes de semáforos en Lima
   en condiciones variadas (mañana nublada, mediodía, noche) y hacer fine-tuning específico
   para mejorar el F1 del amarillo al menos a 75%.

2. (mediano plazo — próximos 2 meses) Completar KR3: sesión de prueba en campo con al
   menos 5 usuarios externos en un vehículo real, midiendo tasa de alertas falsas y
   satisfacción del conductor con daltonismo.

3. (largo plazo — versión 2.0 del producto) Desarrollar un dispositivo físico fijo
   (integrado al tablero o parabrisas) con TTS offline, sin interacción táctil durante
   la marcha, que cumpla con la normativa de tránsito peruana sobre dispositivos al volante.
```

---

---

# PARTE B — AI RISK MATRIX
## Fase T — Transparencia y Ética

---

## SECCIÓN 5 — Identificación de riesgos

### Riesgo 1

| Campo | Detalle |
|---|---|
| **Tipo** | Técnico |
| **Nombre del riesgo** | Modelo entrenado sin datos locales de Lima |
| **Descripción** | El modelo fue entrenado y evaluado únicamente con el dataset Udacity (EE.UU.), sin fine-tuning con imágenes locales. El bloqueante documentado en la Plantilla 2 no fue resuelto antes del entrenamiento final. |
| **Nivel** | Alto |
| **Probabilidad** | Alta |
| **Impacto si ocurre** | El modelo puede fallar sistemáticamente en condiciones específicas de Lima no representadas en el entrenamiento: "panza de burro", semáforos horizontales, contadores numéricos, infraestructura híbrida LED/incandescente, oclusión por transporte masivo. |
| **Mitigación** | No desplegar este modelo en un vehículo real bajo ninguna circunstancia. Es un prototipo de validación de arquitectura. Próximo paso obligatorio antes de cualquier uso real: capturar imágenes locales y realizar fine-tuning (bloqueante documentado en Plantilla 2, aún sin resolver). |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE — el MVP no se presenta como sistema listo para producción; su alcance está declarado explícitamente como prototipo académico |

---

### Riesgo 2

| Campo | Detalle |
|---|---|
| **Tipo** | Ético |
| **Nombre del riesgo** | Falsos negativos en amarillo pueden generar exceso de confianza |
| **Descripción** | El F1 de 62.1% en la clase amarillo significa que el sistema frecuentemente no emite alerta ante semáforos en amarillo. Un conductor que confíe ciegamente en la ausencia de alerta puede interpretar el silencio como "vía libre" y tomar peores decisiones que sin el sistema. |
| **Nivel** | Medio |
| **Probabilidad** | Media |
| **Impacto si ocurre** | Pérdida de confianza del usuario en el sistema, o peor: decisiones de conducción basadas en una alerta ausente que el usuario interpreta incorrectamente como confirmación de estado libre. |
| **Mitigación** | El diseño de fail-safe (silencio cuando la confianza es baja) mitiga parcialmente esto. Se comunica explícitamente al usuario antes de cada uso que el sistema es asistencia y no reemplaza el juicio del conductor — mensaje incluido en la interfaz ("SafeLight es asistencia, no reemplazo"). |
| **¿Está mitigado en el MVP actual?** | PARCIALMENTE — el fail-safe y el mensaje de contexto están implementados; la mejora del F1 en amarillo es la mitigación estructural pendiente |

---

### Riesgo 3

| Campo | Detalle |
|---|---|
| **Tipo** | Legal |
| **Nombre del riesgo** | El sistema podría interpretarse como dispositivo inductor de distracción al volante |
| **Descripción** | Un sistema que emite alertas sobre el estado de un semáforo durante la conducción activa podría interpretarse bajo la normativa peruana de tránsito (D.S. N° 016-2009-MTC y modificaciones) como un dispositivo electrónico que distrae al conductor, lo cual está sancionado. |
| **Nivel** | Medio |
| **Probabilidad** | Baja |
| **Impacto si ocurre** | El MVP actual es una app web de demostración (no un dispositivo físico en circulación). El riesgo legal aplica principalmente al producto final, no al prototipo académico. |
| **Mitigación** | El producto final contemplado (dispositivo fijo, sin pantalla activa durante la marcha, solo salida de audio pasiva) está diseñado para evitar esta infracción. En la documentación y la demo se aclara que el MVP web es una simulación de validación de modelo, no el producto final. |
| **¿Está mitigado en el MVP actual?** | SÍ — el MVP no opera en un vehículo en circulación; es una demo web de laboratorio |

---

### Riesgo 4 *(opcional — recomendado)*

| Campo | Detalle |
|---|---|
| **Tipo** | Privacidad |
| **Nombre del riesgo** | Captura de imágenes de personas y placas en vía pública para entrenamiento |
| **Descripción** | Las imágenes capturadas en campo para fine-tuning local pueden contener rostros de peatones y placas vehiculares identificables, lo que constituye dato personal bajo la Ley N° 29733. |
| **Nivel** | Medio |
| **Probabilidad** | Alta (si se capturan imágenes locales en el siguiente sprint) |
| **Impacto si ocurre** | Uso no autorizado de datos personales, incumplimiento de la Ley N° 29733 y su reglamento D.S. 003-2013-JUS. |
| **Mitigación** | Aplicar blur sobre rostros y placas antes de incorporar cualquier imagen al dataset de entrenamiento. Documentado en la Plantilla 2, Sección 4. |
| **¿Está mitigado en el MVP actual?** | SÍ — el MVP actual usa únicamente datasets públicos internacionales ya anonimizados por sus instituciones |

---

## SECCIÓN 6 — Marco regulatorio

### Regulación aplicable al proyecto

| Regulación | ¿Aplica? | ¿Cómo cumple el MVP con ella? |
|---|---|---|
| **Ley N° 29733** — Protección de Datos Personales (Perú) | SÍ | El MVP actual usa datasets públicos ya anonimizados. Las imágenes locales futuras se anonimizarán (blur de rostros y placas) antes del entrenamiento |
| **D.S. 003-2013-JUS** — Reglamento de la Ley 29733 | SÍ | Aplica a la captura de imágenes en vía pública con personas identificables. Mitigación: anonimización previa al uso |
| **Regulación sectorial específica** *(tránsito: D.S. N° 016-2009-MTC)* | SÍ | El MVP web no opera en vehículo en circulación. El producto final está diseñado para cumplir la prohibición de dispositivos de interacción activa al volante (solo audio pasivo, sin pantalla) |
| **EU AI Act** *(referencial)* | SÍ (referencial) | El sistema sería clasificado como IA de "alto riesgo" bajo el EU AI Act por aplicarse en contexto de seguridad vial. Esto refuerza la necesidad de validaciones en campo antes de cualquier despliegue real |

**¿El proyecto procesa datos personales de usuarios?**
- [x] No — el proyecto no maneja datos personales *(MVP actual: usa solo datasets públicos anonimizados)*
- [ ] Sí — los datos están anonimizados antes de ser usados
- [ ] Sí — contamos con consentimiento explícito de los usuarios
- [ ] Sí — estamos en proceso de obtener autorización

---

## SECCIÓN 7 — Preguntas éticas clave

**¿El usuario sabe que está interactuando con IA?**
```
SÍ — La interfaz incluye el mensaje "SafeLight es un sistema de asistencia por IA.
No reemplaza el juicio del conductor." visible antes de activar el sistema.
El nombre del producto y la naturaleza del sistema son comunicados explícitamente.
```

**¿Qué pasa si la IA comete un error que afecta al usuario?**
```
El sistema está diseñado con fail-safe por omisión: si la predicción tiene confianza
baja, no emite alerta — el silencio nunca debe interpretarse como confirmación de estado
libre. Si el modelo comete un error de alta confianza (falso positivo), no existe una
segunda barrera técnica en este MVP — esta es la limitación más importante del prototipo.
La responsabilidad sobre las decisiones de conducción recae siempre en el conductor;
el sistema es un asistente, no un reemplazante. No existe actualmente un mecanismo formal
de reporte de errores para este MVP académico.
```

**¿El producto funciona igual de bien para todos los segmentos de usuarios?**
```
El sistema fue entrenado con datos de EE.UU., no de Lima, lo que puede generar sesgos
geográficos: conduce mejor en infraestructura semafórica norteamericana que en la limeña.
No fue evaluado con conductores que tengan daltonismo real — las pruebas de usuario se
hicieron con personas sin esta condición, lo que limita la validez de los resultados
para el usuario objetivo principal. Un conductor mayor o con menor familiaridad con
tecnología podría tener dificultades adicionales con la interfaz web.
```

**¿El usuario tiene control sobre sus datos y puede solicitar su eliminación?**
```
El MVP no almacena datos del usuario: el stream de video se procesa en tiempo real y
no se guarda ningún frame ni log de sesión. No hay registro de uso, perfil de usuario
ni datos personales almacenados en el sistema. Por tanto, no aplica solicitud de
eliminación de datos para el MVP actual.
```

---

## SECCIÓN 8 — Declaración de uso de IA en el proyecto

| Herramienta de IA usada | Para qué tarea del proyecto | Fase PROMPT en la que se usó |
|---|---|---|
| Claude (Anthropic) | Redacción y revisión de plantillas, análisis de riesgos, estructuración del Problem Statement | P, R, O, T |
| ChatGPT (OpenAI) | Consultas sobre arquitecturas CNN, revisión de código Python del pipeline | M |
| Google Colab (con GPU gratuita) | Entrenamiento del modelo MobileNetV2 con transfer learning | M |
| TensorFlow / Keras | Framework de entrenamiento e inferencia del modelo de clasificación | M |
| Roboflow | Exploración de datasets y visualización de anotaciones | R |

---

## SECCIÓN 9 — Autoevaluación final del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Los KRs evaluados son exactamente los declarados en la Plantilla 3? | SÍ |
| ¿Los resultados reales tienen evidencia concreta que los respalda? | SÍ — F1 macro de 83.7% y latencia de 60.2 ms medidos con herramientas estándar |
| ¿El equipo no cambió ningún KR después de la Semana 6? | SÍ |
| ¿Hay al menos 1 riesgo técnico, 1 ético y 1 legal identificados? | SÍ |
| ¿Cada riesgo tiene una mitigación concreta y no genérica? | SÍ |
| ¿El equipo conoce la regulación aplicable a su sector? | SÍ — Ley N° 29733, D.S. 003-2013-JUS, D.S. N° 016-2009-MTC |
| ¿El MVP informa visiblemente al usuario que interactúa con IA? | SÍ — mensaje en la interfaz antes de activar el sistema |
| ¿Todos los integrantes pueden responder las preguntas éticas de la Sección 7? | SÍ |

> **Si alguna respuesta es NO → la plantilla no está lista para la sustentación.**

---

*Framework PROMPT v1.1 — AD5018 UTEC | Plantilla 4 de 4*
