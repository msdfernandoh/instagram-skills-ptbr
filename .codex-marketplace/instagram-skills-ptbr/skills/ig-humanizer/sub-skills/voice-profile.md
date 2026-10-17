# Sub-skill: montar / atualizar o perfil de voz e marca

Monta ou atualiza `../../../references/voice-profile.md` para que toda skill de
escrita do pacote rascunhe na voz real do usuário, não numa voz "humana"
genérica. Funciona em qualquer agente: o caminho principal só precisa de textos
do próprio usuário colados. Um token de leitura (Apify) é acelerador opcional.

## Quando roda

- O usuário diz "monta meu perfil de voz", "aprende minha voz" ou chama
  `ig-humanizer --mode profile`.
- Ofereça também na primeira vez que uma skill de escrita encontrar `filled: no`.

## Várias marcas

Se o usuário gerencia mais de uma marca, monte um perfil por marca
(`voice-profile-<marca>.md`, na mesma pasta) e pergunte qual marca antes de
começar. Não misture amostras de marcas diferentes num mesmo perfil.

## Entradas (qualquer uma basta)

1. **Amostras coladas (padrão portátil).** Peça 3-6 posts reais do usuário no Instagram.
2. **Pela camada de leitura (opcional).** Se houver token, puxe a atividade
   recente como amostras extras, somadas às coladas.
3. **Manual.** O usuário também pode só dizer nicho, regras e links.

## Passos

1. Junte 3+ amostras reais.
2. Extraia a impressão digital da voz das amostras, não de suposições: ritmo,
   aberturas recorrentes, pontuação, vocabulário preferido, palavras que evita,
   tratamento (você/vocês/tu/a gente), regionalismos, uso de emoji e hashtag.
3. Deduza nicho, público, cidade/região e pilares pelos temas; confirme em vez de chutar.
4. Registre regras fixas, regras do setor e estilo de CTA/link (WhatsApp,
   direct, link na bio) que as amostras mostram ou o usuário informa.
5. Escreva o arquivo: preencha as seções 1-5, copie as 2-4 linhas mais fortes
   literalmente em "Exemplos de assinatura" e defina o Status como
   `filled: yes`, `source: <pasted|read-layer|manual>`, `updated: <hoje>`.
6. Mostre o perfil preenchido para aprovação antes de salvar; avise que toda
   skill de escrita vai seguir automaticamente.

## Regras fixas

- Monte a impressão digital a partir das amostras REAIS. Nunca invente uma voz.
- Preserve os jeitos do usuário (expressão favorita, ritmo diferente,
  regionalismo). Eles são o ponto; a limpeza de IA vale para rascunhos, não para o perfil.
- Com só 3 amostras, diga que é uma primeira versão; sugira refazer com 10+ posts.
- Nunca coloque no arquivo algo que o usuário não forneceu.

## Relacionado

- O perfil preenchido é lido pelas skills de escrita antes de rascunhar.
- Refaça sempre que a voz ou o foco mudar.
