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
    display_name: Julia 1.9.1
    language: julia
    name: julia-1.9
---

# 分配函数のゼータ函数

黒木玄

2019-09-29

* Copyright 2019 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

$
\newcommand\eps{\varepsilon}
\newcommand\ds{\displaystyle}
\newcommand\Z{{\mathbb Z}}
\newcommand\R{{\mathbb R}}
\newcommand\C{{\mathbb C}}
\newcommand\QED{\text{□}}
\newcommand\root{\sqrt}
\newcommand\bra{\langle}
\newcommand\ket{\rangle}
\newcommand\d{\partial}
\newcommand\sech{\operatorname{sech}}
\newcommand\cosec{\operatorname{cosec}}
\newcommand\sign{\operatorname{sign}}
\newcommand\real{\operatorname{Re}}
\newcommand\imag{\operatorname{Im}}
\newcommand\M{\operatorname{\mathcal M}}
$

事前分布 $\varphi(w)$ とHamiltonian $H(w)\geqq 0$ に関する分配函数 $Z(\beta)$ が

$$
Z(\beta) = \int e^{-\beta H(w)}\varphi(w)\,dw
$$

と定義され, 分配函数に付随するゼータ函数 $\zeta(s)$ が

$$
\zeta(s) = \int H(w)^s \varphi(w)\,dw
$$

と定義される.  このとき, 分配函数のMellin変換 $\M Z(s) = \int_0^\infty Z(\beta)\beta^{s-1}\,d\beta$ は

$$
\int_0^\infty e^{-\beta H(w)}\beta^{s-1}\,d\beta = \Gamma(s)H(w)^{-s}
$$

より

$$
\M Z(s) = 
\int\left(\int_0^\infty e^{-\beta H(w)}\beta^{s-1}\,d\beta\right)\varphi(w)\,dw =
\Gamma(s)\zeta(-s)
$$

となる. このノートでは以上の特別な場合の計算の詳細について解説する.

<!-- #region toc=true -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#設定" data-toc-modified-id="設定-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>設定</a></span></li><li><span><a href="#分配函数" data-toc-modified-id="分配函数-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>分配函数</a></span></li><li><span><a href="#分配函数に付随するゼータ函数" data-toc-modified-id="分配函数に付随するゼータ函数-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>分配函数に付随するゼータ函数</a></span></li><li><span><a href="#Mellin変換と逆Mellin変換" data-toc-modified-id="Mellin変換と逆Mellin変換-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Mellin変換と逆Mellin変換</a></span></li><li><span><a href="#分配函数の漸近挙動" data-toc-modified-id="分配函数の漸近挙動-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>分配函数の漸近挙動</a></span></li></ul></div>
<!-- #endregion -->

## 設定

$w=(w_1,\ldots,w_d)$, $b_i\geqq 0$ であるとし, 事前分布 $\varphi(w)$ を

$$
\varphi(w) = \begin{cases}
b_1\cdots b_d \, w_1^{b_1-1}\cdots w_d^{b_d-1} & (0<w_1,\ldots,w_d<1) \\
0 & \text{(otherwise)}
\end{cases}
$$

と定め, $a_i > 0$ であるとし, Hamiltonian $H(w)$ を

$$
H(w) = w_1^{a_1}\cdots w_d^{a_d}
$$

と定める.


## 分配函数

以上の設定のもとで分配函数 $Z(\beta)$ を

$$
Z(\beta) = \int_0^1\!\!\cdots\!\!\int_0^1 e^{-\beta H(w)}\varphi(w)\,dw_1\cdots dw_d
$$

と定める. このとき,

$$
Z(0) = \int \varphi(w)\,dw = 1, \quad \lim_{\beta\to\infty}Z(\beta) = 0.
$$

$Z(n)$ の $n\to\infty$ での漸近挙動を知りたい.


## 分配函数に付随するゼータ函数

分配函数 $Z(\beta)$ に付随するゼータ函数 $\zeta(-s)$ が

$$
\zeta(-s) = \int_0^1\!\!\cdots\!\!\int_0^1 H(w)^{-s}\varphi(w)\,dw_1\cdots dw_d =
\prod_{i=1}^d \int_0^1 b_i w_i^{-a_i s+b_i-1}\,dw_i
$$

と定義される. この積分は $i=1,\ldots,d$ について $-a_i\real s+b_i > 0$ のとき, すなわち

$$
\real s < \lambda_i := \frac{b_i}{a_i}
$$

のとき収束している.  $\lambda_i = b_i/a_i > 0$ 達を以下のように並べ直しておく:

$$
\lambda := \lambda_1 = \cdots = \lambda_m < \lambda_{m+1} \leqq \cdots \leqq \lambda_d.
$$

上の積分は $\real s < \lambda$ で収束している.  その積分を計算すると,

$$
\zeta(-s) = \prod_{i=1}^d\frac{b_i}{b_i-a_i s} = \prod_{i=1}^d \frac{\lambda_i}{\lambda_i - s}.
$$

$\lambda$ 以外の $\lambda_i$ 達を $\lambda'_j$ ($j=2,\ldots,r$) と書き, それぞれの重複度を $m'_j$ と書くことにする. このとき, $\zeta(-s)$ の極はちょうど $s=\lambda, \lambda'_j$ にあり, それぞれの極の位数は $m, m'_j$ になる.


## Mellin変換と逆Mellin変換

分配函数 $Z(\beta)$ のMellin変換は

$$
\M Z(s) = \int_0^\infty Z(\beta)\beta^{s-1}\,d\beta = \Gamma(s)\zeta(-s)
$$

と計算されるのであった. 前節で扱ったように $\zeta(-s)$ を定義する積分は $\real s < \lambda$ で収束しているので, このMellin変換も $\real s < \lambda$ で収束している. そこで, $0<a<\lambda$ とし, $s=a+it$, $\beta=e^x$, $t, x\in\R$ とおくと,

$$
\M Z(a+it) = \int_{-\infty}^\infty Z(e^x)e^{ax}e^{itx}\,dx.
$$

これは $Z(e^x)e^{ax}$ の逆Fourier変換の形をしているので, Fourier変換によってもとに戻せる:

$$
Z(e^x)e^{ax} = \frac{1}{2\pi}\int_{-\infty}^\infty \M Z(a+it)\,e^{-itx}\,dt.
$$

すなわち,

$$
Z(e^x) = \frac{1}{2\pi}\int_{-\infty}^\infty \M Z(a+it)\,e^{-(a+it)x}\,dt.
$$

これは

$$
Z(\beta) = \frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} \M Z(s)\,\beta^{-s}\,ds =
\frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} \Gamma(s)\zeta(-s)\,\beta^{-s}\,ds
$$

と書き直される.


## 分配函数の漸近挙動

以上によって, $0<a<\lambda$ のとき

$$
Z(\beta) = 
\frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} \Gamma(s)\zeta(-s)\,\beta^{-s}\,ds
$$

が示された.  $\Gamma(s)$ の極は $0$ 以下の整数全体であり, $\zeta(-s)$ の極は $s=\lambda,\lambda'_j$ ($j=2,\ldots,r$) にあり, それぞれの位数は $m, m'_j$ である. ゆえに, $b$ をどの $\lambda, \lambda'_j$ よりも大きくして, 積分経路を $\real s = a$ から $\real s = b$ に移動すると, $s=\lambda,\lambda'_j$ における留数が現われる:

$$
Z(\beta) = 
C\beta^{-\lambda}(\log \beta)^{m-1} + 
\sum_{j=1}^r C'_j\beta^{-\lambda'_j}(\log \beta)^{m'_j-1} +
\frac{1}{2\pi i}\int_{b-i\infty}^{b+i\infty} \Gamma(s)\zeta(-s)\,\beta^{-s}\,ds.
$$

ここで, $C, C'_j$ はある定数である. $\beta\to\infty$ において, 最後の項の積分は $O(\beta^{-b})$ のオーダーであり, $\lambda < \lambda'_j < b$ なので, 支配的な項は最初の項になる.  ゆえに,  $n\to\infty$ において,

$$
Z(n) = C n^{-\lambda} (\log n)^{m-1}(1 + o(1)).
$$

すなわち,

$$
-\log Z(n) = \lambda\log n - (m-1)\log\log n + \log C + o(1).
$$

これが目標としていた結果である.

**注意:** 以上の議論は, 逆Mellin変換で表示された $Z(\beta)$ の漸近挙動は, 縦方向の積分経路を横に平行移動したときに出て来る留数を見ればわかるという形式になっている.  これは解析数論における基本定跡である.  この基本定跡を適用した他の例について知りたいならば

* [ディリクレ級数の滑らかなカットオフ](https://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/A01%20Smooth%20cutoff%20of%20Dirichlet%20series.ipynb)

を参照せよ. $\QED$
