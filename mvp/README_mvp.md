# SafeLight — MVP

## Descripción

SafeLight es un asistente de reconocimiento de semáforos diseñado para apoyar a conductores con daltonismo rojo-verde (deuteranopía y protanopía).

El sistema analiza una imagen capturada desde la cámara o subida por el usuario, identifica automáticamente el estado del semáforo (rojo, amarillo o verde) y comunica el resultado mediante una alerta de voz en español. Además, el estado siempre se muestra como texto en pantalla para evitar depender únicamente del color.

Como mecanismo de seguridad, el MVP implementa un enfoque **fail-safe**: cuando el sistema no tiene suficiente confianza en la predicción, no genera ninguna alerta, evitando entregar información potencialmente incorrecta.

---

## URL de despliegue

https://huggingface.co/spaces/Gonzagl5/safelight

> En el plan gratuito de Hugging Face el Space puede tardar entre 20 y 60 segundos en iniciar después de un periodo de inactividad. Esto es un comportamiento esperado.

---

## Funcionalidades

- Reconocimiento de semáforos mediante visión por computador.
- Detección de los estados rojo, amarillo y verde.
- Alerta de voz en español.
- Visualización del resultado en texto.
- Modo cámara en vivo.
- Modo carga de fotografías.
- Mecanismo **fail-safe** cuando la confianza es insuficiente.
- Panel técnico con información de la clasificación y latencia.

---

## Instrucciones de uso

1. Abrir la URL del MVP.
2. Esperar a que cargue la interfaz de Gradio.
3. Elegir uno de los dos modos disponibles:
   - 📷 **En vivo:** utiliza la cámara del dispositivo.
   - 🖼️ **Foto:** permite subir una imagen.
4. Si se utiliza el modo **En vivo**, aceptar el permiso de cámara solicitado por el navegador.
5. Apuntar la cámara hacia un semáforo o cargar una fotografía donde el semáforo sea claramente visible.
6. Esperar unos segundos mientras el sistema procesa la imagen.
7. Revisar el resultado mostrado en pantalla y escuchar la alerta de voz correspondiente.

---

## Escenario recomendado para la evaluación

Se recomienda probar el sistema utilizando fotografías claras de semáforos en los tres estados (rojo, amarillo y verde).

También puede utilizarse la cámara en vivo cuando exista un semáforo disponible, permitiendo observar el funcionamiento continuo del sistema.

---

## Resultados obtenidos

- **F1-score macro:** 86.4%
- **Accuracy:** 94.9%
- **Latencia de inferencia:** 60–400 ms

El mecanismo **fail-safe** funciona como fue diseñado: ante baja confianza, el sistema permanece en silencio.

---

## Limitaciones conocidas

- No fue entrenado con imágenes reales de semáforos de Lima.
- La clase **amarillo** posee menos ejemplos de entrenamiento.
- El reconocimiento puede disminuir cuando existen malas condiciones de iluminación o movimiento excesivo de la cámara.
- El silencio del mecanismo **fail-safe** puede interpretarse como que la aplicación dejó de funcionar si no existe un indicador visual de **"Escaneando..."**.
- El usuario debe aceptar manualmente el permiso de cámara otorgado por el navegador.

---

## Mejoras futuras

- Incorporar un indicador visual permanente de **"Escaneando..."**.
- Mejorar el flujo de permisos de cámara.
- Entrenar el modelo con imágenes reales de semáforos de Lima.
- Incrementar el conjunto de entrenamiento para la clase amarillo.
- Validar el sistema con una muestra mayor de usuarios.

---

## Evidencias

- `pruebas_usuario.md`
- `resultados_okr.md`

---

## Equipo

### Integrantes

- Sebastián Sánchez
- Gonzalo Gaviño
- Giuseppe Del Negro

### Curso

AD5018 — Inteligencia Artificial para Negocios

### Sección

*(Completar)*

### Fecha

*(Completar con la fecha de sustentación)*
