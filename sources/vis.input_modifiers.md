
**Source:** [vis/input_modifiers.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L0)

-------------------

## [InputModifier](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L9)

input modifierを定義するための抽象クラスです。
[Optimizer.minimize](vis.optimizer.md#optimizerminimize)でinput modifierを使用すると最適化処理中、最適化される入力値に対して、更新前後（`pre`, `post`）に変更を加えることができます。

```python
modifier.pre(seed_input)
# gradient descent update to img
modifier.post(seed_input)
```

-------------------

### [InputModifier.post](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L34)

```python
post(self, inp)
```

勾配降下更新後の入力への変更を実装してください。
もし後処理をしない場合、単にこの実装を無視して下さい（i.e., 実装しない）。
デフォルトで引数`inp`をそのまま返します。

*引数:*

 - **inp**:  N次元のnumpy array。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。

*戻り値:*

編集された入力

-------------------

### [InputModifier.pre](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L21)

```python
pre(self, inp)
```

勾配降下更新前の入力への変更を実装してください。
もし後処理をしない場合、単にこの実装を無視して下さい（i.e., 実装しない）。
デフォルトで引数`inp`をそのまま返します。

*引数:*

 - **inp**:  N次元のnumpy array。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。

*戻り値:*

編集された入力


-------------------

## [Jitter](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L48)


-------------------

### [Jitter.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L50)

```python
__init__(self, jitter=0.05)
```

入力値の更新前に、ランダムJitterを導入するためのinput modifier実装です。
Jitterは鮮明なActivationMaximization画像を生成することがわかっています。

*引数:*

 - **jitter**:  適用されるjitter量をあらわすスカラ値、またはシーケンス。
  スカラ値の場合, 同じjitterが画像の全ての次元に適用されます。シーケンスの場合、`jitter`は画像の次元ごとの値を含むべきです。

  `[0., 1.]`の間の値は画像の寸法に対する比率として解釈されます。（デフォルト値：0.05）
  
-------------------

### [Jitter.post](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L34)

```python
post(self, inp)
```

-------------------

### [Jitter.pre](https://github.com/raghakot/keras-vis/tree/master/vis/input_modifiers.py#L83)

```python
pre(self, img)
```
