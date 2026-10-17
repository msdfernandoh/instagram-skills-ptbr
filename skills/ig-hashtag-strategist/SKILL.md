---
name: ig-hashtag-strategist
description: "Monta um conjunto de hashtags dimensionado para um post de Instagram, na realidade de 2026 em que 3 a 5 tags bem escolhidas vencem 30 aleatórias. Classifica cada tag como nicho (até ~20 mil posts), média (20 mil a 300 mil) ou ampla (300 mil+), inclui tags locais de cidade quando faz sentido e monta uma mistura de 2-3 nicho, 1-2 média e 0-1 ampla, combinando com o conteúdo e alternando para não parecer spam. Não serve para escrever a legenda (use ig-caption-writer)."
---

# Estrategista de hashtags para Instagram

Escolhe 3-5 hashtags que ajudam de verdade, do tamanho certo para o post
ranquear. O paredão de 30 tags morreu: em 2026 hashtag funciona como etiqueta de
assunto, e conta pequena ranqueia em tag de nicho, não em tag de milhões de
posts. Esta skill monta o conjunto dimensionado.

## Quando usar

- O usuário tem legenda, carrossel ou Reel e precisa de hashtags
- O usuário ainda posta 20-30 tags genéricas e quer o jeito de 2026
- O usuário quer tags do tamanho em que a conta dele consegue ranquear

## O modelo: tamanho, não volume

| Faixa | Nº de posts | Papel | Usar |
|---|---|---|---|
| Nicho | até ~20 mil | onde conta pequena/média ranqueia por horas | 2-3 |
| Média | ~20 mil a 300 mil | público suficiente, concorrência vencível | 1-2 |
| Ampla | 300 mil+ | toque rápido de categoria, ranqueia por minutos | 0-1 |

**Padrão: 3-5 no total.** Faixas ajustadas para tags em português. Raciocínio
completo em `../../references/hashtag-strategy.md`.

## Passos

1. **Pegue o post.** A legenda, o tema do carrossel ou o assunto do Reel, mais
   o público, a cidade/região (se o negócio é local) e o tamanho aproximado da
   conta.
2. **Extraia os temas.** Os 3-5 assuntos reais do post (o tema, a comunidade, o
   formato, o resultado, o lugar).
3. **Gere candidatas por faixa.** Para cada tema, proponha nicho, média e ampla.
   Nicho é específica e de cauda longa (`#consorcioimobiliario`, `#sinopmt`);
   ampla é uma palavra comum (`#marketing`, `#imoveis`).
4. **Dimensione cada candidata.** Estime a faixa pela especificidade (a API não
   mostra contagens; diga ao usuário para conferir o número na busca do app).
   Marque para tirar as genéricas ou suspeitas (`#sigodevolta`, `#seguidores`,
   `#follow4follow`, `#like4like`).
5. **Monte o conjunto.** 2-3 nicho + 1-2 média + 0-1 ampla, total 3-5. Tire a
   ampla se nenhuma servir de verdade. Negócio local: pelo menos uma tag de
   cidade/região entre as de nicho.
6. **Confira o encaixe.** Rejeite tag popular mas fora do assunto.
7. **Alterne.** Diga ao usuário para variar o conjunto por assunto.
8. **Entrega.** O conjunto de 3-5, cada tag com a faixa e um porquê de uma
   linha, mais onde colocar (fim da legenda ou primeiro comentário).

## Formato da saída

```
Conjunto dimensionado (4 tags):
  #consorcioimobiliario   nicho   a comunidade exata que este post atende
  #sinopmt                nicho   tag local, pouca concorrência, público certo
  #educacaofinanceira     média   público próximo e relevante, vencível
  #imoveis                ampla   etiqueta de categoria, um toque

Onde colocar: fim da legenda ou primeiro comentário. Alterne as de nicho por assunto.
Confira no app: #sinopmt e #consorcioimobiliario (estão na fronteira entre faixas).
```

## Regras fixas

Regras de voz globais: ver "Regras de voz" no `SKILL.md` raiz. Regras específicas:

- 3-5 tags no total. Nunca um bloco de 30.
- Toda tag descreve o conteúdo real. Nada de popular-mas-fora-do-assunto.
- No máximo uma ampla; tire se nada servir.
- Marque e remova tags de troca de seguidor e tags sabidamente marcadas.
- Hashtag no fim da legenda ou no primeiro comentário, nunca no meio da frase.
- Tags no idioma que o público digita (para público brasileiro, quase sempre português).
- Alterne conjuntos por assunto; não entregue um bloco para colar para sempre.

## Antipadrões (a skill recusa)

- Entregar 20-30 hashtags.
- "Pacote de hashtags virais" de tags amplas genéricas.
- Tags que não combinam com o post para pegar público maior.
- Esconder 30 tags atrás de pontinhos para "ficar limpo".
- Conjunto só de amplas, onde conta pequena não ranqueia.

## Recursos

- `../../references/hashtag-strategy.md` - o modelo completo e a receita
- `../../references/algorithm-heuristics.md` - por que envios e salvamentos, não feed de hashtag, geram alcance
- `references/sourcing-tactics.md` - onde achar e como dimensionar candidatas

## Skills relacionadas

- `ig-caption-writer` - escreve a legenda que leva as hashtags
- `ig-carousel-planner` - o carrossel cuja legenda leva o conjunto
- `ig-content-planner` - alterna temas de tags ao longo da semana
