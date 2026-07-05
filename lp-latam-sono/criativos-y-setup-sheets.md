# Setup: E-mails automáticos no Google Sheets

## Passo a passo (10 minutos, sem servidor)

1. Abra o Google Drive e crie uma nova planilha (Google Sheets). Nomeie a primeira aba de `Leads`.
2. Na primeira linha, crie as colunas: `fecha | email | edad | problema | alimentacion | lugar | tolerancia`
3. Na planilha, vá em **Extensões → Apps Script**.
4. Apague o conteúdo padrão e cole este código:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Leads');
  var data = JSON.parse(e.postData.contents);
  sheet.appendRow([
    data.fecha, data.email, data.edad,
    data.problema, data.alimentacion, data.lugar, data.tolerancia
  ]);
  return ContentService.createTextOutput(JSON.stringify({status: 'ok'}))
    .setMimeType(ContentService.MimeType.JSON);
}
```

5. Clique em **Implantar → Nova implantação**.
6. Tipo: **App da Web**. Executar como: **Eu**. Quem tem acesso: **Qualquer pessoa**.
7. Clique em **Implantar** e copie a URL gerada (termina em `/exec`).
8. Abra o arquivo `plan-sueno.html`, procure a linha:
   ```javascript
   const SHEET_WEBHOOK_URL = '';
   ```
   e cole sua URL entre as aspas.
9. Pronto — cada e-mail capturado no quiz cai direto numa linha da planilha, com todas as respostas do quiz junto (isso já é uma segmentação pronta pra quando você automatizar os e-mails).

**Observação:** o Google Apps Script não devolve confirmação visível no navegador (por isso o `mode: 'no-cors'` no código) — para testar se está funcionando, preencha o quiz uma vez e confira se a linha apareceu na planilha.

---

# Criativos de anúncio — Hook / Dor / Solução

Direção visual: **evite** foto genérica de banco de imagens de "bebê chorando" — é o clichê visual de todo concorrente pesquisado. Prefira: foto real seu (ou de alguém autorizando o uso) num quarto escuro/aconchegante, tela do celular mostrando o resultado do quiz, ou a ilustração SVG que já está na ferramenta (pode printar a tela do quiz rodando como criativo — isso também prova que o produto é real, não promessa vazia).

## Criativo 1 — Estático (dor específica)

**Hook:** "😴 ¿Tu bebé tiene más de 4 meses y sigue despertándose varias veces cada noche?"
**Dor:** "No son solo las noches difíciles. Es el cansancio acumulado, la falta de energía, la sensación de que ya probaste de todo."
**Solución:** "Con el Método Sueño a Medida, tu bebé puede aprender hábitos de sueño saludables — sin dejarlo llorar, sin métodos extremos."
**CTA:** "Descubre tu plan gratis →"

## Criativo 2 — Estático (comparação / personalização)

**Hook:** "¿Por qué el mismo ebook no funciona para dos bebés distintos?"
**Dor:** "Porque cada bebé tiene su ritmo — y las guías genéricas no lo saben."
**Solução:** "Este diagnóstico se ajusta a la edad, la alimentación y el temperamento de tu bebé en tiempo real."
**CTA:** "Haz el diagnóstico (2 minutos) →"

## Criativo 3 — Estático (curiosidade / erro comum)

**Hook:** "El error que la mayoría de padres comete después de los 4 meses..."
**Dor:** "Muchos creen que solo hay que esperar a que 'se le pase'. Pero a partir de los 4 meses el cerebro de tu bebé ya está listo para aprender a dormir mejor."
**Solução:** "Descubre en 2 minutos qué le falta a la rutina de tu bebé, con el Método Sueño a Medida."
**CTA:** "Ver mi diagnóstico →"

## Roteiro de vídeo/Reels (15-20s)

```
[0-3s] HOOK (texto na tela + fala):
"Si tu bebé se despierta 5 veces por noche, esto es para ti."

[4-9s] DOR:
"Ya intentaste rutinas de internet, consejos de la abuela,
el método que usó tu amiga... y nada funciona,
porque tu bebé no es igual al de nadie."

[10-15s] SOLUCIÓN (mostrar tela do quiz rodando):
"Este diagnóstico hace 5 preguntas sobre TU bebé
y te da un plan ajustado a su edad, su alimentación y su rutina.
No genérico. Personalizado."

[16-20s] CTA:
"Es gratis y toma 2 minutos. Link en la bio."
```

## Nota honesta

Esses hooks partem de padrões de copy que funcionam bem no nicho (dor específica + promessa de personalização), mas performance real só se confirma testando no Meta Ads com dados de verdade. Rode os 3 criativos estáticos ao mesmo tempo com o mesmo público e deixe uns 5-7 dias antes de decidir qual escalar.

---

# Prompts de imagem para Gemini ou GPT (DALL·E)

Cole cada prompt inteiro na ferramenta de geração de imagem. Depois você adiciona o texto do hook por cima (Canva, Photoshop, ou até o editor de anúncios do próprio Meta).

## Prompt 1 — Cena noturna, mãe exausta (Criativo 1)

```
Fotografía realista, estilo editorial cálido y cinematográfico.
Una madre joven latina, de espaldas o de perfil (rostro parcialmente
en sombra, sin mostrarlo con claridad), sentada en una mecedora en
un cuarto de bebé a oscuras, iluminada solo por una lámpara de luz
cálida tenue. Sostiene a un bebé dormido contra su pecho. Ambiente
íntimo, colores cálidos (dorado, azul medianoche), textura de foto
real, no ilustración, no dibujo animado, sin texto ni logotipos en
la imagen. Formato vertical 4:5 para Instagram.
```

## Prompt 2 — Comparación / plan personalizado (Criativo 2)

```
Fotografía de un teléfono celular sostenido por una mano, mostrando
en la pantalla una interfaz limpia de app de color azul medianoche
y dorado, con un cuestionario simple. De fondo, desenfocado, una
cuna de bebé en un cuarto acogedor iluminado suavemente. Luz cálida,
composición moderna, fotografía realista, sin texto legible en la
pantalla del teléfono (se agregará después), formato cuadrado 1:1.
```

## Prompt 3 — Autoridad indirecta / sistema, no gurú (Criativo 3)

```
Fotografía realista de un cuarto de bebé minimalista y ordenado,
de noche, con una cuna vacía bien iluminada por una luz de luna
cálida entrando por la ventana. Sin personas en la imagen. Paleta
de colores: azul medianoche profundo, dorado suave, blanco cálido.
Composición serena, casi editorial, sin texto ni logotipos, formato
vertical 4:5.
```

## Prompt 4 — Miniatura para el video/reel

```
Fotografía realista de una madre sonriendo con alivio mientras mira
su teléfono, de noche, en un cuarto de bebé con luz cálida tenue.
Expresión de alivio, no de cansancio extremo. Colores cálidos con
acento dorado y azul medianoche. Sin texto en la imagen. Formato
vertical 9:16 para Reels/TikTok.
```

### Dicas de uso

- Se a ferramenta gerar rostos que pareçam estranhos ou artificiais, peça: "ajusta el rostro para que se vea más natural y menos simétrico" — isso geralmente resolve.
- Gere 3-4 variações de cada prompt e escolha a mais natural — geração de IA às vezes erra mãos e proporções.
- Não use nenhuma dessas imagens com rosto de pessoa real different da que você tem autorização — evite gerar rostos muito parecidos com pessoas reais/celebridades.

---

## Nota sobre elementos descartados de referências de concorrente

Ao revisar exemplos de concorrentes (curso + mentoria), dois elementos foram **deliberadamente excluídos** desta copy:

1. **Números de prova social inventados** (ex: "1.200 familias ya lo aplicaron", depoimentos fictícios) — isso é propaganda enganosa, viola as políticas de anúncio da Meta e pode banir sua conta. Só adicione números reais quando você tiver clientes de verdade.
2. **Contadores de urgência falsos** (ex: "oferta hasta el domingo") — só use prazo real. Prazo falso repetido é outra prática punida pela Meta e que corrói confiança a médio prazo.

Quando você tiver seus primeiros 15-20 clientes reais, me avise — nesse ponto já faz sentido incorporar depoimentos e prova social genuínos na copy.

---

# Bloco de reversão de objeção (colar na página de checkout, antes do preço)

**No necesitas la rutina perfecta para empezar**

- **No exige perfección** — el método fue pensado para la vida real, con noches buenas, noches difíciles y ajustes graduales.
- **No depende de métodos extremos** — la propuesta es previsibilidad, lectura de señales y constancia amable, no endurecer la noche.
- **No es solo otro intento** — recibes un protocolo completo más un bono de implementación rápida, no solo información suelta.
- **No necesitas acertar todo a la primera** — pequeños ajustes constantes generan más resultado que un intento perfecto y aislado.

---

# FAQ (colar antes do botão final de compra)

**¿Esto sustituye a un pediatra?**
No. El Método Sueño a Medida es un material educativo para organizar la rutina de sueño. No sustituye la valoración médica, pediátrica o psicológica cuando sea necesaria.

**¿Para qué edad sirve?**
Está pensado para bebés de 0 a 24 meses, con orientación ajustada según la fase de desarrollo — por eso el diagnóstico personalizado es el primer paso.

**¿Necesito dejar a mi bebé llorando para aplicarlo?**
No. El enfoque es gentil: previsibilidad, entorno adecuado y constancia — no métodos extremos.

**¿En cuánto tiempo se nota diferencia?**
Cada bebé responde distinto, pero en general las primeras mejoras aparecen cuando la rutina deja de ser prueba y error y empieza a seguir un protocolo coherente y repetible — normalmente dentro de los primeros 7-10 días.

**¿Cómo recibo el acceso?**
Acceso digital inmediato después de la confirmación del pago, para consultar desde el celular, tablet o computadora cuando lo necesites.

