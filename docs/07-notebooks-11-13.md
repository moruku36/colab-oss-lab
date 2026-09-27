# 追加ノート11〜13の使い方と注意

このページは、実験11〜13の目的、実行順、必要なGPU、現時点の検証状態をまとめたものです。3冊は同じ「画像・大規模モデル」領域にありますが、役割は異なります。

| # | 主な目的 | 入力 → 出力 | 推奨GPU | 現在の状態 |
|---|---|---|---|---|
| 11 | 画像を読んで判定する | 画像 + 質問 → 日本語回答 | L4以上 | 実行手順を整備。定量評価は未記録 |
| 12 | 文章から画像を生成する | プロンプト → PNG | L4 / A100 | 下書き・本番フローを整備。定量評価は未記録 |
| 13 | 27B GGUF推論／拒否方向の研究 | 文章 → 文章、モデル → 加工モデル | L4以上 | OOM対策と加工コードを整備。加工効果の定量評価は未記録 |

「未記録」は失敗を意味しません。01〜10で行ったような、固定問題・速度・VRAM・正答率をそろえた比較をまだ残していない、という意味です。

## 共通の準備

1. Colabでノートを開く
2. ［ランタイム］→［ランタイムのタイプを変更］でGPUを選ぶ
3. GPU確認セルを実行し、GPU名とVRAMを確認する
4. セルの説明を読み、上から順に実行する
5. 終了後はランタイムを切断・削除する

Hugging Faceのトークンが必要な場合は、コードへ直接書かず、Colabの「シークレット」に保存してください。このリポジトリのノートは、公開前に実行出力・実行回数・Colabユーザー情報を除去しています。

## 11. Qwen3-VLで画像を判定

[ノート](../notebooks/11_qwen3_vl_image_judge.ipynb) / [Colabで開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/11_qwen3_vl_image_judge.ipynb)

既定モデルは`Qwen/Qwen3-VL-8B-Instruct`、量子化はNF4 4bitです。物体・文字・状況の説明や、選択肢を指定した分類を試せます。Gradioが使えない場合はColab標準アップロードへ切り替えます。

画像の見え方に依存するため、業務利用前には正解が分かっている画像を用意し、誤判定・判定不能・小さい文字の読み取りを分けて評価してください。

## 12. Qwen-Image-2.1で画像を生成

[ノート](../notebooks/12_qwen_image_2_1_colab.ipynb) / [Colabで開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/12_qwen_image_2_1_colab.ipynb)

実行順は、GPU確認 → ライブラリ導入 → ランタイム再起動 → モデル読込 → 設定 → 下書き → 本番です。1024帯・25 stepの下書きで構図を確認し、同じseedで本番を生成します。

メモリ不足時は、本番解像度を2048帯から1280帯へ下げ、他のモデルを同じセッションで読み込まないでください。それでも不足する場合はA100へ変更します。同じseedでもライブラリ版やGPUが変わると完全には同じ画像にならない場合があります。

## 13. Qwen 27B GGUF実行とAbliteration

[ノート](../notebooks/13_qwen27b_gguf_abliteration.ipynb) / [Colabで開く](https://colab.research.google.com/github/moruku36/colab-oss-lab/blob/main/notebooks/13_qwen27b_gguf_abliteration.ipynb)

### 前半: GGUF推論

- Qwen3.8-27BのQ4_K_M GGUFを`llama.cpp`で実行します。
- `-ngl 99`のような固定値は使わず、`--fit on --fit-target 2048`で約2 GiBを残しながらGPU層数を自動調整します。
- GPUへ載らない層はCPU側で処理されるため、安定しやすくなる一方で速度は下がる場合があります。

### 後半: Abliteration

有害・無害プロンプトの最終トークン活性化からDifference of Meansを計算し、拒否方向を推定します。その方向へ出力する成分を`o_proj`と`down_proj`からインプレースで除去します。

- 既定のHugging Faceモデルは`Qwen/Qwen2.5-3B-Instruct`で、前半の27B GGUFとは別モデルです。
- 加工後はHugging Face形式で保存されます。GGUFへの変換・量子化は別工程です。
- データセットや層選択に結果が依存し、一般能力が低下する可能性があります。
- 安全性を弱める可能性があります。公開サービスへ直接載せず、加工前後の有害応答率と一般能力を両方評価してください。

## 次に記録したい測定

- GPU名、VRAM使用量、実行時間
- 使用したモデルID・リビジョン・主要パッケージ版
- 成功・失敗した入力と判定基準
- メモリ不足時に変更した設定
- 13では加工前後の拒否率、無害質問の品質、保存物のサイズ
