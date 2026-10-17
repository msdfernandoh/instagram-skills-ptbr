---
name: ig-repurposer
description: "Transforma conteúdo existente em post nativo de Instagram em português. Pega post do LinkedIn, blog, roteiro de YouTube, fio do X, texto de WhatsApp ou e-mail e reconstrói para o Instagram: texto longo vira carrossel ou legenda, com novo gancho antes do corte de 125 caracteres, sem marcas da outra plataforma, publicado via Publora com aprovação. Também adapta conteúdo em inglês para PT-BR sem cara de tradução. Não serve para escrever do zero (use ig-caption-writer ou ig-carousel-planner) nem para revisar (ig-humanizer --mode audit)."
---

# Reaproveitador para Instagram

Transforma algo que você já fez num post que parece ter nascido no Instagram.
Reaproveitar não é copiar e colar. Um post que bombou no LinkedIn ou um fio que
voou no X morre no Instagram se for colado: gancho errado, formato errado, ritmo
errado, sem mídia, e com marcas ("link na bio" de outro lugar, @ do X, paredão
de hashtag) que denunciam que veio de fora.

Esta skill transforma, não gera. Lê a fonte, mantém a ideia e reconstrói a
entrega para o Instagram.

## Quando usar

- "Transforma esse post do LinkedIn num carrossel / legenda"
- "Aproveita meu artigo / newsletter / roteiro do YouTube pro Instagram"
- "Esse fio funcionou no X, adapta pro Instagram"
- "Tenho esse texto que mandei no WhatsApp/lista de transmissão, vira post"
- "Esse conteúdo em inglês é ótimo, faz uma versão nossa em português"

Não serve para rascunho do zero (use `ig-caption-writer` ou
`ig-carousel-planner`) nem para revisar algo que já é do Instagram (use
`ig-humanizer --mode audit`).

## Como funciona

**Perfil de voz primeiro (todo rascunho).** Se `../../references/voice-profile.md` (ou o perfil da marca indicada pelo usuário) estiver com `filled: yes`, carregue e siga a voz, as regras fixas e o estilo de CTA/link do usuário. Se não estiver preenchido, mencione uma vez que `ig-humanizer --mode profile` aprende a voz a partir de alguns posts e siga com as regras de voz gerais.

**Nunca invente os dados.** Mantenha números, datas e nomes da fonte. **Não invente número, data, nome de cliente ou resultado.** Se faltar algo concreto, pergunte uma vez; se continuar faltando, corte a afirmação.

1. **Pegue a fonte.** Qualquer formato: post, parágrafo, roteiro, legenda,
   transcrição, lista, link para ler. Pergunte pela fonte, pela mídia que o
   usuário tem (o Instagram precisa de imagem ou vídeo) e pelo objetivo
   (salvamentos / envios / comentários / seguidores) se não vierem.
2. **Extraia a espinha.** Tire a casca da plataforma de origem e puxe a única
   afirmação, a única história ou o único número que vale manter. A maioria dos
   reaproveitamentos falha porque guarda as palavras em vez do ponto.
3. **Escolha o formato.** Uma afirmação ou um número -> legenda única. Um
   ensinamento, uma lista ou uma construção -> carrossel, um ponto por slide
   (passe a estrutura para `ig-carousel-planner`). Vídeo longo -> carrossel que
   entrega a recompensa, não um resumo.
4. **Novo gancho para o corte.** O Instagram esconde tudo depois de ~125
   caracteres. A linha um (ou o slide gancho) precisa dar o golpe inteiro
   sozinha. O gancho da fonte quase nunca sobrevive; escreva uma primeira linha
   nova com uma fórmula de `../../references/hook-formulas.md`, escolhida pelo objetivo.
5. **Reajuste o formato.** Corpo da legenda fácil de ler, linhas curtas, espaço
   entre as ideias. Carrossel: uma ideia por slide. Corte o tecido conectivo da
   fonte; o Instagram premia a recompensa na frente.
6. **Tire as marcas de fora.** Remova "link na bio" carregado de outra
   plataforma, "se inscreve no canal", "leia mais abaixo", @ do X que só existem
   lá, numeração de fio e qualquer "como eu escrevi no LinkedIn". Post
   reaproveitado não admite que foi reaproveitado.
7. **Fonte em inglês (ou outro idioma).** Não traduza frase a frase. Recrie em
   português brasileiro natural: troque exemplos, moedas (US$ -> R$, só se o
   número for do usuário ou da fonte; nunca converta inventando), unidades e
   referências culturais por equivalentes locais, e corte expressões que soam
   traduzidas ("no final do dia", "isso é um game changer", "faz sentido?").
   Se o conteúdo for de terceiro, use a ideia e dê crédito; não republique como
   se fosse seu.
8. **Lembrete de mídia.** Confirme a imagem, o vídeo ou os slides que o usuário vai anexar.
9. **Hashtags no fim.** Passe o tema para `ig-hashtag-strategist` ou aplique
   3-5 dimensionadas (ver `../../references/hashtag-strategy.md`), num bloco no FIM
   da legenda ou no primeiro comentário.
10. **Passada do humanizador.** Rode `ig-humanizer` V3: vocabulário de IA por
    densidade, travessões acima do teto (nunca trocados por ponto final), trios
    empilhados, aberturas genéricas e pontes de revelação. Mexa só em parágrafo
    mecânico; nunca fabrique variação. Mantenha os números e nomes reais da fonte.
11. **Cartão de aprovação.** Mostre: fonte -> Instagram (o que virou o quê), o
    formato (legenda única ou carrossel de N slides), fórmula, gancho (sinalize
    se passou de 125 caracteres), hashtags, mídia a anexar, objetivo.
12. **Com aprovação.** Publique via `lib.publish(kind="post",
    draft_text=<legenda aprovada>, target_url="https://www.instagram.com/",
    media=[<caminhos locais>], platforms=[<INSTAGRAM_PLATFORM_ID>],
    scheduled_time=<iso_em_UTC_ou_None>)`. Para Reel use `kind="reel"`. Sem mídia
    ou sem chave Publora, devolve um bloco para copiar e o lembrete de anexar a mídia.

## Regras de encaixe (fonte -> Instagram)

- **Post do LinkedIn -> Instagram:** post longo vira carrossel (um ponto por
  slide) ou, se for uma afirmação afiada, legenda única. O LinkedIn tolera
  aquecimento; o Instagram não. O gancho do LinkedIn costuma ser longo demais.
- **Blog / newsletter -> carrossel:** a estrutura vira slides, gancho primeiro,
  recompensa por último. Não cole o texto inteiro numa legenda.
- **Roteiro de YouTube -> carrossel da recompensa:** comece pela recompensa do
  vídeo, não pela introdução.
- **Tweet / fio -> legenda ou carrossel:** um tweet vira legenda; um fio vira
  carrossel, um tweet por slide com seu micro-ponto. Tire @ e numeração.
- **Texto de WhatsApp / lista de transmissão -> legenda:** tire saudações
  ("Bom dia, pessoal!"), áudios citados e "encaminhado"; mantenha o fato e o CTA.

## Regras fixas

Regras de voz globais: ver "Regras de voz" no `SKILL.md` raiz. Regras específicas:

- Mantenha a **afirmação e os fatos** da fonte. Reaproveitar muda a entrega,
  nunca o sentido nem os números.
- A nova primeira linha (ou slide gancho) para a rolagem sozinha antes do corte.
- Nunca cole a fonte e apare. Reconstrua gancho e ritmo a partir da espinha.
- Um número específico quando a fonte oferecer. Mantenha.
- 3-5 hashtags num bloco no fim, nunca 30 e nunca no meio.
- Mídia é obrigatória.
- Sem venda agressiva. No máximo uma menção natural.

## Antipadrões (a skill recusa)

- Copiar a fonte com edições leves.
- Manter marcas da plataforma de origem ("se inscreve", @ do X, numeração de fio).
- Tradução literal que soa como legenda dublada.
- Gancho que só faz sentido depois do corte de 125 caracteres.
- Paredão de hashtag no meio da legenda.
- Apresentar legenda sem mídia como pronta.
- Primeira linha EM CAIXA ALTA para dar intensidade.
- Travessões acima do teto, ou travessão trocado por ponto final.
- "O resultado?" / "Seguinte.." e pilhas picotadas criadas na reescrita.
- Listas de três sem nada concreto.
- "alavancar", "potencializar", "virada de chave", "próximo nível", "jornada".
- Pigarro de bastidor ("publiquei isso originalmente no...").

## Recursos

- `../../references/hook-formulas.md` - os 10 esqueletos para o novo gancho
- `../../references/algorithm-heuristics.md` - regras de ranqueamento (corte, horário, sinais)
- `../../references/hashtag-strategy.md` - a receita de 3-5 hashtags
- `../../references/media-workflow.md` - o fluxo rascunho, upload, agendamento

## Skills relacionadas

- `ig-caption-writer` - escreve uma legenda nova do zero
- `ig-carousel-planner` - estrutura o carrossel (o reaproveitador manda construções longas para lá)
- `ig-hashtag-strategist` - monta o conjunto de hashtags
- `ig-humanizer` - tira a cara de IA, mais `--mode audit` para revisar
