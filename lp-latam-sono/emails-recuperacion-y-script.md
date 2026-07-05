# Secuencia de recuperación — 3 correos automáticos

Objetivo: quien completó el quiz y dejó su e-mail pero no compró, recibe 3 correos personalizados según el dolor que seleccionó, sin mencionar consultoría (no es parte de la oferta).

Los 3 correos usan la misma variable de personalización: el campo `problema` que ya se guarda en tu planilla (despertares / tarda / brazos / siestas).

---

## Email 1 — Día 1 (recordatorio + refuerzo de personalización)

**Asunto:** Tu diagnóstico de sueño sigue esperando por ti 🌙

**Cuerpo:**

```
Hola,

Ayer completaste tu diagnóstico personalizado y descubrimos algo específico
sobre el sueño de tu bebé: {{dolor_especifico}}

Eso no es casualidad ni genérico — fue calculado según las respuestas que
diste sobre su edad, alimentación y rutina actual.

El Método Sueño a Medida toma exactamente ese diagnóstico y lo convierte
en un protocolo de 14 días, paso a paso, para aplicar desde hoy.

👉 [Quiero acceder a mi protocolo completo]

Sin llanto forzado. Sin métodos extremos. A tu ritmo.

Nos vemos del otro lado,
Método Sueño a Medida
```

---

## Email 2 — Día 2 (reversión de objeción)

**Asunto:** "¿Y si no funciona para mi bebé?"

**Cuerpo:**

```
Es la pregunta que más nos hacen, así que vamos directo:

No necesitas la rutina perfecta para empezar.
No necesitas dejar a tu bebé llorando solo.
No necesitas acertar todo a la primera.

El protocolo fue diseñado para la vida real — con noches buenas, noches
difíciles, y ajustes graduales en el camino.

Recuerda: tu plan ya identificó que tu principal desafío es
{{dolor_especifico}}. El protocolo completo ataca exactamente eso,
en el orden correcto, sin adivinar.

👉 [Ver mi protocolo completo]

Incluye además:
✅ Checklist rápido de rutina nocturna (bono incluido)
✅ Acceso inmediato, para empezar esta misma noche
✅ Garantía de 7 días

Método Sueño a Medida
```

---

## Email 3 — Día 3 (cierre directo)

**Asunto:** Última vez que te escribimos sobre esto

**Cuerpo:**

```
No queremos llenar tu bandeja de entrada, así que este es el último
recordatorio sobre tu diagnóstico personalizado.

Sabemos que las noches sin dormir no esperan — por eso el acceso es
inmediato: lo aplicas desde esta noche, a tu ritmo, sin presión.

👉 [Sí, quiero mi protocolo ahora]

Si ya lo resolviste por tu cuenta o decidiste que no es el momento,
no hay problema — no te escribiremos de nuevo sobre esto.

Método Sueño a Medida
```

---

## Mapeo de personalización (variable `{{dolor_especifico}}`)

| Valor en `problema` | Texto a insertar |
|---|---|
| despertares | "tu bebé tiene despertares nocturnos frecuentes que interrumpen el descanso de toda la familia" |
| tarda | "tu bebé tarda mucho tiempo en conciliar el sueño, incluso cuando está agotado" |
| brazos | "tu bebé depende de brazos o pecho para dormirse, lo que hace cada noche agotadora" |
| siestas | "las siestas de tu bebé son inconsistentes, aunque las noches vayan mejor" |

---

# Script actualizado de Google Apps Script

Reemplaza el código anterior en tu Google Sheets (Extensiones → Apps Script) por este. Hace dos cosas: (1) guarda el lead igual que antes, y (2) revisa diariamente quién no compró y envía el correo correspondiente según cuántos días pasaron.

```javascript
const PAIN_TEXT = {
  despertares: "tu bebé tiene despertares nocturnos frecuentes que interrumpen el descanso de toda la familia",
  tarda: "tu bebé tarda mucho tiempo en conciliar el sueño, incluso cuando está agotado",
  brazos: "tu bebé depende de brazos o pecho para dormirse, lo que hace cada noche agotadora",
  siestas: "las siestas de tu bebé son inconsistentes, aunque las noches vayan mejor",
};

const CHECKOUT_LINK = "https://TU-LINK-DE-CHECKOUT-AQUI.com";

// Recibe el lead del quiz (tal como ya tenías) y agrega columnas de control de e-mail
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Leads');
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([
    data.fecha, data.email, data.edad,
    data.problema, data.alimentacion, data.lugar, data.tolerancia,
    "No",  // Comprado (actualiza manualmente a "Si" cuando confirmes la venta)
    "", "", ""  // Email1_enviado, Email2_enviado, Email3_enviado
  ]);
  return ContentService.createTextOutput(JSON.stringify({status: 'ok'}))
    .setMimeType(ContentService.MimeType.JSON);
}

// Ejecuta esta función una vez al día (instrucciones de activador más abajo)
function checkAndSendFollowUps() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Leads');
  var rows = sheet.getDataRange().getValues();
  var now = new Date();

  for (var i = 1; i < rows.length; i++) { // fila 0 = encabezados
    var row = rows[i];
    var fecha = new Date(row[0]);
    var email = row[1];
    var problema = row[3];
    var comprado = row[7];
    var email1Sent = row[8];
    var email2Sent = row[9];
    var email3Sent = row[10];

    if (comprado === "Si" || !email) continue; // ya compró o fila inválida, saltar

    var diffDays = Math.floor((now - fecha) / (1000 * 60 * 60 * 24));
    var dolor = PAIN_TEXT[problema] || "tu bebé todavía no tiene una rutina de sueño estable";

    if (diffDays >= 1 && !email1Sent) {
      sendEmail1(email, dolor);
      sheet.getRange(i + 1, 9).setValue("Si"); // columna Email1_enviado
    } else if (diffDays >= 2 && !email2Sent) {
      sendEmail2(email, dolor);
      sheet.getRange(i + 1, 10).setValue("Si"); // columna Email2_enviado
    } else if (diffDays >= 3 && !email3Sent) {
      sendEmail3(email);
      sheet.getRange(i + 1, 11).setValue("Si"); // columna Email3_enviado
    }
  }
}

function sendEmail1(email, dolor) {
  var subject = "Tu diagnóstico de sueño sigue esperando por ti 🌙";
  var body = "Hola,\n\n" +
    "Ayer completaste tu diagnóstico personalizado y descubrimos algo específico sobre el sueño de tu bebé: " + dolor + ".\n\n" +
    "Eso no es casualidad ni genérico — fue calculado según las respuestas que diste sobre su edad, alimentación y rutina actual.\n\n" +
    "El Método Sueño a Medida toma exactamente ese diagnóstico y lo convierte en un protocolo de 14 días, paso a paso, para aplicar desde hoy.\n\n" +
    "Quiero acceder a mi protocolo completo: " + CHECKOUT_LINK + "\n\n" +
    "Sin llanto forzado. Sin métodos extremos. A tu ritmo.\n\n" +
    "Método Sueño a Medida";
  MailApp.sendEmail(email, subject, body);
}

function sendEmail2(email, dolor) {
  var subject = '"¿Y si no funciona para mi bebé?"';
  var body = "Es la pregunta que más nos hacen, así que vamos directo:\n\n" +
    "No necesitas la rutina perfecta para empezar.\n" +
    "No necesitas dejar a tu bebé llorando solo.\n" +
    "No necesitas acertar todo a la primera.\n\n" +
    "El protocolo fue diseñado para la vida real — con noches buenas, noches difíciles, y ajustes graduales en el camino.\n\n" +
    "Recuerda: tu plan ya identificó que tu principal desafío es " + dolor + ". El protocolo completo ataca exactamente eso, en el orden correcto, sin adivinar.\n\n" +
    "Ver mi protocolo completo: " + CHECKOUT_LINK + "\n\n" +
    "Incluye además:\n" +
    "- Checklist rápido de rutina nocturna (bono incluido)\n" +
    "- Acceso inmediato, para empezar esta misma noche\n" +
    "- Garantía de 7 días\n\n" +
    "Método Sueño a Medida";
  MailApp.sendEmail(email, subject, body);
}

function sendEmail3(email) {
  var subject = "Última vez que te escribimos sobre esto";
  var body = "No queremos llenar tu bandeja de entrada, así que este es el último recordatorio sobre tu diagnóstico personalizado.\n\n" +
    "Sabemos que las noches sin dormir no esperan — por eso el acceso es inmediato: lo aplicas desde esta noche, a tu ritmo, sin presión.\n\n" +
    "Sí, quiero mi protocolo ahora: " + CHECKOUT_LINK + "\n\n" +
    "Si ya lo resolviste por tu cuenta o decidiste que no es el momento, no hay problema — no te escribiremos de nuevo sobre esto.\n\n" +
    "Método Sueño a Medida";
  MailApp.sendEmail(email, subject, body);
}
```

---

## Pasos para activarlo (10 minutos)

1. Abre tu planilla → **Extensiones → Apps Script**.
2. Borra el código anterior y pega el código de arriba completo.
3. Reemplaza `CHECKOUT_LINK` con el link real de tu checkout (Hotmart/Kiwify).
4. En las columnas de tu planilla `Leads`, agrega los encabezados que faltan: `Comprado | Email1_enviado | Email2_enviado | Email3_enviado` (columnas H, I, J, K).
5. En el editor de Apps Script, ve al menú **Activadores (ícono de reloj)** → **Añadir activador**.
6. Configura: función `checkAndSendFollowUps` → tipo de evento **Basado en tiempo** → **Temporizador diario** → elige un horario (ej. 9:00–10:00 a.m.).
7. Guarda. Autoriza los permisos que Google pida (necesita permiso para enviar correos en tu nombre).

## Cómo marcar una venta (por ahora, manual)

Cuando confirmes una venta (por notificación de Hotmart/Kiwify), busca el e-mail correspondiente en la planilla y cambia la columna `Comprado` a **"Si"** — eso detiene la secuencia para esa persona automáticamente.

**Mejora futura (no incluida ahora):** conectar el webhook de venta de Hotmart directamente a la planilla para que esta columna se actualice sola, sin que tengas que hacerlo a mano. Si quieres, lo armamos cuando ya tengas el checkout funcionando y quieras automatizar ese último paso.

## Límite a tener en cuenta

Gmail/Apps Script gratuito permite hasta **100 correos por día** con una cuenta normal de Gmail (o 1,500/día con Google Workspace). Para el volumen que tendrás validando la oferta, es más que suficiente.
