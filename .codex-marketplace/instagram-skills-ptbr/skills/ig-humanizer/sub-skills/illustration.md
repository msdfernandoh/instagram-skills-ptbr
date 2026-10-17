# Sub-skill: gerar ilustração para um post do Instagram

Acrescenta uma imagem opcional a um rascunho (imagem do feed, slide de
carrossel ou card de citação) e anexa na publicação. Usa a camada de imagem
Pixfaro via `lib.illustrate`, que devolve uma URL hospedada que vai direto para
a mídia da Publora.

## Quando roda

- O usuário diz "coloca uma imagem", "faz uma ilustração", "faz um card de
  frase" ou "monta as artes do carrossel".
- Ofereça uma vez depois que o rascunho for aprovado, quando um visual aumentaria o alcance.

## Backends

- **pixfaro** - `PIXFARO_TOKEN` (`pf_live_...`) definido: imagem gerada e anexada.
- **manual** - sem token: a skill escreve o prompt e pede ao usuário que gere a
  imagem e cole a URL. Nunca trava o rascunho.

`lib.image_backend()` informa qual está ativo.

## Passos

1. **Escolha o tipo.** Padrão do Instagram é `carousel` (4:5). Mude com
   `aspect_ratio="w:h"` se precisar. Outros: thumbnail 16:9, carousel/quote/portrait
   4:5, story/cover 9:16, wide/link 16:9, square 1:1.
2. **Escreva o prompt.** Descreva assunto, composição, estilo, paleta. Os
   modelos de imagem entendem melhor prompt em inglês: escreva o prompt de cena
   em inglês, mas todo texto que aparece na arte vai em português via `overlay`.
   Use o estilo da seção 6 do perfil de voz se houver. NÃO coloque as palavras
   do post dentro da arte gerada (use a sobreposição).
3. **Aplique a sobreposição de marca (se o perfil tiver).** Leia a seção 6 de
   `../../../references/voice-profile.md`. Se houver @, cor ou logo, passe um
   `overlay` para compor texto/logo com exatidão:
   ```python
   from lib import illustrate
   r = illustrate("<prompt da cena em inglês>", kind="carousel",
                  overlay={"text": "@perfil", "position": "bottom-right", "color": "#RRGGBB"})
   ```
   Para **card de frase**, coloque a frase do gancho em `text` do overlay (não no
   prompt), `kind="quote"`. Confira acentos no texto do overlay.
4. **Modelo.** Padrão `nano-banana-2`. A sobreposição cuida do texto, então um
   modelo barato serve; só use gemini-pro-image para arte premium. Nunca suba de
   nível sem avisar.
5. **Mostre e confirme.** Apresente a `url` e o `cost` devolvidos. Com
   aprovação, anexe via `media_urls=[r["url"]]` na publicação.
6. **Modo manual.** Se `r["backend"] == "manual"`, mostre `r["message"]` e peça a URL colada.

## Ajuste em vez de gerar de novo

Quando o usuário quer um ajuste ("escurece o fundo", "troca o título", "mais
respiro"), NÃO gere do zero. Guarde o `id` do resultado anterior e edite:

```python
from lib import illustrate, refine
first = illustrate("<cena>", kind="wide")     # -> {"id": "img_...", "url": ...}
fixed = refine(first["id"], "make the background darker and increase contrast")
```

`refine` edita pelo id `img_...` (não pela URL), herda formato e nível quando
você omite `aspect_ratio`/`resolution`, e é mais barato e consistente.

## Controle de custo

- Cada resultado traz `cost` e `balance_after`; se `low_balance` for True, avise
  que o saldo da Pixfaro está baixo antes de gerar mais.
- Padrão `nano-banana-2` + 1K. Modelos premium (`gemini-pro-image`,
  `gpt-5-image`) custam várias vezes mais: só quando o usuário pedir pelo nome.
  A flag `premium` do resultado indica modelo premium; confirme que era a intenção.
- `lib.available_models()` devolve preços e latência ao vivo quando precisar mostrar.

## Regras fixas

- Uma imagem por pedido (`n>1` não suportado); gere os slides um de cada vez.
- Palavras reais no `overlay`, não dentro da arte.
- Modelo barato + 1K por padrão, salvo pedido de premium.
- Nunca anexe imagem que o usuário não viu e aprovou.
- Nunca gere imagem de pessoa real identificável (cliente, concorrente,
  celebridade) nem logotipo de outra marca.
