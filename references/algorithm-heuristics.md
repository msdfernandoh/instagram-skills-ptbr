# Heurísticas de postagem no Instagram (2026)

Sintetizado das declarações públicas do Instagram sobre ranqueamento (posts do
Adam Mosseri para criadores), dos explicadores de algoritmo de 2023-2026 e de
dados observados de criadores. Números marcados como "relatado" são medidos pela
comunidade, não confirmados oficialmente. Cada superfície (Feed, Reels, Explorar,
Stories) tem seu próprio modelo, mas os sinais abaixo valem em geral.

## Sumário

- Peso dos sinais
- Os primeiros 30-60 minutos
- O que derruba o alcance
- O que amplia o alcance
- Limites de formato e mídia (via API)
- Carrosséis
- Reels
- Stories
- Horários (Brasil)
- Salvamentos e envios são as alavancas subestimadas
- Checklist antes de publicar

## Peso dos sinais (impacto relativo no alcance)

| Sinal | Peso relativo | Nota |
|---|---|---|
| **Envio / compartilhamento** (direct para amigo, ou repost no story) | o mais alto | "envios por alcance" é a métrica em que o algoritmo mais se apoia em 2026 |
| **Salvamento** | alto | o sinal de "vou voltar nisso"; alavanca tutoriais, listas, métodos |
| **Comentário** (principalmente com resposta do autor) | alto | conversa real é sinal forte de qualidade |
| **Tempo assistido / conclusão / reexibição do Reel** | alto (Reels) | em vídeo, retenção e reexibição decidem o empurrão |
| **Visita ao perfil seguida de follow** | alto | o sinal de crescimento |
| **Curtida** | baixo | afirmação barata, pouco alcance |
| **Negativo: "não tenho interesse", deixar de seguir, ocultar, denunciar** | penalidade pesada | uma denúncia pesa mais que muitas curtidas |

Resumo: otimize para **envios, salvamentos e comentários**, não curtidas. A
pergunta mais útil antes de postar: alguém mandaria isso para um amigo
específico, ou salvaria para usar depois?

## Os primeiros 30-60 minutos

- A janela inicial define a trajetória. Salvamentos, envios e comentários cedo
  dizem ao algoritmo para abrir a distribuição para não seguidores (Explorar e
  aba Reels).
- **Responda rápido aos primeiros comentários.** Resposta do autor puxa mais
  comentários e sinaliza conversa ativa.
- Um Reel que segura a retenção nos primeiros 3 segundos no teste inicial ganha
  um empurrão maior.

## O que derruba o alcance (evite)

- **Isca de engajamento** ("comenta SIM", "marca 3 amigos", "dá dois toques se")
  é rebaixada explicitamente.
- **Vídeo reaproveitado com marca d'água de outro app** (logo do TikTok, CapCut
  etc.) é suprimido nos Reels.
- **Legenda só de hashtag** (20-30 tags no topo) parece spam em 2026 e não ajuda.
- **Proporções fora de 4:5 a 1,91:1** são cortadas, o que prejudica carrossel e
  imagem no feed.
- **Taxa alta de ocultar/deixar de seguir/denunciar** derruba a distribuição rápido.
- **Mídia misturada no carrossel** é rejeitada pela API.

## O que amplia o alcance

- **Envios são a alavanca.** Post feito para ser mandado para alguém (uma verdade
  do dia a dia, uma lista útil, um "você precisa ver isso") alcança mais que post
  de isca de curtida.
- **Salvamentos se acumulam.** Formatos de salvamento (listas, métodos,
  tutoriais, antes/depois) continuam ganhando alcance por dias pelo Explorar.
- **Reels com retenção forte** são o formato mais empurrado para não seguidores
  em 2026. Áudio original e texto na tela ajudam.
- **Carrossel ganha segunda chance:** quem não arrastou pode ver o slide 2 numa
  nova exibição, o que dá uma cauda mais longa.
- **Responder comentários e directs** na primeira hora mantém o sinal de conversa vivo.

## Limites de formato e mídia (via API de publicação)

| Item | Limite |
|---|---|
| Legenda | 2.200 caracteres (os primeiros ~125 aparecem antes do "mais") |
| Hashtags | máximo 30 por post (mas 3-5 bem dimensionadas é a prática de 2026) |
| Itens no carrossel | 2-10 (API; o app nativo permite 20) |
| Formatos de imagem | JPEG, PNG, WebP (WebP vira JPEG automaticamente) |
| Tamanho máximo de imagem | 8 MB |
| Duração do Reel | até 3 min (180 s) via API; 5-90 s entram na aba Reels |
| Tamanho máximo de vídeo | 300 MB |
| Vídeo no carrossel | até 60 s por clipe |
| Mídia misturada no carrossel | não permitido (só imagens ou só vídeos) |
| Proporção | 4:5 (retrato) a 1,91:1 (paisagem); 4:5 ocupa mais a tela |
| Limite de publicações | ~50 posts / 24 h (algumas contas relatam 25) |

- **Mídia é obrigatória em todo post.** O Instagram rejeita post só de texto.
  As skills escrevem a legenda; o usuário fornece a imagem ou o vídeo.
- Uma imagem vira foto; 2-10 imagens viram carrossel; um vídeo vira Reel por
  padrão (ou Story via `videoType: "STORIES"`).

## Carrosséis

- **O slide 1 é o funil inteiro.** Precisa prometer uma recompensa e abrir um
  laço, senão ninguém arrasta.
- **Valor na frente.** O arraste cai com a profundidade, então o item mais forte
  vai no slide 2 ou 3, não guardado para o 10.
- **Tamanho ideal: 6-10 slides** para carrossel de ensino ou lista. Cada slide
  um ponto, legível sozinho.
- **O último slide rende o salvamento e o follow.** Feche com um resumo em um
  slide mais um único pedido claro.
- **Desenhe para o quadro 4:5.**

## Reels

- **Os primeiros 1-3 segundos decidem o alcance.** Abra com quebra de padrão,
  texto na tela e o ponto. Nada de "oi gente, sejam bem-vindos".
- **Retenção e reexibição são a meta.** Um final que emenda no começo aumenta
  a reexibição.
- **Texto na tela** pega quem assiste sem som e melhora a conclusão.
- **Áudio original ou em alta** ajuda, mas gancho e retenção importam mais.
- Sem marca d'água de outro app de vídeo.

## Stories

- **Stories não trazem alcance** para não seguidores; aprofundam a relação com
  quem já segue e mantêm você lembrado.
- **Repostar seu post do feed no story** equivale a um envio e empurra o post
  original para seguidores que não viram.
- Figurinhas interativas (enquete, caixinha de perguntas, quiz) rendem os toques
  que mantêm você no começo da fila de stories.

## Horários (Brasil, fuso local do público)

Os horários abaixo são pontos de partida. **Sempre confira os insights da conta**
(Painel profissional > Público > Horários mais ativos), porque cada público é um.

| Público | Melhores janelas (horário local) |
|---|---|
| Consumidor final / público geral | dias úteis 11h-13h (almoço) e 18h30-21h30; domingo à noite |
| B2B / empresários / profissionais | terça a quinta, 7h-9h, 12h-13h e 18h-19h |
| Público universitário / EAD | dias úteis 12h-14h e 20h-23h |
| Negócio local (cidade do interior) | 11h30-13h e 19h-21h; sábado de manhã costuma funcionar bem |

- Lembre do fuso: Mato Grosso, Mato Grosso do Sul, Rondônia, Roraima e a maior
  parte do Amazonas estão em UTC-4 (uma hora atrás de Brasília). Agende no fuso
  do público, não no de Brasília por padrão.
- Constância vence frequência bruta. 3-5 posts de qualidade por semana com ritmo
  estável superam um post diário de enchimento.
- Reels podem sair com mais frequência que carrosséis sem cansar, porque cada um
  alcança um público novo.

## Salvamentos e envios são as alavancas subestimadas

- **Salvamento** = a pessoa pretende voltar: recompensa listas, métodos,
  tutoriais e antes/depois (IG5, IG8, IG10, IG1, IG6).
- **Envio** = a pessoa pensou em alguém específico: recompensa verdades do dia a
  dia, mitos derrubados e opiniões contrárias (IG2, IG7, IG3, IG9).

## Checklist antes de publicar

- [ ] Os primeiros 125 caracteres param a rolagem sozinhos.
- [ ] Travessões (`—`) em até cerca de um a cada 100 palavras (nunca trocado por ponto final); sem meia-risca (`–`) entre orações nem hífen duplo (`--`).
- [ ] Sem aglomerado de vocabulário de IA (3+ marcadores no mesmo parágrafo: alavancar, potencializar, crucial, jornada etc.).
- [ ] Pelo menos um número específico com referência clara, quando a afirmação permite.
- [ ] 3-5 hashtags dimensionadas (não 30), no fim ou no primeiro comentário.
- [ ] 0-3 emoji, com intenção, nenhum espalhado.
- [ ] Slide 1 do carrossel promete e abre um laço; o último entrega + pede.
- [ ] Mídia pronta e dentro dos limites (JPEG/PNG/WebP, sem mistura, 2-10 slides).
- [ ] O fechamento é uma aterrissagem ou um pedido de salvar/enviar com motivo, não "o que vocês acham?".
- [ ] Um objetivo principal claro (salvamentos / envios / comentários / seguidores).
