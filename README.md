# Demo - Webhook Transformación Respuesta (n8n)

Workflow de demostración creado en [n8n](https://n8n.io/) que ilustra un flujo sencillo de automatización:

1. **Webhook**: recibe una petición `POST` en `/demo-flow` con un `nombre` en el body.
2. **Transformar datos**: genera varios campos derivados a partir del dato recibido:
   - `nombre_recibido`: el nombre tal cual (o "Invitado" si no se envía).
   - `mensaje`: un mensaje de confirmación personalizado.
   - `nombre_mayusculas`: el nombre en mayúsculas.
   - `fecha_procesado`: fecha/hora de procesamiento en formato ISO.
   - `estado`: marca la operación como `ok`.
3. **Responder al Webhook**: devuelve el JSON resultante como respuesta.

## Archivo

- [`demo-workflow-n8n.json`](./demo-workflow-n8n.json): exportación del workflow, lista para importar directamente en una instancia de n8n (*Import from File*).

## Cómo probarlo

1. Importar `demo-workflow-n8n.json` en n8n.
2. Activar el workflow.
3. Enviar una petición `POST` al webhook con un JSON como:

   ```json
   { "nombre": "Ruben" }
   ```

4. La respuesta incluirá el mensaje personalizado y los campos transformados.
