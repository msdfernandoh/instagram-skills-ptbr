---
name: instagram-marketing
description: "Planejar, escrever, revisar e publicar conteúdo para Instagram em português do Brasil. Use para escrever legenda com gancho nos primeiros 125 caracteres, planejar carrossel slide a slide, descobrir o gancho de um Reel ou carrossel viral, escolher hashtags do jeito de 2026, tirar a cara de IA de uma legenda ou planejar a semana de Reels e carrosséis. O Instagram exige mídia em todo post: as skills escrevem, o usuário fornece a imagem ou o vídeo; a publicação usa o fluxo rascunho, upload, agendamento da API Publora."
---

# Skills de marketing para Instagram (PT-BR)

Um pacote de 9 skills focadas para operação de conteúdo no Instagram em 2026,
adaptadas para o português brasileiro e para o mercado do Brasil. Cada skill tem
uma função só, segue o padrão rascunho, depois aprovação, depois publicação, e
usa a [API Publora](https://publora.com) para o fluxo de publicação com mídia.

**Idioma:** todo conteúdo gerado sai em português brasileiro, salvo pedido
explícito do usuário em outro idioma.

## Quando usar este pacote

- **Escrever uma legenda** (gancho nos primeiros 125 caracteres, corpo, CTA) -> `ig-caption-writer`
- **Planejar um carrossel** slide a slide (do slide gancho ao slide recompensa) -> `ig-carousel-planner`
- **Descobrir o gancho** de um Reel ou carrossel viral -> `ig-hook-extractor`
- **Escolher hashtags** (3-5 de nicho/média/ampla, não 30) -> `ig-hashtag-strategist`
- **Tirar a cara de IA de uma legenda, ou revisar antes de postar** -> `ig-humanizer` (reescrita, mais `--mode audit`)
- **Planejar a semana** de Reels, carrosséis e stories -> `ig-content-planner`
- **Adaptar conteúdo de outra plataforma** (post do LinkedIn, blog, roteiro de YouTube, fio do X, texto de WhatsApp) para Instagram -> `ig-repurposer`
- **Revisar e reescrever o perfil** (foto, campo NOME, bio, link, destaques, as 9 primeiras do grid, fixados) -> `ig-profile-optimizer`
- **Ler o nicho e estatísticas de perfil com dados reais** (o que funciona numa hashtag, números de você ou de um concorrente) -> `ig-audience-insights`

## Padrão central

Toda skill que age segue três passos:

1. **Interpretar a entrada.** Se o usuário mandar uma URL do Instagram, a skill
   usa `lib/url_parser.py` para extrair o usuário e o shortcode.
2. **Rascunhar o conteúdo.** A skill aplica as heurísticas de 2026 (fórmulas de
   gancho, hashtags, horários, regras de voz, sinais de ranqueamento) e mostra o
   rascunho ao usuário.
3. **Esperar aprovação, depois publicar com mídia.** O usuário fornece a mídia;
   com a aprovação, a skill roda o fluxo rascunho, upload e agendamento da Publora.

## Mídia é obrigatória (leia primeiro)

O Instagram rejeita post só de texto. As skills produzem legenda, gancho,
hashtags e roteiro de slides ou cenas. O USUÁRIO fornece a imagem ou o vídeo. O
caminho de publicação é sempre:

```
1. criar rascunho (sem scheduledTime)  -> postGroupId
2. enviar cada arquivo para o S3        (2-10 arquivos num carrossel, na ordem)
3. atualizar o post: status=scheduled + scheduledTime
```

`lib/publora_client.publish_media_post(...)` faz os três passos. Ver
`references/media-workflow.md`.

## Pré-requisitos

**Três níveis, escolha um.**

### Nível 0 - Só rascunho (padrão, sem configuração)

Funciona direto. Sem chave de API, sem cadastro. Toda legenda aprovada volta
como bloco para copiar e colar, com o lembrete de anexar a mídia no app do
Instagram ou no Meta Business Suite.

### Nível 1 - Publicação automática com Publora (~2 min)

Com a aprovação, aponte a skill para os arquivos de mídia locais e ela envia e
agenda o post pela [API Publora](https://publora.com).

1. Cadastro grátis: **https://app.publora.com/signup**
2. Conecte a conta do Instagram Comercial ou de Criador (Channels > Add Channel)
3. Copie a chave de API no painel de API da Publora
4. Coloque no `.env`:
   ```
   PUBLORA_API_KEY=sk_...
   INSTAGRAM_PLATFORM_ID=instagram-...
   ```
5. Rode `pip install -r requirements.txt`

### Nível 2 - Seu próprio publicador (avançado)

Peça ao Claude Code para criar um publicador na Instagram Graph API. Defina
`INSTAGRAM_SKILLS_CUSTOM_POSTER=<seu comando>` e as skills chamam o comando na
aprovação.

### Tipo de conta

A API de publicação do Instagram exige conta **Comercial ou de Criador de
conteúdo**. Conta pessoal não funciona.

## Regras de voz (em todas as skills)

1. Travessão (`—`) com teto de cerca de 1 a cada 100 palavras (1-2 por legenda, 1 por slide); o excesso vira vírgula, dois-pontos, parênteses ou quebra de linha, nunca ponto final.
2. Use `..` ou quebra de linha como pausa leve quando o ritmo pedir.
3. Nomes de pessoas, empresas, produtos e cidades com maiúscula.
4. Início de frase pode ser minúsculo (registro nativo de legenda); nomes continuam com maiúscula.
5. Evite vocabulário de IA em português: `alavancar`, `potencializar`, `jornada`, `crucial`, `fundamental`, `robusto`, `mergulhar`, `desbloquear`, `elevar`, `vale ressaltar`.
6. Número específico vence adjetivo. "47 minutos" vence "rápido"; "R$ 312/mês" vence "parcela baixa".
7. Uma ideia por legenda. Os primeiros 125 caracteres carregam o gancho (o corte do "mais").
8. Hashtag é dimensionamento, não volume: 3-5 bem escolhidas, não 30.

(Referência canônica: `references/voice-rules.md`. Ver também
`references/hook-formulas.md`, `references/algorithm-heuristics.md`,
`references/hashtag-strategy.md` e `references/media-workflow.md`.)

## Como as URLs do Instagram são lidas

| Formato da URL | Vira |
|---|---|
| `https://www.instagram.com/p/SHORTCODE/` | shortcode, tipo `post` |
| `https://www.instagram.com/reel/SHORTCODE/` | shortcode, tipo `reel` |
| `https://www.instagram.com/USUARIO/` | usuário, tipo `profile` |
| `https://www.instagram.com/stories/USUARIO/ID/` | usuário + id, tipo `story` |

`lib.url_parser.parse_instagram_url(url)` devolve `{username, shortcode,
url_type, canonical_url}`. Carrossel e imagem única usam o mesmo caminho `/p/`.

## Pegadinhas conhecidas

- **Mídia é obrigatória.** Sem imagem ou vídeo, sem post.
- **Sem mídia misturada no carrossel.** Só imagens ou só vídeos.
- **Só os primeiros 125 caracteres aparecem.** O resto fica atrás do "mais".
- **Carrossel de 2-10 itens** pela API (o app permite 20).
- **WebP vira JPEG** na publicação.
- **Horário em UTC na API.** Converta do fuso do público (Mato Grosso é UTC-4, Brasília UTC-3).
- **Conta Comercial ou de Criador.** Conta pessoal não usa a API.

## Setores regulados

Se o perfil for de consórcio, financeiro, educação, saúde ou imóveis, nunca
escreva promessa de resultado garantido (contemplação garantida, aprovação
certa, rentabilidade garantida, "vaga garantida"). Respeite as regras de
comunicação do setor que estiverem no perfil de voz e, na dúvida, sinalize ao
usuário em vez de publicar.

## Recursos

- [Documentação da API Publora](https://docs.publora.com)
- `lib/publora_client.py` - cliente Python com o fluxo completo de mídia
- `lib/url_parser.py` - leitor de URL do Instagram

## Créditos

Adaptação para PT-BR do pacote [instagram-skills](https://github.com/sergebulaev/instagram-skills)
de Serge Bulaev (licença MIT). Publicação via [API REST Publora](https://publora.com).
