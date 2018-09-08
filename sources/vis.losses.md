
**Source:** [vis/losses.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L0)

-------------------

## [Loss](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L7)

最小化される損失関数を定義するための抽象クラスです。
損失関数は`build_loss`関数を定義して構築されるべきです。

`name`属性は冗長な出力を伴う損失関数を識別するために定義されるべきです。
オーバーライドしない場合、デフォルトでは'Unnamed Loss'になります。

-------------------

### [Loss.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L14)

```python
__init__(self)
```

自身を初期化します。正確なシグネチャについては`help(type(self))`で参照してください。

-------------------

### [Loss.build_loss](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L20)

```python
build_loss(self)
```

損失関数式を構築するために、この関数を実装します。
この損失関数を構築するために必要な追加の引数は`__init__`経由で渡すことができます。

理想的には、この関数式は全てのKerasバックエンドと、`channels_first`や`channels_last`の画像データフォーマットと互換性を持つべきです。
`utils.slicer`を使えば、データフォーマットを気にせずスライスを定義できます。
（`channels_first`フォーマットで定義すると、`channels_last`フォーマットを使うTensorflow用にインデックスを自動的にシャッフルします。 ）

```python
# theano slice
conv_layer[:, filter_idx, ...]

# TF slice
conv_layer[..., filter_idx]

# Backend agnostic slice
conv_layer[utils.slicer[:, filter_idx, ...]]
```

[utils.get_img_shape](vis.utils.utils.md#get_img_shape)はこれを簡単にするもう一つのオプショナルユーティリティです。

*戻り値:*

  損失式

-------------------

## [ActivationMaximization](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L48)


特定レイヤ内のフィルタセットの活性化を最大化する損失関数です。

この損失は通常とは反対の問い掛けのために使われますーどのような種類の入力画像でネットワークの確信度が増加するか、例えば犬クラスで。
これはネットワークに内在化しているものが'犬'の画像空間であると判断する助けになります。

これを最後の`keras.layers.Dense`レイヤで、'犬'と'人'の出力両方を最大化する入力画像の生成に使うこともできます。

-------------------

### [ActivationMaximization.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L58)

```python
__init__(self, layer, filter_indices)
```

*引数:*

 - **layer**:  最大化するフィルタのKerasレイヤ。これはconv層かdense層を指定できます。
 - **filter_indices**:  最大化するフィルタのindices。
 `keras.layers.Dense`レイヤであれば、`filter_idx`は出力インデックスとして解釈されます。

  もし貴方がクラス出力を最大化するために最後の`keras.layers.Dense`レイヤを最適化する場合、'softmax'とは対照的な活性化である'linear'の方がより良い結果を得る傾向があります。これは`softmax`出力が他のクラスを最小限にすることで最大化されることができるためです。

-------------------

### [ActivationMaximization.build_loss](https://github.com/raghakot/keras-vis/tree/master/vis/losses.py#L75)

```python
build_loss(self)
```
