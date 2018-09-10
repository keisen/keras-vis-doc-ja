## Saliencyとは

*鳥クラス*の学習用画像の全てに樹が含まれていたと仮定します。
CNNが画像内の鳥に関連するピクセルを使っているのか、それとも樹や葉のような鳥以外の特徴を使っているかどうかを、どうやって知ることができますか？
これは実際、あなたが考えるよりも頻繁に起こり、小さな学習セットの場合は特に疑う必要があります。

Saliencyマップはこの論文で最初に扱われました：[Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps](https://arxiv.org/pdf/1312.6034v2.pdf)。

Saliencyの背景にあるアイデアは今となっては かなりシンプルです。
入力画像に関する出力カテゴリの勾配を計算します。

$$\frac{\partial output}{\partial input}$$

これは入力の小さな変化に関して出力値がどのように変化するかを教えてくれます。
出力の最も大きな変化の要因となる入力領域を強調するために、これらの勾配を使うことができます。
これは最も出力への貢献が顕著な(salient)入力領域を直感的に強調するはずです。


## 使い方

Saliencyを可視化するための２つのAPIがあります。

1. [visualize_saliency](../vis.visualization.md#visualize_saliency): Saliencyを可視化するための汎用APIです。
2. [visualize_saliency_with_losses](../vis.visualization.md#visualize_saliency_with_losses): いくつかのカスタムな重み付き付き損失を扱うような研究利用向けです。

[examples/](https://github.com/raghakot/keras-vis/tree/master/examples)にあるコードを参照してください。

### シナリオ

APIはとても一般的な目的で、幅広く様々なシナリオで利用可能です。
最も一般的なユースケースを下記にリストアップします：

#### 分類出力をおこなう全結合層の可視化

`layer_idx`に最後の`全結合`層を設定し、`filter_indices`で出力したいカテゴリを指定することで、
出力ノードに対する活性化に最も貢献する`seed_input`の一部を可視化することができます。

- 複数クラス分類では、filter_indicesは１つのクラスを指すことができます。 
- 複数ラベル分類では、適切なfilter_indicesを指定するだけです。

#### 回帰出力をおこなう全結合層の可視化

回帰出力では、`filter_indices`の出力を

* 増加させる入力
* 減少させる入力
* 維持する入力

に対する注目を可視化できます。
例えば、継続的に操舵出力を回帰する自動運転モデルを考えます。
このモデルは回帰出力の増加、減少または維持に貢献する`seed_input`の一部を可視化できます。

デフォルトでは、Saliencyは出力活性化をどうやって増加させるかを教えてくれます。
自動運転のケースで言えば、操舵角度の増加に貢献する入力画像の一部のみを伝えてくれます。
その他は勾配修飾子オプションを使用して可視化できます。
その名前から連想する通り、入力に関する損失の勾配を編集するために使われます。

- 出力の減少を可視化するには、`grad_modifier='negate'`を使います。
デフォルトでは、`ActivationMaximization`損失は出力を増加させる入力領域に正の勾配を生じさせます。
`grad_modifier='negate'`の設定により、（減少を示す）負の勾配を正として扱えるので、減少するケースを可視化できます。
- 出力に何が貢献したかを可視化するために、正または負のとても小さな値の勾配について考察の必要があります。
これは小さな勾配を増幅するための`grads = abs(1 / grads)`を実行すれば実現できます。
同等のことは`grad_modifier='small_values'`で実現できます。
[gradient_modifiers](../vis.grad_modifiers.md) はとても強力で、他の可視化APIでもよく使われます。

[self diving car](https://github.com/raghakot/keras-vis/tree/master/applications/self_driving)の例で、実際のアプリケーションを見ることができます。

#### ガイド付き（または矯正付き）Saliency

Zielerらは、誤差逆伝播ステップの負で勾配をクリップすることを考えました。
すなわち、出力の増加を伝える正の勾配情報だけを伝搬します。
これを矯正されたSaliencyまたは逆畳み込みのSaliencyと呼びます。
詳細は論文で見ることができます：[Visualizing and Understanding Convolutional Networks](https://arxiv.org/pdf/1311.2901.pdf)。

ガイド付きSaliencyでは、誤差逆伝播ステップは正の活性化を得る正の勾配だけを伝播するよう変更されます。
詳しくは論文を参照してください：[String For Simplicity: The All Convolutional Net](https://arxiv.org/pdf/1412.6806.pdf)

いずれのケースも、それぞれ`backprop_modifier='relu'`や`backprop_modifier='guided'`で実現できます。
クレイジーな研究に挑戦するため、自分だけの[backprop_modifier](../vis.backprop_modifiers.md)を実装することもできます。

#### 畳み込みフィルタのSaliency

`layer_idx`で`畳み込み`層を指して、フィルタに影響を与える画像の一部を可視化できます。
これはフィルタが影響を受けるものについて、気付きを得る助けになります。
ここでは、`filter_indices`はレイヤ内の`畳み込み`フィルタのインデックスを参照しています。

