---
license: cc-by-4.0
pretty_name: AI Image, Video & Audio API Prices by Provider
language:
- en
tags:
- pricing
- api
- image-generation
- video-generation
- text-to-image
- text-to-video
- text-to-speech
- music-generation
size_categories:
- n<1K
configs:
- config_name: latest
  data_files: data/latest.csv
  default: true
- config_name: monthly
  data_files: data/monthly/*.csv
---

# AI Image, Video & Audio API Prices by Provider

Public list prices for popular image, video and audio generation models (GPT Image, Nano Banana, Seedance, Veo, Kling, Wan, MiniMax, FLUX, Grok Imagine, Suno, ElevenLabs and more), side by side for the official API and third-party API providers, at the same output specification.

Maintained by [Yir](https://yir.ai), an image and video API router that bills each job at the public price of the provider that ran it. Updated monthly.

## Files

| Path | Content |
|---|---|
| `data/latest.csv` | The most recent snapshot |
| `data/monthly/YYYY-MM.csv` | One snapshot per month, kept for price history |

```python
from datasets import load_dataset

latest = load_dataset("yir-ai/ai-image-video-api-prices", "latest", split="train")
history = load_dataset("yir-ai/ai-image-video-api-prices", "monthly", split="train")
```

```python
import pandas as pd

df = pd.read_csv("https://huggingface.co/datasets/yir-ai/ai-image-video-api-prices/resolve/main/data/latest.csv")
df[(df.model_id == "bytedance/seedance-2.5") & (df.output_spec == "720p")]
```

## Columns

| Column | Meaning |
|---|---|
| `snapshot_date` | Date the prices were read (UTC) |
| `model_id` | Canonical model ID, e.g. `bytedance/seedance-2.5` |
| `model_name` | Display name |
| `category` | `image`, `video` or `audio` |
| `variant` | Model variant or tier when a model has several (e.g. `Flare (Standard)`), otherwise empty |
| `output_spec` | Output specification the price applies to: resolution, duration, audio on/off, etc. |
| `quality` | Quality tier for models priced per quality level (`low` … `maximum`), otherwise empty |
| `provider` | Who serves the API: the model maker (official) or a third-party provider |
| `provider_tier` | The provider's own product line when it sells several (e.g. `Standard`, `Special`), otherwise empty |
| `official` | `true` for the model maker's own API; its price is the reference point |
| `input` | Input type when price differs by input (`text`, `image`, `reference media`), otherwise empty |
| `price_usd` | Price in US dollars per `unit` |
| `price_usd_max` | Upper end when the provider prices a range; equal to `price_usd` otherwise |
| `unit` | `image`, `second` (of output video or audio), `track`, `1K characters` |
| `estimated` | `true` when the provider bills by tokens or usage and the per-unit price is converted at typical usage |
| `model_page` | Model page with the live table, parameters and API examples |

## How the prices are collected

- Source: the pricing tables on [yir.ai/models](https://yir.ai/models), read from each page's Markdown copy (`https://yir.ai/models/<slug>.md`).
- Prices are each provider's public list price for the same output specification. Official prices come from the model maker's own price list.
- Token- or usage-billed prices are converted to a per-image or per-second figure and flagged with `estimated = true`.
- Limited-time provider promotions are excluded; the dataset tracks standing list prices.
- Prices change between snapshots. For the current price of a model, open its `model_page`.

## License and citation

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Please credit "Yir (yir.ai)" and link to the dataset or [yir.ai/models](https://yir.ai/models).

```bibtex
@misc{yir_api_prices,
  title  = {AI Image, Video \& Audio API Prices by Provider},
  author = {Yir},
  year   = {2026},
  url    = {https://huggingface.co/datasets/yir-ai/ai-image-video-api-prices}
}
```

Corrections and questions: open an issue on [GitHub](https://github.com/yir-ai/ai-image-video-api-prices).
