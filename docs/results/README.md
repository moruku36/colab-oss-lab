# 実験の記録

Colab（L4）で実際に動かした実験の記録です。番号は行った順です。
各記録の最後に、関係する解説ページへのリンクがあります。

| # | 実験 | 日付 | 想定どおりか | 記録 | ノート |
|---|---|---|---|---|---|
| 01 | L4 の確認と、小さいモデルを1回動かす | 2026-09-24 | はい | [01_first_run.md](01_first_run.md) | [ipynb](../../notebooks/01_l4_check_and_tiny_model.ipynb) |
| 02 | 量子化で L4 に載る最大モデル（Gemma 4 31B） | 2026-09-24 | はい | [02_quantization_max.md](02_quantization_max.md) | [ipynb](../../notebooks/02_quantize_largest_model_on_l4.ipynb) |
| 03 | thinking のオン・オフを比べる | 2026-09-25 | はい（ただし正答数に差は出なかった） | [03_thinking_on_off.md](03_thinking_on_off.md) | [ipynb](../../notebooks/03_thinking_on_off.ipynb) |
| 04 | vLLM で速くする | 2026-09-25 | はい | [04_vllm_speedup.md](04_vllm_speedup.md) | [ipynb](../../notebooks/04_vllm_speedup.ipynb) |
| 05 | 量子化の方式・ビット数を比べる（Qwen3-8B） | 2026-09-25 | いいえ（4つ中3つ合格。HQQ 4bit の正答が2問低下） | [05_quantization_compare.md](05_quantization_compare.md) | [ipynb](../../notebooks/05_quantization_compare.ipynb) |
| 06 | QLoRA で追加学習する（Qwen3-8B） | 2026-09-25 | いいえ（6つ中4つ合格。聞き方を変えると 44%、10問が 2 問低下） | [06_qlora.md](06_qlora.md) | [ipynb](../../notebooks/06_qlora.ipynb) |
| 07 | L4 の上限を探る（Qwen3-32B / Seed-OSS-36B） | 2026-09-25 | はい | [07_l4_limit.md](07_l4_limit.md) | [ipynb](../../notebooks/07_l4_limit.ipynb) |
| 08 | RAG・API を作る（Gemma 4 26B-A4B + 資料検索） | 2026-09-25 | いいえ（5つ中3つ合格。正答 64%、検索 76%） | [08_rag_api.md](08_rag_api.md) | [ipynb](../../notebooks/08_rag_api.ipynb) |

## 数字でくらべる

| # | モデル | 形式 | VRAM | 1件ずつの速さ | 日本語の説明 | 5問（計算・数え上げ） |
|---|---|---|---|---|---|---|
| 01 | Qwen2.5-1.5B-Instruct | bf16（量子化なし） | 小さい | 測っていない | 不正確 | 実施せず |
| 02 | Gemma 4 31B-it | bitsandbytes 4bit | 17.0GB | 約 5.5 トークン/秒 | 正確 | 計算1問は正解 |
| 03 | Gemma 4 31B-it | bitsandbytes 4bit | 18.2GB | 約 6.1 トークン/秒 | 正確 | 5/5（thinking オン・オフとも） |
| 04-A | Gemma 4 31B-it | vLLM + AWQ 4bit | 19.2GB | 13.3 トークン/秒 | 正確 | 5/5 |
| 04-B | Gemma 4 26B-A4B-it（MoE） | vLLM + AWQ 4bit | 20.4GB | 53.5 トークン/秒 | 正確 | 5/5 |

05 は別のモデル（Qwen3-8B）で、量子化の方式・ビット数だけを変えて比べた:

| 形式 | VRAM | PPL 英語（bf16 比） | 10問 | 速さ |
|---|---|---|---|---|
| bf16 | 15.26GB | ±0% | 9/10 | 15.1 トークン/秒 |
| bnb 8bit | 8.79GB | -0.1% | 8/10 | 4.4 |
| bnb 4bit NF4 | 5.67GB | +5.8% | 8/10 | 13.0 |
| bnb 4bit FP4 | 5.67GB | +8.5% | 8/10 | 13.2 |
| HQQ 4bit | 6.00GB | +2.0% | 7/10 | 2.4 |
| HQQ 3bit | 5.33GB | +59.3% | 5/10 | 1.6 |
| HQQ 2bit | 4.36GB | 約 3 万倍（壊れる） | 0/10 | 2.5 |

## 図

- [量子化で 62.5GB → 16.6GB（VRAM の図）](../images/02_vram.svg) … [02](02_quantization_max.md)
- [ChatGPT のどのモデルに近いか（GPQA Diamond）](../images/02_chatgpt_position.svg) … [02](02_quantization_max.md)
- [thinking オン・オフの回答時間](../images/03_thinking_time.svg) … [03](03_thinking_on_off.md)
- [vLLM の速さ](../images/04_vllm_speed.svg) … [04](04_vllm_speedup.md)
- [量子化の方式・ビット数の比較](../images/05_quantization_compare.svg) … [05](05_quantization_compare.md)
- [QLoRA の学習前と学習後](../images/06_qlora.svg) … [06](06_qlora.md)
- [L4 の上限（VRAM と文章の長さ）](../images/07_l4_limit.svg) … [07](07_l4_limit.md)
- [RAG と LoRA の正答率・API の速さ](../images/08_rag_api.svg) … [08](08_rag_api.md)

次に試せそうなこと: [次の実験の候補](../next-experiments.md)
