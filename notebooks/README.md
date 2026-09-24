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

## 08. thinking（考えてから答える）のオン・オフを比べる

- ファイル: [08_thinking_on_off.ipynb](08_thinking_on_off.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/08_thinking_on_off.ipynb)
- 結果の置き場: [docs/results/08_thinking_on_off.md](../docs/results/08_thinking_on_off.md)

内容:

1. 02 と同じ `google/gemma-4-31B-it`（4bit）を L4 に載せる
2. 答えが決まっている5問を、thinking オフ / オン で解かせる
3. 正答数・時間・トークン数・VRAM を比べる
