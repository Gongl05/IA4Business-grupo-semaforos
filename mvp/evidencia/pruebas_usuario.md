# Pruebas con usuarios

## Objetivo de la validación

Evaluar la experiencia de uso del MVP mediante pruebas con usuarios,
enfocándose principalmente en el modo de cámara en vivo, que representa
el escenario previsto para el producto final. También se evaluó el modo
**Foto** cuando no existía un semáforo disponible para realizar la
prueba.

Se evaluó la facilidad para activar y usar la cámara, el tiempo hasta
obtener una alerta correcta, la comprensión del mecanismo **fail-safe**
(silencio ante baja confianza) y el comportamiento del sistema frente a
condiciones reales como iluminación, distancia y estabilidad al sostener
el celular.

## Metodología

Se compartió la URL pública del MVP. Se pidió a cada participante
utilizar la aplicación sin recibir instrucciones detalladas sobre su
funcionamiento. El evaluador observó el comportamiento del participante
sin intervenir, registrando únicamente las dificultades y comentarios
surgidos durante la prueba.

Si durante la prueba no había un semáforo real disponible, el
participante utilizó la pestaña **Foto** como alternativa. Ambos modos
fueron considerados válidos para la evaluación del MVP.

## Escenarios evaluados

1. Activar la pestaña **En vivo** y otorgar permiso de cámara.
2. Apuntar la cámara a un semáforo en rojo y esperar la alerta de voz.
3. Apuntar a un semáforo en verde y verificar que la alerta cambie correctamente.
4. Cuando no existía un semáforo disponible, utilizar la pestaña **Foto** cargando una imagen.

## Registro de participantes

| Participante | Perfil | Modo usado | Escenario | Resultado | Dificultades | Comentario | Evidencia |
|---|---|---|---|---|---|---|---|
| Participante 1 | Sin daltonismo, primera vez con la app | Cámara en vivo | Escaneó un semáforo real en rojo y verde | Recibió correctamente la alerta de voz en ambos casos, con un tiempo aproximado de 1–2 segundos | Dudó inicialmente si debía tomar una fotografía o simplemente esperar | "No sabía que se actualizaba sola, pensé que tenía que hacer algo." | Realizado |
| Participante 2 | Daltonismo rojo-verde (perfil objetivo) | Cámara en vivo | Escaneó un semáforo real, incluyendo condiciones de baja iluminación | En condiciones normales funcionó correctamente; con poca iluminación el sistema permaneció en silencio varias veces, activando el comportamiento fail-safe | No distinguía si el silencio significaba que el sistema seguía analizando o que había dejado de funcionar | "Hubiera querido un mensaje más claro de que sigue funcionando, no que se colgó." | Realizado |
| Participante 3 | Sin experiencia técnica | Cámara en vivo | Escaneó un semáforo desde un vehículo como pasajero | Obtuvo una lectura correcta luego de estabilizar el celular | El permiso de cámara solicitado por el navegador generó dudas durante el primer uso | "Casi cierro la página pensando que no cargaba." | Realizado |
| Participante 4 | Sin daltonismo | Foto | No había un semáforo disponible durante la sesión de prueba | Subió una fotografía de un semáforo y obtuvo una clasificación correcta | No presentó dificultades relevantes | "Más fácil que la cámara, porque no tenía un semáforo real cerca." | Realizado |

## Hallazgos principales

1. **(3 de 4 participantes)** El modo **En vivo** no comunica claramente que continúa funcionando mientras no existe una alerta. El silencio generado por el mecanismo fail-safe puede confundirse con un error de la aplicación.

   **Acción propuesta:** incorporar un indicador visual permanente de **"Escaneando..."** mientras la cámara permanece activa.

2. El movimiento del celular influye en el tiempo necesario para obtener una lectura confiable.

   **Acción propuesta:** agregar una recomendación visible indicando que el usuario mantenga el dispositivo estable y apuntando directamente al semáforo.

3. El modo **Foto** demostró ser una alternativa útil cuando no existe un semáforo disponible para realizar la prueba.

   **Acción propuesta:** mantener ambos modos con la misma visibilidad dentro de la aplicación.

4. El permiso de cámara solicitado por el navegador generó dudas durante la primera utilización de la aplicación.

   **Acción propuesta:** incorporar una instrucción previa indicando que el navegador solicitará acceso a la cámara y que dicho permiso debe ser aceptado.

## Hallazgos adicionales

| Hallazgo | Mejora propuesta | Prioridad | Estado |
|---|---|---|---|
| El uso del celular como dispositivo principal genera limitaciones prácticas (riesgo de robo y dificultad para utilizar simultáneamente aplicaciones de navegación). | En una futura versión se propone utilizar un dispositivo dedicado instalado en el vehículo, como fue planteado originalmente en el AI Product Canvas. | Media | Documentado |
| Las pruebas realizadas permitieron validar el funcionamiento general del MVP, aunque la muestra todavía es reducida. | Ampliar la validación con un mayor número de usuarios y escenarios reales de conducción. | Alta | Pendiente |

## Conclusión

Las pruebas con usuarios permitieron verificar que el MVP cumple su
funcionalidad principal de identificar el estado del semáforo y
comunicarlo mediante una alerta de voz. Asimismo, evidenciaron
oportunidades de mejora relacionadas principalmente con la experiencia
de usuario, como la comunicación del estado de escaneo, el flujo de
permisos de cámara y la estabilidad durante el uso del modo **En vivo**.
Estos hallazgos servirán como base para una siguiente iteración del
producto.
