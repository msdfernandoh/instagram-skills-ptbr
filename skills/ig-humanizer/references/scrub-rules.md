# Regras de limpeza (Instagram, V3, PT-BR)

Catálogos por nível que o humanizador aplica. Carregue este arquivo quando for
de fato executar uma limpeza. Dois níveis: forense (sempre ligado) e estrito
(ligado por padrão), mais as limpezas de formato do Instagram. V3: vocabulário
contado por **densidade por parágrafo**, não apagado palavra a palavra.
Travessão com **teto** de cerca de 1 a cada 100 palavras, não proibido. Ritmo
forçado é sinal, não solução.

Todos os padrões abaixo assumem texto em português brasileiro, com acentos.
Rode as expressões com `re.I` e trate o texto como Unicode (Python 3 já faz isso).

## Sumário

- Contagem por densidade (como toda regra de vocabulário é aplicada)
- Nível FORENSE (sempre ligado)
- Nível ESTRITO (ligado por padrão)
- Limpezas de formato do Instagram (sempre)
- Passada 2 - Ritmo (só proteção contra uniformidade)
- Passada 3 - Inserções proibidas (sinceridade, ressalvas)
- Preserve (voz do usuário, não limpar)

---

## Contagem por densidade

O princípio do aglomerado: leitores identificam texto de IA por aglomerados de
marcadores, não por uma palavra isolada. Um "fundamental" num parágrafo é
português. "Fundamental", "jornada" e uma nominalização no mesmo parágrafo é assinatura.

```python
def score_paragraph(paragraph: str, markers: dict) -> dict:
    """Conta marcadores por parágrafo (ou por slide). Devolve as ocorrências e a ação."""
    hits = []
    for name, pattern in markers.items():
        for m in re.finditer(pattern, paragraph, flags=re.I):
            hits.append((name, m.group(0)))
    n = len(hits)
    always = [h for h in hits if h[0] in ("reveal_bridge", "neg_parallel", "sincerity_marker")]
    if n >= 3:
        action = "REWRITE_PARAGRAPH"   # 3+ marcadores = sinal. Reescreva o parágrafo, não palavra por palavra.
    elif always:
        action = "REPLACE"             # ponte de revelação / paralelismo negativo / sinceridade sai sempre,
                                       # mesmo dividindo o parágrafo com um marcador comum (checado ANTES de n == 2)
    elif n == 2:
        action = "FLAG_ONLY"           # 2 = limite. Reporte e deixe as palavras.
    else:
        action = "LEAVE"               # uma palavra comum não é veredito
    return {"hits": hits, "count": n, "action": action}
```

Como aplicar:
- Marcadores forenses contam à parte: uma ocorrência = apagar, sem limite de densidade.
- Duas contagens são por legenda: trios (3+ na legenda = reduzir ao primeiro
  natural) e fragmentos soltos (3+ na legenda = juntar de volta, ver Passada 2).
- Nunca troque uma palavra por sinônimo da mesma lista. "Alavancar" por
  "potencializar" não é correção.
- Ao reescrever um parágrafo, reescreva no registro do autor (casual minúsculo
  continua casual minúsculo, os emoji dele ficam), não num registro "neutro".
  Neutralidade uniforme também é assinatura.

---

## Nível FORENSE (sempre ligado)

Vazamento real de modelo que ninguém digita. Apague ou sinalize na hora.

| Padrão | Ação |
|---|---|
| `oaicite`, `contentReference`, `turn0search0`, `attached_file`, `grok_card` | apagar o marcador |
| "Até a minha última atualização", "Com base no meu conhecimento até", "Não consigo navegar", "Como modelo de linguagem" | apagar a linha |
| `[Seu Nome]`, `[Nome da Empresa]`, `[inserir X aqui]`, `[Cidade]`, `DD/MM/AAAA` sem preencher | sinalizar, pedir ao usuário |
| Travessões acima do teto (abaixo) | trocar o excesso por vírgula, dois-pontos, parênteses ou reescrita; nunca ponto final |
| Resto de tradução automática: aspas inglesas misturadas (`"..."` com `“...”`), "Sr./Sra." onde a fonte tinha "Mr.", números no formato americano (`1,250.00`, `$`) num texto em reais | normalizar para o padrão brasileiro (`R$ 1.250,00`) se o número for do usuário; sinalizar se for ambíguo |

### Teto de travessão (cerca de 1 a cada 100 palavras)

Em português o travessão é ainda mais comum que em inglês (diálogo, aposto,
ênfase). O caractere não denuncia IA; a densidade denuncia. O que continua
forense é o vício antigo de colar a legenda inteira com travessões: 3+ numa
legenda curta.

```python
def em_dash_excess(text: str) -> int:
    """Quantos travessões passam do teto (~1 a cada 100 palavras, mínimo 1, máximo 2 por legenda).
    0 = deixe todos. Um slide de carrossel é uma unidade com teto 1."""
    words = len(text.split())
    cap = max(1, min(2, round(words / 100)))
    return max(0, text.count("—") - cap)

# Ordem de troca para o EXCESSO (mantenha o que trabalha mais, geralmente o primeiro):
#   1. vírgula        se o travessão liga uma oração à frase principal
#   2. dois-pontos    se introduz revelação, lista ou consequência
#   3. parênteses     se o par de travessões envolve um aposto
#   4. quebra de linha  nativo do Instagram: o aposto ganha linha própria
#   5. reescrita      se nada acima soa natural
# NUNCA ponto final. "X. Y." de um travessão dividido cria pilha de fragmentos, que é pior que o travessão.
# Travessão de diálogo (início de fala, em legenda que conta uma conversa) não conta para o teto.
```

---

## Nível ESTRITO (ligado por padrão)

O que leitores atentos citam quando identificam texto de IA: vocabulário e
estrutura das frases. Todas as listas de vocabulário e gramática passam por
`score_paragraph()`; pontes de revelação, paralelismo negativo e marcadores de
sinceridade saem com uma ocorrência só.

### Pontuação

- `--` -> vírgula ou quebra de linha (não ponto final).
- Meia-risca (`–`) entre orações -> vírgula. Intervalos numéricos (7-9, 18h-21h) ficam.
- Travessões tratados por `em_dash_excess()`, não removidos.
- Reticências de três pontos em toda frase ("e aí... deu certo...") -> mantenha
  no máximo uma; o `..` de pausa leve é voz e fica.
- Exclamação múltipla ("!!!") e frases todas terminando em "!" -> no máximo uma
  exclamação na legenda, salvo se for a voz do perfil.

### Vocabulário: marcadores de IA em português (por densidade)

Palavras que modelos escolhem bem acima da taxa humana em português. São
português comum, então uma por parágrafo é ok. Três num parágrafo é assinatura.

| Marcador | Troca preferida quando o parágrafo passa do limite |
|---|---|
| alavancar, potencializar, impulsionar, turbinar | usar, aumentar, ajudar (ou o verbo concreto: "vender mais", "fechar 3 contratos") |
| otimizar, maximizar | melhorar, aproveitar melhor (ou o número) |
| crucial, fundamental, essencial, primordial | apagar, ou "o que importa é" (melhor: diga o porquê) |
| robusto, sólido, abrangente, holístico, completo | diga o que tem dentro |
| significativo, expressivo, considerável | um número ("31% mais salvamentos"; pergunte se não houver) |
| estratégico, assertivo | apagar, ou diga a decisão |
| incrível, poderoso, transformador, disruptivo, revolucionário, inovador | apagar, ou o fato que impressiona |
| jornada | caminho, processo (ou o nome real: "os 6 meses até a contemplação") |
| cenário, panorama, universo (de algo), ecossistema | mercado, área, setor (ou apagar) |
| insights | diga o que aprendeu |
| mindset | jeito de pensar (ou a decisão concreta) |
| mergulhar (em), aprofundar (figurado) | entrar em, ver de perto |
| desbloquear, destravar | abrir, liberar |
| elevar (figurado), levar ao próximo nível | melhorar (com o número) |
| navegar (figurado) | lidar com |
| fomentar, catalisar | criar, puxar |
| agregar valor | diga qual valor |
| ressignificar | mudar o jeito de ver (ou apagar) |
| protagonismo, protagonista da sua história | apagar |
| sinergia, paradigma | apagar |
| transformar vidas, fazer a diferença | o resultado concreto para uma pessoa |
| excelência, qualidade (como slogan) | a prova da qualidade |

Advérbios de enchimento (cada um conta como um marcador; apague acima do limite):
fundamentalmente, essencialmente, basicamente, efetivamente, certamente,
notavelmente, indiscutivelmente, verdadeiramente, definitivamente, literalmente
(quando não é literal), realmente (repetido), simplesmente (repetido).

Conectores de redação escolar (cada um conta como marcador; abrindo parágrafo pesam dobrado):

```python
CONNECTORS_PT = [
    r"\bvale (ressaltar|destacar|lembrar|mencionar) que\b",
    r"\bé importante (destacar|ressaltar|lembrar|frisar|mencionar|notar) que\b",
    r"\bcabe (ressaltar|destacar|lembrar) que\b",
    r"(?m)^(nesse|neste) sentido,",
    r"(?m)^diante disso,",
    r"(?m)^dessa (forma|maneira),",
    r"(?m)^além disso,",
    r"(?m)^(por fim|em suma|em resumo|em síntese|concluindo),",
    r"(?m)^ademais,",
    r"\bnão (só|somente|apenas) \w+ como também\b",
]
```

### Marcadores gramaticais (por densidade; a assinatura estrutural)

```python
GRAMMAR_MARKERS_PT = {
    # Gerúndio abrindo frase: "Pensando nisso, ...", "Visando otimizar, ...", "Buscando ..., a gente ..."
    "gerund_opener": r"(?m)^[\s>*\-]*[A-Za-zÀ-ú]+(ando|endo|indo|ondo)\b[^.\n]{0,60},",
    # Nominalização: verbo virado substantivo que esconde quem faz. "a implementação de", "o desenvolvimento de"
    "nominalisation": r"\b[ao]s? (\w+(?:ção|ções|são|mento|mentos|ância|ência|agem)) d[eoa]s?\b",
    # Substantivos abstratos empilhados
    "abstract_stack": r"\b(transformação|inovação|otimização|eficiência|escalabilidade|sinergia|excelência|crescimento|resultados)\b.{0,40}\b(transformação|inovação|otimização|eficiência|escalabilidade|sinergia|excelência|crescimento|resultados)\b",
    # Gerundismo de telemarketing: "vou estar enviando", "vamos estar fazendo"
    "gerundismo": r"\b(vou|vamos|vai|vão|irei|iremos) estar \w+(ando|endo|indo)\b",
}
# Correção do gerúndio de abertura: comece pelo sujeito. "Pensando nisso, criamos X" -> "por isso a gente criou X."
# Correção da nominalização: use o verbo. "a implementação do novo processo" -> "quando a gente mudou o processo"
# Gerundismo: corrija sempre ("vou estar enviando" -> "vou enviar" / "te mando").
# A nominalização só conta quando esconde a ação; "o lançamento do evento" como nome de coisa é normal.
```

### Cara de tradução (por densidade; uma ocorrência já merece atenção)

Expressões calcadas do inglês que denunciam texto gerado ou traduzido:

```python
CALQUES_PT = [
    r"\bno final do dia\b",                  # at the end of the day
    r"\bfaz sentido\?",                      # makes sense?
    r"\bisso dito,",                         # that said
    r"\bdito isso,",
    r"\bsem mais delongas\b",
    r"\bcomo um todo\b",
    r"\b(uma|a) (incrível|enorme|grande) (oportunidade|jornada|experiência)\b",  # ordem de adjetivo do inglês, como clichê
    r"\bestar (animad[oa]|empolgad[oa]) (em|para) (anunciar|compartilhar)\b",   # excited to announce/share
    r"\bé com (grande )?(alegria|satisfação|orgulho) que\b",
    r"\bnão poderia(m)? estar mais (feliz|felizes|orgulhos[oa]s?)\b",          # couldn't be happier
    r"\bjogo (mudou|virou)\b|\bmudou o jogo\b",                                # game changer
    r"\bcuidar de si mesm[oa]\b",
    r"\bsua melhor versão\b",
]
```

### Camada de idiomas de modelo (por densidade)

Expressões que eram jeito de legenda humana em 2023-24 e viraram vício de
modelo em 2026. Cada uma conta como marcador; "deixa isso entrar" e "pensa
nisso" como fechamento saem com uma ocorrência.

```python
IDIOM_LAYER_PT = [
    r"\bsilenciosamente\b",                   # "silenciosamente lançou"
    r"(?m)^\w+ importa\.$",                   # "constância importa." sozinho na linha
    r"\bjuros compostos\b(?! d)",             # usado como metáfora ("constância é juros compostos")
    r"\bo trabalho (invisível|de base)\b",
    r"\bsobre isso\.$",                       # "é sobre isso." / "e tá tudo bem."
    r"\be (tá|está) tudo bem\.?$",
    r"\bé sobre (isso|aquilo)\b",
    r"\b(deixa|deixe) isso (entrar|assentar)\b",
    r"\bpensa(e)? nisso\.?$",
    r"\bfoi aí que (tudo mudou|eu entendi)\b",
    r"\bvirada de chave\b",
    r"\bsair da zona de conforto\b",
    r"\bplot twist\b",
]
```

### Pontes de revelação (uma ocorrência = trocar)

```python
REVEAL_BRIDGES_PT = [
    (r"(?im)^(o|a) (resultado|resposta|lição|pegadinha|verdade|segredo|detalhe)\?\s*", ""),   # "O resultado?"
    (r"(?i)\bnão é (só|apenas|sobre) [^,.]{1,40}[,.] é\b", None),        # "Não é só X, é Y" (reescreva como afirmações pareadas)
    (r"(?i)\bnão se trata de [^,.]{1,40}, (mas|e sim) de\b", None),        # "Não se trata de X, mas de Y"
    (r"(?i)^pare de [^,.]{1,40}[.,] comece a\b", None),                    # "Pare de X. Comece a Y"
    (r"(?im)^(seguinte|é o seguinte|olha só|sabe o que é)[:.!]\s*", ""),   # "Seguinte:"
    (r"(?im)^(vamos|bora) falar (sobre|de)\b[^.\n]{0,40}[.:?!]\s*", ""),   # "Vamos falar sobre.."
    (r"(?im)^(spoiler|plot twist|a reviravolta)[:!]\s*", ""),
    (r"(?im)^(e sabe (o que|qual) (aconteceu|foi o resultado))\?\s*", ""),
    (r"(?im)^(você sabia que)\b", None),                                   # só sai se a frase seguinte não trouxer dado surpreendente
]
# Correção: apague a ponte e deixe a frase seguinte em pé. Ela já era o ponto.
```

### Paralelismo negativo (uma ocorrência = reescrever)

Tire as construções "não é X, é Y" / "não se trata de X, mas de Y" e todas as
irmãs ("A questão não é X, é Y", "Isso não é X. Isso é Y.", "Não é sobre X. É
sobre Y."). Reescreva como afirmações pareadas, não por substituição automática,
e sinalize ao usuário, porque preservar o sentido exige julgamento.

### Regra de três (estrita por densidade; um trio natural é permitido)

```python
NO_NO_JUST_PT = r"\b(sem \w+)[,.] (sem \w+)[,.] ((?:só|apenas|somente) \w+)"   # "Sem X. Sem Y. Só Z."

def detect_triads(text: str) -> list:
    patterns = [
        r"(\w+), (\w+),? e (\w+)",                          # trio de palavras
        r"(\w+ \w+), (\w+ \w+),? e (\w+ \w+)",              # trio de expressões curtas
        r"(?m)^(\w+)\. (\w+)\. (\w+)\.$",                   # "Simples. Rápido. Fácil."
        r"(?i)\bmais (\w+), mais (\w+) e mais (\w+)\b",     # "mais rápido, mais barato e mais fácil"
        NO_NO_JUST_PT,
    ]
    return [m for p in patterns for m in re.finditer(p, text, flags=re.I)]

HOLLOW_ADJECTIVES_PT = {"dinâmico", "dinâmica", "inovador", "inovadora", "rápido", "rápida", "barato",
    "barata", "melhor", "simples", "eficiente", "eficaz", "fácil", "prático", "prática", "moderno",
    "moderna", "seguro", "segura", "confiável", "completo", "completa", "incrível", "poderoso",
    "poderosa", "acessível", "personalizado", "personalizada", "exclusivo", "exclusiva"}
ABSTRACT_NOUNS_PT = {"crescimento", "impacto", "valor", "inovação", "eficiência", "resultados",
    "sucesso", "clareza", "liberdade", "escala", "constância", "consistência", "mindset", "estratégia",
    "visão", "propósito", "qualidade", "segurança", "confiança", "credibilidade", "excelência",
    "praticidade", "tranquilidade", "conquista", "realização"}

# hollow(): um trio é oco quando os itens são intercambiáveis: todos adjetivos ou substantivos
# abstratos das listas acima, e nenhum traz um "recibo" (nome próprio, número, R$, %).
# "Racon, Gauchinho e UNIFECAF" é trio concreto e natural. "segurança, praticidade e tranquilidade"
# é oco. Maiúscula no início de frase não é nome próprio.

# triad_action(): por legenda. Trio oco sai na hora. "Sem X. Sem Y. Só Z." sai sempre.
# Trios naturais: um ou dois passam; com 3+ na legenda, reduza ao PRIMEIRO natural.
# Reescreva como 2 itens, ou 4 com um que quebre o padrão.
```

Atenção especial a perfis comerciais: "segurança, solidez e tranquilidade",
"agilidade, transparência e confiança", "qualidade, preço justo e bom
atendimento" são trios ocos típicos de texto institucional. Troque por uma
prova concreta ("47 anos de mercado", "parcela de R$ 612 sem juros", "atendimento
no WhatsApp até 22h").

### Frases mortas (apagar ou reescrever)

- "no mundo de hoje", "nos dias de hoje", "em um mundo cada vez mais (conectado|competitivo|digital)", "na era digital", "na era da IA", "atualmente, mais do que nunca"
- "no final das contas" (como muleta), "a verdade é que", "a dura verdade é que"
- "game changer", "virada de chave", "próximo nível", "subir de nível", "imperdível", "o segredo que ninguém te conta", "sem sombra de dúvidas", "mudança de paradigma"
- "o universo de {coisa}", "o mundo de {coisa}"
- "não perca essa oportunidade", "vagas limitadas" (só se for verdade e com número)

### Fechamentos mortos (reescrever como aterrissagem ou pedido específico)

- "O que vocês acham?" / "E você, o que acha?" sem pergunta específica
- "Comentem aí!" / "Conta pra mim nos comentários!"
- "Dá dois toques se concorda." / "Curte se você também..."
- "Marca 3 amigos que precisam ver isso."
- "Comenta SIM aqui embaixo." (isca sem entrega)
- "Pensa nisso." / "Fica a reflexão."
- "Fica a dica!" como fechamento genérico
- Linha final de uma palavra ("Bora!" "Simples assim.")

Um pedido de salvar ou enviar que diz o motivo ("salva pra consultar antes da
assembleia") não é fechamento morto; é o CTA que o Instagram premia. "Comenta
TABELA que eu te mando no direct" é válido se a automação existe e entrega.

### Chuva de emoji

- 4+ emoji na legenda, ou emoji em toda linha: corte para 0-3 com intenção.
- A assinatura de IA de 2024-25 (foguete, brilhos, fogo, 100, mãos pra cima,
  setinha para baixo em fila; ou o check verde em toda linha de lista) sai como
  conjunto com uma ocorrência.
- Os 1-3 emoji intencionais do autor são voz e ficam.

## Limpezas de formato do Instagram (sempre)

- Gancho que precisa da linha 2: reescreva para os 125 primeiros caracteres se sustentarem.
- 6+ hashtags, ou hashtag no meio da frase: corte para 3-5 dimensionadas no fim
  ou no primeiro comentário (ver `../../../references/hashtag-strategy.md`).
- Título seco no slide 1: reescreva como promessa + laço aberto.
- Legenda acima de 2.200 caracteres: aperte.
- Texto na arte: no máximo um travessão por slide, e só se trabalhar.

---

## Passada 2 - Ritmo (só proteção contra uniformidade)

Corrige ritmo só onde soa mecânico, remove variação fabricada em todo lugar,
nunca adiciona variação como tática.

```python
STACCATO_TELLS_PT = [
    r"(?m)^\w+[.!]$",                                                 # linha de uma palavra para drama: "Pois." "Exato!"
    r"(?m)^(\w+\. ){2,}\w+\.$",                                       # "Simples. Rápido. Fácil."
    r"(?i)\bsem \w+\. sem \w+\. (só|apenas|somente) \w+",            # "Sem X. Sem Y. Só Z."
    r"(?i)\btodo o \w+\. nada d[eoa] \w+",                            # "Todo o X. Nada do Y."
    r"(?im)^(o|a) (resultado|resposta|lição|verdade|segredo)\?",      # "O resultado?" (também ponte estrita)
    r"(?i)\b(por quê|como|o que aconteceu)\? (porque|simples|fácil)\b",  # pergunta-resposta pseudo-socrática
    r"(?i)\b(é isso|só isso|ponto final|simples assim|sem mais)\.$",
]

# is_standalone_fragment(s): linha curta (< 4 palavras) sem sujeito + verbo conjugado: "Pois é.",
# "Exatamente.", "Sem desculpas.", "Toda santa vez." Uma frase curta completa ("Deu certo.",
# "As vendas subiram.") não é fragmento e nunca conta para o limite.

# restore_rhythm(text): mesmo algoritmo da versão original:
#   1. Mate o ritmo encenado: junte cada pilha picotada numa frase completa com oração de verdade.
#      "Sem filtro. Sem preset. Só luz." -> "sem filtro nem preset, só a luz da janela."
#   2. Limite fragmentos soltos a 2 por LEGENDA; o terceiro em diante se junta à frase anterior
#      com vírgula ou dois-pontos.
#   3. Desachate SÓ parágrafo mecânico: 4+ frases, todas a ±3 palavras da média, nenhuma oração
#      subordinada. Aí junte a frase de mais conteúdo à vizinha com uma oração que trabalha
#      (porque / que / quando / depois que), uma vez por parágrafo.
#   4. Nunca longa/curta/longa/curta. Se o parágrafo alterna (4+ frases, curtas < 8 palavras,
#      longas >= 16), dobre a SEGUNDA curta na frase anterior.
#   5. Legendas de uma ideia por linha e slides de carrossel: não mexa no ritmo.
#   A passada edita frases, nunca as quebras de linha.
```

Layout vs ritmo: linhas de uma frase com linha em branco entre elas são o layout
nativo de legenda e **não** são tocadas. "cresci de 0 a 10 mil seguidores em 4
meses." numa linha só é layout. "Pois é." numa linha só é fragmento para drama.

## Passada 3 - Inserções proibidas (sinceridade, ressalvas)

A Passada 3 só acrescenta concretude (número quebrado com referência, nome
próprio, fato seco e datado). Nunca acrescenta o que está abaixo, e a Passada 1
estrita remove quando o rascunho já traz como abertura ou virada:

```python
SINCERITY_MARKERS_PT = [
    # Ancorado no início da linha OU após fim de frase, para pegar a virada na mesma linha.
    r"(?im)(?:^|(?<=[.!?]\s))(vou ser (sincer[oa]|honest[oa]|diret[oa]|bem sincer[oa])|sendo (bem |muito )?(sincer[oa]|honest[oa])|sinceramente\?|papo reto|real oficial|verdade seja dita|vou te contar (uma coisa|um segredo|a verdade)|posso ser (sincer[oa]|vulnerável)|confesso que|opinião impopular|senta que lá vem( a)? história|preciso desabafar|não vou mentir|vou abrir o jogo)[:,.!]?\s*",
    r"(?im)(?:^|(?<=[.!?]\s))pov:\s*(?!.*\b(você|vc|sua|seu)\b)",   # "POV:" em algo que não é ponto de vista
    r"(?i)\b(posso estar errad[oa],? mas|talvez,? só talvez|parece que|na minha humilde opinião|me arrisco a dizer)\b",  # ressalvas inseridas: só limpe se NÃO estiverem nas amostras de voz do autor
]
# Correção: apague o marcador e mantenha a frase seguinte. Se a frase seguinte não é um fato
# específico, o marcador estava fazendo o papel da vulnerabilidade. Peça o fato ao autor.
```

## Preserve (voz do usuário, não limpar)

- Registro casual minúsculo, se é assim que o usuário escreve
- `..` como pausa leve e quebras de linha simples como batidas
- Um ou dois fragmentos intencionais ("toda vez.") - o limite é 2 por legenda, não 0
- Um travessão a cada ~100 palavras. Não empurre para zero.
- Um trio natural com itens concretos e não intercambiáveis
- Uma frase realmente longa por parágrafo
- Os 1-3 emoji intencionais do autor
- Oralidade e contrações do português falado ("pra", "tá", "tô", "a gente", "né", "cê")
- Regionalismos e gírias do autor ("bah", "tchê", "uai", "égua", "mano", "véi", "trem")
- Números específicos com referência e nomes próprios (acrescente MAIS, nunca tire)
- Detalhes sensoriais em primeira pessoa
- Reações e opiniões do autor, inclusive as diretas. Tom chapado na legenda
  inteira é assinatura de humanizador
- Um marcador comum isolado num parágrafo ("fundamental", "estratégico" como
  termo técnico). Um não é veredito
- A história real do autor. Nunca invente um detalhe para a legenda funcionar
