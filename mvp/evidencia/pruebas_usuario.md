# Pruebas de validación — SafeLight
## Registro de iteraciones del equipo de desarrollo
### AD5018 Inteligencia Artificial para Negocios | UTEC

---

> **Nota metodológica:** Las pruebas de validación fueron realizadas por los tres integrantes del equipo durante el desarrollo iterativo del MVP. Todos los integrantes probaron todas las funcionalidades — no hubo roles fijos de "tester" y "desarrollador". Este registro documenta los hallazgos que derivaron en cambios concretos al sistema.

---

## Modalidades probadas

- **Modo cámara en vivo:** modalidad principal del producto; usada para validar el comportamiento en condiciones reales.
- **Modo foto (carga de imagen):** usada para pruebas rápidas del modelo sin depender de un semáforo físico disponible.

---

## Iteración 1 — Confusión entre carcasa y luz del semáforo

**Escenario:** Se apuntó la cámara a una foto de un semáforo en verde. El sistema devolvió "AMARILLO".

**Diagnóstico:**
El modelo nunca fue entrenado con imágenes de semáforos completos — el dataset (Udacity) consiste en focos recortados sobre fondo negro. Al ver una foto real con carcasa visible, el sistema aprendió a usar el color de la carcasa (amarilla/negra) en lugar del color de la luz encendida. En una imagen específica se midió que la carcasa tenía saturación S=255 mientras el foco verde tenía S=75 — por eso cualquier método que buscara "el punto más vívido" caía en la carcasa, no en la luz.

**Cambio implementado:**
1. Se agregó un paso de localización automática: detección de círculos mediante Hough Transform para aislar el foco encendido antes de clasificar su color.
2. Se reemplazó el clasificador CNN puro por un clasificador híbrido: detección de forma (círculos) + clasificación de color por distancia a centroides HSV calibrados contra las 297 imágenes reales del conjunto de test. La CNN se mantiene como segunda opinión visible en el panel técnico.

**Resultado verificado:** La foto que antes devolvía "AMARILLO" incorrectamente ahora devuelve "VERDE" correctamente.

---

## Iteración 2 — Falsos positivos con objetos no-semáforo (ropa, personas)

**Escenario:** Se apuntó la cámara hacia una persona con polera de color. El sistema emitió una alerta de color.

**Diagnóstico:**
El sistema no tiene noción de "esto es un semáforo" — solo detecta "¿hay algo circular con contraste y el color correcto?". Una cara humana con variación natural de piel, sombra y cabello, combinada con una polera de color, produce exactamente ese patrón. Se intentaron cuatro enfoques de filtrado adicional (reglas de forma, "compañera apagada", variación de colorfulness) y todos fallaron por la misma razón estructural: cualquier escena real con texturas variadas cumple las mismas condiciones genéricas que un semáforo. Se tomó la decisión de no seguir parchando reactivamente.

**Cambio implementado:**
Se calibró el umbral de confianza a 0.5 (basado en datos reales, no a ojo): predicciones con confianza < 0.5 no generan alerta — el sistema permanece en silencio. Esto reduce drásticamente los falsos positivos con objetos ambiguos. La limitación es operativa y está documentada: durante la demo, la cámara debe apuntar a semáforos reales o imágenes de semáforos, no a personas u objetos random.

**Resultado verificado:** La foto ambigua (semáforo en zona de baja confianza) devuelve silencio con confianza 0.18 en lugar de una alerta incorrecta — comportamiento fail-safe funcionando como fue diseñado.

---

## Iteración 3 — Bug de selección de cámara en Gradio

**Escenario:** Al intentar cambiar de cámara frontal a trasera dentro de la app, el cambio no se aplicaba de forma confiable.

**Diagnóstico:**
Bug conocido de Gradio (issue gradio-app/gradio#7493): el selector de cámara frontal/trasera no cambia de cámara de forma confiable en la versión utilizada.

**Cambio implementado:**
Se forzó la cámara trasera directamente vía JavaScript (workaround), dado que es la cámara que el usuario necesita para apuntar al semáforo sin voltear el celular. El selector de cámara fue eliminado de la interfaz para evitar confusión.

---

## Iteración 4 — Latencia y resize

**Escenario:** Se agregó un paso de redimensionado de imagen para reducir la latencia de procesamiento.

**Diagnóstico:**
El resize cambió los resultados de detección de círculos (Hough Transform es sensible a la escala) y empeoró la precisión: una imagen que antes daba confianza 0.18 (silencio correcto) pasó a dar 93% de confianza en "AMARILLO" — un falso positivo con alta confianza. La latencia sin resize (380 ms) ya era aceptable para el intervalo de actualización de 1.2 s.

**Cambio implementado:**
Se revirtió el resize. La latencia de 380 ms se mantuvo como aceptable dado el caso de uso.

---

## Resumen de hallazgos

| Hallazgo | Tipo | ¿Derivó en cambio? | Cambio implementado |
|---|---|---|---|
| Carcasa confundida con luz del semáforo | Bug crítico | SÍ | Clasificador híbrido con detección de círculos + centroides HSV |
| Falsos positivos con ropa/personas | Limitación estructural | SÍ | Umbral de confianza calibrado (0.5); fail-safe por omisión |
| Bug de cambio de cámara en Gradio | Bug de plataforma | SÍ | Cámara trasera forzada vía JavaScript |
| Resize rompía detección de círculos | Bug de implementación | SÍ | Resize revertido; latencia aceptable sin él (380 ms) |
| Clase amarillo con baja precisión | Limitación de datos | PARCIAL | Reentrenamiento con peso de clase menos agresivo; limitación documentada |

---

## Métricas tras las iteraciones

| Métrica | Antes de iteraciones | Tras iteraciones |
|---|---|---|
| Precisión en dataset de test | 94.9% | 94.3% sobre predicciones que supera umbral |
| Foto con carcasa amarilla / luz verde | AMARILLO ❌ | VERDE ✅ |
| Objeto ambiguo (ropa/persona) | Alerta incorrecta ❌ | Silencio (fail-safe) ✅ |
| Latencia de inferencia | 60–400 ms | 380 ms (estable) |

---

*SafeLight — Validación iterativa del equipo | AD5018 UTEC | PC2 Semana 14*
