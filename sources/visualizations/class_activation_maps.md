## Class Activation Mapとは

Class activation mapsまたはgrad-CAMは、入力の注目領域を可視化するもう１つの方法です。モデルの出力（[saliency](saliency.md)を参照）に関する勾配を使う代わりに、grad-CAMは後ろから２番目（`全結合`層の前）の`畳み込み`層の出力を使います。
直感的には、`全結合`層で完全に失われる空間情報を利用するために、最も出力に近い`畳み込み`層を使うことです。

keras-visでは、[Class Activation maps](http://cnnlocalization.csail.mit.edu/)よりも汎用的で熟考された[grad-CAM](https://arxiv.org/pdf/1610.02391.pdf)を使います。

## 使い方

grad-CAMで可視化するための２つのAPIがあり、それらは[saliencyの使い方](saliency.md#Usage)とほとんど同じです。

1. [visualize_cam](../vis.visualization.md#visualize_cam): これはgrad-CAMで可視化するための汎用APIです。
2. [visualize_cam_with_losses](../vis.visualization.md#visualize_cam_with_losses): これはいくつかのカスタムな重み付き付き損失を扱うような研究利用向けです。

注目するべき追加は`penultimate_layer_idx`パラメータだけです。これにより特定のpre-layerの出力の勾配が使われるようにできます。デフォルトでは、keras-visは最も近いレイヤをフィルタで探します。

### シナリオ

[saliencyのシナリオ](saliency.md#scenarios)を参照して下さい。すべては、追加された`penultimate_layer_idx`パラメータに期待して下さい。

## 了解

grad-CAMは後ろから２番目のレイヤと可視化されるレイヤが近いときのみ、上手く機能します。これは`畳み込み`フィルタの可視化にも言えます。あなたのモデルがこのケースに該当しない場合はsaliencyを使うほうが良いです。
