# Revisão de post do Instagram

Passa qualquer legenda (e o texto dos slides) pelo checklist de ranqueamento de
2026. Pega sinais de IA, violações de formato (gancho de 125, tamanho,
hashtags, emoji), o que derruba alcance (isca, mídia misturada), erros de
português e fraquezas de estrutura antes de publicar. Este é o fluxo
`ig-humanizer --mode audit`: só detecção, sem reescrever.

## Quando usar

- Antes de publicar uma legenda ou carrossel escrito à mão ou por IA
- Quando `ig-caption-writer` ou `ig-carousel-planner` termina um rascunho (automático)
- Quando um post recente foi mal e o usuário quer entender por quê

## Entrada

- Uma legenda, e num carrossel o texto dos slides
- Opcional: público, horário agendado, superfície (imagem / carrossel / Reel)

## Saída

- Cabeçalho **APROVADO / REPROVADO**
- **Bloqueios** (corrigir antes de publicar)
- **Alertas** (risco de publicar)
- **Correções sugeridas** para cada problema
- **Densidade de sinais por parágrafo** (marcadores, travessões a cada 100
  palavras, fragmentos, trios). Sem nota de detector
- **Recomendação de horário** para o público (no fuso dele)

## Checagens

### Bloqueios (reprova automática)

1. Densidade de travessão acima de cerca de 1 a cada 100 palavras (mais de um
   por slide); meia-risca entre orações; hífen duplo. Um travessão sozinho não bloqueia.
2. O gancho precisa da segunda linha (os 125 primeiros caracteres não se sustentam).
3. Legenda acima de 2.200 caracteres.
4. 20-30 hashtags, ou hashtag no meio da frase.
5. Isca de engajamento ("comenta SIM", "marca 3 amigos", "dá dois toques se concorda").
6. Abre com "No mundo de hoje" ou similar, ponte de revelação ("Seguinte..",
   "Vamos falar sobre..", "Pare de X, comece a Y") ou anúncio de sinceridade
   ("papo reto", "vou ser sincero", "POV:" sem ser POV).
7. Termina com "O que vocês acham?", "Comentem aí!" ou "Pensa nisso." (pedido
   de salvar/enviar com motivo é ok).
8. Parágrafo com 3+ marcadores, ou paralelismo negativo / ponte "O resultado?"
   (ver `../references/scrub-rules.md`).
9. Erro de português que custa credibilidade (concordância, crase, "mas/mais",
   gerundismo).
10. Promessa de resultado garantido em setor regulado (consórcio, crédito,
    educação, saúde, imóveis).
11. Carrossel mistura imagem e vídeo, ou tem mais de 10 slides.
12. Sem plano de mídia.

### Alertas (sinalizar com correção)

13. Mais de 5 hashtags, ou conjunto só de amplas.
14. 4+ emoji, emoji em toda linha, ou a fila foguete/brilho/fogo/setinha.
15. Parágrafo mecânico (4+ frases do mesmo tamanho, sem oração trabalhando).
    Sinalize só ele; nunca sugira adicionar variação.
16. Pilhas picotadas, linha de uma palavra para drama, mais de 2 fragmentos,
    gangorra longa/curta.
17. Nenhum número quebrado com referência onde a afirmação permitiria.
18. Nenhum nome próprio (pessoa, marca, ferramenta, cidade).
19. Trio empilhado, paralelo ou oco, ou 3+ trios na legenda.
20. Pilha de ressalvas ou confissão enquadrada no meio da legenda (como
    abertura é o bloqueio 6).
21. Cara de tradução ("no final do dia", "faz sentido?", "é com grande alegria que").
22. Limpou demais (só ao revisar uma reescrita com o original em mãos).
23. Slide 1 do carrossel é título seco.
24. O ponto mais forte do carrossel está enterrado no fim.
25. Sem objetivo principal claro (busca salvamentos, envios, comentários e
    seguidores ao mesmo tempo). Ver "Divisão por objetivo" em
    `../../../references/hook-formulas.md`.
26. CTA genérico ou três CTAs competindo.

### Informativo

27. Janela de postagem sugerida para o público, no fuso dele.
28. Superfície recomendada (imagem vs carrossel vs Reel) para o material.
29. Oportunidade de salvamento/envio: se é lista/método/tutorial, estruture para
    salvamentos; se é opinião contrária ou mito, estruture para envios.

## Passos

1. Detecte a superfície: imagem única, carrossel ou Reel.
2. Meça a legenda: caracteres, onde cortam os 125 primeiros, quantidade e
   dimensionamento das hashtags, emoji, travessões a cada 100 palavras.
3. Rode os bloqueios. Se algum falhar, devolva **REPROVADO** com correções
   específicas; ofereça passar para `ig-humanizer` reescrever.
4. Sem bloqueios, rode os alertas.
5. Reporte a densidade por parágrafo. Não estime nota de detector.
6. Devolva o relatório com nota de horário e lembrete de mídia.

## Relacionado

- `ig-humanizer` - reescrita proporcional se a revisão reprovar
- `ig-caption-writer` / `ig-carousel-planner` - refazer com uma fórmula testada
