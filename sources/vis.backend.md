
**Source:** [vis/backend/__init__.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/backend/__init__.py#L0)

### [modify_model_backprop](https://github.com/raghakot/keras-vis/tree/master/vis/backend/tensorflow_backend.py#L52)

```python
modify_model_backprop(model, backprop_modifier)
```

カスタムOPを使用し、誤差逆伝播の動作を変更するために全ての活性化関数を変更してモデルのコピーを作成します。 

*引数:*

 - **model**:   `keras.models.Model` インスタンス
 - **backprop_modifier**:  `{'guided', 'rectified'}`のいずれか１つ

*戻り値:*

活性化関数の逆伝播時の計算に変更を加えたモデルのコピー

-------------------

### [set_random_seed](https://github.com/raghakot/keras-vis/tree/master/vis/backend/tensorflow_backend.py#L120)

```python
set_random_seed(seed_value=1337)
```

再現のためランダムシードを設定します。

*引数:*

 - **seed_value**:  シード値（デフォルト値=悪名高い1337）
