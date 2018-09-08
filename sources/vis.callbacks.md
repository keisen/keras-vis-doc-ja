
**Source:** [vis/callbacks.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L0)

-------------------

## [OptimizerCallback](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L16)

[Optimizer.minimize](vis.optimizer.md#optimizerminimize)で使用するコールバックを定義するための抽象クラスです。

-------------------

### [OptimizerCallback.callback](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L20)

```python
callback(self, i, named_losses, overall_loss, grads, wrt_value)
```

この関数は[optimizer.minimize](vis.optimizer.md#minimize)の中で呼び出されます。

*引数:*

 - **i**:  最適化の反復数
 - **named_losses**:  `(損失名, 損失値)`のタプルのリスト
 - **overall_loss**:  全ての重み付き損失
 - **grads**:  `wrt_value`に関する入力画像の勾配
 - **wrt_value**:  現時点の`wrt_value`

-------------------

### [OptimizerCallback.on_end](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L32)

```python
on_end(self)
```

最適化プロセスの最後に呼び出されます。
この関数は通常、最適化の最後に使用していたリソースのクリーンアップやクローズに使われます。

-------------------

## [Print](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L39)


最適化中の値をプリントするためのコールバックです。

-------------------

### [Print.callback](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L42)

```python
callback(self, i, named_losses, overall_loss, grads, wrt_value)
```

-------------------

### [Print.on_end](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L32)

```python
on_end(self)
```

-------------------

## [GifGenerator](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L47)

最適化画像のGIFを構築するためのコールバックです。

-------------------

### [GifGenerator.`__init__`](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L50)

```python
__init__(self, path)
```

*引数:*

 - **path**:  保存するGIFファイルのパス。

-------------------

### [GifGenerator.callback](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L60)

```python
callback(self, i, named_losses, overall_loss, grads, wrt_value)
```

-------------------

### [GifGenerator.on_end](https://github.com/raghakot/keras-vis/tree/master/vis/callbacks.py#L65)

```python
on_end(self)
```
