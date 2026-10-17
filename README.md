# Skills de marketing para Instagram em português (Claude Code e Codex)

Adaptação para **português do Brasil** do pacote
[instagram-skills](https://github.com/sergebulaev/instagram-skills) de Serge
Bulaev (licença MIT). As 9 skills transformam o Claude num time de conteúdo para
Instagram: escrevem legenda e gancho de Reel na sua voz, escolhem hashtags do
tamanho certo, planejam carrossel e a semana inteira, leem o seu nicho com dados
reais e otimizam o perfil. Todo rascunho passa por um humanizador calibrado para
os vícios de texto de IA **em português** e espera sua aprovação antes de publicar.

## O que mudou em relação ao original

- **Tudo em português:** instruções, exemplos, modelos de gancho, checklists e
  mensagens da biblioteca Python.
- **Humanizador reescrito para PT-BR:** lista de vocabulário de IA em português
  (alavancar, potencializar, jornada, crucial, vale ressaltar...), gerúndio
  abrindo frase, gerundismo ("vou estar enviando"), nominalizações, cara de
  tradução ("no final do dia", "faz sentido?"), trios ocos de texto
  institucional ("segurança, solidez e tranquilidade") e checagem de português
  correto. Os padrões regex foram testados.
- **Hashtags para o mercado brasileiro:** faixas de volume ajustadas para tags
  em português e estratégia de **tags locais** (cidade + UF, cidade + tema).
- **Horários no Brasil e fuso do público:** janelas por tipo de público e
  lembrete de que Mato Grosso, MS, RO, RR e boa parte do AM estão em UTC-4.
- **CTA de palavra-chave no comentário** ("comenta SIMULAÇÃO que eu te mando no
  direct"): permitido quando há automação que entrega algo real; isca vazia continua proibida.
- **WhatsApp** como canal de conversão no otimizador de perfil e nos CTAs.
- **Setores regulados:** nada de promessa de resultado garantido (consórcio,
  crédito, educação, saúde, imóveis).
- **Várias marcas:** um perfil de voz por marca.
- O nome do plugin passou a ser `instagram-skills-ptbr` para não conflitar com o original.

## Exemplos de pedido

- "Escreve uma legenda pra esse Reel sobre consórcio imobiliário"
- "Planeja minha semana no Instagram, estou lançando um evento em Sinop"
- "O que está funcionando na #consorcioimobiliario agora?"
- "Tira a cara de IA dessa legenda: [texto]"
- "Transforma esse post do LinkedIn num carrossel"

A skill certa ativa sozinha. Você revisa e aprova.

## Instalação

### Claude Code (CLI / VS Code / JetBrains)

Publique esta pasta num repositório seu no GitHub e rode:

```
/plugin marketplace add <seu-usuario>/<seu-repo>
/plugin install instagram-skills-ptbr@instagram-skills-ptbr
```

Ou abra a pasta como diretório de trabalho do Claude Code.

### claude.ai / Claude Desktop

Em **Configurações > Skills**, use "Adicionar do GitHub" apontando para o seu
repositório, ou envie as pastas de `skills/` uma a uma.

### Qualquer agente (skills CLI)

```
npx skills add <seu-usuario>/<seu-repo>
```

## As 9 skills

| Skill | O que faz |
|---|---|
| **ig-caption-writer** | Legenda com gancho nos primeiros 125 caracteres, corpo fácil de ler e um CTA, com fórmula escolhida pelo objetivo |
| **ig-carousel-planner** | Carrossel slide a slide (até 10): gancho que abre laço, valor na frente, slide final que rende salvamento |
| **ig-hook-extractor** | Descobre o gancho de um Reel ou carrossel viral e devolve um modelo em branco em português |
| **ig-hashtag-strategist** | Conjunto de 3-5 hashtags dimensionadas (nicho / média / ampla), com tags locais |
| **ig-humanizer** | Tira a cara de IA do português, limita travessões, corrige erros; inclui `--mode audit` e `--mode profile` |
| **ig-content-planner** | Plano semanal com mistura de Reels, carrosséis e stories, horários no fuso do público e meta de salvamentos |
| **ig-repurposer** | Transforma post do LinkedIn, blog, YouTube, X ou WhatsApp (e conteúdo em inglês) em post nativo |
| **ig-profile-optimizer** | Revisa foto, campo NOME, bio, link/WhatsApp, categoria, destaques, grid e fixados |
| **ig-audience-insights** | Lê o nicho e perfis com dados reais via Apify |

## Mídia é obrigatória

O Instagram não aceita post só de texto. As skills escrevem; **você fornece a
imagem ou o vídeo**. Com a Publora conectada, a skill envia a mídia e agenda o
post (rascunho -> upload -> agendamento, nessa ordem).

## Opcional: publicar automaticamente com Publora

1. Cadastro grátis em https://app.publora.com/signup
2. Conecte o Instagram (conta **Comercial ou de Criador**): Channels > Add Channel
3. Copie o ID da plataforma (`instagram-...`) e crie uma chave de API (`sk_...`)
4. Copie `.env.example` para `.env` e preencha
5. `pip install requests python-dotenv`

Sem Publora, toda legenda aprovada volta como bloco para copiar e colar.

## Opcional: ilustrações com Pixfaro

Coloque `PIXFARO_TOKEN=pf_live_...` no `.env` para gerar artes de carrossel e
cards de frase com o @ e a cor da sua marca sobrepostos. Sem chave, a skill
escreve o prompt para você gerar onde preferir.

## Regras de voz (resumo)

1. Travessão com teto de cerca de 1 a cada 100 palavras.
2. Nomes próprios com maiúscula.
3. Sem aglomerado de vocabulário de IA (3+ marcadores num parágrafo = reescrever).
4. Número específico vence adjetivo ("R$ 312/mês" vence "parcela baixa").
5. O gancho mora nos primeiros 125 caracteres.
6. 3-5 hashtags, 0-3 emoji.

Detalhes em `references/voice-rules.md`.

## Licença e créditos

MIT. Pacote original de [Serge Bulaev / Creative Content Crafts](https://github.com/sergebulaev/instagram-skills).
Publicação via [Publora](https://publora.com).
