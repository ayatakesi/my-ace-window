---
title: ace-windowのREADME
tags:
  - Emacs
  - ace-window
private: true
updated_at: '2025-01-01T16:56:55+09:00'
id: e914530f99058dcbe4b2
organization_url_name: null
slide: false
ignorePublish: false
---

- ace-window0.9.0のREADMEの日本語訳です

- 元の文書: [https://github.com/abo-abo/ace-window/commit/0577c426a9833ab107bab46c60d1885c611b2fb9](https://github.com/abo-abo/ace-window/commit/0577c426a9833ab107bab46c60d1885c611b2fb9)

- ライセンス: GNU Emacsの一部なのでGNU Emacsと同じです

- - -

# ace-window

[![GNU ELPA](https://elpa.gnu.org/packages/ace-window.svg)](https://elpa.gnu.org/packages/ace-window.html)
[![MELPA](https://melpa.org/packages/ace-window-badge.svg)](https://melpa.org/#/ace-window)
[![MELPA Stable](https://stable.melpa.org/packages/ace-window-badge.svg)](https://stable.melpa.org/#/ace-window)

**切り替え先ウィンドウ選択のためのGNU Emacs向けパッケージ**

- - -

* [ace-window](#ace-window)
   * [何をどうする](#何をどうする)
   * [セットアップ](#セットアップ)
   * [使い方](#使い方)
   * [ウィンドウの入れ替えと削除](#ウィンドウの入れ替えと削除)
   * [アクションの途中変更](#アクションの途中変更)
   * [カスタマイゼーション](#カスタマイゼーション)
      * [aw-keys](#aw-keys)
      * [aw-scope](#aw-scope)
      * [aw-background](#aw-background)
      * [aw-dispatch-always](#aw-dispatch-always)
      * [aw-dispatch-alist](#aw-dispatch-alist)
      * [aw-minibuffer-flag](#aw-minibuffer-flag)
      * [aw-ignored-buffers](#aw-ignored-buffers)
      * [aw-ignore-on](#aw-ignore-on)
      * [aw-ignore-current](#aw-ignore-current)
      
- - -

## 何をどうする

`other-window`コマンドは知っている筈だ。これはウィンドウ2つまでなら素晴らしいがウィンドウの数が増えてくると急速に価値を失っていく。何回も呼び出さねばならず、簡単には予測できないので、お目当てのウィンドウに到達したかを毎回チェックしなければならない。

`windmove-left``windmove-up`等を使うという別のアプローチもある。これらのコマンドは高速だし予測可能だ。欠点はキーバインディングが4つ必要なことだ。キーバインディングの1つにshift+arrowsがあるが、指が届かない。

これは`windmove`のスピードと予測可能な点はそのままに、`other-window`のように単一のキーバインディングにパックするためのパッケージだ。

## セットアップ

ウィンドウ切り替えは頻繁に行うタスクなので、`ace-window`を短いキーバインディングに割り当てるだけ。<kbd>M-o</kbd>はデフォルトのEmacsでは重要な何かにバインドされている訳ではないのでお勧め。

## 使い方

`ace-window`はウィンドウが2つあれば`other-window`を呼び出す(`aw-dispatch-always`が非nilの場合を除く)。もっとある場合にはウィンドウそれぞれにたいして、左上隅にハイライトされたウィンドウラベルの最初の1文字が与えられる。その文字を押下すればそのウィンドウに切り替わるか、特定のウィンドウの選択するために次の文字絞り込む。`ace-jump-mode`と異なりポイント位置は変更されない(`other-window`と同じ挙動)。

`aw-make-frame-char`が定義するスペシャル文字(デフォルトは`z`)は新たにフレームを作成して、そのフレームのウィンドウをターゲットとすることを意味する。新たなフレームの位置は`aw-frame-offset`によって与えられる、その前に選択されていたフレームから相対的な位置にセットされる。新たなフレームのサイズは`aw-frame-size`であ与えられる。詳細についてはドキュメント文字列を参照のこと。

ウィンドウの順序は上から下、左から右となる。これはウィンドウレイアウトを覚えておけば、標識文字を見ずにウィンドウが切り替えられることを意味する。たとえば左上隅のウィンドウは常に`1`(ウィンドウ文字にアルファベットを使用している場合には`a`。

![in-action gif](http://oremacs.com/download/ace-window.gif)

上記イメージのように、`ace-window`は複数のフレームを跨いで機能する。


## ウィンドウの入れ替えと削除

- プレフィックス引数<kbd>C-u</kbd>とともに`ace-window`を呼び出すとウィンドウを入れ替える(swap)

- 2連プレフィックス引数<kbd>C-u C-u</kbd>とともに`ace-window`を呼び出すと、選択されたウィンドウを削除できる(delete)

## アクションの途中変更

`ace-window`を呼び出して開始した後にアクションを`delete`や`swap`等に切り替えるといったことも可能、デフォルトのバインディングは以下の通り:

- <kbd>x</kbd> - ウィンドウの削除(delete)
- <kbd>m</kbd> - ウィンドウの入れ替え(swap)
- <kbd>M</kbd> - ウィンドウの移動(move)
- <kbd>c</kbd> - ウィンドウのコピー(copy)
- <kbd>j</kbd> - バッファーの選択(select)
- <kbd>n</kbd> - 前(previous)のウィンドウを選択
- <kbd>u</kbd> - 他(other)のウィンドウでバッファーを選択
- <kbd>c</kbd> - 垂直(vertically)あるいは水平(horizontally)にウィンドウを等分に分割(split)
- <kbd>v</kbd> - ウィンドウを垂直に分割
- <kbd>b</kbd> - ウィンドウを水平に分割
- <kbd>o</kbd> - カレントウィンドウを最大化
- <kbd>?</kbd> - これらのバインディングの表示

正しく操作を行うために、これらのキーを`aw-keys`に*含めてはならない*。更に2つ以下のウィンドウでこれらのキーを機能させたい場合には、`aw-dispatch-always`に`t`をセットする必要がある。

## カスタマイゼーション
`ace-window`のバインディング以外では:

```lisp
    (global-set-key (kbd "M-o") 'ace-window)
```

以下のカスタマイゼーションが利用できる:

### aw-keys
`aw-keys` - ウィンドウラベルに使用するイニシャル文字のリスト:

```lisp
    (setq aw-keys '(?a ?s ?d ?f ?g ?h ?j ?k ?l))
```

`aw-keys`のデフォルトは0から9であり、これはこれで理に適ったデフォルトではあるが、上記ではキーをホームポジションにセットアップしている。

### aw-scope
デフォルトは`global`(`ace-window`はフレームを跨いで機能する)。`frame`にセットすれば`ace-window`はカレントフレームのウィンドウだけを提案する。

### aw-background

ウィンドウ切り替え文字の視認性を高めるために、デフォルトでは`ace-window`は利用可能なウィンドウのバックグラウンドカラーを除去して一時的にグレーにセットする。この挙動は`ace-jump-mode`から継承している。

見るべき位置(ウィンドウのそれぞれ左上隅)が既に判っていれば、この挙動は不要かもしれない。グレーのバックグラウンドは以下でオフに切り替えられる:

```lisp
    (setq aw-background nil)
```

### aw-dispatch-always

非nilならたとえウィンドウが1つでも`ace-window`は`read-char`を割り当てる。これによりウィンドウが1つ、あるいは2つの際の`ace-window`と`other-window`の動作に差異が生じるだろう。これはアクションを途中で変更して、デフォルトの*ジャンプ*とは異なる他のアクションを実行する場合に役に立つ。デフォルトでは`nil`。

### aw-dispatch-alist

`ace-window`からデフォルトの*ジャンプ*以外にトリガーできるアクションのリスト。デフォルトは以下の通り:

```lisp
        (defvar aw-dispatch-alist
	  '((?x aw-delete-window "Delete Window")
		(?m aw-swap-window "Swap Windows")
		(?M aw-move-window "Move Window")
		(?c aw-copy-window "Copy Window")
		(?j aw-switch-buffer-in-window "Select Buffer")
		(?n aw-flip-window)
		(?u aw-switch-buffer-other-window "Switch Buffer Other Window")
		(?c aw-split-window-fair "Split Fair Window")
		(?v aw-split-window-vert "Split Vert Window")
		(?b aw-split-window-horz "Split Horz Window")
		(?o delete-other-windows "Delete Other Windows")
		(?? aw-show-dispatch-help))
	  "List of actions for `aw-dispatch-default'.")
```

ace-windowを使用する際にアクション文字の後に文字列が続いていれば、そのアクションのターゲットとなるウィンドウ選択のために`ace-window`が再度呼び出される。文字列が続いていなければ、カレントウィンドウが選択される。

### aw-minibuffer-flag

非nilなら`ace-window`がアクティブの際に、ミニバッファーにも文字列`ace-window-mode`を表示する。横並びのウィンドウの数が多いために、モードラインのマイナーモードエリアの文字列`ace-window-mode`が切り捨てられているときに役に立つ。

### aw-ignored-buffers

ウィンドウリストからウィンドウを選択する際に無視すべき、バッファーおよびメジャーモードのリスト。`aw-ignore-on`が非nilの場合のみアクティブ。しかしウィンドウを識別する固有のラベルをタイプすれば、それらのバッファーを表示するウィンドウでも依然として選択できる。

### aw-ignore-on

tなら`ace-window`は`aw-ignored-buffers`で指定されているバッファーとメジャーモードを無視する。この値を切り替えるには`M-0 ace-window`を使用する。

### aw-ignore-current

tなら`ace-window`は`selected-window`のリターン値を無視する。
