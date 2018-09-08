
**Source:** [vis/grad_modifiers.py#L0](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L0)



-------------------

### [negate](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L8)

```python
negate(grads)
```

勾配の符号を反転します。すなわち-1との積。

*引数:*

 - **grads**:  勾配をあらわすNumpy array。

*戻り値:*

符号反転した勾配。

-------------------

### [absolute](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L20)

```python
absolute(grads)
```

勾配の絶対値を計算します。

*引数:*

 - **grads**:  勾配をあらわすNumpy array。

*戻り値:*

勾配の絶対値。

-------------------

### [invert](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L32)

```python
invert(grads)
```

勾配の逆数を算出します。

*引数:*

 - **grads**: 勾配をあらわすNumpy array。

*戻り値:*

勾配の逆数

-------------------

### [relu](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L44)

```python
relu(grads)
```


負の勾配の値を切り捨てます。すなわちmax(0, grads)。

*引数:*

 - **grads**: 勾配をあらわすNumpy array。

*戻り値:*

整流された勾配

-------------------

### [small_values](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L57)

```python
small_values(grads)
```

小さな勾配の値ほど強調することができます。すなわちabsolute(invert(grads))。

*引数:*

 - **grads**: 勾配をあらわすNumpy array。

*戻り値:*

小さな値を強調するよう変更された勾配

-------------------

### [get](https://github.com/raghakot/keras-vis/tree/master/vis/grad_modifiers.py#L69)

```python
get(identifier)
```








