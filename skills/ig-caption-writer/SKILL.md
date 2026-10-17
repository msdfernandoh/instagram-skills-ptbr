---
name: ig-caption-writer
description: "Escreve legenda de Instagram em português com o gancho nos primeiros 125 caracteres, corpo fácil de ler e um CTA claro, usando uma fórmula de gancho de 2026 escolhida pelo objetivo (salvamentos, envios, comentários, seguidores). Passa pelo humanizador e, com aprovação, publica com a imagem ou o vídeo que você fornecer via Publora. Use para legenda de imagem única ou Reel. Não serve para carrossel slide a slide (use ig-carousel-planner) nem para revisar rascunho (use ig-humanizer --mode audit)."
---

# Redator de legendas para Instagram

Escreve uma legenda que ganha o toque e o salvamento. O Instagram esconde tudo
depois de ~125 caracteres atrás do "mais", então o gancho faz o trabalho todo na
primeira linha. Esta skill coloca o gancho na frente, deixa o corpo fácil de
ler e fecha com um único chamado para ação.

## Quando usar

- "Escreve uma legenda pro Instagram sobre X"
- O usuário tem uma imagem ou um Reel e precisa da legenda
- O usuário quer um gancho mais forte para um rascunho que já tem
- O usuário quer uma legenda que publica junto com a mídia após aprovação

## Fórmulas que esta skill usa (formatos de legenda)

| Código | Fórmula | Objetivo principal | Melhor para |
|---|---|---|---|
| IG1 | Resultado com número primeiro | salvamentos | um resultado com número quebrado + promessa de "pode copiar" |
| IG2 | Verdade contrária | envios | uma afirmação forte e defensável contra a corrente |
| IG3 | Situação que todo mundo vive | comentários | um momento específico e compartilhado, sem introdução |
| IG4 | Mini-história confissão | comentários, seguidores | uma história real em primeira pessoa, com custo |

Esqueletos completos em `../../references/hook-formulas.md`. Para carrossel
(IG5-IG8) use `ig-carousel-planner`; em Reels (IG9-IG10) o gancho está no vídeo e
esta skill escreve a legenda de apoio.

### Escolha primeiro pelo objetivo

| Objetivo | Use |
|---|---|
| Salvamentos | IG1 |
| Envios | IG2 |
| Comentários | IG3, IG4 |
| Seguidores | IG4 |

## Passos

**Perfil de voz primeiro (todo rascunho).** Se `../../references/voice-profile.md` (ou o perfil da marca indicada pelo usuário) estiver com `filled: yes`, carregue e siga a voz, as regras fixas e o estilo de CTA/link do usuário. Se não estiver preenchido, mencione uma vez que `ig-humanizer --mode profile` aprende a voz a partir de alguns posts e siga com as regras de voz gerais.

**Nunca invente os dados.** As regras abaixo pedem número concreto, data e nome próprio, porque é isso que separa um post real de um gerado. Use o que o usuário disse nesta conversa. **Não invente número, data, nome de cliente ou resultado, e não transforme uma afirmação vaga num número que parece plausível.** Se o usuário não tem nada concreto para um trecho, pergunte uma vez; se continuar sem, corte a afirmação em vez de enfeitar. Número inventado publicado vira retratação; número que falta só deixa o post mais fraco.

1. **Reúna as informações.** Tema, ângulo, a mídia (imagem ou Reel), público e
   o objetivo (salvamentos / envios / comentários / seguidores).
2. **Escolha a fórmula.** Use a tabela de objetivos e sugira 2-3 que também
   combinem com o tema; deixe o usuário escolher.
3. **Escreva o gancho (125 primeiros caracteres).** Precisa entregar o
   resultado, a tensão ou o número antes do corte e fazer sentido sozinho.
4. **Escreva o corpo.** Linhas curtas, espaço entre as ideias, uma ideia só.
   Ensine ou conte a história que o gancho prometeu. Menos de 2.200 caracteres.
5. **Escreva um CTA.** Um pedido de salvar ou enviar com motivo ("salva pra
   consultar antes de dar o lance", "manda pra quem tá pensando em comprar
   imóvel"), uma pergunta real, ou um próximo passo único (direct, WhatsApp,
   link). Nunca "o que vocês acham?".
6. **Hashtags.** Passe o tema para `ig-hashtag-strategist` ou aplique o
   conjunto de 3-5 você mesmo (ver `../../references/hashtag-strategy.md`). No
   fim ou no primeiro comentário. Para negócio local, inclua uma tag de cidade.
7. **Passada do humanizador.** Corte vocabulário de IA por densidade de
   parágrafo, limite travessões (cerca de um a cada 100 palavras; um travessão
   sozinho não denuncia, nunca troque por ponto final), quebre trios empilhados,
   aberturas genéricas e pontes de revelação. Mexa só em parágrafo que soa
   mecânico; nunca fabrique variação ou linhas de uma palavra para dar drama.
   Regras canônicas: `ig-humanizer` V3.
8. **Revisão opcional.** Chame `ig-humanizer --mode audit` para um passa/não passa.
9. **Cartão de aprovação.** Mostre: fórmula usada, o gancho (sinalize se passou
   de 125 caracteres), legenda completa, contagem de caracteres, hashtags,
   objetivo principal e a mídia que o usuário precisa fornecer.
10. **Com aprovação, publique com mídia.** Chame `lib.publish(kind="post",
    draft_text=<legenda aprovada>, target_url="https://www.instagram.com/",
    media=[<caminhos locais>], platforms=[<INSTAGRAM_PLATFORM_ID>],
    scheduled_time=<iso_em_UTC_ou_None>)`. Para Reel use `kind="reel"`. Sem mídia
    ou sem chave Publora, devolve um bloco para copiar e o lembrete de anexar a mídia.

## Regras fixas

Regras de voz globais: ver "Regras de voz" no `SKILL.md` raiz. Regras específicas:

- Os primeiros 125 caracteres precisam parar a rolagem sozinhos. Reescreva todo
  gancho que precise da segunda linha.
- Uma ideia por legenda. Duas ideias são dois posts.
- Um número específico onde a afirmação permitir.
- 3-5 hashtags dimensionadas, nunca 30. 0-3 emoji, com intenção.
- Mídia é obrigatória. Nunca apresente uma legenda como pronta sem lembrar de
  anexar a imagem ou o vídeo.
- Sem venda agressiva. No máximo uma menção natural ao próximo passo.
- Sem promessa de resultado garantido em setor regulado (consórcio, crédito,
  educação, saúde, imóveis).

## Antipadrões (a skill recusa)

- Gancho que só faz sentido depois do corte.
- Travessões acima do teto, ou travessão trocado por ponto final.
- Aberturas "Vamos falar sobre..", "Seguinte..", "Bora falar de.."; "O resultado?" como revelação.
- Sinceridade anunciada ("papo reto", "vou ser sincero", "POV:" sem ser POV) sem fato datado por trás.
- Pilhas picotadas ("Sem X. Sem Y. Só Z.") e linhas de uma palavra para dar drama.
- 20-30 hashtags no topo.
- Isca de engajamento ("dá dois toques se concorda", "comenta SIM", "marca 3 amigos").
- Listas de três sem nada concreto.
- "alavancar", "potencializar", "virada de chave", "próximo nível", "jornada", "mergulhar".
- Encher uma ideia de uma linha para parecer substancial.

## Recursos

- `../../references/hook-formulas.md` - as 10 fórmulas (IG1-IG4 são de legenda)
- `../../references/algorithm-heuristics.md` - regras de ranqueamento e horários no Brasil
- `../../references/hashtag-strategy.md` - a receita de 3-5 hashtags
- `../../references/media-workflow.md` - o fluxo rascunho, upload, agendamento
- `references/caption-checklist.md` - a lista de verificação por legenda

## Skills relacionadas

- `ig-carousel-planner` - quando a ideia é um carrossel
- `ig-hashtag-strategist` - monta o conjunto de hashtags
- `ig-humanizer` - tira a cara de IA, mais `--mode audit` para revisão
- `ig-hook-extractor` - descobre o gancho de um post que você admira
