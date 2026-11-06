# Aplica con tu crónica · Perpetuo

Página para que las personas apliquen con ideas de crónica para El estelar. Pensada para `cronicas.perpetuo.global` (ver `CNAME`).

## Archivos
- `index.html`: toda la página (HTML, CSS y JS en un solo archivo, sin paso de compilación). Misma base visual que Escribe (azul con hoja de papel), con animación: el titular entra palabra por palabra, la Exclamación cruza la portada, la hoja sube, los bloques aparecen al hacer scroll, la línea roja de la cabeza avanza con lo que se llena, palomitas en cada campo bien lleno, un personaje se asoma junto a la parte que estás llenando (solo en pantallas anchas) y confeti al enviar. Todo se apaga con «reducir movimiento».
- `assets/`: logo, personajes y stickers de `videos/brand/stickers` (lápiz, lentes, lupa, corre, piensa) y `partes/` con la Exclamación por piezas.

## Contenido
- **Portada:** solo el titular «Convocatoria de Crónicas» y cuatro datos (250 USD por texto, principios de 2027, cualquier país, una idea y textos tuyos). La Exclamación está armada por piezas (`assets/partes`, las mismas de eventos): entra corriendo, frena, salta, luego respira, saluda cada tanto y brinca al darle clic.
- **01 Tú:** nombre completo, país de residencia, correo, WhatsApp (opcional) y «¿Ya estás suscrito a Perpetuo?» (si dice que no, aparece el enlace a perpetuo.substack.com).
- **02 La idea:** de qué va (100 palabras o menos; la página no deja enviar más), por qué tiene que contarse ahora y qué recordará la humanidad de la historia en cien años.
- **03 Lo que has escrito:** enlaces a textos suyos o un texto adjunto (uno de los dos es obligatorio) y, opcional, el borrador de la crónica (archivo o enlace). Archivos de hasta 8 MB.
- **Preguntas frecuentes y crónicas de ejemplo** (El palacio negro, Noticias de otro gran secuestro, Añelo, El sueño del Suri).

El borrador del formulario se guarda en `localStorage` (sin archivos) y se borra cuando el envío sale bien.

## Configuración
```js
const CONFIG = { WEBHOOK_URL: "https://esperpetuo.app.n8n.cloud/webhook/cronicas", MAX_PALABRAS: 100, MAX_MB: 8, ... };
```
Con `WEBHOOK_URL` vacío funciona en **modo demostración**.

Se envía un `POST` `multipart/form-data` con: `nombre, pais, correo, whatsapp, suscrito (si/no), idea, porQueAhora, cienAnos, textos, borradorEnlace, folio, fecha` y, si los hay, los archivos `muestra` y `borrador`.
Si n8n no responde o falla, la página le ofrece a la persona mandar sus respuestas por correo a general@perpetuo.global (el `mailto:` ya lleva todo escrito y le pide adjuntar sus archivos).

## Backend (n8n)
Workflow **«Crónicas · Aplicaciones (webhook → Notion + #estelares)»** (`aWXkjBcLMHiX3Riu`):
1. Valida (nombre, correo, país, las tres respuestas y al menos un ejemplo de escritura) y responde 400 si falta algo.
2. Si trae archivos, los sube a la carpeta de Drive «Escribe con nosotros · Archivos» con el nombre `Crónica · folio · nombre · Texto previo/Borrador · archivo`.
3. Crea una ficha en **🛄 Propuestas de Crónicas** (Notion, dentro de Estelares): Autor, Correo, País, WhatsApp, ¿Suscrito?, La idea, ¿Por qué ahora?, En cien años, Textos previos, Muestra adjunta, Borrador, Título/ Tema (inicio de la idea), Estatus = Sin contactar, Estelar o Carta? = Crónica, Origen = Formulario web, Fecha de recepción. Las respuestas completas también van en el cuerpo de la página.
4. Responde 200 `{ok, folio}` y avisa en **#estelares** mencionando a Alonso y JL. Si Notion falla, responde 500.

CORS permitido desde `https://cronicas.perpetuo.global`, `https://escribe.perpetuo.global`, `https://perpetuo.global`, `https://www.perpetuo.global` y `http://localhost:8768`.

La integración de Notion «n8n - perpetuo» necesita acceso a la base 🛄 Propuestas de Crónicas (••• → Conexiones).
