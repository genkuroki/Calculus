---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.10.3
  kernelspec:
    display_name: Wolfram Language 13.1
    language: Wolfram Language
    name: wolframlanguage13.1
---

# Borwein積分

黒木玄

2019-06-13

* Copyright 2019 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

* http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/A03%20Borwein%20integral.ipynb

* https://genkuroki.github.io/documents/Calculus/A03%20Borwein%20integral.pdf

このファイルは [Free Wolfram Engine](https://www.wolfram.com/engine/) を [Jupyter](https://jupyter.org/) で[使えるようにする](https://github.com/WolframResearch/WolframLanguageForJupyter)と利用できる. 詳しくは次の解説を参照せよ.

* [Free Wolfram EngineをJupyterで使う方法](https://nbviewer.jupyter.org/github/genkuroki/msfd28/blob/master/Free%20Wolfram%20Engine.ipynb)

$
\newcommand\eps{\varepsilon}
\newcommand\ds{\displaystyle}
\newcommand\Z{{\mathbb Z}}
\newcommand\R{{\mathbb R}}
\newcommand\C{{\mathbb C}}
\newcommand\T{{\mathbb T}}
\newcommand\QED{\text{□}}
\newcommand\root{\sqrt}
\newcommand\bra{\langle}
\newcommand\ket{\rangle}
\newcommand\d{\partial}
\newcommand\sech{\operatorname{sech}}
\newcommand\cosec{\operatorname{cosec}}
\newcommand\sign{\operatorname{sign}}
\newcommand\sinc{\operatorname{sinc}}
\newcommand\real{\operatorname{Re}}
\newcommand\imag{\operatorname{Im}}
\newcommand\Li{\operatorname{Li}}
\newcommand\PROD{\mathop{\coprod\kern-1.35em\prod}}
\newcommand\Si{\operatorname{Si}}
\newcommand\Ci{\operatorname{Ci}}
\newcommand\si{\operatorname{si}}
\newcommand\Cin{\operatorname{Cin}}
\newcommand\Fourier{\operatorname{\mathscr{F}}}
$

<!-- #region toc=true -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Borwein積分の紹介" data-toc-modified-id="Borwein積分の紹介-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Borwein積分の紹介</a></span></li><li><span><a href="#Borwein積分の公式" data-toc-modified-id="Borwein積分の公式-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Borwein積分の公式</a></span><ul class="toc-item"><li><span><a href="#Borwein積分の公式の証明" data-toc-modified-id="Borwein積分の公式の証明-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Borwein積分の公式の証明</a></span></li><li><span><a href="#公式($\boldsymbol{**}$)の証明" data-toc-modified-id="公式($\boldsymbol{**}$)の証明-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>公式($\boldsymbol{**}$)の証明</a></span></li><li><span><a href="#sinの積をsinまたはcosの和で表す公式の証明" data-toc-modified-id="sinの積をsinまたはcosの和で表す公式の証明-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>sinの積をsinまたはcosの和で表す公式の証明</a></span></li></ul></li><li><span><a href="#Borwein積分とFourier変換の関係" data-toc-modified-id="Borwein積分とFourier変換の関係-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Borwein積分とFourier変換の関係</a></span><ul class="toc-item"><li><span><a href="#Fourier解析からの準備" data-toc-modified-id="Fourier解析からの準備-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Fourier解析からの準備</a></span></li><li><span><a href="#Borwein積分のたたみ込み積表示とその応用" data-toc-modified-id="Borwein積分のたたみ込み積表示とその応用-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Borwein積分のたたみ込み積表示とその応用</a></span></li></ul></li></ul></div>
<!-- #endregion -->

```wolfram language
JupyterImageResolution = 84;
JupyterOutTextForm = "TeX";

TeX[x_] := ToString[TeXForm[x]]
TeX[x_, y__] := StringJoin[TeX[x], TeX[y]]
TeXRaw[x__, y_] := StringJoin[x, TeX[y]]

MappedBy[x_] := x
MappedBy[x_, F___, G_] := MappedBy[x, F] // G

SetAttributes[TeXEq, HoldFirst]
TeXEq[x_] := TeX[HoldForm[x] == MappedBy[x, ReleaseHold, FullSimplify]]
TeXEq[x_, F__] := TeX[HoldForm[x] == MappedBy[x, ReleaseHold, F]]
TeXEqRaw[x_] := TeX[HoldForm[x] == MappedBy[x, ReleaseHold]]
```

## Borwein積分の紹介

以下では $a_0,a_1,\ldots,a_r$ は正の実数であるとし, $\sinc$ 函数を

$$
\sinc x = \begin{cases}
\dfrac{\sin x}{x} & (x\ne 0) \\
\;\;\,1 & (x=0) \\
\end{cases}
$$

と定めて利用する.

次の形の積分を[**Borwein積分**](https://en.wikipedia.org/wiki/Borwein_integral)と呼ぶ:

$$
\int_{-\infty}^\infty\prod_{k=0}^r\sinc(a_k x)\,dx = 
\int_{-\infty}^\infty \sinc(a_0 x)\sinc(a_1 x)\cdots\sinc(a_r x)\,dx
$$

必要ならば条件収束する広義積分でこれを定義しておく. 例えば,

```wolfram language
Integrate[Sinc[x], {x,-Infinity,Infinity}] // TeXEq
```

上の結果はDirichlet積分の公式

$$
\int_{-\infty}^\infty \frac{\sin(a x)}{x}\,dx = \pi\sign(a)
$$

の特別な場合として有名である.

```wolfram language
Integrate[Sinc[x]Sinc[x/3], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5]Sinc[x/7], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5]Sinc[x/7]Sinc[x/9], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5]Sinc[x/7]Sinc[x/9]Sinc[x/11], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5]Sinc[x/7]Sinc[x/9]Sinc[x/11]Sinc[x/13], {x,-Infinity,Infinity}] // TeXEq
```

以上のBorwien積分の値はすべて $\pi$ になった. しかし, これの次の積分は以下のように $\pi$ とは異なる $\pi$ に非常に近い値になる!

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5]Sinc[x/7]Sinc[x/9]Sinc[x/11]Sinc[x/13]Sinc[x/15], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
467807924713440738696537864469/467807924720320453655260875000 // TeXEq[#, N[#, 20]&]&
```

このように

$$
\frac{467807924713440738696537864469}{467807924720320453655260875000}\pi
$$

の値は非常に $\pi$ に近い.  「これは使用しているWolfram言語のバグなのではないだろうか?」と疑う人がいても不思議ではない結果である.


## Borwein積分の公式

以下で紹介する結果については

* David Borwein and Jonathan M. Borwein.  Some Remarkable Properties of Sinc and Related Integrals.  The Ramanujan Journal, March 2001, Volume 5, Issue 1, pp 73–89. [PDF](http://www.thebigquestions.com/borweinintegrals.pdf)

を参照した.

$a_0,a_1,\ldots,a_r>0$ と仮定する. この節では次の公式を示そう:

$$
\begin{aligned}
\int_{-\infty}^\infty \prod_{k=0}^r \sinc(a_k x)\,dx &=
\frac{\pi}{r! 2^r a_0 a_1\cdots a_r}
\\ &\,\times 
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^r
\sign
\left(a_0+\sum_{k=1}^r\eps_k a_k\right).
\end{aligned}
\tag{$*$}
$$

さらに次の公式も示す:

$$
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^k =
\begin{cases}
0 & (k=0,1,\ldots,r-1) \\
r! 2^r a_1\cdots a_r & (k=r) 
\\
(r+1)! 2^r a_0 a_1\cdots a_r & (k=r+1). \\
\end{cases}
\tag{$**$}
$$

特に $a_0 > a_1+\cdots+a_r$ ならば($*$)の $\sign$ の因子がすべて $1$ になり, ($**$)の $k=r$ の場合を使うと,

$$
\int_{-\infty}^\infty \prod_{k=0}^r \sinc(a_k x)\,dx = \frac{\pi}{a_0}
\quad (a_0 > a_1+\cdots+a_r)
$$

が得られる. 前節の計算例の最後以外はこの公式の $a_0=1$ の場合になっている.

さらに, $a_0 < a_1+\cdots+a_r$ でかつ $\eps_1=\cdots=\eps_r=-1$ 以外のとき $a_0+\sum_{k=1}^r\eps_k a_k>0$ となるならば, 

$$
\int_{-\infty}^\infty \prod_{k=0}^r \sinc(a_k x)\,dx = 
\frac{\pi}{a_0}\left(
1 - \frac{\left(a_1+\cdots+a_r-a_0\right)^r}{r! 2^{r-1} a_1\cdots a_r}
\right)
$$

が得られる.  前節の計算例の最後はこの公式の $a_0=1$ の場合になっている.

```wolfram language
1/3+1/5+1/7+1/9+1/11+1/13 // TeXEq
```

```wolfram language
1/3+1/5+1/7+1/9+1/11+1/13+1/15 // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/3]Sinc[x/5]Sinc[x/7]Sinc[x/9]Sinc[x/11]Sinc[x/13]Sinc[x/15], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Pi(1 - (1/3+1/5+1/7+1/9+1/11+1/13+1/15-1)^7/(7! 2^6 (1/3)(1/5)(1/7)(1/9)(1/11)(1/13)(1/15))) // TeXEq
```

### Borwein積分の公式の証明

三角函数に関する簡単な計算によって以下が成立していることを示せる.

$r$ が偶数のとき,

$$
\prod_{k=0}^r \sin(a_k x) =
\frac{(-1)^{r/2}}{2^r}
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r \sin\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)x\right).
$$

$r$ が奇数のとき, 

$$
\prod_{k=0}^r \sin(a_k x) =
\frac{(-1)^{(r+1)/2}}{2^r}
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r \cos\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)x\right).
$$

これらの公式の両辺を $r$ 回 $x$ で微分すると, $r$ の偶奇によらず, 結果は次のようになることがすぐにわかる:

$$
\left(\frac{\d}{\d x}\right)^r
\prod_{k=0}^r \sin(a_k x) =
\frac{1}{2^r}
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^r
\sin\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)x\right).
$$

この公式を使うと, Borwein積分の公式($*$)は部分積分によって容易に証明される.

$$
\frac{1}{r!}\left(-\frac{\d}{\d x}\right)^r\frac{1}{x} = \frac{1}{x^{r+1}}
$$

を使って, 部分積分を $r$ 回繰り返すと, 

$$
\begin{aligned}
\int_{-\infty}^\infty \prod_{k=0}^r \sinc(a_k x)\,dx &=
\frac{1}{a_0a_1\cdots a_r}
\int_{-\infty}^\infty \frac{1}{x^{r+1}} \prod_{k=0}^r \sin(a_k x)\,dx
\\ &=
\frac{1}{r! a_0a_1\cdots a_r}
\int_{-\infty}^\infty \frac{1}{x}\;\left(\frac{\d}{\d x}\right)^r\prod_{k=0}^r \sin(a_k x)\,dx
\\ &=
\frac{1}{r! 2^r a_0a_1\cdots a_r}
\\ &\,\times
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^r
\int_{-\infty}^\infty\frac{1}{x}
\sin\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)x\right)
\,dx
\\ &=
\frac{1}{r! 2^r a_0a_1\cdots a_r}
\\ &\,\times
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^r
\sign
\left(a_0+\sum_{k=1}^r\eps_k a_k\right).
\end{aligned}
$$

これが示したかったBorwein積分の公式($*$)である:

$$
\begin{aligned}
\int_{-\infty}^\infty \prod_{k=0}^r \sinc(a_k x)\,dx &=
\frac{\pi}{r! 2^r a_0 a_1\cdots a_r}
\\ &\,\times 
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^r
\sign
\left(a_0+\sum_{k=1}^r\eps_k a_k\right).
\end{aligned}
\tag{$*$}
$$


### 公式($\boldsymbol{**}$)の証明

$$
\begin{aligned}
e^{a_0 t}\prod_{k=1}^r(e^{a_k t}-e^{-a_k t}) &=
e^{a_0 t}\prod_{k=1}^r\sum_{\eps_k=\pm1}\eps_k e^{\eps_k a_k t}
\\ &=
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r 
\exp\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)t\right).
\end{aligned}
$$

左辺の $t$ に関するべき級数の展開は

$$
(1+a_0 t + O(t^2))(2^r a_1\cdots a_r t^r + O(t^{r+2})) =
2^r a_1\cdots a_r t^r + 2^r a_0 a_1\cdots a_r t^{r+1} + O(t^{r+2})
$$

となり, 右辺は

$$
\sum_{k=0}^\infty \frac{t^k}{k!}
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^k
$$

となる. $k=0,1,\ldots,r,r-1$ に関する $t^k$ の係数を比較すると公式 ($**$) が得られる:

$$
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r
\left(a_0+\sum_{k=1}^r\eps_k a_k\right)^k =
\begin{cases}
0 & (k=0,1,\ldots,r-1) \\
r! 2^r a_1\cdots a_r & (k=r) 
\\
(r+1)! 2^r a_0 a_1\cdots a_r & (k=r+1). \\
\end{cases}
\tag{$**$}
$$


### sinの積をsinまたはcosの和で表す公式の証明

sinの積に関する公式も証明しておこう.

$$
\sin z = \dfrac{e^{iz}-e^{-iz}}{2i} =
\frac{1}{2i}\sum_{\eps=\pm1}\eps e^{i\eps z}
$$

を使うと, 

$$
\begin{aligned}
\prod_{k=0}^r\sin(a_k x) &=
\frac{1}{(2i)^{r+1}}(e^{ia_0x}-e^{-ia_0x})
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r e^{i\left(\sum_{k=1}^r \eps_k a_k\right) x}
\\ &=
\frac{1}{(2i)^{r+1}}\left(
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r e^{i\left(a_0+\sum_{k=1}^r \eps_k a_k\right) x} -
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r e^{i\left(-a_0+\sum_{k=1}^r \eps_k a_k\right) x}
\right)
\\ &=
\frac{1}{(2i)^{r+1}}\left(
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r e^{i\left(a_0+\sum_{k=1}^r \eps_k a_k\right) x} -
(-1)^r
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r e^{-i\left(a_0+\sum_{k=1}^r \eps_k a_k\right) x}
\right)
\\ &=
\frac{1}{(2i)^{r+1}}
\sum_{\eps_1,\ldots,\eps_r=\pm1}\eps_1\cdots\eps_r
\left(
e^{i\left(a_0+\sum_{k=1}^r \eps_k a_k\right) x} -
(-1)^r
e^{-i\left(a_0+\sum_{k=1}^r \eps_k a_k\right) x}
\right).
\end{aligned}
$$

3番目の等号で括弧の中の2つ目の和の $\eps_k$ 達をすべて $-1$ 倍にした. この結果は, $r$ が偶数のとき,

$$
\prod_{k=0}^r \sin(a_k x) =
\frac{(-1)^{r/2}}{2^r}
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r \sin\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)x\right)
$$

になり, $r$ が奇数のとき, 

$$
\prod_{k=0}^r \sin(a_k x) =
\frac{(-1)^{(r+1)/2}}{2^r}
\sum_{\eps_1,\ldots,\eps_r=\pm1}
\eps_1\cdots\eps_r \cos\left(\left(a_0+\sum_{k=1}^r\eps_k a_k\right)x\right)
$$

になる.  これが示したい公式であった.  

このように奇数個の $\sin$ の積は $\sin$ の和に展開され, 偶数個の $\sin$ の積は $\cos$ の和に展開される.


## Borwein積分とFourier変換の関係


### Fourier解析からの準備

Fourier変換 $f(x)\mapsto\Fourier[f(x)](p)$ とその逆変換 $\phi(p)\mapsto\Fourier^{-1}[\phi(p)](x)$ が次のように定義される:

$$
\Fourier[f(x)](p) = \int_{-\infty}^\infty e^{-ipx}f(x)\,dx, \quad
\Fourier^{-1}[\phi(p)](x) = \int_{-\infty}^\infty e^{ipx}\phi(p)\,\frac{dp}{2\pi}.
$$

さらに, 函数 $\phi(p),\psi(p)$ のたたみ込み積が

$$
\phi*\psi(p)\, = \int_{-\infty}^\infty \phi(q)\psi(p-q)\,\frac{dq}{2\pi}
$$

と定義される. このとき, 

$$
\int_{-\infty}^\infty \phi*\psi(p)\,\frac{dp}{2\pi} =
\left(\int_{-\infty}^\infty \phi(p)\,\frac{dp}{2\pi}\right)
\left(\int_{-\infty}^\infty \psi(p)\,\frac{dp}{2\pi}\right).
$$

さらに, 函数 $f(x),g(x)$ に関する適切な仮定のもとで次が成立することを示せる:

$$
\Fourier[f(x)g(x)] = \Fourier[f(x)]*\Fourier[g(x)].
$$

**例:** $a>0$ に対して, $\chi_a(p)$ を

$$
\chi_a(p) = 
\begin{cases}
\;1 & (-a<p<a) \\
1/2 & (p=\pm a) \\
\;0 & (\text{otherwise}). \\
\end{cases}
$$

と定める. このとき,

$$
\Fourier^{-1}[\chi_a(p)](x) =
\int_{-a}^a e^{ipx}\,\frac{dp}{2\pi} = 
\frac{e^{iax}-e^{-iax}}{2\pi i x} =
\frac{\sin(a x)}{\pi x} = 
\frac{a}{\pi}\sinc(ax).
$$

ゆえに

$$
\Fourier\left[\sinc(ax)\right](p) = \frac{\pi}{a}\chi_a(p).
$$

さらに

$$
\int_{-\infty}^\infty \frac{\pi}{a}\chi_a(p)\,\frac{dp}{2\pi} =
\int_{-a}^a \frac{\pi}{a}\,\frac{dp}{2\pi} = 1
$$

が成立していることにも注意せよ. $\QED$


### Borwein積分のたたみ込み積表示とその応用

Borwein積分は次のFourier変換の $p=0$ での値に等しい:

$$
I(p) = 
\Fourier\left[\prod_{k=0}^r \sinc(a_k x)\right](p)=
\int_{-\infty}^\infty e^{-ipx}\prod_{k=0}^r \sinc(a_k x)\,dx.
$$

積のFourier変換はFourier変換のたたみ込み積に等しいので,

$$
I(p) = g_0*g_1*\cdots*g_r(p).
$$

ここで,

$$
g_k(p) = \Fourier[\sinc(a_k x)](p) = \frac{\pi}{a}\chi_a(p)
$$

特に $p=0$ のとき,

$$
I(0) = \frac{\pi}{a_0}\int_{-a_0}^{a_0} g_1*\cdots*g_r(p)\,\frac{dp}{2\pi}.
$$

一方, $\int_{-\infty}^\infty g_k(p) dp/(2\pi)=1$ とたたみ込み積の一般的な性質より, 

$$
\int_{-\infty}^{\infty} g_1*\cdots*g_r(p)\,\frac{dp}{2\pi} = 1.
$$

したがって, もしも $g_1*\cdots*g_r(p)$ の台が区間 $[-a_0,a_0]$ に含まれているならば, 

$$
I(0) = \frac{\pi}{a_0}
$$

となり, 非負値函数になる $g_1*\cdots*g_r(p)$ の台が区間 $[-a_0,a_0]$ より真に広ければ, 

$$
I(0) < \frac{\pi}{a_0}
$$

となる.  $g_k(p)$ の台は $[-a_k,a_k]$ なので, $g_1*\cdots*g_r(p)$ の台は $[-(a_1+\cdots+a_r), a_1+\cdots+a_r]$ に等しい.  以上をまとめることによって次を得る.

**定理:** $a_0 \geqq a_1+\cdots+a_r$ のとき $\ds I(0)=\frac{\pi}{a_0}$ となり, $a_0 < a_1+\cdots+a_r$ のとき $\ds I(0)<\frac{\pi}{a_0}$ となる. $\QED$


**例:** $a_0=1$< $a_1=1/2$, $a_2=1/3$, $a_4=1/4$ の場合の $G_1(p)=g_1(p)$, $G_2(p)=g_1*g_2(p)$, $G_3(p)=g_1*g_2*g_3(p)$ を計算し, $G_k(p)/(2\pi)$ をプロットし, 積分してみよう.

```wolfram language
1/2+1/3 > 1 // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/2]Sinc[x/3], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
1/2+1/3+1/4 > 1 // TeXEq
```

```wolfram language
Integrate[Sinc[x]Sinc[x/2]Sinc[x/3]Sinc[x/4], {x,-Infinity,Infinity}] // TeXEq
```

```wolfram language
g[a_, p_] := Pi/a If[-a <= p <= a, 1, 0]
g[a,p] // TeXEq
```

```wolfram language
Assuming[a > 0, Integrate[g[a,p]/(2Pi), {p,-Infinity,Infinity}] // TeXEq]
```

```wolfram language
G1[p_] = g[1/2, p];
G1[p] // TeX[HoldForm[G1[p]], "=", #]&
```

```wolfram language
Plot[G1[p]/(2Pi), {p,-1.5,1.5}]
```

```wolfram language
Integrate[G1[p]/(2Pi), {p,-Infinity,Infinity}] // TeXEq
```

```wolfram language
G2[p_] = Integrate[g[1/2,q]g[1/3,p-q]/(2Pi), {q,-Infinity,Infinity}];
G2[p] // TeX[HoldForm[G2[p]], "=", #]&
```

```wolfram language
Plot[G2[p]/(2Pi), {p,-1.5,1.5}]
```

```wolfram language
Integrate[G2[p]/(2Pi), {p,-Infinity,Infinity}] // TeXEq
```

```wolfram language
G3[p_] = Integrate[G2[q]g[1/4,p-q]/(2Pi), {q,-Infinity,Infinity}];
G3[p] // TeX[HoldForm[G3[p]], "=", #]&
```

```wolfram language
Plot[G3[p]/(2Pi), {p,-1.5,1.5}]
```

```wolfram language
Integrate[G3[p]/(2Pi), {p,-Infinity,Infinity}] // TeXEq
```

```wolfram language
Pi Integrate[G3[p]/(2Pi), {p,-1,1}] // TeXEq
```

次に $I_k(p) = g_0*g_1*\cdots*g_k(p)$ を計算して, $I_k(0)$ を計算し, $I_k(p)/\pi$ プロットしてみよう.

```wolfram language
I0[p_] = g[1,p];
I0[p] // TeX[HoldForm[I0[p]], "=", #]&
```

```wolfram language
I0[0] // TeXEq
```

```wolfram language
Plot[I0[p]/Pi, {p,-2,2}]
```

```wolfram language
I1[p_] = Integrate[I0[q]g[1/2,p-q]/(2Pi), {q,-Infinity,Infinity}];
I1[p] // TeX[HoldForm[I1[p]], "=", #]&
```

```wolfram language
I1[0] // TeXEq
```

```wolfram language
Plot[I1[p]/Pi, {p,-2,2}]
```

```wolfram language
I2[p_] = Integrate[I1[q]g[1/3,p-q]/(2Pi), {q,-Infinity,Infinity}];
I2[p] // TeX[HoldForm[I2[p]], "=", #]&
```

```wolfram language
I2[0] // TeXEq
```

```wolfram language
Plot[I2[p]/Pi, {p,-2,2}]
```

```wolfram language
I3[p_] = Integrate[I2[q]g[1/4,p-q]/(2Pi), {q,-Infinity,Infinity}];
I3[p] // TeX[HoldForm[I3[p]], "=", #]&
```

```wolfram language
I3[0] // TeXEq
```

```wolfram language
Plot[I3[p]/Pi, {p,-2,2}]
```
