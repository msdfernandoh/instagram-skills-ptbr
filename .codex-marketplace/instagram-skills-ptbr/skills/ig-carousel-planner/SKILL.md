---
name: ig-carousel-planner
description: "Planeja carrossel de Instagram em português slide a slide, até 10 slides: slide gancho que abre um laço, slides de valor com o melhor na frente e slide final que rende salvamento e follow. Escolhe uma fórmula de 2026 (lista, antes/depois, mitos, método) pelo objetivo, escreve o texto de cada slide e a legenda, depois publica as imagens que você fornecer via Publora. Use para estruturar carrossel. Não serve para legenda de imagem única (use ig-caption-writer)."
---

# Planejador de carrossel para Instagram

Planeja um carrossel que é arrastado até o fim e salvo. O jogo todo é o slide 1
(o gancho que ganha o arraste) e o último slide (a recompensa salvável). Esta
skill mapeia cada slide, escreve o texto da arte e a legenda que acompanha.

## Quando usar

- O usuário quer transformar um tema ou anotações num carrossel
- O usuário tem uma lista, um método ou uma transformação para ensinar
- O usuário quer a estrutura slide a slide, não só a legenda

## Fórmulas que esta skill usa (formatos de carrossel)

| Código | Fórmula | Objetivo principal | Melhor para |
|---|---|---|---|
| IG5 | Carrossel de lista | salvamentos | lista numerada que ensina, um item por slide |
| IG6 | Antes e depois | salvamentos, seguidores | prova de resultado com os passos no meio |
| IG7 | Derrubando mitos | envios | corrigir crenças do nicho |
| IG8 | Rouba esse método | salvamentos | um método com nome, repetível |

Esqueletos completos em `../../references/hook-formulas.md`.

## Arquitetura de slides (a espinha de 10)

Um carrossel tem 2-10 slides (limite da API). A espinha confiável:

| Slide | Papel |
|---|---|
| **1 (gancho)** | a promessa + um laço aberto ("quase todo mundo erra a 4"). Ganha o arraste. Texto grande, uma ideia. |
| **2-3** | o valor mais forte, na frente (o arraste cai com a profundidade). |
| **4 a N-1** | um ponto por slide, cada um se sustenta sozinho, legível em 2 segundos. |
| **N (recompensa)** | o resumo em um slide (o artefato salvável) + um pedido claro (salvar / seguir). |

6-10 slides é o ponto ideal para carrossel de ensino. Menos de 4 costuma ser uma
imagem única. Ver `references/slide-architecture.md`.

## Passos

**Perfil de voz primeiro (todo rascunho).** Se `../../references/voice-profile.md` (ou o perfil da marca indicada pelo usuário) estiver com `filled: yes`, carregue e siga a voz, as regras fixas e o estilo de CTA/link do usuário. Se não estiver preenchido, mencione uma vez que `ig-humanizer --mode profile` aprende a voz a partir de alguns posts e siga com as regras de voz gerais.

**Nunca invente os dados.** Use números, datas e nomes que o usuário disse nesta conversa. **Não invente número, data, nome de cliente ou resultado.** Se faltar algo concreto, pergunte uma vez; se continuar faltando, corte a afirmação em vez de enfeitar.

1. **Reúna as informações.** Tema, a lista/método/transformação, público e o
   objetivo (salvamentos / envios / seguidores).
2. **Escolha a fórmula.** Use a tabela de objetivos; sugira 2-3 e deixe o
   usuário escolher.
3. **Defina o número de slides.** Pelo conteúdo real (6-10 é o comum). Nunca
   encha para chegar a 10.
4. **Escreva o slide 1.** A promessa e o laço. Texto grande, uma ideia só.
   Número na frente se couber.
5. **Mapeie os slides de valor.** O ponto mais forte no slide 2 ou 3. Um ponto
   por slide, cada um se sustentando sozinho. Texto curto, legível em 2 segundos
   num quadro 4:5.
6. **Escreva o slide de recompensa.** Um resumo em um slide que vale salvar
   sozinho, mais um único pedido (salvar / seguir / chamar no direct).
7. **Escreva a legenda.** Pode ser curta: retoma o gancho para o corte, dá uma
   linha de contexto, carrega o CTA e as hashtags (via `ig-hashtag-strategist`
   ou a receita de 3-5).
8. **Passada do humanizador** no texto dos slides e na legenda. Vocabulário de
   IA por densidade (por slide, por parágrafo), no máximo um travessão por slide,
   sem trios empilhados nem pontes de revelação. Slide é curto por natureza:
   nunca corte nem encha um slide por ritmo. Regras canônicas: `ig-humanizer` V3.
9. **Cartão de aprovação.** Mostre: fórmula, roteiro slide a slide com o texto
   de cada um, legenda, hashtags, objetivo principal e o lembrete de que o
   usuário fornece 2-10 imagens (só imagens, sem mistura) nesta ordem.
10. **Com aprovação, publique com mídia.** Chame `lib.publish(kind="carousel",
    draft_text=<legenda aprovada>, target_url="https://www.instagram.com/",
    media=[<imagens dos slides na ordem>], platforms=[<INSTAGRAM_PLATFORM_ID>],
    scheduled_time=<iso_em_UTC_ou_None>)`. Sem mídia ou sem chave Publora,
    devolve o plano como bloco para copiar, com a ordem dos slides.

## Regras fixas

Regras de voz globais: ver "Regras de voz" no `SKILL.md` raiz. Regras específicas:

- Slide 1 é promessa com laço aberto, nunca título seco.
- Valor na frente, nos slides 2-3; nunca enterre o melhor ponto no fim.
- Um ponto por slide, legível em 2 segundos num quadro 4:5.
- Só 2-10 slides. Nunca encha para chegar num número redondo.
- Só imagens, sem mistura (a API rejeita carrossel com imagem e vídeo).
- O último slide precisa render o salvamento com um resumo e um pedido.

## Antipadrões (a skill recusa)

- Slide 1 só com título, sem promessa nem laço.
- Carrossel de 10 slides esticado de uma ideia de 4 pontos.
- O ponto mais forte guardado para o último slide.
- Três CTAs competindo no slide final.
- Imagem e vídeo no mesmo carrossel.
- Paredão de texto na arte que não dá para ler num arraste.
- Travessões acima do teto (mais de um por slide), aglomerado de vocabulário de IA, trios ocos.
- "O resultado?" e pilhas picotadas para dar impacto.

## Recursos

- `../../references/hook-formulas.md` - as fórmulas de carrossel (IG5-IG8)
- `../../references/algorithm-heuristics.md` - regras de carrossel e arraste
- `../../references/hashtag-strategy.md` - a receita de 3-5 hashtags
- `../../references/media-workflow.md` - enviando 2-10 imagens na ordem
- `references/slide-architecture.md` - espinhas por fórmula e regras do texto na arte

## Ilustração opcional

Ofereça uma imagem gerada quando um visual aumentaria o alcance. Escreva um
prompt e chame `lib.illustrate(prompt, kind="carousel")`, puxando @ e cor da
marca da seção 6 do perfil de voz para a sobreposição. Mostre a `url` e o
`cost` devolvidos e anexe com `media_urls=[url]` (a Publora busca a URL hospedada
no servidor); o planejador pode passar 2-10 URLs para um carrossel. Fluxo
completo: `../ig-humanizer/sub-skills/illustration.md`. Sem chave Pixfaro, a
skill escreve o prompt para você gerar manualmente.

## Skills relacionadas

- `ig-caption-writer` - quando a ideia é uma imagem única
- `ig-hashtag-strategist` - monta o conjunto de hashtags
- `ig-humanizer` - limpa o texto dos slides e a legenda
- `ig-hook-extractor` - estuda um carrossel que você admira
