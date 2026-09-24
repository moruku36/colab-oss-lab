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
