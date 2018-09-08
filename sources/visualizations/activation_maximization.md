## Activation Maximizationとは

CNNでは、それぞれの畳み込み層に入力画像内によく似たテンプレートパターンを見つけると、その出力を最大化するいくつかの学習済み*テンプレートマッチング*フィルタがあります。最初の畳み込み層は簡単に解釈で、単に重みを画像として可視化するだけです。畳み込み層がしていることを見るためには、入力ピクセルにフィルタを適用するのが簡単な方法です。後続の畳み込みフィルタが前の畳み込みフィルタの出力を操作する（いくつかのテンプレートの存在有無を示す）ため、それらの解釈が難しくなります。

Activation Maximizationの背景にあるアイデアは、シンプルにフィルタ出力の活性化を最大にする入力画像を生成することです。
つまり下記を計算すること。

$$\frac{\partial ActivationMaximizationLoss}{\partial input}$$

そして、その推定値を使って入力を更新することです。[ActivationMaximization](../vis.losses.md#activationmaximization)損失関数は
フィルタの活性化を大きくするために、その出力を小さくします（勾配降下の反復中に損失を最小化します）。
これにより特定の入力パターンに対して活性化するフィルタを理解することができます。
例えば、入力画像内の眼の存在によって活性化するEyeフィルタがあるかもしれません。

## 使い方

ActivationMaximizationを実行するための２つのAPIがあります。

1. [visualize_activation](../vis.visualization.md#visualize_activation):これは活性化を可視化するための汎用APIです。
2. [visualize_activation_with_losses](../vis.visualization.md#visualize_activation_with_losses):これはいくつかのカスタムな重み付き損失の最小化を扱うような研究利用向けです。

[examples/](https://github.com/raghakot/keras-vis/tree/master/examples)にあるコードを参照してください。


### シナリオ

APIはとても汎用的な目的で、幅広く様々なシナリオで利用可能です。もっとも一般的なユースケースを下記にリストアップします：

#### 分類出力をおこなう全結合層の可視化

ネットワークが過学習（または未学習）しているか、一般化が適切かをどのように評価できますか？
入力画像を与えると、CNNはそれが猫なのか鳥なのかなどを分類できます。
それが鳥であることが何を意味するか、その正しい概念をとらえているかどうか確認できますか？

それらの問いに対する１つの解は、逆の問いかけをすることです：
> 鳥クラスに対応する最終の全結合層を最大化する入力画像を生成する。

これは最終の全結合層を`layer_idx`で指し、`filter_indices`に望む出力分類を設定することで実現できます。

- 複数クラス分類問題では、`filter_indices`は１つのクラスを指すことができます。
あなたは"猫 魚"がどのように見えるかを確認するために複数の分類を指すこともできます。
- 複数ラベル分類では、適切な`filter_indices`を指定するだけです。

#### 回帰出力をおこなう全結合層の可視化

クラス活性化の可視化とは異なり、回帰出力では増加する入力、または減少する入力の可視化ができます。

回帰された`filter_indices`出力。例えばリンゴを数えるモデルを学習して、その回帰出力を増加させることは入力画像内のリンゴを増やすことと一致するはずです。同様に回帰出力を減らすこともできます。これは`grad_modifier`オプションを使うことで達成できます。名前から連想するように、入力に関する損失の勾配を変更するために使われます。デフォルトでは、`ActivationMaximization`損失は出力を増加させるために使われます。
`grad_modifier='negate'`を設定することで勾配の符号反転ができ、これにより出力値を減少させます。
[gradient_modifiers](../vis.grad_modifiers.md)はとてもパワフルで、他の可視化APIでもよく使われます。

#### 畳み込みフィルタの可視化

`layer_idx`が`畳み込み`層を指すことで、フィルタを活性化するパターンを可視化できます。これはフィルタがどう計算されているかを理解するのに役立ちます。ここでは、`filter_indices`はレイヤ内の`畳み込み`フィルタのインデックスを参照します。

### 高度な使い方

[backprop_modifiers](../vis.backprop_modifiers.md)を使えば誤差逆伝播の振る舞いを編集することができます。たとえば、
`backprop_modifier='relu'`を使うことで陽の勾配だけを伝播させるよう微調整できます。このパラメータも関数を受け付け、あなたのクレイジーな研究アイデアを実装するために使うことができます。 :-)

## Tipsとトリック

- どうしようもない可視化を得てしまったら、勾配降下反復中の様々なロスを確認するために`verbose=True`を設定してみて下さい。デフォルトでは、`visualize_activation`はより自然な画像を得るために`TotalVariation`と`LpNorm`の正則化を使います。`ActivationMaximization Loss`が正則化損失の重み係数に支配され、値が前後に跳ねる様子を見ることができるでしょう。全ての重み係数をゼロに設定して、徐々にTV損失の重み係数を増やしてみてください。
- よりシャープな画像を得るために、[Jitter](../vis.input_modifiers.md#jitter)入力修飾子を使って下さい。
- 回帰モデルは通常、意味ある入力画像を生成するために十分な勾配情報を提供しません。もし意味をなさい場合は、`seed_input`を使って初期値を与えてみてください。

あなたが発見した有用なTipsやトリックを追加するために、PullRequestを投げることを検討してください。