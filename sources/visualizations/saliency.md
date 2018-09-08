## Saliencyとは

Suppose that all the training images of *bird* class contains a tree with leaves. How do we know whether the CNN is 
using bird-related pixels, as opposed to some other features such as the tree or leaves in the image? This actually happens more often than you think and you should be especially suspicious if you have a small training set. 

*鳥クラス*の学習用画像の全てに樹が含まれていたと仮定します。CNNが画像内の鳥に関連するピクセルを使っているかどうか、それとは対照的に樹や葉のようなその他の特徴を使っているか、どうやって知ることができますか？これは実際あなたが考える以上によく起こり、小さな学習セットの場合、特に疑わなければなりません。

Saliency maps was first introduced in the paper: 
[Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps](https://arxiv.org/pdf/1312.6034v2.pdf)

Saliencyマップは論文で最初に導入されました：[Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps](https://arxiv.org/pdf/1312.6034v2.pdf)

The idea behind saliency is pretty simple in hindsight. We compute the gradient of output category with respect to 
input image. 

Saliencyの背景にあるアイデアは、後になって考えればかなりシンプルです。入力画像に関する出力カテゴリの勾配を計算します。

$$\frac{\partial output}{\partial input}$$

This should tell us how the output value changes with respect to a small change in inputs. We can use these gradients 
to highlight input regions that cause the most change in the output. Intuitively this should highlight salient image 
regions that most contribute towards the output.

これは入力への小さな変化に関する出力の変化を教えてくれます。
出力の最も大きな変化の要因となる入力領域を強調するために、これらの勾配を使うことができます。
直感的に、これは最も出力への貢献が顕著な入力領域を強調するはずです。

## 使い方

There are two APIs exposed to visualize saliency.

Saliencyを可視化するための２つのAPIがあります。

1. [visualize_saliency](../vis.visualization.md#visualize_saliency): This is the general purpose API for visualizing
saliency.
1. [visualize_saliency](../vis.visualization.md#visualize_saliency): これはSaliencyを可視化するための汎用APIです。

2. [visualize_saliency_with_losses](../vis.visualization.md#visualize_saliency_with_losses): This is intended for 
research use-cases where some custom weighted loss can be used.
2. [visualize_saliency_with_losses](../vis.visualization.md#visualize_saliency_with_losses): これはいくつかのカスタムな重み付き付き損失を扱うような研究利用向けです。

[examples/](https://github.com/raghakot/keras-vis/tree/master/examples)にあるコードを参照してください。


### シナリオ

APIはとても汎用的な目的で、幅広く様々なシナリオで利用可能です。もっとも一般的なユースケースを下記にリストアップします：

#### 分類出力をおこなう全結合層の可視化

By setting `layer_idx` to final `Dense` layer, and `filter_indices` to the desired output category, we can visualize 
parts of the `seed_input` that contribute most towards activating the corresponding output nodes,

`layer_idx`を最後の`全結合`層に設定し、`filter_indices`で出力カテゴリを指定することで、
出力ノードに対する活性化に最も貢献する`seed_input`の一部を可視化することができます。

- For multi-class classification, `filter_indices` can point to a single class.
- For multi-label classifier, simply set the appropriate `filter_indices`.

複数クラス分類問題では、filter_indicesは１つのクラスを指すことができます。 
複数ラベル分類では、適切なfilter_indicesを指定するだけです。

#### Regression Dense layer visualization

For regression outputs, we could visualize attention over input that 

- increases
- decreases
- maintains

the regressed `filter_indices` output. For example, consider a self driving model with continuous regression steering 
output. One could visualize parts of the `seed_input` that contributes towards increase, decrease or maintenance of 
predicted output.

By default, saliency tells us how to increase the output activations. For the self driving car case, this only tells
us parts of the input image that contribute towards steering angle increase. Other use cases can be visualized by 
using `grad_modifier` option. As the name suggests, it is used to modify the gradient of losses with respect to inputs. 

- To visualize decrease in output, use `grad_modifier='negate'`. By default, `ActivationMaximization` loss yields 
positive gradients for inputs regions that increase the output. By setting `grad_modifier='negate'` you can treat negative
gradients (which indicate the decrease) as positive and therefore visualize decrease use case.

- To visualize what contributed to the predicted output, we want to consider gradients that have very low positive
or negative values. This can be achieved by performing `grads = abs(1 / grads)` to magnifies small gradients. Equivalently, 
you can use `grad_modifier='small_values'`, which does the same thing. [gradient_modifiers](../vis.grad_modifiers.md) 
are very powerful and show up in other visualization APIs as well.

You can see a practical application for this in the 
[self diving car](https://github.com/raghakot/keras-vis/tree/master/applications/self_driving) example.

#### Guided / rectified saliency

Zieler et al. has the idea of clipping negative gradients in the backprop step. i.e., only propagate positive gradient
information that communicates the increase in output. We call this rectified or deconv saliency. Details can be found 
in the paper: [Visualizing and Understanding Convolutional Networks](https://arxiv.org/pdf/1311.2901.pdf).

In guided saliency, the backprop step is modified to only propagate positive gradients for positive activations.
For details see the paper: [String For Simplicity: The All Convolutional Net](https://arxiv.org/pdf/1412.6806.pdf).

For both these cases, we can use `backprop_modifier='relu'` and `backprop_modifier='guided'` respectively. You 
can also implement your own [backprop_modifier](../vis.backprop_modifiers.md) to try your crazy research idea :)

#### Conv filter saliency

By pointing `layer_idx` to `Conv` layer, you can visualize parts of the image that influence the filter. This might 
help you discover what a filter cares about. Here, `filter_indices` refers to the index of the `Conv` filter within 
the layer.
