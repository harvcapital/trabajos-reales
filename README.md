# Demos de Automatización con n8n

Colección de workflows de demostración creados en [n8n](https://n8n.io/) para mostrar distintos casos de uso de automatización: procesamiento de datos vía webhook, consulta a APIs externas, e integración de un agente de IA con memoria y herramientas.

## 1. Webhook Transformación Respuesta

Flujo sencillo que recibe datos por webhook y devuelve una respuesta transformada.

1. **Webhook**: recibe una petición `POST` en `/demo-flow` con un `nombre` en el body.
2. **Transformar datos**: genera varios campos derivados a partir del dato recibido:
   - `nombre_recibido`: el nombre tal cual (o "Invitado" si no se envía).
   - `mensaje`: un mensaje de confirmación personalizado.
   - `nombre_mayusculas`: el nombre en mayúsculas.
   - `fecha_procesado`: fecha/hora de procesamiento en formato ISO.
   - `estado`: marca la operación como `ok`.
3. **Responder al Webhook**: devuelve el JSON resultante como respuesta.

**Archivo:** [`demo-workflow-n8n.json`](./demo-workflow-n8n.json)

**Cómo probarlo:**
1. Importar el archivo en n8n y activar el workflow.
2. Enviar un `POST` al webhook con `{ "nombre": "Ruben" }`.
3. La respuesta incluye el mensaje personalizado y los campos transformados.

## 2. Clima en Vivo

Consulta el clima actual de cualquier ciudad combinando dos APIs públicas.

1. **Webhook**: recibe un `POST` en `/clima-demo` con una `ciudad` en el body.
2. **Buscar Ciudad**: geocodifica el nombre de la ciudad (API de [Open-Meteo](https://open-meteo.com/)).
3. **¿Ciudad encontrada?**: si no hay resultados, responde con un error 404.
4. **Consultar Clima**: obtiene temperatura y viento actuales para las coordenadas encontradas.
5. **Formatear Respuesta**: construye un mensaje legible con ciudad, país, temperatura y viento.
6. **Responder Éxito / Responder Error**: devuelve el resultado en JSON.

**Archivo:** [`demo-clima-en-vivo.json`](./demo-clima-en-vivo.json)

**Cómo probarlo:**
1. Importar el archivo en n8n y activar el workflow.
2. Enviar un `POST` al webhook con `{ "ciudad": "Madrid" }`.
3. La respuesta incluye la temperatura y el viento actuales de esa ciudad.

*No requiere credenciales: las APIs de Open-Meteo son públicas y gratuitas.*

## 3. Bot de Atención al Cliente (Telegram)

Agente de IA conectado a Telegram que responde dudas de atención al cliente, con memoria de conversación, soporte de notas de voz y escalado a un agente humano.

1. **Telegram Trigger**: escucha mensajes entrantes (texto o nota de voz).
2. **¿Es nota de voz?**: si es audio, lo descarga y transcribe con Whisper (OpenAI) antes de continuar.
3. **Normalizar Entrada**: unifica el texto del usuario venga de mensaje o de transcripción.
4. **Agente Soporte TechZone**: agente de IA (GPT-4.1-mini) con un *system prompt* que define el negocio ficticio (envíos, devoluciones, garantía, horarios, pagos) y responde con memoria por chat.
5. **Notificar a Humano** (herramienta del agente): si detecta una queja seria o una petición explícita, avisa a un agente humano por Telegram con un resumen de la conversación.
6. **¿Responder con voz?**: si el usuario escribió por voz, genera la respuesta también en audio; si no, responde en texto.

**Archivo:** [`demo-bot-atencion-cliente.json`](./demo-bot-atencion-cliente.json)

**Requisitos para probarlo:**
- Credenciales propias de **Telegram Bot API** y **OpenAI API** (los IDs del export original se han sustituido por `REPLACE_WITH_YOUR_CREDENTIAL_ID`/`REPLACE_WITH_HUMAN_AGENT_CHAT_ID`).
- Sustituir el `chatId` del nodo "Notificar a Humano" por el chat de Telegram del agente humano que deba recibir los avisos.

---

Cada archivo `.json` es una exportación directa de n8n, lista para importar (*Import from File*) en cualquier instancia de n8n.
