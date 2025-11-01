# Chatbot — Notas de Inspección (IA-Pymes)

Instrucciones rápidas:

1. Crear un webhook en Make.com:
   - Entra a Make -> Create a new Scenario -> añade el módulo "Webhooks" -> "Custom webhook".
   - Crea un nuevo webhook y copia la URL proporcionada (será algo como https://hook.integromat.com/xxxxx).

2. Configurar la página:
   - Abre `IA-Pymes/index.html` y reemplaza en la etiqueta <body> el atributo `data-webhook-url="REEMPLAZA_POR_TU_WEBHOOK_DE_MAKE"`
     por la URL que copiaste de Make.
   - (Opcional) añade un token en `data-webhook-token` y crea un filtro en Make que verifique ese header `X-Webhook-Token`.

3. Flujo recomendado en Make:
   - Webhook (recibe multipart/form-data).
   - Módulo "Parse JSON" (si enviabas JSON) o usar directamente los archivos desde el body.
   - Procesa/almacena: guarda en Google Sheets / Airtable / Base de datos, o pasa al "agente" (otro HTTP/AI).
   - Si tu agente espera JSON, en Make crea un cuerpo JSON y realiza un HTTP request al endpoint del agente con los campos:
     {
       "text": "...",
       "timestamp": "...",
       "location": {"lat":..., "lon":..., "accuracy_m":...},
       "files": [ ... ] // Make detecta archivos del multipart/form-data
     }

4. Prueba:
   - Abre tu sitio (p. ej. https://achamizofotex.github.io/IA/ o donde lo tengas publicado),
   - Escribe una nota, añade foto y/o presiona 📍 para capturar ubicación,
   - Pulsa Enviar. En Make verás la recepción del webhook.

Notas de seguridad:
- No pongas la URL del webhook públicamente si quieres control de acceso. Usa el header `X-Webhook-Token` (igual en el atributo data-webhook-token) y en Make añade un filtro que verifique el header.
- Si quieres firmar payloads, añade HMAC en JS antes de enviar y valida en Make.