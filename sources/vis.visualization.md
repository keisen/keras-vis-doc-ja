
**Source:** [vis/visualization/__init__.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/__init__.py#L0)

-------------------

### [visualize_activation_with_losses](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/activation_maximization.py#L13)

```python
visualize_activation_with_losses(input_tensor, losses, wrt_tensor=None, seed_input=None, \
    input_range=(0, 255), **optimizer_params)
```

重み付き`losses`を最小にする`input_tensor`を生成します。
この関数はカスタム損失を必要とする高度なユースケースを対象にしています。

*引数:*

 - **input_tensor**:  入力テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
 - **wrt_tensor**:  損失の勾配は、このテンソルに関して計算されます。
  Noneを指定した場合、これは`input_tensor`と同じであると想定される。（デフォルト値： None）
 - **losses**:  ([Loss](vis.losses.md#Loss), weight)のタプルのリスト。
 - **seed_input**:  最適化の開始画像にシードします。Noneを指定した場合、ランダム値で初期化されます。（デフォルト値：None）
 - **input_range**:  入力値の範囲を`(min, max)`のタプルで指定します。
  これは最終的に最適化された入力を、指定されたの範囲にスケールするために使用されます（デフォルト値：(0, 255)）。
 - **optimizer_params**:  [オプティマイザの\*\*kwargs引数](vis.optimizer.md#optimizerminimize). 
  オプティマイザの必須引数がない場合、適切なデフォルト値が設定されます。

*戻り値:*

重み付き`losses`を最小にするモデル入力

-------------------

### [get_num_filters](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/__init__.py#L15)

```python
get_num_filters(layer)
```

指定されたレイヤのフィルタ数を調べます。

*引数:*

 - **layer**:  Kerasレイヤ

*戻り値:*

レイヤ内のフィルタ数。`keras.layers.Dense`レイヤの場合、これは出力数になります。


-------------------

### [overlay](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/__init__.py#L33)

```python
overlay(array1, array2, alpha=0.5)
```

`array1`を`alpha`の透明度で`array2`の上に重ねます。

*Args:*

 - **array1**:  numpy array.
 - **array2**:  numpy array.
 - **alpha**:  透明度。 この値は[0, 1]の間であること。
   0を与えると`array2`のみが、1を与えると`array1`のみが描画されます(デフォルト値 = 0.5).

*戻り値:*

`array1`を`alpha`の透明度で`array2`の上に重ねた画像


-------------------

### [visualize_saliency_with_losses](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/saliency.py#L50)

```python
visualize_saliency_with_losses(input_tensor, losses, seed_input, wrt_tensor=None, \
    grad_modifier="absolute")
```

重み付き損失に関する`input_tensor`の正の勾配で、`seed_input`の注目ヒートマップを生成します。

この関数はカスタム損失を必要とする高度なユースケースを対象にしています。
汎用的なユースケースは`visualize_saliency`を参照してください。

Saliencyの完全な説明は次の論文を参照してください：[Deep Inside Convolutional Networks: Visualising Image Classification Models and Saliency Maps](https://arxiv.org/pdf/1312.6034v2.pdf)

*引数:*

 - **input_tensor**:  入力テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
 - **losses**:  ([Loss](vis.losses.md#Loss), weight)のタプルのリスト。
 - **seed_input**:  注目マップが可視化されるのに必要なモデルへの入力
 - **wrt_tensor**:  損失の勾配は、このテンソルに関して計算されます。
   Noneを指定した場合、これは`input_tensor`と同じであると想定される。（デフォルト値： None）
 - **grad_modifier**:  勾配修飾子。[grad_modifiers](vis.grad_modifiers.md)を参照してください。
   デフォルトでは絶対値（`absolute`）の勾配が使用されます。
   正または負の勾配を可視化するには、それぞれ`relu`および`negate`を使用して下さい。

*戻り値:*

重み付き損失に関する`seed_input`の正規化された勾配

-------------------

### [visualize_activation](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/activation_maximization.py#L54)

```python
visualize_activation(model, layer_idx, filter_indices=None, wrt_tensor=None, seed_input=None, \
    input_range=(0, 255), backprop_modifier=None, grad_modifier=None, act_max_weight=1, \
    lp_norm_weight=10, tv_weight=10, **optimizer_params)
```

指定された`layer_idx`の全ての`filter_indices`の出力を最大化するモデル入力を生成します。

*引数:*

 - **model**:  `keras.models.Model`インスタンス。モデル入力の形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`でなければなりません。
 - **layer_idx**:  可視化されるフィルタを含む`model.layers`内のレイヤインデックス
 - **filter_indices**:  最大化されるレイヤ内のフィルタインデックス。
  Noneが指定された場合、全てのフィルタが可視化されます。(デフォルト値： None)。
  `keras.layers.Dense`の場合、`filter_idx`は出力インデックスとして解釈されます。
  もし最終層の`keras.layers.Dense`を可視化するなら、より良い結果を得るために、[utils.apply_modifications](vis.utils.utils.md#apply_modifications)を使って、'softmax'活性化を'linear'に変更することを検討してください。
 - **wrt_tensor**:  損失の勾配は、このテンソルに関して計算されます。
  Noneを指定した場合、これは`input_tensor`と同じであると想定される。（デフォルト値： None）
 - **seed_input**:  最適化の開始画像にシードします。Noneを指定した場合、ランダム値で初期化されます。（デフォルト値：None）
 - **input_range**:  入力値の範囲を`(min, max)`のタプルで指定します。
  これは最終的に最適化された入力を、指定されたの範囲にスケールするために使用されます（デフォルト値：(0, 255)）。
 - **backprop_modifier**:  誤差逆伝播修飾子. [backprop_modifiers](vis.backprop_modifiers.md)を参照してください。もし何も指定しない場合、誤差逆伝播修飾は何も適用されません（デフォルト値：None）
 - **grad_modifier**:  勾配修飾子。[grad_modifiers](vis.grad_modifiers.md)を参照してください。もし何も指定しない場合、勾配は編集されません(デフォルト値：None)
 - **act_max_weight**:  `ActivationMaximization`損失の重み係数。ゼロまたはNoneを指定するとこのロスは使われません。(デフォルト値：1)
 - **lp_norm_weight**:  `LPNorm`正規化損失の重み係数。ゼロまたはNoneを指定するとこのロスは使われません。(デフォルト値：10)
 - **tv_weight**:  `TotalVariation`正規化損失の重み係数。ゼロまたはNoneを指定するとこのロスは使われません。(デフォルト値：10)
 - **optimizer_params**:  [オプティマイザの\*\*kwargs引数](vis.optimizer.md#optimizerminimize). オプティマイザの必須引数がない場合、適切なデフォルト値が設定されます。

*例:*

最終層が`keras.layers.Dense`で、出力インデックス２２を最大化する入力画像を可視化したいのであれば、
`filter_indices = [22]`かつ`layer_idx = dense_layer_idx`となります。

`filter_indices = [22, 23]`にすると、両方のクラスの特徴を示す入力画像が生成されるはずです。

*戻り値:*

与えられた`layer_idx`内の`filter_indices`の出力を最大化するモデル入力

-------------------

### [visualize_saliency](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/saliency.py#L83)

```python
visualize_saliency(model, layer_idx, filter_indices, seed_input, wrt_tensor=None, \
    backprop_modifier=None, grad_modifier="absolute")
```

指定された`layer_idx`の`filter_indices`の出力を最大化するための`seed_input`に、注目ヒートマップを生成します。

*引数:*

 - **model**:  `keras.models.Model`インスタンス。モデル入力の形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`でなければなりません。
 - **layer_idx**:  可視化されるフィルタを含む`model.layers`内のレイヤインデックス
 - **filter_indices**:  最大化されるレイヤ内のフィルタインデックス。
  Noneが指定された場合、全てのフィルタが可視化されます。(デフォルト値： None)。
  `keras.layers.Dense`の場合、`filter_idx`は出力インデックスとして解釈されます。
  もし最終層の`keras.layers.Dense`を可視化するなら、より良い結果を得るために、[utils.apply_modifications](vis.utils.utils.md#apply_modifications)を使って、'softmax'活性化を'linear'に変更することを検討してください。
 - **seed_input**:  注目マップが可視化されるのに必要なモデルへの入力
 - **wrt_tensor**:  損失の勾配は、このテンソルに関して計算されます。
   Noneを指定した場合、これは`input_tensor`と同じであると想定される。（デフォルト値： None）
 - **backprop_modifier**:  誤差逆伝播修飾子. [backprop_modifiers](vis.backprop_modifiers.md)を参照してください。もし何も指定しない場合、誤差逆伝播修飾は何も適用されません（デフォルト値：None）
 - **grad_modifier**:  勾配修飾子。[grad_modifiers](vis.grad_modifiers.md)を参照してください。もし何も指定しない場合、勾配は編集されません(デフォルト値：'absolute')

*例:*

もし'鳥'カテゴリへの注目を可視化したいなら、最終層の`keras.layers.Dense`の出力インデックスは２２なので、
`filter_indices = [22]`かつ`layer = dense_layer`となります。

また複数の値をフィルタインデックスに設定できます。例えば、 `filter_indices = [22, 23]`は（上手くいけば）
２２と２３のカテゴリ両方に関連する注目マップを得られるはずです。

*戻り値:*

`filter_indices`の出力の最大化に最も貢献する`seed_input`の領域を示したヒートマップ画像

-------------------

### [visualize_cam_with_losses](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/saliency.py#L130)

```python
visualize_cam_with_losses(input_tensor, losses, seed_input, penultimate_layer, grad_modifier=None)
```

重み付き損失に関する`input_tensor`の正の勾配を使って、CAMベースの勾配(grad-CAM)を生成します。

grad-CAMの詳細は論文を参照して下さい： [Grad-CAM: Why did you say that? Visual Explanations from Deep Networks via Gradient-based Localization](https://arxiv.org/pdf/1610.02391v1.pdf)

いくつかの例ではわずかながらネットワーク構造に変更が必要となるCAMとは異なり、grad-CAMはより適用範囲が広いです。

Saliancyと比較すると、grad-CAMはクラスを区別できます；　すなわち`猫`の説明には猫の領域のみを強調し、犬の領域は強調しません。そしてその逆もまた同様です。

*引数:*

 - **input_tensor**:  入力テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。
 - **losses**:  ([Loss](vis.losses.md#Loss), weight)のタプルのリスト。
 - **seed_input**:  注目マップが可視化されるのに必要なモデルへの入力
 - **penultimate_layer**:  フィルタ出力に関する勾配を計算するために使われるべき特徴マップを得るための`layer_idx`の１つ前のレイヤ。
 - **grad_modifier**:  勾配修飾子。[grad_modifiers](vis.grad_modifiers.md)を参照してください。
   何も指定しなければ勾配は変更されません（デフォルト値： None）

*戻り値:*

重み付き損失に関する`seed_input`の正規化済み勾配

-------------------

### [visualize_cam](https://github.com/raghakot/keras-vis/tree/master/vis/visualization/saliency.py#L192)

```python
visualize_cam(model, layer_idx, filter_indices, seed_input, penultimate_layer_idx=None, \
    backprop_modifier=None, grad_modifier=None)
```

指定された`layer_idx`の`filter_indices`の出力を最大化する、CAMベースの勾配（grad-CAM）を生成します。

*引数:*

 - **model**:  `keras.models.Model`インスタンス。モデル入力の形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`でなければなりません。
 - **layer_idx**:  可視化されるフィルタを含む`model.layers`内のレイヤインデックス
 - **filter_indices**:  最大化されるレイヤ内のフィルタインデックス。
  Noneが指定された場合、全てのフィルタが可視化されます。(デフォルト値： None)。
  `keras.layers.Dense`の場合、`filter_idx`は出力インデックスとして解釈されます。
  もし最終層の`keras.layers.Dense`を可視化するなら、より良い結果を得るために、[utils.apply_modifications](vis.utils.utils.md#apply_modifications)を使って、'softmax'活性化を'linear'に変更することを検討してください。
 - **seed_input**:  注目マップが可視化されるのに必要なモデルへの入力
 - **penultimate_layer_idx**:  フィルタ出力に関する勾配を計算するために使われるべき特徴マップを得るための`layer_idx`の１つ前のレイヤ。　何も指定しない場合、`layer_idx`に最も近い`畳み込み`または`プーリング`レイヤが設定されます。
 - **backprop_modifier**:  誤差逆伝播修飾子. [backprop_modifiers](vis.backprop_modifiers.md)を参照してください。もし何も指定しない場合、誤差逆伝播修飾は何も適用されません（デフォルト値：None）
 - **grad_modifier**:  勾配修飾子。[grad_modifiers](vis.grad_modifiers.md)を参照してください。もし何も指定しない場合、勾配は編集されません(デフォルト値：None

*例:*

もし`鳥`カテゴリへの注目を可視化したいなら、最終層の`keras.layers.Dense`の出力インデックスは２２なので、
`filter_indices = [22]`かつ`layer = dense_layer`となります。

また複数の値をフィルタインデックスに設定できます。例えば、 `filter_indices = [22, 23]`は（上手くいけば）
２２と２３のカテゴリ両方に関連する注目マップを得られるはずです。

*戻り値:*

`filter_indices`の出力の最大化に最も貢献する入力の領域を示すヒートマップ画像
