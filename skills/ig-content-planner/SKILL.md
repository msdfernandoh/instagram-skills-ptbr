---
name: ig-content-planner
description: "Gera o plano semanal de Instagram em português a partir de tema, público e pilares de conteúdo. Monta a mistura por dia de Reels, carrosséis, imagens e stories, cada um com fórmula de gancho, ângulo, horário no fuso do público e objetivo, mais a meta semanal de salvamentos e envios (a alavanca de 2026), a rotina de stories e uma checagem de equilíbrio. Use para planejar a semana em vez de postar no improviso. Não serve para escrever uma legenda (use ig-caption-writer) nem um carrossel (use ig-carousel-planner)."
---

# Planejador de conteúdo para Instagram

Monta um plano semanal com disciplina de pilares e a mistura de formatos que o
Instagram premia: Reels para alcance, carrosséis para salvamento, stories para
relacionamento. O plano define uma meta semanal de salvamentos e envios, porque
envios por alcance e salvamentos são as alavancas de 2026.

## Quando usar

- "Planeja minha semana no Instagram" / "o que eu posto essa semana"
- O usuário quer sair do improviso e criar ritmo
- Antes de uma semana de lançamento, campanha, evento ou vestibular

## Entrada

- **Tema** (opcional): ex. "lançamento do evento PR1ME", "vestibular de verão"
- **Público:** ex. "investidores do norte de MT", "jovens de 17 a 24 anos"
- **Cidade / fuso do público** (para os horários): padrão é o fuso do usuário
- **Mistura de pilares** (opcional): padrão 40% Educativo / 30% História /
  20% Engajamento / 10% Promoção
- **Cadência** (opcional): padrão 4-5 posts no feed por semana + stories diários
- **Datas da semana** (opcional): feriados, datas comerciais, eventos locais
- **Amostras de voz** (opcional): legendas antigas para calibrar

## Saída

Um plano em markdown com:

### Calendário de 7 dias

| Dia | Formato | Pilar | Fórmula | Ângulo em 1 linha | Objetivo | Horário |
|---|---|---|---|---|---|---|
| Seg | Reel | Educativo | IG9 Quebra-padrão | "você posta Reels no pior horário" | envios | 12h |
| Ter | Carrossel | Educativo | IG5 Lista | "7 erros no portfólio" | salvamentos | 11h30 |
| Qua | Stories | Engajamento | enquete + caixinha | "qual layout você clicaria?" | comentários | 19h |
| Qui | Carrossel | História | IG6 Antes e depois | "do primeiro R$ 0 aos primeiros R$ 4 mil" | seguidores | 11h |
| Sex | Imagem única | Engajamento | IG3 Situação do dia a dia | "abrir o Canva pra uma arte só" | comentários | 13h |
| Sáb | Reel | Educativo | IG10 Como eu fiz | "como eu edito um Reel em 20 min" | salvamentos | 10h |
| Dom | Stories | Promoção | repost + link | "inscrições abertas" | seguidores | 18h |

(A skill preenche ângulos reais a partir do tema. Stories rodam todo dia por
baixo dos posts do feed.)

### Meta semanal de salvamentos

- Uma **meta concreta de salvamentos + envios** para a semana. Heurística
  padrão: pelo menos 3 posts do feed em formato de salvamento (IG1, IG5, IG6,
  IG8, IG10) e pelo menos 2 de envio (IG2, IG7, IG9).
- Acompanhe salvamentos e envios como métrica principal, não curtidas.

### Rotina de stories

- 2-4 telas por dia: bastidor, uma enquete ou caixinha de perguntas e pelo menos
  um repost de post recente do feed.
- Stories aprofundam a relação com quem segue; não buscam alcance novo.

### Checagem de equilíbrio

- [ ] Formatos: pelo menos 2 Reels, pelo menos 2 carrosséis, stories todo dia
- [ ] Pelo menos 3 posts de salvamento e 2 de envio
- [ ] Pelo menos 1 post de história real em primeira pessoa
- [ ] Nenhum pilar acima de 50% dos posts do feed
- [ ] Nenhuma fórmula repetida mais de duas vezes
- [ ] Promoção com no máximo 1-2 posts
- [ ] Objetivos espalhados entre salvamentos, envios, comentários, seguidores

## Mistura de objetivos

| Objetivo | Fórmulas | Meta semanal |
|---|---|---|
| Salvamentos | IG1, IG5, IG6, IG8, IG10 | pelo menos 3 |
| Envios | IG2, IG7, IG9 | pelo menos 2 |
| Comentários | IG3, IG4 | pelo menos 1 |
| Seguidores | IG4, IG6, IG8 | pelo menos 1 |

## Regras

- **Constância vence frequência.** 4-5 posts de qualidade por semana com stories
  diários vencem um post diário de enchimento.
- **Reels para alcance, carrossel para salvamento, stories para relacionamento.**
- **Reels podem sair com mais frequência** que carrosséis sem cansar.
- **Horários:** use as janelas de `../../references/algorithm-heuristics.md`
  (seção "Horários (Brasil)") no fuso do público, e os insights da conta quando
  houver. Mato Grosso é UTC-4, uma hora atrás de Brasília.
- **Datas brasileiras:** considere feriados nacionais e locais, datas comerciais
  (Dia das Mães, Black Friday, 13º salário, volta às aulas) e eventos da região.
  Não planeje promoção pesada em feriado prolongado sem motivo.
- **Promoção com no máximo 1-2 posts por semana.**
- **Uma fórmula por vaga, variada na semana.** Não empilhe três carrosséis IG5.

## Fórmula -> pilar

| Pilar | Fórmulas preferidas |
|---|---|
| Educativo | IG5 Lista, IG8 Método, IG10 Como eu fiz, IG1 Número primeiro |
| História | IG4 Confissão, IG6 Antes e depois |
| Engajamento | IG3 Situação do dia a dia, IG9 Quebra-padrão, figurinhas nos stories |
| Promoção | IG6 (resultado que implica o produto), IG1 (seus próprios dados) |

## Passos

1. Reúna as informações. Pergunte tema, público, cidade/fuso e pilares se faltarem.
2. Confira que os pilares somam 100%; avise se algum passa de 50%.
3. Para cada dia, escolha: formato, pilar, fórmula (sem repetir demais), ângulo,
   horário (fuso do público) e objetivo.
4. Defina a meta semanal de salvamentos + envios e a rotina de stories.
5. Rode a checagem de equilíbrio e de objetivos; sinalize o que faltar.
6. Devolva em markdown, mais JSON opcional para importar.

## Exemplo

Ver `references/example-week.md`.

## Arquivos

- `SKILL.md` - este arquivo
- `references/example-week.md` - plano de 7 dias preenchido
- `references/pillars-framework.md` - a disciplina de pilares explicada

## Skills relacionadas

- `ig-caption-writer` - escreve cada legenda de imagem única do plano
- `ig-carousel-planner` - escreve cada carrossel do plano
- `ig-hashtag-strategist` - alterna conjuntos de hashtags na semana
- `ig-hook-extractor` - estuda posts de concorrentes durante o planejamento
