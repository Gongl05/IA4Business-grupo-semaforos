# Plantilla 1 — Problem Statement Canvas
## Framework PROMPT | Fase P — Problema de Negocio
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: Sebastian Sanchez
- Integrante 2: Gonzalo Gaviño
- Integrante 3: Giuseppe Del Negro

**Fecha de entrega:** 30 de junio de 2026
**Versión del canvas:** v3

---

## SECCIÓN 1 — Definición del problema

### 1.1 Usuario afectado
> *¿Quién sufre el problema? Sé específico: no "las empresas" sino "el jefe de ventas de una pyme retail".*

```
Conductores adultos con daltonismo rojo-verde (deuteranopía o protanopía) que operan vehículos privados en entornos urbanos de Lima Metropolitana y deben interpretar el estado de los semáforos convencionales en tiempo real para circular con seguridad y dentro del marco legal de tránsito.

```

---

### 1.2 Problema específico
> *¿Qué está pasando exactamente? Describe la situación actual sin mencionar soluciones.*

```
Los conductores con daltonismo rojo-verde tienen dificultad para identificar el estado del semáforo solo por el color. En Lima, esto se agrava por factores como el sol directo, la garúa, halos de luz LED, congestión, semáforos horizontales o mala visibilidad. Por ello, suelen guiarse por la posición de la luz o el comportamiento del tráfico, pero estas estrategias pueden fallar en situaciones de poca visibilidad o decisiones rápidas.


```

---

### 1.3 Causa raíz
> *¿Por qué ocurre el problema? No el síntoma — la causa real.*

```
La normativa peruana de diseño semafórico no exige criterios de accesibilidad para personas con deficiencia en la percepción del color. El MTC mantiene el color como único código de señalización, sin obligar el uso de formas, símbolos o patrones. Esto traslada el riesgo de interpretación al conductor afectado.

```

---

### 1.4 Consecuencia medible
> *¿Qué pierde el usuario sin solución? Expresa en números cuando sea posible (tiempo, dinero, clientes, errores).*

```
En Lima Metropolitana y Callao habría aproximadamente 200,000 conductores con daltonismo rojo-verde, quienes pueden tardar entre 0.5 y 1.2 segundos más en identificar un semáforo. A 60 km/h, ese retraso implica avanzar entre 8 y 20 metros sin reacción efectiva. Esto aumenta el riesgo de siniestros, especialmente considerando que el incumplimiento de señales es una de las principales causas de accidentes viales.

```

---

### 1.5 Declaración del problema — formato obligatorio
> *Completa la siguiente frase. Esta declaración debe sintetizar las 4 secciones anteriores en una sola oración.*

**"[Tipo de usuario] tiene dificultad para [acción específica] porque [causa raíz], lo que genera [consecuencia medible]."**

```
"Los conductores adultos con daltonismo rojo-verde tienen dificultad para identificar de forma autónoma y en tiempo real el estado de los semáforos convencionales durante la conducción porque la normativa de diseño semafórico peruana no exige criterios de accesibilidad cromática, lo que genera un retraso de reacción de hasta 1.2 segundos ante señales críticas, incrementando el riesgo de infracciones involuntarias, colisiones y pérdida de autonomía de conducción segura para los aproximadamente 200,000 conductores afectados en Lima Metropolitana."

```

---

## SECCIÓN 2 — Filtro de validación IA

> *Responde cada pregunta con SÍ o NO y una justificación breve. Sé honesto — una respuesta incorrecta aquí arruina todo lo que sigue.*

| Pregunta | SÍ / NO | Justificación (1 línea) |
|---|---|---|
| ¿Una hoja de cálculo o formulario resuelve esto? | NO | El problema requiere clasificación de imagen en tiempo real desde una cámara vehicular bajo condiciones variables; ningún formulario puede procesar frames de video ni emitir alertas accionables en milisegundos |
| ¿El problema escala con volumen de datos o usuarios? | SÍ | Mayor diversidad de condiciones lumínicas, tipos de semáforos, distritos y variantes de infraestructura urbana exige más datos de entrenamiento para que el modelo generalice correctamente |
| ¿Hay un patrón repetitivo difícil de procesar manualmente? | SÍ | La detección del estado del semáforo desde frames de video es continua, sensible al contexto visual y completamente inabordable por un humano en tiempo real durante la conducción |
| ¿El problema requiere generar contenido o razonar en lenguaje natural? | NO | La salida al usuario es una etiqueta predefinida convertida en alerta de voz mediante síntesis; no requiere generación libre ni razonamiento lingüístico abierto |
| ¿Necesitas predecir Y luego explicar o actuar sobre el resultado? | SÍ | El sistema clasifica el estado del semáforo y esa clasificación activa inmediatamente una alerta de voz accionable para el conductor, sin intervención humana intermedia |


**Conclusión del filtro:**
> *¿Por qué la IA es la respuesta correcta y no otra solución más simple?*

```
Una solución de IA es necesaria porque el problema implica visión computacional en tiempo real (detectar y clasificar el estado del semáforo desde cámara), combinado con generación de una respuesta comprensible para el usuario. Ninguna solución manual, formulario o regla fija puede adaptarse a las variaciones de luz, ángulo y contexto urbano que enfrenta el usuario en cada cruce.

```

---

## SECCIÓN 3 — Tipo de IA elegido

Marca con una X la opción elegida:

- [ ] **IA Generativa** — el problema involucra lenguaje natural, conversación o generación de contenido
- [X] **ML Tradicional** — el problema requiere predecir, clasificar o segmentar con datos históricos
- [ ] **Combinación** — se necesita predecir Y comunicar/actuar sobre el resultado

### Justificación de la elección
> *¿Por qué este tipo de IA y no los otros dos? Argumenta en función del problema, no de la preferencia del equipo.*

```
El problema requiere detectar y clasificar el estado del semáforo desde frames
de video en tiempo real: tarea de clasificación supervisada con visión
computacional (CNN). La comunicación del resultado al conductor mediante síntesis
de voz (Web Speech API) es lógica de aplicación determinista sobre una de tres
etiquetas fijas (rojo / amarillo / verde) — no constituye un componente de IA
Generativa. Un LLM añadiría latencia, variabilidad y costo innecesarios cuando
la salida siempre es una de tres frases predefinidas. La IA Generativa no aplica
porque el problema no involucra lenguaje natural abierto, conversación ni
generación de contenido.
```

### Si elegiste ML Tradicional — especifica el tipo:

- [X] Supervisado — Clasificación *(ej: ¿este cliente se irá? ¿este email es spam?)*
- [ ] Supervisado — Regresión *(ej: ¿cuánto venderemos este mes?)*
- [ ] No Supervisado — Clustering *(ej: ¿qué segmentos de clientes tenemos?)*

### Si elegiste Combinación — describe el flujo:

```
No aplica. El sistema usa únicamente ML Tradicional — Clasificación Supervisada.
La síntesis de voz (Web Speech API) es lógica de aplicación determinista, no un
componente de IA adicional.
```

---

## SECCIÓN 4 — Autoevaluación del equipo

> *Antes de entregar, el equipo responde estas preguntas internamente.*

| Pregunta de control | Respuesta |
|---|---|
| ¿El problema está descrito sin mencionar tecnología? | SÍ |
| ¿La declaración del problema sigue el formato exacto? | SÍ |
| ¿La elección del tipo de IA se justifica con el problema, no con la preferencia? | SÍ |
| ¿Todos los integrantes pueden explicar este canvas sin leerlo? | SÍ |

> **Si alguna respuesta es NO → el canvas no está listo para entregar.**

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 1 de 4*
