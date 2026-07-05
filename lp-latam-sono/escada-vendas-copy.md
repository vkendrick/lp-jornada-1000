# Escada de vendas completa — copy pronta pra subir

Estrutura: Produto principal → Order Bump (mesma tela do checkout) → Upsell (tela seguinte, só aparece pra quem comprou) → Downsell (só aparece pra quem recusar o upsell).

Configure essa sequência direto na plataforma de checkout (Hotmart, Kiwify ou Lastlink têm campo próprio pra order bump e páginas de upsell/downsell no fluxo pós-compra).

---

## 1. Produto principal

**Protocolo de Sueño de 14 Días** — $14.90

*(copy já usada na ferramenta interativa — ver plan-sueno.html)*

---

## 2. Order Bump (aparece ANTES de finalizar o pagamento, com checkbox)

**Produto:** Planilla de Seguimiento de Sueño (14 días)

**Copy (texto curto, formato checkbox):**

> ☐ **Sí, agrega mi Planilla de Seguimiento de Sueño por solo $4.90**
> La herramienta que te muestra en blanco y negro que el progreso es real, incluso en las noches que sientes que nada cambió.

**Preço:** $4.90 (order bump = baixo atrito, sempre bem mais barato que o principal)

---

## 3. Upsell (tela única, logo após a confirmação do pagamento)

**Produto:** Protocolo de Siestas

**Headline:**
> Espera — ¿ya pensaste en las siestas?

**Corpo:**
> Acabas de dar el primer paso para que tu bebé duerma bien de noche. Pero si las siestas siguen siendo un caos, ese desorden del día termina afectando la noche también — son la misma moneda, dos caras distintas.
>
> El **Protocolo de Siestas** te da la estructura exacta por edad, los horarios ideales, y cómo evitar los 3 errores que más alargan el problema — en un protocolo de 7 días, para usar junto con el que ya tienes.

**Bullets:**
- Ventanas de siesta exactas según la edad de tu bebé
- Cómo evitar que la última siesta arruine la hora de dormir
- Ajustes para cuando la siesta dura menos de 45 minutos

**Price block:**
> ~~$19~~ **$9.90** — solo en esta página, precio especial por comprar el protocolo principal hoy.

**CTA principal:** "Sí, quiero también el Protocolo de Siestas →"
**CTA de recusa (link discreto, não botão):** "No, gracias, continuar solo con mi protocolo de sueño"

---

## 4. Downsell (só aparece se ela clicar em "no, gracias" no upsell)

**Headline:**
> Antes de irte — una última oferta

**Corpo:**
> Entendemos que $9.90 puede no ser el momento. Te dejamos la guía rápida de siestas (versión resumida, sin el protocolo completo de 7 días) a un precio simbólico.

**Price block:**
> **$4.90** — Guía rápida de siestas (versión resumida)

**CTA:** "Sí, quiero la guía rápida →"
**CTA de recusa:** "No, gracias, ir a mi compra"

*(Nota: para o downsell, você pode simplesmente entregar as primeiras 2 seções do Protocolo de Siestas completo como "versão resumida" — não precisa criar um terceiro documento do zero.)*

---

## Resumo de preços da esteira

| Produto | Onde aparece | Preço |
|---|---|---|
| Protocolo de Sueño 14 Días | Produto principal | $14.90 |
| Planilla de Seguimiento | Order bump | +$4.90 |
| Protocolo de Siestas | Upsell | $9.90 |
| Guía rápida de siestas | Downsell | $4.90 |

**Ticket médio potencial se ela compra tudo:** ~$29.70 por cliente, contra $14.90 do produto principal sozinho — quase o dobro, sem precisar de mais tráfego.

---

## Checklist final pra subir tudo junto

- [ ] Subir `Protocolo-Sueno-14-Dias.docx` como produto principal na plataforma de checkout (exportar como PDF antes de subir)
- [ ] Subir `Planilla-Seguimiento-Sueno.docx` (como PDF) como order bump
- [ ] Subir `Protocolo-Siestas.docx` (como PDF) como upsell
- [ ] Configurar a página de upsell com a copy acima na plataforma
- [ ] Configurar a página de downsell com a copy acima
- [ ] Conectar o link de checkout do produto principal no botão final de `plan-sueno.html`
- [ ] Publicar o Google Apps Script e colar a URL em `SHEET_WEBHOOK_URL`
- [ ] Gerar as imagens dos criativos com os prompts do outro arquivo e subir os anúncios
