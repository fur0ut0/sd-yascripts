`--config` で渡すことができる設定ファイルに関する説明です。

現在は DreamBooth の手法向けのデータセットの設定のみに対応しています。
fine tuning の手法に関わる設定及びデータセットに関わらない設定には未対応です。

## 概要

設定ファイルを渡すことにより、ユーザが細かい設定を行えるようにします。

* 複数のデータセットを指定できるようになります。
    * 例えば `resolution` や `shuffle_keep_tokens` をデータセットごとに設定して、それらを混合して学習できます。

`--config` オプションを使った場合、役割が重複しているコマンドライン引数は無視されます。

設定ファイルの形式は JSON か TOML を利用できます。
記述のしやすさを考えると [TOML](https://toml.io/ja/v1.0.0-rc.2) を利用するのがオススメです。

## データセットに関する設定

データセットに関する設定として登録可能なアイテムを説明します。

* `general`
    * 全データセットに適用されるオプションを指定します。
* `dataset`
    * 特定のデータセットに適用されるオプションを指定します。
* `dataset.subset`
    * データセット内のサブセットのオプションを指定します。
    * 1つのディレクトリが1つのサブセットに対応します。

各アイテムは指定可能なオプションが決まっています。
また、学習方法が対応しているモードによって指定可能なオプションが変化します。

* DreamBooth の手法が使えるモード
* fine tuning の手法が使えるモード
* caption dropout が使えるモード

以下、各アイテム及び各モードで利用可能なオプションを説明します。

コマンドライン引数と共通のオプションの説明については割愛します。
他の README を参照してください。

### 全モード共通オプション

| オプション名 | general | dataset | dataset.subset |
| ---- | ---- | ---- | ---- |
| `batch_size` | o | o | - |
| `bucket_no_upscale` | o | o | - |
| `bucket_reso_steps` | o | o | - |
| `cache_latents` | o | o | o |
| `color_aug` | o | o | o |
| `enable_bucket` | o | o | - |
| `face_crop_aug_range` | o | o | o |
| `flip_aug` | o | o | o |
| `max_bucket_reso` | o | o | - |
| `min_bucket_reso` | o | o | - |
| `num_repeats` | o | o | o | o |
| `image_dir` | - | - | o（必須） |
| `random_crop` | o | o | o |
| `resolution` | o | o | - |
| `shuffle_caption` | o | o | o |
| `shuffle_keep_tokens` | o | o | o |

* `batch_size`
    * コマンドライン引数の `--train_batch_size` と同等です。
* `num_repeats`
    * サブセットの画像の繰り返し回数を指定します。デフォルトは 1 です。
* `image_dir`
    * 画像が入ったディレクトリパスを指定します。画像はディレクトリ直下に置かれている必要があります。
* `shuffle_keep_tokens`
    * コマンドライン引数の `--keep_tokens` と同等です。

### DreamBooth の手法が使えるモードで追加で利用可能なオプション

| オプション名 | general | dataset | dataset.subset |
| ---- | ---- | ---- | ---- |
| `caption_extension` | o | o | o |
| `class_tokens` | - | - | o |
| `is_reg` | - | - | o |

* `class_tokens`
    * クラストークンを設定します。例えば `sks girl` などを指定します。
    * 画像と対応する caption ファイルが存在しない場合にのみ学習時に使われます。判定は画像ごとに行います。
* `is_reg`
    * サブセットが正規化用かどうかを指定します。デフォルトは false です。

### fine tuning の手法が使えるモードで追加で利用可能なオプション

| オプション名 | general | dataset | dataset.subset |
| ---- | ---- | ---- | ---- |
| `in_json` | - | - | o |

### caption dropout 対応モードで追加で利用可能なオプション

| オプション名 | general | dataset | dataset.subset |
| ---- | ---- | ---- | ---- |
| `caption_dropout_every_n_epochs` | o | o | o |
| `caption_dropout_rate` | o | o | o |
| `caption_tag_dropout_rate` | o | o | o |

## 設定ファイルの例

[samples](./samples/config_sample.toml) に例を載せているので、そちらを参照してください。

## 無視されるコマンドライン引数

`--config` オプションを使った場合に無視されるコマンドライン引数について説明します。

TODO: 説明を書く