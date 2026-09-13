# Portfolio de Automatización y Desarrollo Web

Colección de proyectos para candidatura: workflows de automatización creados en [n8n](https://n8n.io/) y webs desarrolladas con [Base44](https://base44.com/).

## Workflows de n8n

Procesamiento de datos vía webhook, consulta a APIs externas, e integración de un agente de IA con memoria y herramientas.

### 1. Webhook Transformación Respuesta

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

### 2. Clima en Vivo

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

### 3. Bot de Atención al Cliente (Telegram)

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

### 4. Digest Diario de Noticias con IA

Automatización proactiva (no espera a que la llames): cada mañana revisa las noticias, una IA elige y resume lo más relevante, y te lo manda por Telegram sin que tengas que pedirlo.

1. **Programación Diaria**: se dispara sola todos los días a las 8:00.
2. **Leer Noticias**: descarga los titulares de un feed RSS de noticias.
3. **Limitar a las 8 más recientes**: se queda con los últimos 8 titulares para no saturar a la IA.
4. **Preparar Texto para IA**: junta título, enlace y resumen de cada noticia en un solo texto.
5. **Agente Resumen de Noticias**: un agente de IA (GPT-4.1-mini) elige las 4-5 noticias más interesantes y redacta un resumen ameno con emojis y enlaces.
6. **Enviar Digest por Telegram**: entrega el resumen final por Telegram.

**Archivo:** [`demo-digest-noticias-ia.json`](./demo-digest-noticias-ia.json)

**Cómo probarlo:**
1. Importar el archivo en n8n, añadir tu credencial de **OpenAI API** en "Modelo GPT" y tu credencial de **Telegram Bot API** en "Enviar Digest por Telegram".
2. Sustituir `REPLACE_WITH_YOUR_CHAT_ID` por tu chat de Telegram (el ID al que quieres recibir el digest).
3. Activar el workflow, o ejecutarlo manualmente una vez para probarlo sin esperar a las 8:00.
4. Revisa tu Telegram: deberías recibir un resumen de las noticias del día generado por IA.

*Usa un feed RSS público (sin API key) como fuente de noticias, así que solo necesitas las credenciales de Telegram y OpenAI que ya tienes configuradas para el bot de atención al cliente.*

---

Cada archivo `.json` es una exportación directa de n8n, lista para importar (*Import from File*) en cualquier instancia de n8n.

## Webs desarrolladas con Base44

### Estudio Ruben
Portafolio profesional de servicios de diseño visual y desarrollo de aplicaciones web, orientado a captar clientes que buscan proyectos de alta gama.

🔗 https://webs-ruben.base44.app

### Aser Alba Jardinería Profesional
Web de negocio de jardinería con catálogo de servicios (diseño de jardines, mantenimiento, riego automático, poda, jardines zen y paisajismo), sección de proyectos realizados y formulario de contacto/presupuesto.

🔗 https://aser-alba-jardin.base44.app

### Aurum Motors
Plataforma de venta de vehículos de ocasión premium, dirigida a compradores interesados en coches de marcas de lujo de segunda mano.

🔗 https://aurummotor.base44.app

### Academy
Campus de formación online para capacitación y educación a distancia.

🔗 https://academias.base44.app

### Laboratorio MacDental
Web de laboratorio dental especializado en prótesis CAD/CAM para clínicas odontológicas.

🔗 https://macdental.base44.app

### Orderly
Plataforma de optimización digital y gestión operativa para pequeños negocios, con panel de administración de leads, contenido y configuración.

🔗 https://orderly-flow-ops.base44.app

### Radical RPM Moto Parts
Tienda online de repuestos, accesorios de personalización y productos de mantenimiento para motocicletas.

🔗 https://radical-moto-flow.base44.app

### FAM Formación
Aula inteligente basada en IA para la preparación integral de oposiciones a Examinador de Tráfico en España, con tests, simulacros, preparador con IA, analíticas y planificador de estudio.

🔗 https://fam-pro-hub.base44.app

### Turfmaster
Plataforma de comercialización de césped artificial profesional, dirigida a empresas y profesionales del sector deportivo, paisajismo y construcción.

🔗 https://quick-mindful-task-snap.base44.app
