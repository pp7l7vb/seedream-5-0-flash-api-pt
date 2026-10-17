# Seedream 5.0 Flash API (`seedream-5-0-flash`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.0137 per image** on the APIMart route — um único endpoint compatível com OpenAI em `https://api.apimart.ai/v1`, sem plano mensal. *(observado em 2026-10-12)*

**[Obter uma chave de API](https://go.apimart.ai/k-11fbaa)** · **[Preços ao vivo](https://go.apimart.ai/k-b6474b)** · **[Página do modelo](https://go.apimart.ai/k-e53a5c)** · [⚡ Início rápido em 60 segundos](#quickstart)

**Por que os times usam Seedream 5.0 Flash (`seedream-5-0-flash`) via APIMart**

- **Uma chave, o catálogo inteiro.** A mesma base URL `https://api.apimart.ai/v1` e o mesmo cabeçalho `Authorization` alcançam Seedream 5.0 Flash (`seedream-5-0-flash`) e mais de 300 modelos de imagem, vídeo e linguagem — troque o campo `model`, não o seu cliente.
- **Mínimo de $1, pague conforme o uso.** Sem assinatura e sem plano pré-pago para dimensionar antes: recarregue a partir de $1 e gaste em chamadas.
- **A cobrança volta na resposta.** Toda tarefa concluída informa o valor cobrado (`cost` / `credits_cost`); uma tarefa marcada como `failed` é reembolsada.
- **Assíncrono por design.** Envie, pegue o `task_id`, consulte `GET /v1/tasks/{id}` — batching e retries são trabalho comum de fila, não uma integração sob medida.

<!-- /conv-kit:v1 -->

**Também conhecido como:** Dreamina

Seedream 5.0 Flash is ByteDance's fast, lower-cost text-to-image model, served on APIMart as seedream-5-0-flash. Seedream 5.0 Flash generates cinematic 1K, 1.5K, and 2K images—or exact pixel sizes—at one per-image price, with strong text rendering, multi-reference style fusion, and unified generation plus editing. Built for batch jobs and rapid iteration, Seedream 5.0 Flash runs in a single seedream-5-0-flash API call, so teams adopt seedream-5-0-flash in minutes.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `seedream-5-0-flash` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-e53a5c) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.017143 | 0.0137144 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-b6474b) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-e53a5c) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"seedream-5-0-flash","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "seedream-5-0-flash", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Qual é o id do modelo Seedream 5.0 Flash?**

`seedream-5-0-flash`. Veja a tabela de rotas acima.

**Como é a cobrança?**

**from $0.0137 per image** on the APIMart route. A tarefa concluída informa o valor exato no campo `cost`; uma tarefa com falha é reembolsada.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-11fbaa) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [seedream-5-0-flash-prompts-pt](https://github.com/pp7l7vb/seedream-5-0-flash-prompts-pt)
- **Same model, other languages** — 10 more language builds: English `seedream-5-0-flash-api` · 中文 [seedream-5-0-flash-api-cn](https://github.com/jikbtcp30/seedream-5-0-flash-api-cn) · Deutsch [seedream-5-0-flash-api-de](https://github.com/mlcu5yaq7/seedream-5-0-flash-api-de) · Español [seedream-5-0-flash-api-es](https://github.com/w4c1mqh4329/seedream-5-0-flash-api-es) · Français `seedream-5-0-flash-api-fr` · Relay/gateway `seedream-5-0-flash-api-gateway` · Bahasa [seedream-5-0-flash-api-id](https://github.com/zosfl56hg/seedream-5-0-flash-api-id) · 日本語 `seedream-5-0-flash-api-ja` · 한국어 `seedream-5-0-flash-api-ko` · Русский [seedream-5-0-flash-api-ru](https://github.com/ac0y48cgee/seedream-5-0-flash-api-ru)
- **More models in this language** — [midjourney-api-pt](https://github.com/l-ca0/midjourney-api-pt) · [gemini-2-5-flash-image-preview-api-pt](https://github.com/ycx8l0/gemini-2-5-flash-image-preview-api-pt) · [nano-banana-api-pt](https://github.com/erujt71i1lx/nano-banana-api-pt) · [gemini-3-1-flash-image-preview-api-pt](https://github.com/hutjwux0nt9h/gemini-3-1-flash-image-preview-api-pt) · [nano-banana-2-api-pt](https://github.com/ydkd8kdkl8n/nano-banana-2-api-pt) · [gemini-3-pro-image-preview-api-pt](https://github.com/qss2d-66qb2n7/gemini-3-pro-image-preview-api-pt)
- **Topic hubs** — every build of this model, all languages: [seedream-5-0-flash](https://github.com/topics/seedream-5-0-flash) · [seedream-5-0-flash-api](https://github.com/topics/seedream-5-0-flash-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-e53a5c> | `apimart.ai/pt/model/seedream-5-0-flash` |
| Live pricing | <https://go.apimart.ai/k-b6474b> | `apimart.ai/pt/pricing` |
| Get an API key | <https://go.apimart.ai/k-11fbaa> | `apimart.ai/pt/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedream 5.0 Flash is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
