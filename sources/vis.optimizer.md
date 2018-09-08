
**Source:** [vis/optimizer.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/optimizer.py#L0)

-------------------

## [Optimizer](https://github.com/raghakot/keras-vis/tree/master/vis/optimizer.py#L18)

-------------------

### [Optimizer.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/optimizer.py#L20)

```python
__init__(self, input_tensor, losses, input_range=(0, 255), wrt_tensor=None, norm_grads=True)
```

重み付き損失関数を最小化するオプティマイザを生成します。

*引数:*

 - **input_tensor**:  入力テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
 - **losses**:  ([Loss](vis.losses.md#Loss), weight)のタプルのリスト.
 - **input_range**:  入力値の範囲を`(min, max)`のタプルで指定します。
  これは最終的に最適化された入力を、指定されたの範囲にスケールするために使用されます（デフォルト値：(0, 255)）。
 - **wrt_tensor**:  `losses`から集計した損失を`wtr_tensor`に関して最小化する必要があることをオプティマイザに指示します。
  `wrt_tensor`にはモデルグラフのいずれかのテンソルを指定できます。
  デフォルト値はNoneで、これは単に損失を`input_tensor`に関して最小化することを意味します。
 - **norm_grads**:  勾配を正規化する場合はTrue。
  正規化は小さすぎるまたは大きすぎる勾配を回避し、滑らかな勾配降下プロセスを確保します。
  実際の勾配を扱う場合は（例えば、注目領域の可視化）、Falseをセットしてください。

-------------------

### [Optimizer.minimize](https://github.com/raghakot/keras-vis/tree/master/vis/optimizer.py#L108)

```python
minimize(self, seed_input=None, max_iter=200, input_modifiers=None, grad_modifier=None, \
    callbacks=None, verbose=True)
```

定義された損失に関して、入力画像の勾配降下をおこなう。

*引数:*

 - **seed_input**:  N次元のnumpy array。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
  Noneが設定された場合、ランダムなノイズでシードされる。（デフォルト値：None）
 - **max_iter**:  勾配降下を繰り返す最大数。（デフォルト値：２００）
 - **input_modifiers**:  最適化プロセス中、最適化される入力をどのように変更するか、`pre`と`post`で指定された[InputModifier](vis.input_modifiers.md#inputmodifier)インスタンスのリスト。
  `pre`はリストの並び順に適用され、`post`は逆順に適用されます。例えば、`input_modifiers = [f, g]`は
  `pre_input = g(f(inp))`と`post_input = f(g(inp))`を意味します。
 - **grad_modifier**:  使用するgradient modifierについては[grad_modifiers](vis.grad_modifiers.md)を参照してください。
 　　何も指定しない場合、勾配は変更されません。（デフォルト値：None）
 - **callbacks**:  トリガーする[OptimizerCallback](vis.callbacks.md#optimizercallback)インスタンスのリスト。
 - **verbose**:  各勾配降下反復の最後に、各損失のログを出力する。損失係数を見積もるのにとても有用です。（デフォルト値：True）

*戻り値:*

勾配降下後の`(optimized input, grads with respect to wrt, wrt_value)`のタプル




