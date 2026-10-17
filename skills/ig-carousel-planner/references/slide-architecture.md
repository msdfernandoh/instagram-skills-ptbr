# Arquitetura de slides do carrossel

Espinhas por fórmula e regras de texto na arte que `ig-carousel-planner` usa.
Um carrossel tem 2-10 slides (limite da API; o app permite 20). Desenhe para o
quadro retrato 4:5 (1080 x 1350 px), que ocupa mais tela.

## A espinha universal

| Slide | Papel | Texto na arte |
|---|---|---|
| 1 (gancho) | promessa + laço aberto | uma ideia grande, 6-12 palavras, número se couber |
| 2 | valor mais forte | o melhor ponto, na frente |
| 3 a N-1 | um ponto cada | curto, se sustenta sozinho, legível em 2 segundos |
| N (recompensa) | resumo salvável + um pedido | o resumo em um slide, depois salvar/seguir |

O arraste cai com a profundidade, então a ordem do valor importa mais que a
quantidade. O melhor ponto vai no slide 2 ou 3, nunca no 10.

## Regras do texto na arte

- **Uma ideia por slide.** Se o slide precisa de dois pensamentos, divida.
- **Legível em 2 segundos.** Letra grande, poucas palavras. O slide é outdoor,
  não parágrafo. Detalhe vai na legenda.
- **Numere os slides** quando o formato é lista (slide 2 = "1.", etc.), para o
  laço ("quase todo mundo erra a 4") se pagar.
- **Consistência visual.** Mesma fonte, mesmas margens, mesma cor de destaque.
- **Português na arte sem erro.** Erro de digitação numa arte vira print e
  comentário. Revise acentos e crase antes de exportar.
- **No máximo um travessão por slide**, e só se fizer trabalho real. O excesso
  vira dois-pontos ou quebra de linha, nunca ponto final.

## IG5 - Carrossel de lista (objetivo: salvamentos)

```
Slide 1: {N} {coisas} que {recompensa}. (quase todo mundo erra a {k})
Slide 2: 1. {item} + um exemplo concreto
Slide 3: 2. {item} + exemplo
...
Slide N-1: {N}. {item} + exemplo
Slide N: a lista resumida num slide + "salva pra usar de verdade"
```

Coloque o item mais surpreendente na posição 1 ou 2, não no fim.

## IG6 - Antes e depois (objetivo: salvamentos, seguidores)

```
Slide 1: {o depois, com número ou visual marcante}
Slide 2: onde começou (o antes) - o laço é a distância entre os dois
Slide 3 a N-1: os passos exatos, um por slide
Slide N: o resultado de novo + "segue pra ver o método completo de {tema}"
```

Os passos precisam ser concretos o bastante para fazer amanhã.

## IG7 - Derrubando mitos (objetivo: envios)

```
Slide 1: {N} mitos sobre {tema} que estão te custando {perda específica}
Slide 2 a N-1: "Mito: {crença}" depois "Verdade: {o que funciona}"
Slide N: a virada em uma linha + "manda pra quem ainda acredita no #1"
```

O pedido de envio no último slide é o ponto todo; mito derrubado é compartilhado.

## IG8 - Rouba esse método (objetivo: salvamentos)

```
Slide 1: o método {nome} que eu uso pra {resultado}
Slide 2 a N-1: uma parte do método por slide, com o porquê
Slide N: o método inteiro em um slide (a isca de salvamento) + pedido de salvar
```

O resumo em um slide é o que as pessoas salvam. Ele precisa se bastar.

## Quantos slides

| Conteúdo | Slides |
|---|---|
| Lista ou método enxuto | 6-8 |
| Conjunto de ensino mais rico ou transformação | 8-10 |
| Menos de 4 pontos reais | publique como imagem única |

Nunca encha até 10. Uma ideia de 5 pontos é um carrossel de 5 pontos (7 slides
com gancho e recompensa).

## Legenda que acompanha

A legenda do carrossel pode ser curta, porque os slides carregam o conteúdo. Ela
deve: retomar o gancho para o corte do "mais", dar uma linha de contexto,
carregar o CTA e as 3-5 hashtags. Ver `../../../references/hashtag-strategy.md`.
