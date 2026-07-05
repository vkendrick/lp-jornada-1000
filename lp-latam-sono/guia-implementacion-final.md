# Guía de Implementación Final — Método Sueño a Medida

Checklist único, en el orden correcto, para publicar todo el ecosistema: ferramienta + productos + checkout + tráfico + automatización.

---

## Fase 1 — Preparar los productos (30-40 min)

- [ ] Abrir `Protocolo-Sueno-14-Dias.docx`, `Planilla-Seguimiento-Sueno.docx`, `Protocolo-Siestas.docx` y `Checklist-Rutina-Nocturna.docx`
- [ ] Exportar cada uno como PDF (Word/Google Docs → "Guardar como PDF" o "Exportar")
- [ ] Revisar una vez más que no haya ningún texto de marcador de posición (placeholder) olvidado

## Fase 2 — Configurar la plataforma de checkout (Hotmart/Kiwify/Lastlink)

- [ ] Crear el producto principal: **Método Sueño a Medida** — protocolo de 14 días, precio $14.90 (o el equivalente configurado en tu moneda base)
- [ ] Subir el PDF del protocolo como archivo de entrega
- [ ] Configurar el **Order Bump**: Planilla de Seguimiento — copy en `escada-vendas-copy.md`
- [ ] Configurar el **Upsell**: Protocolo de Siestas — copy en el mismo archivo
- [ ] Configurar el **Downsell**: Guía rápida de siestas (versión resumida) — copy en el mismo archivo
- [ ] Pegar el bloque de **reversión de objeción** y el **FAQ** en la descripción de la página de checkout (están en `criativos-y-setup-sheets.md`)
- [ ] Copiar el **link de checkout final** — lo necesitas para el siguiente paso

## Fase 3 — Conectar la herramienta (`plan-sueno.html`)

- [ ] Abrir el archivo y buscar la línea del botón final de compra:
  ```
  onclick="alert('Aquí conectas tu checkout...')"
  ```
  Reemplazar por:
  ```
  onclick="window.location.href='TU-LINK-DE-CHECKOUT-AQUI'"
  ```
- [ ] Buscar `SHEET_WEBHOOK_URL = ''` y pegar la URL de tu Google Apps Script (ver Fase 4)

## Fase 4 — Google Sheets + automatización de e-mail

- [ ] Crear la planilla con la pestaña `Leads` y encabezados: `fecha | email | edad | problema | alimentacion | lugar | tolerancia | Comprado | Email1_enviado | Email2_enviado | Email3_enviado`
- [ ] Pegar el script actualizado (de `emails-recuperacion-y-script.md`) en Extensiones → Apps Script
- [ ] Reemplazar `CHECKOUT_LINK` dentro del script por el link real de checkout
- [ ] Publicar como **App Web** (Implementar → Nueva implementación → Tipo: App Web → Acceso: Cualquier persona)
- [ ] Copiar la URL `/exec` generada y pegarla en `SHEET_WEBHOOK_URL` dentro de `plan-sueno.html` (Fase 3)
- [ ] Crear el **activador diario** en Apps Script (ícono de reloj → Añadir activador → función `checkAndSendFollowUps` → diario)
- [ ] Autorizar los permisos que Google solicite

## Fase 5 — Meta Ads (Pixel + Campaña)

- [ ] Crear el Pixel de Meta en el Administrador de Eventos (Meta Business Suite)
- [ ] Insertar el código base del Pixel en `plan-sueno.html` (dentro de `<head>`)
- [ ] Agregar evento `Lead` disparado cuando se envía el e-mail (función `submitEmail`)
- [ ] Verificar que el evento `Purchase` se dispare desde la página de confirmación del checkout (Hotmart/Kiwify normalmente lo integran automáticamente si conectas el Pixel en su panel)
- [ ] Crear la campaña: objetivo **Leads**, una sola campaña, un solo conjunto de anuncios, targeting amplio, Advantage+ Placements y Advantage+ Budget activados
- [ ] Subir los 3 creativos estáticos + el guion de video (generar las imágenes con los prompts de `criativos-y-setup-sheets.md`)
- [ ] Definir presupuesto diario (~$16-17/día con tu presupuesto mensual actual)

## Fase 6 — Revisión final antes de publicar

- [ ] Probar el quiz completo de principio a fin en el navegador (no solo mirar el código)
- [ ] Probar el botón final: ¿redirige al checkout correcto?
- [ ] Hacer una compra de prueba si la plataforma lo permite, para confirmar que el PDF llega bien
- [ ] Completar el quiz con un e-mail de prueba y confirmar que aparece una fila nueva en la planilla de Google Sheets
- [ ] Revisar que el aviso de moneda y las banderas se vean bien en celular (la mayoría del tráfico de Meta Ads es móvil)

## Fase 7 — Publicar y esperar

- [ ] Activar la campaña
- [ ] **No tocar nada durante 5-7 días** — dejar que el algoritmo reúna datos antes de sacar conclusiones
- [ ] Revisar métricas: costo por Lead, tasa de conversión del quiz, tasa de apertura de los 3 e-mails
- [ ] Marcar manualmente "Comprado = Si" en la planilla a medida que lleguen las ventas confirmadas

---

## Archivos de referencia por fase

| Fase | Archivo |
|---|---|
| Productos | Protocolo-Sueno-14-Dias.docx, Planilla-Seguimiento-Sueno.docx, Protocolo-Siestas.docx, Checklist-Rutina-Nocturna.docx |
| Checkout / copy | escada-vendas-copy.md, criativos-y-setup-sheets.md |
| Herramienta | plan-sueno.html |
| E-mails + Sheets | emails-recuperacion-y-script.md |

Si algo falla en cualquier fase, vuelve a este documento y ubica en qué paso te quedaste — no hace falta repetir todo desde cero.
