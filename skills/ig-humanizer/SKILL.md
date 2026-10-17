---
name: ig-humanizer
description: "Tira a cara de IA de legenda ou slide de carrossel em português: vocabulário de IA por densidade (alavancar, potencializar, jornada, crucial), pontes de revelação, frases picotadas, trios empilhados, sinceridade encenada, chuva de emoji, cara de tradução; limita travessões. Inclui --mode audit (gancho de 125, tamanho, hashtags, emoji, CTA, mídia, português correto) e --mode profile. Não serve para enganar detector de IA. Não serve para escrever do zero (use ig-caption-writer ou ig-carousel-planner)."
---

# Humanizador para Instagram V3 (PT-BR)

Reescreve legenda ou texto de slide para tirar os sinais de IA que leitores
humanos percebem, e revisa uma legenda pronta contra o checklist do Instagram de
2026. Calibrado para **português brasileiro**: os vícios de texto gerado em
português não são os mesmos do inglês (excesso de "alavancar", "jornada",
"vale ressaltar", gerúndio abrindo frase, cara de tradução), e as regras abaixo
foram reescritas para eles.

Base: a taxonomia de "sinais de escrita por IA", a literatura de estilometria de
2025-2026, padrões do Instagram (o corte de 125 caracteres, o registro casual
de legenda, hashtags dimensionadas, estrutura de salvamento e envio) e os vícios
específicos do português gerado por modelo. **V3:** vocabulário contado por
densidade, travessão com teto em vez de proibição, ritmo forçado tratado como
sinal (não como solução) e proteção contra exagero na correção.

**O que esta skill não faz:** não faz o texto "passar" em detectores (GPTZero,
Turnitin, Originality etc.). Ninguém promete isso, e em texto curto como legenda
a nota de detector é ruído. O valor real é outro: leitores atentos identificam
texto de IA pelo vocabulário e pela estrutura das frases, e no Instagram uma
legenda com cara de conta institucional não ganha nem salvamento nem envio.
Esta skill remove o que esses leitores percebem.

## O que mudou na V3

- **Vocabulário por densidade, não lista de proibidas.** Palavras como
  "crucial", "fundamental", "estratégico" são português normal. Uma por
  parágrafo não é veredito. Três no mesmo parágrafo é assinatura.
- **Travessão deixou de ser sinal.** Em português o travessão é ainda mais comum
  que em inglês. Proibir esteriliza a legenda. Regra nova: teto de cerca de 1 a
  cada 100 palavras (1-2 por legenda); o excesso vira vírgula, dois-pontos,
  parênteses ou reescrita. Nunca ponto final (dividir um travessão empilha
  fragmentos). Zero travessão numa legenda longa também é sinal.
- **Ritmo forçado é o sinal nº 1, não a solução.** "Simples. Rápido. Eficiente.",
  "Sem X. Sem Y. Só Z.", linhas de uma palavra para drama e "O resultado?" são os
  sinais que leitores mais citam hoje. A Passada 2 só protege contra uniformidade
  mecânica; nunca fabrica variação.
- **Regra de três continua sendo sinal, por densidade.** Um trio natural com
  itens concretos fica. Trio empilhado, perfeitamente paralelo ou oco sai.
- **Injeção de "humanidade" estava meio errada.** Nomes próprios e
  concretude ajudam; um número quebrado com referência nos primeiros 125
  caracteres é o gancho mais forte. Ressalvas inseridas e confissões
  enquadradas pioram: "vou ser sincero", "papo reto", "POV:" sem ser POV são
  sinais nomeados. A Passada 3 pede um fato datado, seco e desconfortável.
- **Proteção contra exagero.** Texto "humanizado" demais tem sua própria
  assinatura. A Passada 4 verifica se as passadas anteriores criaram os padrões
  que deviam remover. Edição proporcional ao problema real. Na dúvida, deixe.

## Quando usar

- Antes de publicar qualquer legenda ou carrossel rascunhado por IA (modo reescrita)
- Revisão final de uma legenda pronta (modo audit, ver `sub-skills/post-audit.md`)
- Quando a legenda "tá estranha" e você não sabe por quê
- Quando o texto veio traduzido do inglês e está com cara de dublagem

## Entrada

Qualquer texto: legenda, texto da arte de um carrossel, legenda de Reel.
Opcional: amostras de voz do usuário (legendas antigas).

## Saída

- Texto reescrito sem os sinais de IA
- Um diff mostrando o que mudou e por quê
- Contagem de caracteres (sinalizando se o gancho passou dos 125)
- Densidade de sinais por parágrafo (marcadores por parágrafo; 3+ dispararam reescrita)
- Leitura provável: "soa humano", "misto", "soa IA" (estimativa de leitor, não nota de detector)

## Modos

```bash
# Padrão: limpa sinais de IA (forense + estrito) e corrige formato do Instagram
ig-humanizer <texto>

# Só forense - toque mínimo, só remove vazamento de modelo
ig-humanizer --mode forensic <texto>

# Audit - só detecção, passa/não passa, sem reescrever
# Roda o checklist de 2026: gancho de 125, tamanho, hashtags, emoji, CTA,
# lembrete de mídia, português correto. Ver sub-skills/post-audit.md.
ig-humanizer --mode audit <texto>

# Profile - monta/atualiza o perfil de voz e marca. Ver seção abaixo.
ig-humanizer --mode profile
```

## As quatro passadas

### Passada 1 - LIMPAR (contar, depois cortar ou trocar)

Aplique os catálogos de `references/scrub-rules.md`. A unidade de julgamento é
o **parágrafo (ou slide), não a palavra**: conte marcadores por parágrafo,
reescreva o parágrafo com 3+, deixe um marcador isolado em paz a menos que seja
ponte de revelação, paralelismo negativo, marcador de sinceridade ou vazamento forense.

- **Forense** (sempre ligado): vazamento real de modelo que ninguém digita.
  Marcadores de ferramenta (oaicite, contentReference, turn0search0), avisos de
  corte de conhecimento ("Até a minha última atualização"), lacunas de modelo
  ([Seu Nome], [Nome da Empresa]), e travessões acima do teto.
- **Estrito** (ligado por padrão): o que leitores percebem. Vocabulário de IA
  em português por densidade (alavancar, potencializar, impulsionar, crucial,
  fundamental, robusto, abrangente, jornada, cenário, ecossistema, insights,
  mergulhar, desbloquear, elevar), marcadores gramaticais (gerúndio abrindo
  frase, nominalizações "a implementação de", conectores de redação "vale
  ressaltar", "nesse sentido"), cara de tradução ("no final do dia", "faz
  sentido?", "isso dito"), pontes de revelação ("O resultado?", "Seguinte:",
  "Pare de X, comece a Y", "Spoiler:"), todo paralelismo negativo ("não é só X,
  é Y"), trios empilhados ou ocos, frases mortas ("no mundo de hoje", "virada de
  chave", "próximo nível"), chuva de emoji e fechamentos mortos ("o que vocês
  acham?", "dá dois toques se concorda").
- **Formato do Instagram** (sempre): gancho de 125 que se sustenta, hashtags
  dimensionadas, limite de emoji, promessa no slide 1, tamanho da legenda.
- **Português correto** (sempre, sem mudar o registro): concordância, crase,
  "mas/mais", "há/a", "porque/por que", "a gente vai" (não "a gente vamos").
  Corrija erro; não corrija gíria ou oralidade intencional.

### Passada 2 - RITMO (só proteção contra uniformidade)

Legendas têm tamanho médio, então ritmo não é alavanca de alcance nem solução.
O que leitores percebem é a uniformidade mecânica (toda frase do mesmo tamanho)
e, pior, a variação encenada que "humanizadores" adicionam. A Passada 2 corrige
ritmo só onde soa mecânico e remove variação fabricada em todo lugar. Nunca
adiciona variação como tática.

- Por parágrafo: uma frase realmente longa ao lado de uma curta é variação
  humana normal. Duas ou três frases médias seguidas também. Edite só quando
  toda frase do parágrafo tem o mesmo tamanho e soa chapada, e aí edite uma
  frase, não o parágrafo.
- Fragmentos soltos: no máximo 2 por legenda. "toda vez." uma vez é jeito de
  falar. Três numa legenda é padrão.
- Proibidos (reescreva como frase completa): revelação "O X? Y."; "Sem X. Sem Y.
  Só Z."; "Todo o X. Nada do Y."; pilha de adjetivos "Simples. Rápido. Fácil.";
  linha de uma palavra para drama ("Pois é." "Exatamente."); pergunta-resposta
  pseudo-socrática ("Por quê? Porque..."); "Curto. Direto. Feito.".
- Layout não é ritmo. Uma ideia por linha com linha em branco entre elas é
  formatação nativa de legenda e fica. O sinal é o fragmento-para-drama dentro
  dessas linhas. Mantenha o layout, arrume as frases.
- Texto de slide é curto por natureza; nunca corte nem encha um slide por ritmo.
  Nunca alterne longa/curta/longa/curta. Essa gangorra é a assinatura do humanizador.

### Passada 3 - ACRESCENTAR (marcas humanas)

Exija onde o conteúdo permite:
- Um número quebrado COM referência: quem, o quê, quando ou quanto custou
  ("de 0 a 10 mil em 4 meses, postando 3 vezes por semana", não "cresci rápido"
  nem "10 mil"). Número solto não é marca; a referência carrega o sinal.
- Um nome próprio (pessoa real, marca, data, ferramenta, cidade)
- Um detalhe concreto em primeira pessoa (o que você viu, quanto custou, o que quebrou)
- Um fato específico, datado e desconfortável dito seco, sem frase de moldura
  antes ou depois. Não "papo reto, essa doeu: perdi o cliente." Só "perdi meu
  maior cliente no dia 14 de fevereiro." O fato carrega a vulnerabilidade. A
  moldura transforma em sinceridade encenada.
- O registro casual minúsculo, se for a voz do perfil

Proibido como abertura ou virada (anúncio de sinceridade, sinal nomeado):
"vou ser sincero", "sendo bem honesto", "vou ser direto", "papo reto", "real
oficial", "verdade seja dita", "vou te contar uma coisa", "posso ser
vulnerável?", "opinião impopular:" antes de opinião popular, "POV:" em algo que
não é ponto de vista, "senta que lá vem história". Também proibido inserir
ressalvas que o autor não escreveu ("talvez", "posso estar errado, mas",
"parece que"). Hesitação encenada é bem mais comum em texto de IA; acrescentar
deixa o rascunho mais IA, não menos.

Se a entrada não tem nada disso, peça ao usuário um número, um nome ou um
momento. Não invente.

### Passada 4 - AUTOCONFERÊNCIA (proteção contra exagero)

Antes de devolver, releia o resultado uma vez e responda três perguntas:

(a) A Passada 2 criou pilhas picotadas, ponte "O resultado?", linhas de uma
    palavra ou gangorra longa/curta? Se sim, junte os fragmentos em frases completas.
(b) A Passada 3 acrescentou confissão enquadrada, anúncio de sinceridade ou
    ressalva que o autor nunca escreveu? Se sim, tire a moldura e deixe só o fato.
(c) A limpeza achatou a voz do autor: tom uniforme, sem reação, sem detalhe
    concreto, todos os travessões sumiram, todos os trios sumiram, todos os
    emoji sumiram de uma voz que usa, o registro minúsculo foi capitalizado, a
    gíria regional foi "corrigida"? Se sim, devolva o que o autor tinha.

Se alguma resposta for sim, recue em vez de limpar mais. Edição proporcional ao
problema: uma legenda limpa recebe dois ou três toques, não uma cota. Na dúvida
se o padrão é do autor ou do modelo, deixe.

## Regras inegociáveis

Regras de voz globais: ver "Regras de voz" no `SKILL.md` raiz. Regras específicas:

- **A limpeza está sempre no escopo.** Quando pedirem para humanizar, tirar cara
  de IA, finalizar ou publicar uma legenda, rode pelo menos as passadas forense
  + estrita antes de sair. Vale quando o usuário escreveu o rascunho, diz que
  amou como está ou está com pressa. A passada forense + estrita não muda o
  sentido e leva segundos. Se uma restrição realmente proíbe mexer no texto,
  diga explicitamente e liste cada sinal que ficou.
- **Limpe na proporção.** Uma passada que não acha nada não muda nada. Não
  invente edição para justificar a execução, e não reporte nota de detector
  como resultado; reporte os sinais achados e corrigidos.
- Preserve a afirmação e o sentido do usuário. "Preservar a voz" cobre jeitos de
  falar e o que ele afirma, NÃO pontes de revelação, pilhas picotadas ou
  parágrafo com 3+ marcadores.
- Nunca introduza fatos que não estavam na entrada. Faltou número, pergunte.
- Nunca introduza marcador de sinceridade, ressalva ou moldura confessional.
- Mantenha os jeitos do usuário (início minúsculo, `..`, um travessão a cada
  ~100 palavras, um trio natural, os 1-3 emoji intencionais, regionalismos como
  "bah", "uai", "égua", "mano" quando são dele).
- Nunca prometa resultado em detector. Se perguntarem "vai passar no GPTZero?",
  responda com honestidade: ninguém garante, e a nota numa legenda de 150
  palavras é ruído.
- Respeite a superfície: não transforme texto de slide em legenda ou vice-versa
  sem avisar.
- Nunca apresente legenda como publicável sem o lembrete de mídia.

## Sinais específicos do Instagram que esta skill pega

- Gancho que precisa da segunda linha (o corte do "mais" come).
- Legenda abrindo com "Vamos falar sobre..", "Seguinte..", "Bora falar de..",
  "Você sabia que..?" sem dado surpreendente, ou "POV:" sem ser POV.
- 20-30 hashtags no topo em vez de 3-5 dimensionadas.
- Emoji espalhado em toda linha (4+ por legenda).
- Título seco no slide 1 em vez de promessa + laço.
- Regra de três empilhada ou oca ("mais rápido, mais barato, melhor").
- Isca de engajamento ("comenta SIM", "marca 3 amigos", "dá dois toques se").
- Pilhas picotadas e linhas de uma palavra (a assinatura do humanizador), ou
  parágrafo em que toda linha soa chapada.
- Cara de tradução: expressões calcadas no inglês, "você" repetido em toda
  frase, ordem de adjetivo do inglês ("incrível oportunidade").

## Exemplo

Ver `references/examples.md`.

## Arquivos

- `SKILL.md` - este arquivo (limpeza + entrada do modo audit)
- `references/scrub-rules.md` - padrões V3 em português por nível, densidade, teto de travessão, ritmo, inserções proibidas
- `references/examples.md` - reescritas antes/depois comentadas
- `references/audit-checklist.md` - checklist de pré-publicação com limites
- `sub-skills/post-audit.md` - fluxo de revisão pré-publicação (só detecção)
- `sub-skills/voice-profile.md` - monta/atualiza o perfil de voz (`--mode profile`)
- `sub-skills/illustration.md` - fluxo opcional de imagem com Pixfaro

## Modo perfil de voz (`--mode profile`)

`ig-humanizer --mode profile` monta ou atualiza o perfil de voz e marca em
`../../references/voice-profile.md` a partir de 3-6 posts reais do usuário
colados (portátil, sem token) ou, se houver token de leitura, da atividade
puxada. Depois de preenchido, toda skill de escrita do pacote rascunha na voz do
usuário automaticamente. Ver `sub-skills/voice-profile.md`. Gatilhos: "monta meu
perfil de voz", "aprende minha voz".

## Skills relacionadas

- `ig-caption-writer` - gera legendas que já passam pelo humanizador
- `ig-carousel-planner` - gera carrosséis que já passam pelo humanizador
