# Regras de classificação de gancho (Instagram)

Extração de traços e notas que `ig-hook-extractor` usa para encaixar um post em
uma das 10 fórmulas de `../../../references/hook-formulas.md`.

## Passo 1: detectar a superfície

| Sinal | Superfície |
|---|---|
| Uma foto + legenda | imagem única (olhe IG1-IG4) |
| 2-10 imagens empilhadas, pontinhos de arraste | carrossel (olhe IG5-IG8) |
| Vídeo vertical, com áudio | Reel (olhe IG9-IG10) |

A URL ajuda: `/reel/` é Reel; `/p/` é imagem única ou carrossel (pergunte ao usuário).

## Passo 2: extrair os traços do gancho

### Ganchos de legenda (primeiros ~125 caracteres)

| Traço na primeira linha | Fórmula |
|---|---|
| Número quebrado + promessa de "pode copiar / é assim que faz" | IG1 Resultado com número primeiro |
| Afirmação seca que contradiz o conselho comum | IG2 Verdade contrária |
| Momento específico e compartilhado, sem introdução, sem número | IG3 Situação que todo mundo vive |
| Marcador de tempo em primeira pessoa + custo ou surpresa admitidos | IG4 Mini-história confissão |

### Ganchos de carrossel (slide 1 + estrutura)

| Traço | Fórmula |
|---|---|
| Slide 1 promete N itens numerados; corpo é um item por slide | IG5 Lista |
| Slide 1 mostra o "depois"; slide 2 o "antes"; corpo são passos | IG6 Antes e depois |
| Slides do corpo pareiam "Mito:" com "Verdade:" | IG7 Derrubando mitos |
| Slide 1 nomeia um método; último slide resume tudo | IG8 Rouba esse método |

### Ganchos de Reel (primeiros 1-3 segundos)

| Traço | Fórmula |
|---|---|
| Quebra de padrão / texto de laço aberto na tela, sem introdução lenta | IG9 Reel quebra-padrão |
| "Como eu {resultado} em {prazo}" + passos rotulados | IG10 Reel como eu fiz |

## Passo 3: nota de confiança

- **Alta (0,8+):** o gancho bate com um esqueleto e o corpo segue a estrutura.
- **Média (0,5-0,8):** o gancho serve mas o corpo é solto, ou duas fórmulas se
  sobrepõem (comum: IG1 e IG10). Devolva as 2 melhores.
- **Baixa (abaixo de 0,5):** sem encaixe limpo. Descreva a estrutura literalmente
  e sugira a fórmula mais próxima como ponto de partida.

Desempate pelo **objetivo principal** e pela superfície: resultado só na
legenda é IG1; o mesmo resultado num Reel com passos é IG10.

## Passo 4: nomear o objetivo principal

| Fechamento / estrutura | Objetivo provável |
|---|---|
| "salva isso", lista, método, tutorial | salvamentos |
| "manda pra..", opinião contrária, mito derrubado | envios |
| situação do dia a dia, pergunta, confissão | comentários |
| prova de transformação, "segue pra.." | seguidores |

## Passo 5: montar o modelo em branco

Copie o esqueleto da fórmula em `../../../references/hook-formulas.md`, mantenha a
estrutura dos campos e renomeie os campos para o tema do usuário, em português.
Não traduza literalmente expressões em inglês; adapte para como o público
brasileiro fala. Tire os sinais de IA do original (não leve aglomerado de
vocabulário nem bloco de 30 hashtags para o modelo; um travessão no original é ok).

## Passo 6: revisar a fonte

Aponte, para o usuário não copiar:
- travessões acima de cerca de um a cada 100 palavras; meia-risca entre orações
- aglomerado de vocabulário de IA (3+ marcadores num parágrafo)
- 20-30 hashtags no topo
- isca de engajamento ("comenta SIM", "marca 3 amigos")
- gancho que só faz sentido depois do "mais"
- promessa de resultado garantido em setor regulado
