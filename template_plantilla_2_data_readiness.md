# Plantilla 2 — Data Readiness Checklist
## Framework PROMPT | Fase R — Recursos de Datos
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

**Equipo:**
- Integrante 1: _______________________________________________
- Integrante 2: _______________________________________________
- Integrante 3: _______________________________________________

**Fecha de entrega:** _______________
**Tipo de IA del proyecto:** _______________

---

> **Instrucción general:**
> Completa esta plantilla SOLO después de haber verificado la existencia y calidad de los datos — no antes. Cada semáforo debe estar respaldado por evidencia concreta. Un checklist optimista sin evidencia no tiene valor.

---

## SECCIÓN 1 — Inventario de datos

> *Lista todos los conjuntos de datos o fuentes de información que el proyecto necesita para funcionar.*

### Para IA Generativa — Inventario de conocimiento

| # | Tipo de información | Dónde está actualmente | Formato | ¿Está disponible? |
|---|---|---|---|---|
| 1 | | | | SÍ / NO / PARCIAL |
| 2 | | | | SÍ / NO / PARCIAL |
| 3 | | | | SÍ / NO / PARCIAL |
| 4 | | | | SÍ / NO / PARCIAL |
| 5 | | | | SÍ / NO / PARCIAL |

**Estrategia de contexto elegida:**
- [ ] Prompt simple *(base de conocimiento pequeña y estable)*
- [ ] RAG — Retrieval Augmented Generation *(base de conocimiento grande o cambiante)*
- [ ] Memoria de sesión *(asistente conversacional con contexto acumulativo)*

---

### Para ML Tradicional — Inventario de datos históricos

| # | Dataset | Fuente | Formato | N° de registros aprox. | ¿Tiene etiquetas? |
|---|---|---|---|---|---|
| 1 | | | | | SÍ / NO |
| 2 | | | | | SÍ / NO |
| 3 | | | | | SÍ / NO |

**Variable objetivo (target):**
> *¿Qué es exactamente lo que el modelo debe predecir o clasificar?*

```
Escribe aquí:


```

**Tipo de problema confirmado:**
- [ ] Clasificación — predice una categoría (SÍ/NO, A/B/C)
- [ ] Regresión — predice un número continuo
- [ ] Clustering — agrupa sin etiquetas previas

---

### Para Combinación — completa ambas secciones anteriores y describe la conexión:

```
¿Cómo fluyen los datos entre el componente ML y el componente GenAI?




```

---

## SECCIÓN 2 — Evaluación de calidad con semáforo

> *Instrucciones para el semáforo:*
> 🟢 **Verde** = listo, sin acciones pendientes
> 🟡 **Amarillo** = necesita trabajo, hay un plan concreto
> 🔴 **Rojo** = bloqueante, requiere replantear antes de continuar

### Dataset / Fuente principal: _______________

| Dimensión | Semáforo | Evidencia que respalda la evaluación | Plan de acción (si es 🟡 o 🔴) |
|---|---|---|---|
| **Disponibilidad** — ¿Los datos existen y son accesibles? | 🟢/🟡/🔴 | | |
| **Volumen** — ¿Hay suficientes datos para el tipo de IA elegido? | 🟢/🟡/🔴 | | |
| **Calidad** — ¿Los datos están completos y son consistentes? | 🟢/🟡/🔴 | | |
| **Relevancia** — ¿Los datos representan el problema definido en Fase P? | 🟢/🟡/🔴 | | |
| **Legalidad** — ¿Hay autorización para usar estos datos? | 🟢/🟡/🔴 | | |

---

### Dataset / Fuente secundaria *(si aplica)*: _______________

| Dimensión | Semáforo | Evidencia | Plan de acción |
|---|---|---|---|
| **Disponibilidad** | 🟢/🟡/🔴 | | |
| **Volumen** | 🟢/🟡/🔴 | | |
| **Calidad** | 🟢/🟡/🔴 | | |
| **Relevancia** | 🟢/🟡/🔴 | | |
| **Legalidad** | 🟢/🟡/🔴 | | |

---

## SECCIÓN 3 — Plan de resolución de bloqueantes

> *Por cada semáforo 🔴 en cualquier dimensión, el equipo debe completar este plan. Si no hay ningún 🔴, escribir "No aplica".*

### Bloqueante 1
```
Dimensión afectada:
Descripción del problema:
Acción concreta para resolverlo:
Responsable dentro del equipo:
Fecha límite de resolución:
¿Qué pasa si no se resuelve? (Plan B):
```

### Bloqueante 2 *(si aplica)*
```
Dimensión afectada:
Descripción del problema:
Acción concreta para resolverlo:
Responsable dentro del equipo:
Fecha límite de resolución:
¿Qué pasa si no se resuelve? (Plan B):
```

---

## SECCIÓN 4 — Privacidad y legalidad de los datos

> *Responde cada pregunta con SÍ, NO o N/A.*

| Pregunta | Respuesta | Detalle |
|---|---|---|
| ¿Los datos contienen información personal de usuarios? | | |
| ¿Se cuenta con consentimiento explícito para usar esos datos? | | |
| ¿Los datos serán anonimizados antes de usarlos en el proyecto? | | |
| ¿Aplica la Ley N° 29733 de Protección de Datos Personales del Perú? | | |
| ¿Hay alguna restricción contractual o de confidencialidad? | | |

---

## SECCIÓN 5 — Autoevaluación del equipo

| Pregunta de control | Respuesta |
|---|---|
| ¿Cada semáforo tiene evidencia concreta que lo respalda? | SÍ / NO |
| ¿Todos los 🔴 tienen un plan de acción con fecha y responsable? | SÍ / NO |
| ¿El equipo verificó el acceso real a los datos antes de completar este checklist? | SÍ / NO |
| ¿La estrategia de contexto o tipo de ML es coherente con los datos disponibles? | SÍ / NO |

> **Si alguna respuesta es NO → el checklist no está listo para entregar.**

---

*Framework PROMPT v1.0 — AD5018 UTEC | Plantilla 2 de 4*
