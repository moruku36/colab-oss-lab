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

## 06. QLoRA で追加学習する

- ファイル: [06_qlora.ipynb](06_qlora.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/06_qlora.ipynb)
- 学習データ: [data/06_qlora_facts.json](../data/06_qlora_facts.json)
- 結果の置き場: [docs/results/06_qlora.md](../docs/results/06_qlora.md)

内容:

1. `Qwen/Qwen3-8B` を 4bit NF4 で読み込み、このリポジトリの実験結果など 25 個の知識を LoRA で学習する
2. 学習に使っていない聞き方で、覚えたかを確かめる
3. 元々の賢さ（PPL・10問）が落ちていないか、学習時間・VRAM・部品の大きさを測る

## 07. L4 の上限を探る

- ファイル: [07_l4_limit.ipynb](07_l4_limit.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/07_l4_limit.ipynb)
- 結果の置き場: [docs/results/07_l4_limit.md](../docs/results/07_l4_limit.md)

内容:

1. Qwen3-32B（32.8B）と Seed-OSS-36B（36.2B）を 4bit NF4 で GPU だけに載せられるか試す
2. 載ったら、扱える文章の長さの上限（1K〜32K トークン）、速さ、10問の正答を測る
3. Gemma 4 31B（実験02）と並べて、L4 の上限を決める

## 08. RAG・API を作る

- ファイル: [08_rag_api.ipynb](08_rag_api.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/08_rag_api.ipynb)
- 結果の置き場: [docs/results/08_rag_api.md](../docs/results/08_rag_api.md)

内容:

1. vLLM で Gemma 4 26B-A4B を OpenAI 互換の API サーバーとして起動し、`openai` ライブラリで呼ぶ
2. このリポジトリの README・解説・実験記録を検索できるようにして、資料を見ながら答えさせる（RAG）
3. 06 と同じ 25 問で RAG なし / あり（/ QLoRA）を比べ、資料にない質問で断れるか、同時に聞いたときの速さも測る

## 09. 画像も入れてみる

- ファイル: [09_image_input.ipynb](09_image_input.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/09_image_input.ipynb)
- 結果の置き場: [docs/results/09_image_input.md](../docs/results/09_image_input.md)

内容:

1. vLLM で Gemma 4 26B-A4B を画像の部品つきで API サーバーとして起動する（08 から `--language-model-only` を外し、`--max-num-batched-tokens` を 4096 に）
2. 答えが分かっている画像（グラフ・レシート・スクショ・図形・計算・このリポジトリの図・写真）を作り、`image_url` で送って 14 問を採点する
3. 画像 1 枚のトークン数、速さ、メモリ、画像の細かさ（70 / 280 / 1120 トークン）による小さい字の読み取りの差を測る

## 10. RAG の検索をよくする

- ファイル: [10_rag_retrieval.ipynb](10_rag_retrieval.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/10_rag_retrieval.ipynb)
- 結果の置き場: [docs/results/10_rag_retrieval.md](../docs/results/10_rag_retrieval.md)

内容:

1. 08 と同じモデル・資料（コミット `e4570f0` に固定）・質問で、検索のやり方だけを A〜F の 6 段階で変える
2. リポジトリ名を外す → 大きい埋め込み（e5-large）→ BM25 とのハイブリッド → リランカー（bge-reranker-v2-m3）→ 質問の言い換え
3. それぞれで検索（1 位・上位 5・上位 10・MRR）、RAG の正答率、資料にない質問で断れるか、検索の時間を測る

## 11. Qwen3-VLで画像を判定する

- ファイル: [11_qwen3_vl_image_judge.ipynb](11_qwen3_vl_image_judge.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/11_qwen3_vl_image_judge.ipynb)
- 詳細: [追加ノート11〜13のガイド](../docs/07-notebooks-11-13.md#11-qwen3-vlで画像を判定)

内容:

1. `Qwen/Qwen3-VL-8B-Instruct`をbitsandbytes NF4 4bitで読み込む
2. 画像と日本語の質問を渡す判定関数を作る
3. Gradio UIまたはColab標準アップロードから画像を入力する

L4向けの既定値です。30B版はより大きなVRAMが必要です。現時点では定量評価の記録はなく、用途に合わせた正解データで別途評価してください。

## 12. Qwen-Image-2.1で画像を生成する

- ファイル: [12_qwen_image_2_1_colab.ipynb](12_qwen_image_2_1_colab.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/12_qwen_image_2_1_colab.ipynb)
- 詳細: [追加ノート11〜13のガイド](../docs/07-notebooks-11-13.md#12-qwen-image-21で画像を生成)

内容:

1. GPU・VRAM・ディスク容量を確認し、Diffusersなどを準備する
2. プロンプト、縦横比、seedを設定して1024帯の下書きを生成する
3. 同じseedで解像度とstep数を上げ、本番PNGを保存する

L4またはA100を推奨します。インストール後はランタイム再起動が必要です。L4で不安定な場合は本番解像度を1280帯へ下げてください。

## 13. Qwen 27B GGUF実行とAbliteration

- ファイル: [13_qwen27b_gguf_abliteration.ipynb](13_qwen27b_gguf_abliteration.ipynb)
- [Colab で開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/13_qwen27b_gguf_abliteration.ipynb)
- 詳細: [追加ノート11〜13のガイド](../docs/07-notebooks-11-13.md#13-qwen-27b-gguf実行とabliteration)

内容:

1. `llama.cpp`をCUDA対応でビルドし、Qwen3.8-27B Q4_K_Mを実行する
2. `--fit`で空きVRAMに合わせてGPU層数を自動調整する
3. 別のHugging Face CausalLMから拒否方向を抽出し、`o_proj`・`down_proj`を直交化して保存する

GGUF推論とAbliterationは独立した実験です。Abliteration後のモデルをGGUF推論へ自動反映する処理は含みません。拒否挙動を弱める処理は、隔離された研究環境でのみ使用してください。
