# Fluxo de publicação de mídia no Instagram

O Instagram exige mídia em todo post. Não existe post só de texto: a API
rejeita. Então as skills deste pacote produzem a **legenda, o gancho, as
hashtags e o roteiro de slides ou cenas**, e o **usuário fornece a imagem ou o
vídeo**. Publicar é sempre um fluxo de mídia.

## Sumário

- O fluxo (nível Publora)
- Regras de contêiner (no que a lista de mídia se transforma)
- Limites de mídia
- Nível manual (sem Publora)
- Limpando um upload que falhou

## O fluxo (nível Publora)

Publicar no Instagram tem três passos. Criar o post já com `scheduledTime` faz o
agendador do Instagram correr contra o upload no S3, então a ordem é obrigatória:

```
1. POST /create-post          -> cria um RASCUNHO (sem scheduledTime), recebe postGroupId
2. POST /get-upload-url       -> URL S3 pré-assinada para um arquivo
   PUT  {uploadUrl}            -> envia os bytes do arquivo para o S3
   (repita o passo 2 para cada imagem de um carrossel de 2-10 itens, na ordem dos slides)
3. PUT  /update-post/:postGroupId  -> status="scheduled" + scheduledTime
```

Neste pacote, `lib/publora_client.py` faz o fluxo inteiro em um método:

```python
from lib import PubloraClient

client = PubloraClient()  # lê PUBLORA_API_KEY
result = client.publish_media_post(
    content="o passo a passo tá aqui embaixo, pode copiar\n\n#nicho #media #ampla",
    platforms=["instagram-11223344"],   # INSTAGRAM_PLATFORM_ID
    media=["slide1.jpg", "slide2.jpg", "slide3.jpg"],  # caminhos locais, na ordem
    scheduled_time="2026-11-03T15:00:00.000Z",  # em UTC; omita para ~agora
)
# -> {"postGroupId": ..., "media": [...], "scheduled": {...}}
```

**Atenção ao fuso:** `scheduled_time` é em UTC. 12h em Sinop/Cuiabá (UTC-4) =
`16:00:00.000Z`. 12h em Brasília (UTC-3) = `15:00:00.000Z`. Sempre confirme com o
usuário o horário local e converta.

Ou use o atalho `publish()`, que escolhe entre manual / Publora / próprio e só
publica automaticamente quando há caminhos de arquivo de mídia:

```python
from lib import publish

publish(
    "carousel",
    draft_text="<legenda aprovada>",
    target_url="https://www.instagram.com/",
    media=["slide1.jpg", "slide2.jpg", "slide3.jpg"],
    scheduled_time="2026-11-03T16:00:00.000Z",
)
```

Se algo falhar depois que o rascunho foi criado, `publish_media_post` apaga o
rascunho para não deixar um post quebrado com upload abandonado.

## Regras de contêiner

| Mídia enviada | Vira | Notas |
|---|---|---|
| 1 imagem (JPEG/PNG/WebP) | post de foto única | WebP vira JPEG automaticamente |
| 2-10 imagens | carrossel, na ordem da lista | sem mistura, só imagens |
| 1 vídeo (MP4/MOV) | Reel por padrão | Story via `videoType: "STORIES"` |
| imagens + vídeo misturados | rejeitado | a API não permite carrossel misto |

Defina a superfície com `platform_settings`:

```json
{ "platformSettings": { "instagram": { "videoType": "REELS" } } }
```

`videoType` é `"REELS"` (padrão) ou `"STORIES"`. O pacote define isso
automaticamente para `kind="reel"` e `kind="story"`.

## Limites de mídia (na publicação)

| Item | Limite |
|---|---|
| Formatos de imagem | JPEG, PNG, WebP (WebP vira JPEG) |
| Tamanho máximo de imagem | 8 MB |
| Itens no carrossel | 2-10 (API) |
| Duração do Reel | até 3 min (180 s); 5-90 s entram na aba Reels |
| Tamanho do vídeo | 300 MB |
| Clipe de vídeo no carrossel | até 60 s |
| Proporção | 4:5 retrato a 1,91:1 paisagem; 4:5 ocupa melhor o feed |
| Legenda | 2.200 caracteres (os primeiros ~125 visíveis) |
| Tipo de conta | Comercial ou Criador de conteúdo (conta pessoal não é suportada) |

## Nível manual (sem Publora)

Sem `PUBLORA_API_KEY`, ou quando o usuário não apontou arquivos de mídia, toda
skill devolve a legenda aprovada como bloco para copiar e colar, mais o lembrete
de anexar a mídia no app do Instagram (ou agendar pelo Meta Business Suite).
O Instagram ainda precisa da imagem ou do vídeo, então o lembrete não é opcional.

## Limpando um upload que falhou

Se a chamada `get-upload-url` funcionar mas o PUT no S3 falhar, o rascunho fica
com uma referência de mídia quebrada que impede reagendar. Apague o rascunho
inteiro e recomece:

```python
client.delete_post(post_group_id)   # DELETE /delete-post/:id
```

`publish_media_post` já faz isso quando algo falha.
