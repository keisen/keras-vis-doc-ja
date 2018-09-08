
**Source:** [vis/utils/utils.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L0)

**グローバル変数**
---------------

- **slicer**

様々な`image_data_format`の画像のスライスを統一するためのユーティリティ。

-------------------

### [reverse_enumerate](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L44)

```python
reverse_enumerate(iterable)
```

コピーを作成することなく、適切なインデックスを保持したまま逆順に並べたイテレータの列挙を返します。

-------------------

### [listify](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L50)

```python
listify(value)
```

リストであることを保障します。
引数がリストでなかった場合、引数を追加したリスト返します。

-------------------

### [add_defaults_to_kwargs](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L58)

```python
add_defaults_to_kwargs(defaults, **kwargs)
```

`kwargs`を`defaults`のディクショナリで更新します。

*引数:*

 - **defaults**:  ディクショナリ
 - **\*\*kwargs**: 更新対象の`kwargs`

*戻り値:*

更新後のディクショナリ


-------------------

### [get_identifier](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L73)

```python
get_identifier(identifier, module_globals, module_name)
```

文字列で指定された呼び出し可能な関数を探索するためのヘルパユーティリティです。

*引数:*

 - **identifier**:  識別子。文字列か関数が指定できる。
 - **module_globals**:  モジュールのグローバルオブジェクト
 - **module_name**:  モジュール名

*戻り値:*

指定された呼び出し可能な関数

-------------------

### [apply_modifications](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L95)

```python
apply_modifications(model, custom_objects=None)
```

新しいグラフを作ることでモデルレイヤに変更を適用します。
例えば、単に`model.layers[idx].activation = new activation`といった変更をしてもグラフは変化しません。
レイヤを構築する関数が変更されたため、グラフ全体を変更されたインバウンドとアウトバウンドのテンソルとともに更新する必要があります。

*引数:*

 - **model**:  `keras.models.Model`インスタンス

*戻り値:*

変更を適用済みのモデル。オリジナルの`model`は変更しません。

-------------------

### [random_array](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L118)

```python
random_array(shape, mean=128.0, std=20.0)
```

与えられた`mean`と`std`から一様分布を生成します。

*引数:*

 - **shape**:  形状
 - **mean**: 　平均 (デフォルト値 = 128)
 - **std**:  分散 (デフォルト値 = 20)

*戻り値:
与えられた`mean`と`std`から生成した一様分布

-------------------

### [find_layer_idx](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L136)

```python
find_layer_idx(model, layer_name)
```

`model`から`layer_name`に対応するレイヤのインデックスを調べます。

*引数:*

 - **model**:  `keras.models.Model`インスタンス
 - **layer_name**:  調べたいレイヤ名

*戻り値:*

見つかったレイヤインデックス。見つからなかった場合は例外(ValueError)が発生する。


-------------------

### [deprocess_input](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L157)

```python
deprocess_input(input_array, input_range=(0, 255))
```

Utility function to scale the `input_array` to `input_range` throwing away high frequency artifacts.

*引数:*

 - **input_array**:  N次元のnumpy array
 - **input_range**:  `input_array`をリスケールするため、入力の範囲を`(min, max)`のタプルで指定する

*戻り値:*

リスケールされた`input_array`

-------------------

### [stitch_images](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L181)

```python
stitch_images(images, margin=5, cols=5)
```

`margin`をとりながら画像をつなぎ合わせるためのユーティリティ関数です。

*引数:*

 - **images**:  つなぎ合わせる２次元画像
 - **margin**:  画像間の黒いボーダーの幅 (デフォルト値 = 5)
 - **cols**:  画像の最大列数。画像の数が列数を超えた場合、新たに行を追加します。(デフォルト値 = 5)

*戻り値:*

入力画像をつなぎ合わせた１つのnumpy array

-------------------

### [get_img_shape](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L216)

```python
get_img_shape(img)
```

バックエンドに依存せずに、画像の形状を返します。

*引数:*

 - **img**:  画像テンソル。形状は、`channels_first`の場合は`(samples, channels, image_dims...)`、
  `channels_last`の場合は`(samples, image_dims..., channels)`。

*戻り値:*

`(samples, channels, image_dims...)`で画像の形状情報を持つタプル

-------------------

### [load_img](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L238)

```python
load_img(path, grayscale=False, target_size=None)
```

ディスクから画像をロードするユーティリティ関数です。

*引数:*

 - **path**:  画像のパス
 - **grayscale**:  Trueならグレースケースに変換します(デフォルト値 = False)
 - **target_size**:  (w, h)にリサイズします(デフォルト値 = None)

*戻り値:*

  ロードしたnumpy画像

-------------------

### [lookup_imagenet_labels](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L255)

```python
lookup_imagenet_labels(indices)
```

最終全結合層の出力インデックスから、ImageNetラベルをで返すユーティリティ関数です。

*引数:*

 - **indices**:  探索するラベルのインデックスを１つまたはインデックスの配列で指定できます。

*戻り値:*

カテゴリに関連するImageNetラベル


-------------------

### [draw_text](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L273)

```python
draw_text(img, text, position=(10, 10), font="FreeSans.ttf", font_size=14, color=(0, 0, 0))
```

画像上にテキストを描画します。Pillowライブラリを必要とします。

*引数:*

 - **img**:  画像
 - **text**:  描画する文字列
 - **position**:  テキスト位置(x, y)(デフォルト値 = (10, 10))
 - **font**:  TTFまたはOpenTypeフォント(デフォルト値 = 'FreeSans.ttf')
 - **font_size**:  フォントサイズ(デフォルト値= 12)
 - **color**:  テキストの色(r, g, b) (デフォルト値 = (0, 0, 0))

*Returns:*

テキストを描画した画像

-------------------

### [bgr2rgb](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L302)

```python
bgr2rgb(img)
```

Converts an RGB image to BGR and vice versa
RGB画像をBGR画像に（または逆向きに）変換します。

*引数:*

 - **img**:  RGBまたはBGRフォーマットのnumpy array

*Returns:*

変換した画像

-------------------

### [normalize](https://github.com/raghakot/keras-vis/tree/master/vis/utils/utils.py#L313)

```python
normalize(array, min_value=0.0, max_value=1.0)
```

numpy arrayを(min_value, max_value)に正規化します。

*引数:*

 - **array**:  numpy array
 - **min_value**:  正規化した画像の最小値 (デフォルト値 = 0)
 - **max_value**:  正規化した画像の最大値 (デフォルト値 = 1)

*戻り値:*

(min_value, max_value)の範囲に正規化したarray
