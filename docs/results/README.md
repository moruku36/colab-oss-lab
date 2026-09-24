# 実験の記録

Colab（L4）で実際に動かした実験の記録です。番号は行った順です。
各記録の最後に、関係する解説ページへのリンクがあります。

| # | 実験 | 日付 | 想定どおりか | 記録 | ノート |
|---|---|---|---|---|---|
| 01 | L4 の確認と、小さいモデルを1回動かす | 2026-09-24 | はい | [01_first_run.md](01_first_run.md) | [ipynb](../../notebooks/01_l4_check_and_tiny_model.ipynb) |
| 02 | 量子化で L4 に載る最大モデル（Gemma 4 31B） | 2026-09-24 | はい | [02_quantization_max.md](02_quantization_max.md) | [ipynb](../../notebooks/02_quantize_largest_model_on_l4.ipynb) |
| 03 | thinking のオン・オフを比べる | 2026-09-25 | はい（ただし正答数に差は出なかった） | [03_thinking_on_off.md](03_thinking_on_off.md) | [ipynb](../../notebooks/03_thinking_on_off.ipynb) |
| 04 | vLLM で速くする | 2026-09-25 | はい | [04_vllm_speedup.md](04_vllm_speedup.md) | [ipynb](../../notebooks/04_vllm_speedup.ipynb) |

## 数字でくらべる

| # | モデル | 形式 | VRAM | 1件ずつの速さ | 日本語の説明 | 5問（計算・数え上げ） |
|---|---|---|---|---|---|---|
| 01 | Qwen2.5-1.5B-Instruct | bf16（量子化なし） | 小さい | 測っていない | 不正確 | 実施せず |
| 02 | Gemma 4 31B-it | bitsandbytes 4bit | 17.0GB | 約 5.5 トークン/秒 | 正確 | 計算1問は正解 |
| 03 | Gemma 4 31B-it | bitsandbytes 4bit | 18.2GB | 約 6.1 トークン/秒 | 正確 | 5/5（thinking オン・オフとも） |
| 04-A | Gemma 4 31B-it | vLLM + AWQ 4bit | 19.2GB | 13.3 トークン/秒 | 正確 | 5/5 |
| 04-B | Gemma 4 26B-A4B-it（MoE） | vLLM + AWQ 4bit | 20.4GB | 53.5 トークン/秒 | 正確 | 5/5 |

## 図

- [量子化で 62.5GB → 16.6GB（VRAM の図）](../images/02_vram.svg) … [02](02_quantization_max.md)
- [ChatGPT のどのモデルに近いか（GPQA Diamond）](../images/02_chatgpt_position.svg) … [02](02_quantization_max.md)
- [thinking オン・オフの回答時間](../images/03_thinking_time.svg) … [03](03_thinking_on_off.md)
- [vLLM の速さ](../images/04_vllm_speed.svg) … [04](04_vllm_speedup.md)

次に試せそうなこと: [次の実験の候補](../next-experiments.md)
