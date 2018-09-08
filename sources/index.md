# keras-vis: Keras Visualization Toolkit
[![Build Status](https://travis-ci.org/raghakot/keras-vis.svg?branch=master)](https://travis-ci.org/raghakot/keras-vis)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/raghakot/keras-vis/blob/master/LICENSE)
[![Slack](https://img.shields.io/badge/slack-discussion-E01563.svg)](https://keras-vis.herokuapp.com/)

keras-visはKerasモデルの可視化とデバッグのためのハイレベルツールキットです。
現在サポートされている可視化:

- Activation maximization
- Saliency maps
- Class activation maps

全ての可視化はデフォルトでN次元画像入力をサポートしています。すなわち、モデルへのN次元画像入力を一般化しています。

keras-visは簡潔で使いやすく、拡張可能なインタフェースを備えています。上記の可視化全てをエネルギー最小化問題として一般化しています。
theanoとtensorflow両方のバックエンドと互換性があり、'channels_first'と'channels_last'両方のデータ形式を扱えます。

### クイックリンク
* [https://keisen.github.io/keras-vis-docs-ja](https://keisen.github.io/keras-vis-docs-ja) のドキュメントを読んでください。
* 質問や議論のためにSlackの [チャンネル](https://keras-vis.herokuapp.com/) に参加して下さい。
* 私たちは[waffle.io](https://waffle.io/raghakot/keras-vis)で新しい機能やタスクを追跡しています。PullRequestで協力してもらえると嬉しいです。

## Getting Started
画像の誤差逆伝搬問題は、損失関数を最小にする入力画像を生成することがゴールです。画像の誤差逆伝搬問題の設定は簡単です。

**重み付き損失関数を定義**

様々な有用な損失関数が[losses](vis.losses.md)で定義されます。
カスタム損失関数は[Loss.build_loss](vis.losses.md/#lossbuild_loss)を実装することで定義できます。

```python
from vis.losses import ActivationMaximization
from vis.regularizers import TotalVariation, LPNorm

filter_indices = [1, 2, 3]

# タプルは(損失関数, 重み係数)で構成されています。
# 必要に応じて正則化(regularizers)も追加してください。
losses = [
    (ActivationMaximization(keras_layer, filter_indices), 1),
    (LPNorm(model.input), 10),
    (TotalVariation(model.input), 10)
]
```

**重み付き損失を最小化するための最適化設定**

自然な画像を生成するために、画像探索空間は正則化ペナルティを用いて制限されます。
いくつかの汎用的な正則化は[regularizers](vis.regularizers.md)に定義されています。
損失関数のように、[Loss.build_loss](vis.losses.md/#lossbuild_loss)を実装することでカスタム正則化を定義できます。

```python
from vis.optimizer import Optimizer

optimizer = Optimizer(model.input, losses)
opt_img, grads, _ = optimizer.minimize()
```

サポートされている可視化の具体例は[examples folder](https://github.com/raghakot/keras-vis/tree/master/examples)にあります。


## インストール

!!! Note
    現在PyPIに登録されているkeras-visは古いバージョンのため、多くの不具合を含みます。
    ソースコードからインストールすることをお勧めします。

1) theanoまたはtensorflowバックエンドと共に[keras](https://github.com/keras-team/keras/blob/master/README.md#installation)をインストールします。
Kerasのバージョンが2.0以上であることに注意して下さい。

2) keras-visをインストールします。
> ソースコードからインストール
```bash
sudo python setup.py install
```

> PyPIパッケージからインストール
```bash
sudo pip install keras-vis
```

### 可視化とは

!!! Note
    現在、リンクは壊れておりドキュメント全体を修正中です。 サンプルは`examples/`配下を参照して下さい。

ニューラルネットはブラックボックスです。近年では、畳み込みネットワークを理解し可視化するための、いくつかのアプローチが開発されています。
それらの論文は私達にブラックボックに耳を傾け、誤分類を診断し、ネットワークが過学習（または未学習）かどうかを評価する方法を提供します。

Guided backprop can also be used to create [trippy art](https://deepdreamgenerator.com/gallery), neural/texture 
[style transfer](https://github.com/jcjohnson/neural-style) among the list of other growing applications.

さまざまな視覚化がそれぞれのページで文書化され、ここに要約されています。

<hr/>

### [Conv filter visualization](visualizations/conv_filters)
<img src="https://raw.githubusercontent.com/raghakot/keras-vis/master/images/conv_vis/cover.jpg?raw=true"/>

*畳み込みフィルタは、類似のテンプレートパターンが入力画像内に見られるとき、出力を最大化する'テンプレートマッチング'フィルタを学習します。
アクティベーションの最大化によってこれらのテンプレートを視覚化します。*

<hr/>

### [Dense layer visualization](visualizations/dense)

<img src="https://raw.githubusercontent.com/raghakot/keras-vis/master/images/dense_vis/cover.png?raw=true"/>

*私たちは、ネットワークが過学習（または未学習）か、一般化が適切かどうかをどのように評価できますか？*

<hr/>

### [Attention Maps](visualizations/attention)

<img src="https://raw.githubusercontent.com/raghakot/keras-vis/master/images/attention_vis/cover.png?raw=true"/>

*私たちはネットワークがその出力を決定する際、画像の正しい部分を注視しているかどうかをどのように評価できますか？*

<hr/>

### 最適化の進捗をGIFアニメで出力する

[callbacks](vis.callbacks.md)を活用して最適化の進捗をGIFアニメとして生成可能です。
次の例は、'オゼル'クラス（output_index: 20）のActivation maximizationによる可視化です。

```python
from vis.losses import ActivationMaximization
from vis.regularizers import TotalVariation, LPNorm
from vis.modifiers import Jitter
from vis.optimizer import Optimizer

from vis.callbacks import GifGenerator
from vis.utils.vggnet import VGG16

# VGG16をImageNetのWeightsで構築する
model = VGG16(weights='imagenet', include_top=True)
print('Model loaded.')

# 可視化したいレイヤの名前（vggnet.pyのモデル定義を参照して下さい）
layer_name = 'predictions'
layer_dict = dict([(layer.name, layer) for layer in model.layers[1:]])
output_class = [20]

losses = [
    (ActivationMaximization(layer_dict[layer_name], output_class), 2),
    (LPNorm(model.input), 10),
    (TotalVariation(model.input), 10)
]
opt = Optimizer(model.input, losses)
opt.minimize(max_iter=500, verbose=True, image_modifiers=[Jitter()], callbacks=[GifGenerator('opt_progress')])

```

出力がどのように変化するか注目して下さい。　これは鮮明な活性化最大化画像を生成することが知られている[ImageModifier]の一種である[Jitter](vis.modifiers.md/#jitter)を使っているからです。

練習として次に挑戦して下さい。

- Jitterを取り除く
- 損失の重みを変化させる

![opt_progress](https://raw.githubusercontent.com/raghakot/keras-vis/master/images/opt_progress.gif?raw=true "Optimization progress")

<hr/>

## 引用

もしkeras-visがあなたの研究の役立ったなら、資料には引用をお願いします。ここにBibTeXエントリの例を示します。

```
@misc{raghakotkerasvis,
  title={keras-vis},
  author={Kotikalapudi, Raghavendra and contributors},
  year={2017},
  publisher={GitHub},
  howpublished={\url{https://github.com/raghakot/keras-vis}},
}
```
