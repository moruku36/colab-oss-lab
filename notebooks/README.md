# notebooks

実際に Colab で動かすノートです。

## 01. L4 の確認と、小さいモデルを1回動かす

- ファイル: [01_l4_check_and_tiny_model.ipynb](01_l4_check_and_tiny_model.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/01_l4_check_and_tiny_model.ipynb)
- 結果の置き場: [docs/results/01_first_run.md](../docs/results/01_first_run.md)

内容:

1. GPUが L4 かどうか確認する
2. `Qwen/Qwen2.5-1.5B-Instruct` を1個読み込む
3. 短い日本語を1回だけ生成する

実行前に、ランタイムを **L4 GPU** にしてください。

## 02. 量子化で L4 に載る「いちばん大きいモデル」を試す

- ファイル: [02_quantize_largest_model_on_l4.ipynb](02_quantize_largest_model_on_l4.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/02_quantize_largest_model_on_l4.ipynb)
- 結果の置き場: [docs/results/02_quantization_max.md](../docs/results/02_quantization_max.md)

内容:

1. `google/gemma-4-31B-it`（約31B、bf16 で約62GB）を 4bit 量子化して L4 1枚に載せる
2. VRAM 使用量と生成速度を測る
3. 日本語で2問聞き、「想定どおり動いたか」を判定する

## 03. thinking（考えてから答える）のオン・オフを比べる

- ファイル: [03_thinking_on_off.ipynb](03_thinking_on_off.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/03_thinking_on_off.ipynb)
- 結果の置き場: [docs/results/03_thinking_on_off.md](../docs/results/03_thinking_on_off.md)

内容:

1. 02 と同じ `google/gemma-4-31B-it`（4bit）を L4 に載せる
2. 答えが決まっている5問を、thinking オフ / オン で解かせる
3. 正答数・時間・トークン数・VRAM を比べる

## 04. vLLM で速くする

- ファイル: [04_vllm_speedup.ipynb](04_vllm_speedup.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/04_vllm_speedup.ipynb)
- 結果の置き場: [docs/results/04_vllm_speedup.md](../docs/results/04_vllm_speedup.md)

内容:

1. vLLM を入れる
2. Gemma 4 31B（AWQ 4bit）と Gemma 4 26B-A4B（MoE、AWQ 4bit）を vLLM で動かす
3. 1件ずつ / 16件まとめての速さ、03 と同じ5問の正答数、VRAM を測り、03（transformers + bitsandbytes）と比べる

## 05. 量子化の方式・ビット数を比べる

- ファイル: [05_quantization_compare.ipynb](05_quantization_compare.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/05_quantization_compare.ipynb)
- 結果の置き場: [docs/results/05_quantization_compare.md](../docs/results/05_quantization_compare.md)

内容:

1. 同じ `Qwen/Qwen3-8B` を、bf16 / bitsandbytes 8bit・4bit（NF4・FP4）/ HQQ 4・3・2bit の7通りで読み込む
2. VRAM、パープレキシティ（英語・日本語）、10問の正答数、速さを測る
3. 「どこまで下げても大丈夫か」を判定する
