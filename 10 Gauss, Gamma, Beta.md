---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.1'
      jupytext_version: 1.2.1
  kernelspec:
    display_name: Julia 1.4.1
    language: julia
    name: julia-1.4
---

# 10 Gauss積分, ガンマ函数, ベータ函数

黒木玄

2018-06-21～2019-04-03, 2020-04-25

* Copyright 2018,2019,2020 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

* http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/10%20Gauss%2C%20Gamma%2C%20Beta.ipynb

* https://genkuroki.github.io/documents/Calculus/10%20Gauss%2C%20Gamma%2C%20Beta.pdf

このファイルは <a href="https://juliabox.com">Julia Box</a> で利用できる.

自分のパソコンに<a href="https://julialang.org/">Julia言語</a>をインストールしたい場合には

* [WindowsへのJulia言語のインストール](http://nbviewer.jupyter.org/gist/genkuroki/81de23edcae631a995e19a2ecf946a4f)

* [Julia v1.1.0 の Windows 8.1 へのインストール](https://nbviewer.jupyter.org/github/genkuroki/msfd28/blob/master/install.ipynb)

を参照せよ. 前者は古く, 後者の方が新しい.

論理的に完璧な説明をするつもりはない. 細部のいい加減な部分は自分で訂正・修正せよ.

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
\newcommand\sinc{\operatorname{sinc}}
\newcommand\real{\operatorname{Re}}
\newcommand\imag{\operatorname{Im}}
\newcommand\Li{\operatorname{Li}}
\newcommand\PROD{\mathop{\coprod\kern-1.35em\prod}}
$

<!-- #region {"toc": true} -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Gauss積分" data-toc-modified-id="Gauss積分-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Gauss積分</a></span><ul class="toc-item"><li><span><a href="#Gauss積分の公式" data-toc-modified-id="Gauss積分の公式-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>Gauss積分の公式</a></span></li><li><span><a href="#Gauss積分を使う簡単な計算例" data-toc-modified-id="Gauss積分を使う簡単な計算例-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>Gauss積分を使う簡単な計算例</a></span><ul class="toc-item"><li><span><a href="#Gauss積分" data-toc-modified-id="Gauss積分-1.2.1"><span class="toc-item-num">1.2.1&nbsp;&nbsp;</span>Gauss積分</a></span></li><li><span><a href="#正規分布" data-toc-modified-id="正規分布-1.2.2"><span class="toc-item-num">1.2.2&nbsp;&nbsp;</span>正規分布</a></span></li><li><span><a href="#Lebesgueの収束定理が成立しない場合2" data-toc-modified-id="Lebesgueの収束定理が成立しない場合2-1.2.3"><span class="toc-item-num">1.2.3&nbsp;&nbsp;</span>Lebesgueの収束定理が成立しない場合2</a></span></li><li><span><a href="#正規分布のモーメント" data-toc-modified-id="正規分布のモーメント-1.2.4"><span class="toc-item-num">1.2.4&nbsp;&nbsp;</span>正規分布のモーメント</a></span></li></ul></li><li><span><a href="#Gauss分布のFourier変換" data-toc-modified-id="Gauss分布のFourier変換-1.3"><span class="toc-item-num">1.3&nbsp;&nbsp;</span>Gauss分布のFourier変換</a></span></li><li><span><a href="#Gauss積分の公式の導出" data-toc-modified-id="Gauss積分の公式の導出-1.4"><span class="toc-item-num">1.4&nbsp;&nbsp;</span>Gauss積分の公式の導出</a></span><ul class="toc-item"><li><span><a href="#方法1:-高さ-$z$-で輪切りにする方法" data-toc-modified-id="方法1:-高さ-$z$-で輪切りにする方法-1.4.1"><span class="toc-item-num">1.4.1&nbsp;&nbsp;</span>方法1: 高さ $z$ で輪切りにする方法</a></span></li><li><span><a href="#方法2:-極座標を使う方法" data-toc-modified-id="方法2:-極座標を使う方法-1.4.2"><span class="toc-item-num">1.4.2&nbsp;&nbsp;</span>方法2: 極座標を使う方法</a></span></li><li><span><a href="#方法3:-$y=x-\tan\theta$-と変数変換する方法" data-toc-modified-id="方法3:-$y=x-\tan\theta$-と変数変換する方法-1.4.3"><span class="toc-item-num">1.4.3&nbsp;&nbsp;</span>方法3: $y=x \tan\theta$ と変数変換する方法</a></span></li></ul></li></ul></li><li><span><a href="#ガンマ函数とベータ函数" data-toc-modified-id="ガンマ函数とベータ函数-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>ガンマ函数とベータ函数</a></span><ul class="toc-item"><li><span><a href="#ガンマ函数とベータ函数の定義" data-toc-modified-id="ガンマ函数とベータ函数の定義-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>ガンマ函数とベータ函数の定義</a></span><ul class="toc-item"><li><span><a href="#ガンマ函数のGauss積分型表示" data-toc-modified-id="ガンマ函数のGauss積分型表示-2.1.1"><span class="toc-item-num">2.1.1&nbsp;&nbsp;</span>ガンマ函数のGauss積分型表示</a></span></li><li><span><a href="#ガンマ函数のスケール変換" data-toc-modified-id="ガンマ函数のスケール変換-2.1.2"><span class="toc-item-num">2.1.2&nbsp;&nbsp;</span>ガンマ函数のスケール変換</a></span></li><li><span><a href="#ベータ函数の様々な表示" data-toc-modified-id="ベータ函数の様々な表示-2.1.3"><span class="toc-item-num">2.1.3&nbsp;&nbsp;</span>ベータ函数の様々な表示</a></span></li><li><span><a href="#ガンマ函数を定義する積分の被積分函数のグラフ" data-toc-modified-id="ガンマ函数を定義する積分の被積分函数のグラフ-2.1.4"><span class="toc-item-num">2.1.4&nbsp;&nbsp;</span>ガンマ函数を定義する積分の被積分函数のグラフ</a></span></li><li><span><a href="#ベータ函数を定義する積分の被積分函数のグラフ" data-toc-modified-id="ベータ函数を定義する積分の被積分函数のグラフ-2.1.5"><span class="toc-item-num">2.1.5&nbsp;&nbsp;</span>ベータ函数を定義する積分の被積分函数のグラフ</a></span></li></ul></li><li><span><a href="#ガンマ函数の特殊値と函数等式" data-toc-modified-id="ガンマ函数の特殊値と函数等式-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>ガンマ函数の特殊値と函数等式</a></span><ul class="toc-item"><li><span><a href="#ガンマ函数の1と1/2での値." data-toc-modified-id="ガンマ函数の1と1/2での値.-2.2.1"><span class="toc-item-num">2.2.1&nbsp;&nbsp;</span>ガンマ函数の1と1/2での値.</a></span></li><li><span><a href="#ガンマ函数の函数等式" data-toc-modified-id="ガンマ函数の函数等式-2.2.2"><span class="toc-item-num">2.2.2&nbsp;&nbsp;</span>ガンマ函数の函数等式</a></span></li><li><span><a href="#ガンマ函数の正の半整数での値" data-toc-modified-id="ガンマ函数の正の半整数での値-2.2.3"><span class="toc-item-num">2.2.3&nbsp;&nbsp;</span>ガンマ函数の正の半整数での値</a></span></li><li><span><a href="#ガンマ函数の-Ramanujan's-master-theorem-型解析接続" data-toc-modified-id="ガンマ函数の-Ramanujan's-master-theorem-型解析接続-2.2.4"><span class="toc-item-num">2.2.4&nbsp;&nbsp;</span>ガンマ函数の Ramanujan's master theorem 型解析接続</a></span></li></ul></li><li><span><a href="#Riemannのゼータ函数の積分表示と函数等式と負の整数と正の偶数における特殊値" data-toc-modified-id="Riemannのゼータ函数の積分表示と函数等式と負の整数と正の偶数における特殊値-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Riemannのゼータ函数の積分表示と函数等式と負の整数と正の偶数における特殊値</a></span><ul class="toc-item"><li><span><a href="#Riemannのゼータ函数の積分表示1" data-toc-modified-id="Riemannのゼータ函数の積分表示1-2.3.1"><span class="toc-item-num">2.3.1&nbsp;&nbsp;</span>Riemannのゼータ函数の積分表示1</a></span></li><li><span><a href="#Riemannのゼータ函数の0以下での整数での値" data-toc-modified-id="Riemannのゼータ函数の0以下での整数での値-2.3.2"><span class="toc-item-num">2.3.2&nbsp;&nbsp;</span>Riemannのゼータ函数の0以下での整数での値</a></span></li><li><span><a href="#交代版のRiemannのゼータ函数の積分表示" data-toc-modified-id="交代版のRiemannのゼータ函数の積分表示-2.3.3"><span class="toc-item-num">2.3.3&nbsp;&nbsp;</span>交代版のRiemannのゼータ函数の積分表示</a></span></li><li><span><a href="#Hurwitzのゼータ函数0以下の整数での特殊値がBernoulli多項式で書けること" data-toc-modified-id="Hurwitzのゼータ函数0以下の整数での特殊値がBernoulli多項式で書けること-2.3.4"><span class="toc-item-num">2.3.4&nbsp;&nbsp;</span>Hurwitzのゼータ函数0以下の整数での特殊値がBernoulli多項式で書けること</a></span></li><li><span><a href="#べき乗和のBernoulli多項式による表示" data-toc-modified-id="べき乗和のBernoulli多項式による表示-2.3.5"><span class="toc-item-num">2.3.5&nbsp;&nbsp;</span>べき乗和のBernoulli多項式による表示</a></span></li><li><span><a href="#Riemannのゼータ函数の積分表示2(テータ函数のMellin変換)と函数等式" data-toc-modified-id="Riemannのゼータ函数の積分表示2(テータ函数のMellin変換)と函数等式-2.3.6"><span class="toc-item-num">2.3.6&nbsp;&nbsp;</span>Riemannのゼータ函数の積分表示2(テータ函数のMellin変換)と函数等式</a></span></li></ul></li><li><span><a href="#ベータ函数とガンマ函数の関係" data-toc-modified-id="ベータ函数とガンマ函数の関係-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>ベータ函数とガンマ函数の関係</a></span><ul class="toc-item"><li><span><a href="#方法1:-置換積分と積分の順序交換のみを使う方法" data-toc-modified-id="方法1:-置換積分と積分の順序交換のみを使う方法-2.4.1"><span class="toc-item-num">2.4.1&nbsp;&nbsp;</span>方法1: 置換積分と積分の順序交換のみを使う方法</a></span></li><li><span><a href="#方法2:-極座標変換を使う方法" data-toc-modified-id="方法2:-極座標変換を使う方法-2.4.2"><span class="toc-item-num">2.4.2&nbsp;&nbsp;</span>方法2: 極座標変換を使う方法</a></span></li><li><span><a href="#方法3:-y-=-tx-と変数変換する方法" data-toc-modified-id="方法3:-y-=-tx-と変数変換する方法-2.4.3"><span class="toc-item-num">2.4.3&nbsp;&nbsp;</span>方法3: y = tx と変数変換する方法</a></span></li><li><span><a href="#ガンマ函数の1/2での値をベータ函数経由で計算" data-toc-modified-id="ガンマ函数の1/2での値をベータ函数経由で計算-2.4.4"><span class="toc-item-num">2.4.4&nbsp;&nbsp;</span>ガンマ函数の1/2での値をベータ函数経由で計算</a></span></li><li><span><a href="#ベータ函数の応用の雑多の例" data-toc-modified-id="ベータ函数の応用の雑多の例-2.4.5"><span class="toc-item-num">2.4.5&nbsp;&nbsp;</span>ベータ函数の応用の雑多の例</a></span></li><li><span><a href="#B(s,-1/2)の級数展開" data-toc-modified-id="B(s,-1/2)の級数展開-2.4.6"><span class="toc-item-num">2.4.6&nbsp;&nbsp;</span>B(s, 1/2)の級数展開</a></span></li></ul></li><li><span><a href="#ガンマ函数の無限積表示" data-toc-modified-id="ガンマ函数の無限積表示-2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>ガンマ函数の無限積表示</a></span><ul class="toc-item"><li><span><a href="#ガンマ函数に関するGaussの公式" data-toc-modified-id="ガンマ函数に関するGaussの公式-2.5.1"><span class="toc-item-num">2.5.1&nbsp;&nbsp;</span>ガンマ函数に関するGaussの公式</a></span></li><li><span><a href="#指数函数の上からと下からの評価を用いた厳密な議論" data-toc-modified-id="指数函数の上からと下からの評価を用いた厳密な議論-2.5.2"><span class="toc-item-num">2.5.2&nbsp;&nbsp;</span>指数函数の上からと下からの評価を用いた厳密な議論</a></span></li><li><span><a href="#ガンマ函数に関するWeierstrassの公式" data-toc-modified-id="ガンマ函数に関するWeierstrassの公式-2.5.3"><span class="toc-item-num">2.5.3&nbsp;&nbsp;</span>ガンマ函数に関するWeierstrassの公式</a></span></li></ul></li><li><span><a href="#sinとガンマ函数の関係" data-toc-modified-id="sinとガンマ函数の関係-2.6"><span class="toc-item-num">2.6&nbsp;&nbsp;</span>sinとガンマ函数の関係</a></span><ul class="toc-item"><li><span><a href="#sinの奇数倍角の公式を用いたsinの無限積表示の導出" data-toc-modified-id="sinの奇数倍角の公式を用いたsinの無限積表示の導出-2.6.1"><span class="toc-item-num">2.6.1&nbsp;&nbsp;</span>sinの奇数倍角の公式を用いたsinの無限積表示の導出</a></span></li><li><span><a href="#Euler's-reflection-formula" data-toc-modified-id="Euler's-reflection-formula-2.6.2"><span class="toc-item-num">2.6.2&nbsp;&nbsp;</span>Euler's reflection formula</a></span></li><li><span><a href="#cosの偶数倍角の公式を用いたcosの無限積表示の導出" data-toc-modified-id="cosの偶数倍角の公式を用いたcosの無限積表示の導出-2.6.3"><span class="toc-item-num">2.6.3&nbsp;&nbsp;</span>cosの偶数倍角の公式を用いたcosの無限積表示の導出</a></span></li></ul></li><li><span><a href="#Wallisの公式" data-toc-modified-id="Wallisの公式-2.7"><span class="toc-item-num">2.7&nbsp;&nbsp;</span>Wallisの公式</a></span><ul class="toc-item"><li><span><a href="#2種類のWallisの公式の同値性" data-toc-modified-id="2種類のWallisの公式の同値性-2.7.1"><span class="toc-item-num">2.7.1&nbsp;&nbsp;</span>2種類のWallisの公式の同値性</a></span></li><li><span><a href="#sinの無限積表示を用いたWallisの公式の証明" data-toc-modified-id="sinの無限積表示を用いたWallisの公式の証明-2.7.2"><span class="toc-item-num">2.7.2&nbsp;&nbsp;</span>sinの無限積表示を用いたWallisの公式の証明</a></span></li><li><span><a href="#ベータ函数の極限でガンマ函数が得らえることを用いWallisの公式の証明" data-toc-modified-id="ベータ函数の極限でガンマ函数が得らえることを用いWallisの公式の証明-2.7.3"><span class="toc-item-num">2.7.3&nbsp;&nbsp;</span>ベータ函数の極限でガンマ函数が得らえることを用いWallisの公式の証明</a></span></li><li><span><a href="#Wallisの公式からGauss積分の値を得る方法" data-toc-modified-id="Wallisの公式からGauss積分の値を得る方法-2.7.4"><span class="toc-item-num">2.7.4&nbsp;&nbsp;</span>Wallisの公式からGauss積分の値を得る方法</a></span></li></ul></li><li><span><a href="#Legendre's-duplication-formula" data-toc-modified-id="Legendre's-duplication-formula-2.8"><span class="toc-item-num">2.8&nbsp;&nbsp;</span>Legendre's duplication formula</a></span></li><li><span><a href="#sinとガンマ函数の関係の再証明" data-toc-modified-id="sinとガンマ函数の関係の再証明-2.9"><span class="toc-item-num">2.9&nbsp;&nbsp;</span>sinとガンマ函数の関係の再証明</a></span><ul class="toc-item"><li><span><a href="#Legendre's-duplication-formula-を用いた-Euler's-reflection-formula-の再証明" data-toc-modified-id="Legendre's-duplication-formula-を用いた-Euler's-reflection-formula-の再証明-2.9.1"><span class="toc-item-num">2.9.1&nbsp;&nbsp;</span>Legendre's duplication formula を用いた Euler's reflection formula の再証明</a></span></li><li><span><a href="#sinの無限積表示の再証明" data-toc-modified-id="sinの無限積表示の再証明-2.9.2"><span class="toc-item-num">2.9.2&nbsp;&nbsp;</span>sinの無限積表示の再証明</a></span></li></ul></li><li><span><a href="#Lerchの定理とゼータ正規化積" data-toc-modified-id="Lerchの定理とゼータ正規化積-2.10"><span class="toc-item-num">2.10&nbsp;&nbsp;</span>Lerchの定理とゼータ正規化積</a></span><ul class="toc-item"><li><span><a href="#Lerchの定理-(Hurwitzのゼータ函数とガンマ函数の関係)" data-toc-modified-id="Lerchの定理-(Hurwitzのゼータ函数とガンマ函数の関係)-2.10.1"><span class="toc-item-num">2.10.1&nbsp;&nbsp;</span>Lerchの定理 (Hurwitzのゼータ函数とガンマ函数の関係)</a></span></li><li><span><a href="#digamma,-trigamma,-polygamma-函数" data-toc-modified-id="digamma,-trigamma,-polygamma-函数-2.10.2"><span class="toc-item-num">2.10.2&nbsp;&nbsp;</span>digamma, trigamma, polygamma 函数</a></span></li><li><span><a href="#ゼータ正規化積" data-toc-modified-id="ゼータ正規化積-2.10.3"><span class="toc-item-num">2.10.3&nbsp;&nbsp;</span>ゼータ正規化積</a></span></li><li><span><a href="#Lerchの定理を用いたBinetの公式の証明" data-toc-modified-id="Lerchの定理を用いたBinetの公式の証明-2.10.4"><span class="toc-item-num">2.10.4&nbsp;&nbsp;</span>Lerchの定理を用いたBinetの公式の証明</a></span></li></ul></li></ul></li><li><span><a href="#Stirlingの公式とLaplaceの方法" data-toc-modified-id="Stirlingの公式とLaplaceの方法-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Stirlingの公式とLaplaceの方法</a></span><ul class="toc-item"><li><span><a href="#Stirlingの公式" data-toc-modified-id="Stirlingの公式-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Stirlingの公式</a></span></li><li><span><a href="#Wallisの公式のStirlingの公式を使った証明" data-toc-modified-id="Wallisの公式のStirlingの公式を使った証明-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>Wallisの公式のStirlingの公式を使った証明</a></span></li><li><span><a href="#Gauss's-multiplication-formula" data-toc-modified-id="Gauss's-multiplication-formula-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>Gauss's multiplication formula</a></span></li><li><span><a href="#Laplaceの方法" data-toc-modified-id="Laplaceの方法-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Laplaceの方法</a></span><ul class="toc-item"><li><span><a href="#Laplaceの方法の解説" data-toc-modified-id="Laplaceの方法の解説-3.4.1"><span class="toc-item-num">3.4.1&nbsp;&nbsp;</span>Laplaceの方法の解説</a></span></li><li><span><a href="#Laplaceの方法によるStirlingの公式の導出" data-toc-modified-id="Laplaceの方法によるStirlingの公式の導出-3.4.2"><span class="toc-item-num">3.4.2&nbsp;&nbsp;</span>Laplaceの方法によるStirlingの公式の導出</a></span></li></ul></li><li><span><a href="#Laplaceの方法の弱形" data-toc-modified-id="Laplaceの方法の弱形-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>Laplaceの方法の弱形</a></span></li></ul></li></ul></div>
<!-- #endregion -->

```julia
using Base.MathConstants
using Base64
using Printf
using Statistics
const e = ℯ
endof(a) = lastindex(a)
linspace(start, stop, length) = range(start, stop, length=length)

using Plots
#gr(); ENV["PLOTS_TEST"] = "true"
pyplot(fmt=:svg)
#clibrary(:colorcet)
clibrary(:misc)

function pngplot(P...; kwargs...)
    sleep(0.1)
    pngfile = tempname() * ".png"
    savefig(plot(P...; kwargs...), pngfile)
    showimg("image/png", pngfile)
end
pngplot(; kwargs...) = pngplot(plot!(; kwargs...))

showimg(mime, fn) = open(fn) do f
    base64 = base64encode(f)
    display("text/html", """<img src="data:$mime;base64,$base64">""")
end

using SymPy
#sympy.init_printing(order="lex") # default
#sympy.init_printing(order="rev-lex")

const latex = sympy.latex
using LaTeXStrings

using SpecialFunctions
SpecialFunctions.lgamma(x::Real) = logabsgamma(x)[1]

using QuadGK
```

## Gauss積分


### Gauss積分の公式

$$
\int_{-\infty}^\infty e^{-x^2}\,dx = \sqrt{\pi}
$$

を**Gauss積分の公式**と呼ぶことにする. 証明は後で行う.

このノートの筆者は大学新入生が習う積分の公式の中でこれが**最も重要**であると考えている. ガウス積分が重要だと考える理由は以下の通り.

(1) この公式自体が非常に面白い形をしている. 左辺を見てもどこにも円周率は見えないが, 右辺には円周率が出て来る. しかも円周率がそのまま出て来るのではなく, その平方根が出て来る.

(2) 様々な方法を使ってGauss積分の公式を証明できる.

(3) Gauss積分の公式は確率論や統計学で正規分布を扱うときには必須である.  正規分布は中心極限定理によって特別に重要な役目を果たす確率分布である. 

(4) Gauss積分はガンマ函数に一般化される. 

(5) Gauss積分はLaplaceの方法の基礎である.  Laplaceの方法はある種の積分の漸近挙動を調べるための最も基本的な方法であり, 解析学の応用において基本的かつ重要である.

(6) 特にGauss積分で階乗に等しい積分を近似することによって, Stirlingの公式が得られる.  (Stirlingの公式 $n!\sim n^n e^{-n}\sqrt{2\pi n}$ の平行根の因子はGauss積分を経由して得られる.)

以上のようにGauss積分は純粋数学的にも応用数学的にも基本的かつ重要である.


### Gauss積分を使う簡単な計算例


#### Gauss積分

**問題:** 上の公式を使って, $a>0$ のとき,

$$
\int_{-\infty}^\infty e^{-y^2/a}\,dy = \sqrt{a\pi}
$$

となることを示せ. 

**注意:** $a$ を $1/a$ で置き換えれば

$$
\int_{-\infty}^\infty e^{-ay^2}\,dy = \sqrt{\frac{\pi}{a}}
$$

も得られる.

**解答例:** Gauss積分の公式で $\ds x=\frac{y}{\sqrt{a}}$ と置換積分すると

$$
\sqrt{\pi} = \int_{-\infty}^\infty e^{-x^2}\,dx  =
\frac{1}{\sqrt{a}}\int_{-\infty}^\infty e^{-y^2/a}\,dy
$$

なので, 両辺に $\sqrt{a}$ をかければ示したい公式が得られる. $\QED$


#### 正規分布

**問題:** 分散 $\sigma^2>0$, 平均 $\mu$ の正規分布の確率密度函数 $p(x)$ が

$$
p(x) = \frac{1}{\sqrt{2\pi\sigma^2}} e^{-(x-\mu)^2/(2\sigma^2)}
$$

で定義される. このとき

$$
\int_{-\infty}^\infty p(x)\,dx = 1
$$

となることを示せ. (この問題より, 確率統計学においてGauss積分の公式は必須であることがわかる.)

**解答例:** $x=y+\mu$ と置換し, 上の問題の結果を使うと, 

$$
\begin{aligned}
\int_{-\infty}^\infty p(x)\,dx &=
\frac{1}{\sqrt{2\pi\sigma^2}} \int_{-\infty}^\infty e^{-(x-\mu)^2/(2\sigma^2)}\,dx =
\frac{1}{\sqrt{2\pi\sigma^2}} \int_{-\infty}^\infty e^{-y^2/(2\sigma^2)}\,dy 
\\ &=
\frac{1}{\sqrt{2\pi\sigma^2}} \sqrt{2\sigma^2\pi} = 1.
\qquad \QED
\end{aligned}
$$


#### Lebesgueの収束定理が成立しない場合2

**問題(Lebesgueの収束定理の結論が成立しない場合2):** 函数列 $f_n(x)$ を

$$
f_n(x)=\frac{1}{\sqrt{n\pi}}e^{-x^2/n}
$$

と定める. 以下を示せ.

(1) $\ds\int_{-\infty}^\infty f_n(x)\,dx = 1$.

(2) 各 $x\in\R$ ごとに $\ds\lim_{n\to\infty}f_n(x)= 0$.

(3) したがって $\ds\lim_{n\to\infty}\int_{-\infty}^\infty f_n(x)\,dx \ne \int_{-\infty}^\infty \lim_{n\to\infty}f_n(x)\,dx$.

**解答例:** (1)はGauss積分の公式から得られる(詳細は自分で計算して確認せよ). (3)は(1)と(2)からただちに得られるので, あとは(2)のみを示せば十分である. $x\in\R$ を任意に取って固定する. このとき $n\to\infty$ で $\dfrac{x^2}{n}\to 0$, $\dfrac{1}{\sqrt{n\pi}}\to 0$ となるので, $f_n(x)\to 0$ となることもわかる. $\QED$


**問題:** すぐ上の問題の函数 $f_n(x)$ のグラフを描いてみよ. 

**解答例:** 以下のセルのようになる. 

$n$ が大きくなると, $f_n(x)$ の「分布」は広く拡がる. $\QED$

```julia
f(n,x) = exp(-x^2/n)/√(n*π)
x = -10.0:0.05:12.0
#cycls = [:solid, :dash, :dashdot, :dashdotdot]
cycls = [:solid, :dash, :dashdot, :dot]
ns = [1,2,3,4,5,10, 30, 100]
P = plot(size=(420,250))
for k in 1:lastindex(ns)
    plot!(x, f.(ns[k],x), label="n = $(ns[k])", ls=cycls[mod1(k, lastindex(cycls))], lw=1.3)
end
P
```

#### 正規分布のモーメント

**問題:** 次を示せ: $a>0$ と $k=0,1,2,\ldots$ について

$$
\int_{-\infty}^\infty e^{-ax^2}x^{2k}\,dx = 
\sqrt{\pi}\; \frac{1\cdot3\cdots(2k-1)}{2^k} a^{-(2k+1)/2} =
\sqrt{\pi}\; \frac{(2k)!}{2^{2k}k!} a^{-(2k+1)/2}.
\tag{1}
$$

**注意:** $a$ を $1/a$ で置き換えれば次も得られる:

$$
\int_{-\infty}^\infty e^{-x^2/a}x^{2k}\,dx = 
\frac{1\cdot3\cdots(2k-1)}{2^k} \sqrt{a^{2k+1}\pi} =
\frac{(2k)!}{2^{2k}k!} \sqrt{a^{2k+1}\pi}.
\tag{2}
$$

**解答例:** Gauss積分の公式から得られる公式

$$
\int_{-\infty}^\infty e^{-ax^2}\,dx = \sqrt{\pi}\;a^{-1/2}
$$

の両辺を $a$ で微分して $-1$ 倍する操作を繰り返すと((K)を使う),

$$
\begin{aligned}
&
\int_{-\infty}^\infty e^{-ax^2}x^2\,dx = \sqrt{\pi}\;\frac{1}{2}a^{-3/2},
\\ &
\int_{-\infty}^\infty e^{-ax^2}x^4\,dx = \sqrt{\pi}\;\frac{1}{2}\frac{3}{2}a^{-5/2},
\\ &
\int_{-\infty}^\infty e^{-ax^2}x^6\,dx = \sqrt{\pi}\;\frac{1}{2}\frac{3}{2}\frac{5}{2}a^{-7/2}.
\end{aligned}
$$

$k$ 回その操作を繰り返すと, 

$$
\int_{-\infty}^\infty e^{-ax^2}x^{2k}\,dx = 
\sqrt{\pi}\;\frac{1}{2}\frac{3}{2}\cdots\frac{2k-1}{2}a^{-(2k+1)/2}.
$$

これより, (1)の前半が成立することがわかる. 後半の成立は

$$
\frac{1\cdot3\cdots(2k-1)}{2^k} =
\frac{1\cdot3\cdots(2k-1)}{2^k} \frac{2\cdot4\cdots(2k)}{2^k k!} =
\frac{(2k)!}{2^{2k}k!}
\tag{3}
$$

によって確認できる. $\QED$


**注意:** 奇数の積 $1\cdot3\cdots(2k-1)$ について(3)の計算法はよく使われる:

$$
1\cdot3\cdots(2k-1) = 
1\cdot3\cdots(2k-1) \frac{2\cdot4\cdots(2k)}{2^k k!} =
\frac{(2k)!}{2^k k!}.
$$

例えば二項係数に関する

$$
\begin{aligned}
(-1)^k\binom{-1/2}{k} &=
(-1)^k\frac{(-1/2)(-3/2)\cdots(-(2k-1)/2)}{k!} \\ &=
\frac{1\cdot3\cdots(2k-1)}{2^k k!} =
\frac{(2k)!}{2^{2k}k!k!} =
\frac{1}{2^{2k}}\binom{2k}{k}
\end{aligned}
$$

もよく出て来る. $\QED$


### Gauss分布のFourier変換


$a>0$ であるとする.  $e^{-x^2/a}$ 型の函数を**Gauss分布函数**と呼ぶことがある.

一般に函数 $f(x)$ に対して,

$$
\hat{f}(p) = \int_{-\infty}^\infty e^{-ipx} f(x)\,dx
$$

を $f$ の**Fourier変換**(フーリエ変換)と呼ぶ. もしも実数値函数 $f(x)$ が偶函数であれば, 

$$
e^{-ipx} f(x) = f(x)\cos(px) - i f(x)\sin(px)
$$

の虚部は奇函数になり, その積分は消えるので

$$
\hat{f}(p) = \int_{-\infty}^\infty f(x)\cos(px)\,dx
$$

となる.


**問題:** $a>0$ とする. $f(x)=e^{-x^2/a}$ のFourier変換を求めよ.

**解答例1:** $\ds\cos(px)=\sum_{k=0}^\infty\frac{(-p^2)^k x^{2k}}{(2k)!}$ より,

$$
\begin{align}
\hat{f}(p) &=
\int_{-\infty}^\infty e^{-x^2/a} \cos(px)\,dx =
\sum_{k=0}^\infty\frac{(-p^2)^k}{(2k)!}\int_{-\infty}^\infty e^{-x^2/a}x^{2k}\,dx
\\ &=
\sum_{k=0}^\infty\frac{(-p^2)^k}{(2k)!}\frac{(2k)!}{2^{2k}k!} \sqrt{a^{2k+1}\pi} =
\sqrt{a\pi}\sum_{k=0}^\infty\frac{(-ap^2/4)^k}{k!} = \sqrt{a\pi}\;e^{-ap^2/4}.
\end{align}
$$

3つ目の等号で上の方の問題の結果を用いた. $\QED$


**解答例2:** 複素解析を用いる. 複素解析さえ認めて使えば, 形式的によりわかり易く計算できる.

$$
-\frac{x^2}{a}-ipx = 
-\frac{1}{a}\left(x^2 + iapx\right) =
-\frac{1}{a}\left(\left(x+\frac{iap}{2}\right)^2-\frac{-a^2p^2}{4}\right) =
-\frac{1}{a}\left(x+\frac{iap}{2}\right)^2 - \frac{ap^2}{4}
$$

と平方完成し, $\ds x=y-\frac{iap}{2}$ と置換すると,

$$
\begin{aligned}
\hat{f}(p) &= \int_{-\infty}^\infty e^{-x^2/a}e^{-ipx}\,dx =
\int_{-\infty}^\infty e^{-(x^2/a+ipx)}\,dx 
\\ &=
\int_{-\infty}^\infty \exp\left(-\frac{1}{a}\left(x+\frac{iap}{2}\right)^2 - \frac{ap^2}{4}\right)\,dx =
e^{-ap^2/4} \int_{-\infty+iap/2}^{\infty+iap/2} e^{-y^2/a}\,dy.
\end{aligned}
$$

Cauchyの積分定理より,

$$
\int_{-\infty+iap/2}^{\infty+iap/2} e^{-y^2/a}\,dy =
\int_{-\infty}^{\infty} e^{-y^2/a}\,dy = \sqrt{a\pi}.
$$

したがって, 

$$
\hat{f}(p) = \int_{-\infty}^\infty e^{-x^2/a}e^{-ipx}\,dx = \sqrt{a\pi}\;e^{-ap^2/4}.
\qquad \QED
$$


**補足:** 複素平面上の経路 $C$ を次のように定める: まず $-R$ から $R$ に直線的に移動する. 次に $R$ から $R+iap/2$ に直線的に移動する. その次に $R+iap/2$ から $-R+iap/2$ に直線的に移動する. 最後に $-R+iap/2$ から $-R$ に直線的に移動する. これによって得られる長方形型の経路が $C$ である. 上の解答例2の中のCauchyの積分定理をこの経路 $C_R$ に適用した場合を使っている. $R\to\infty$ とすると, 左右の縦方向に移動する経路上での積分が $0$ に収束することを使う. $\QED$


$e^{-ax^2}$ のFourier変換については

* 黒木玄, <a href="https://genkuroki.github.io/documents/20160501StirlingFormula.pdf">ガンマ分布の中心極限定理とStirlingの公式</a>

の第6節も参照せよ.


### Gauss積分の公式の導出


Gauss積分の計算の仕方については

* 黒木玄, <a href="https://genkuroki.github.io/documents/20160501StirlingFormula.pdf">ガンマ分布の中心極限定理とStirlingの公式</a>

の第7節および

* 高木貞治, 解析概論, 岩波書店 (1983)

の<a href="https://twitter.com/genkuroki/status/1018654177164083201">第3章§35の例5,6</a>を参照せよ.


$\ds I = \int_{-\infty}^\infty e^{-x^2}\,dx$ とおく. $I=\sqrt{\pi}$ であることを示したい. そのためには

$$
\begin{aligned}
I^2 &= \int_{-\infty}^\infty e^{-x^2}\,dx\cdot \int_{-\infty}^\infty e^{-y^2}\,dy
\\ &=
\int_{-\infty}^\infty \left(\int_{-\infty}^\infty e^{-x^2}\,dx\right)e^{-y^2}\,dy =
\int_{-\infty}^\infty\left(\int_{-\infty}^\infty e^{-(x^2+y^2)}\,dx\right)\,dy
\end{aligned}
$$

が $\pi$ に等しいことを証明すればよい. 上の計算の2つ目と3つ目の等号で積分の線形性(A)を用いた.


#### 方法1: 高さ $z$ で輪切りにする方法

$\ds I^2 = \int_{-\infty}^\infty\left(\int_{-\infty}^\infty e^{-(x^2+y^2)}\,dx\right)\,dy$ は2変数函数 $z=e^{-(x^2+y^2)}$ の $xyz$ 空間内のグラフと $xy$ 平面 $z=0$ のあいだに挟まれた山型の領域の体積を意味する. 

なぜならば, $S(y) = \ds \int_{-\infty}^\infty e^{-(x^2+y^2)}\,dx$ はその領域の $y$ を固定したときの切断面の面積に等しく, $\int_{-\infty}^\infty S(y)\,dy$ はその切断面の面積の積分なので領域全体の体積に等しいからである.  一般に, 長さを積分すれば面積になり、面積を積分すれば体積になる.

その山型の領域の体積は高さ $z$ での切断面の面積の $z=0$ から $z=1$ までの積分に等しい.  高さ $z$ での切断面は半径 $\sqrt{x^2+y^2}=\sqrt{-\log z}$ の円盤になり, その面積は $-\pi\log z$ になる. ゆえに

$$
I^2 = \int_0^1 (-\pi\log z)\,dz = -\pi\,[z\log z - z]_0^1 = \pi.
$$

これより $I=\int_{-\infty}^\infty e^{-x^2}\,dx = \sqrt{\pi}$ であることがわかる.


#### 方法2: 極座標を使う方法

以下の方法は2重積分の積分変数の変換の仕方(Jacobianが出て来る)を知っておかなければ使えない. 

$x=r\cos\theta$, $y=r\sin\theta$ とおくと,

$$
I^2 = \int_0^{2\pi}d\theta\int_0^\infty r e^{-r^2}\,dr =
2\pi \left[\frac{e^{-r^2}}{-2}\right]_0^\infty = 2\pi\frac{1}{2}=\pi.
$$

ゆえに $I=\sqrt{\pi}$.


#### 方法3: $y=x \tan\theta$ と変数変換する方法 

$I^2$ は次のようにも表せる:

$$
I^2 = 2\int_0^\infty\left(\int_{-\infty}^\infty e^{-(x^2+y^2)}\,dy\right)\,dx.
$$

この積分内で $x$ は $x>0$ を動くと考える.

内側の積分で積分変数を $-\infty<y<\infty$ から $-\pi/2<\theta<\pi/2$ に $y=x\tan\theta$ によって変換すると,

$$
\cos\theta > 0, \quad
dy = \frac{x}{\cos^2\theta}\,d\theta, \quad
x^2+y^2 = x^2(1+\tan^2\theta) = \frac{x^2}{\cos^2\theta}
$$

なので

$$
I^2 = 2 \int_0^\infty\left(\int_{-\pi/2}^{\pi/2} \exp\left(-\frac{x^2}{\cos^2\theta}\right)\frac{x}{\cos^2\theta}\,d\theta\right)\,dx.
$$

ゆえに積分の順序を交換すると((J)を使う),

$$
\begin{aligned}
I^2 &= 2 \int_{-\pi/2}^{\pi/2}\left(\int_0^\infty \exp\left(-\frac{x^2}{\cos^2\theta}\right)\frac{x}{\cos^2\theta}\,dx\right)\,d\theta
\\ &=
2 \int_{-\pi/2}^{\pi/2}\left[\frac{1}{-2}\exp\left(-\frac{x^2}{\cos^2\theta}\right)\right]_{x=0}^{x=\infty}\,d\theta =
2 \int_{-\pi/2}^{\pi/2}\frac{1}{2}\,d\theta = 2\frac{\pi}{2} = \pi.
\end{aligned}
$$

したがって $I=\sqrt{\pi}$.

**注意:** 極座標変換 $(x,y)=(r\cos\theta, r\sin\theta)$ が有効な場面では, $y=x\tan\theta$ という変数変換も有効なことが多い. $\tan\theta$ の幾何的な意味は「原点を通る直線の傾き」であった. その意味でも $y=x\tan\theta$ は自然な変数変換だと言える. $\QED$


## ガンマ函数とベータ函数


### ガンマ函数とベータ函数の定義

$s>0$, $p>0$, $q>0$ と仮定する. $\Gamma(s)$ と $B(p,q)$ を次の積分で定義する:

$$
\Gamma(s) = \int_0^\infty e^{-x}x^{s-1}\,dx, \quad
B(p,q) = \int_0^1 x^{p-1}(1-x)^{q-1}\,dx.
$$

$\Gamma(s)$ をガンマ函数と, $B(p,q)$ をベータ函数と呼ぶ.


#### ガンマ函数のGauss積分型表示

**問題(ガンマ函数のGauss積分型の表示):** 次を示せ:

$$
\Gamma(s) = 2\int_0^\infty e^{-y^2} y^{2s-1}\,dy.
$$

**解答例:** ガンマ函数の積分による定義式において $x=y^2$ と置換すると, 

$$
\Gamma(s) = \int_0^\infty e^{-y^2} y^{2s-2}\,2y\,dy = 2\int_0^\infty e^{-y^2} y^{2s-1}\,dy.
\qquad\QED
$$

**注意:** この公式より, ガンマ函数は本質的にGauss積分の一般化になっていることがわかる. $\QED$

```julia
y = symbols("y")
s = symbols("s", positive=true)
sol = 2*integrate(e^(-y^2)*y^(2s-1), (y,0,oo))
latexstring(raw"\ds 2\int_0^\infty e^{-y^2} y^{2s-1} \,dy =", latex(sol))
```

**問題:** 次を示せ: $r>0$ について

$$
\int_0^\infty e^{-x^r}\,dx = 
\frac{1}{r}\Gamma\left(\frac{1}{r}\right).
$$

**略解:** $x=t^{1/r}$ と置換すればただちに得られる. $\QED$


**注意:** ガンマ函数の函数等式(下の方で示す)もしくは部分積分によって $\ds \frac{1}{r}\Gamma\left(\frac{1}{r}\right)=\Gamma\left(1+\frac{1}{r}\right)$ が成立することもわかる. $\QED$

```julia
x = symbols("x")
r = symbols("r", positive=true)
sol = integrate(e^(-x^r), (x,0,oo))
latexstring(raw"\ds \int_0^\infty e^{-x^r} \,dx =", latex(sol))
```

#### ガンマ函数のスケール変換

**問題(ガンマ函数のスケール変換):** 次を示せ:

$$
\int_0^\infty e^{-x/\theta}x^{s-1}\,dx = \theta^s\Gamma(s) \quad (\theta>0,\ s>0).
$$

ガンマ函数はこの形式でも非常によく使われる.

**解答例:** $x=\theta y$ と置換すると, $x^{s-1}\,dx = \theta^s y^{s-1}\,dy$ なので示したい公式が得られる. $\QED$.

```julia
x = symbols("x")
s = symbols("s", positive=true)
t = symbols("t", positive=true)
sol = simplify(integrate(e^(-x/t)*x^(s-1), (x,0,oo)))
latexstring(raw"\ds \int_0^\infty e^{-x/t} x^{s-1} \,dx =", latex(sol))
```

#### ベータ函数の様々な表示

**問題(ベータ函数の様々な表示):** 次を示せ:

$$
B(p,q) = 
2\int_0^{\pi/2} (\cos\theta)^{2p-1}(\sin\theta)^{2q-1}\,d\theta =
\int_0^\infty \frac{t^{p-1}}{(1+t)^{p+q}}\,dt =
\frac{1}{p}\int_0^\infty \frac{du}{(1+u^{1/p})^{p+q}}.
$$

ベータ函数のこれらの表示もよく使われる.

**解答例:** $B(p,q)=\int_0^1 x^{p-1}(1-x)^{q-1}\,dx$ で $x=\cos^2\theta$ と置換すると,

$$
dx = -2\cos\theta\;\sin\theta\;d\theta
$$

より, 

$$
B(p,q) = 2\int_0^{\pi/2} (\cos\theta)^{2p-1}(\sin\theta)^{2q-1}\,d\theta.
$$

$B(p,q)=\int_0^1 x^{p-1}(1-x)^{q-1}\,dx$ で $\ds x=\frac{t}{1+t}=1-\frac{1}{1+t}$ と置換すると,

$$
dx = \frac{dt}{1+t}
$$

より, 

$$
B(p,q) = 
\int_0^\infty \left(\frac{t}{1+t}\right)^{p-1} \left(\frac{1}{1+t}\right)^{q-1}\,\frac{dt}{(1+t)^2} =
\int_0^\infty \frac{t^{p-1}}{(1+t)^{p+q}}\,dt
$$

さらに $t=u^{1/p}$ と置換すると, 

$$
t^{p-1}\,dt = \frac{1}{p} \, du
$$

より, 

$$
B(p,q) = \int_0^\infty \frac{t^{p-1}}{(1+t)^{p+q}}\,dt =
\frac{1}{p}\int_0^\infty \frac{du}{(1+u^{1/p})^{p+q}}.
\qquad \QED
$$


**問題:** 次を示せ. $a<b$, $p>0$, $q>0$ のとき,

$$
\int_a^b (x-a)^{p-1}(b-x)^{q-1}\,dx = (b-a)^{p+q-1} B(p,q).
$$

**証明:** $x=(1-t)a+tb=a+(b-a)t$ と積分変数を置換すると,

$$
\int_a^b (x-a)^{p-1}(b-x)^{q-1}\,dx =
\int_0^1 ((b-a)t)^{p-1}((b-a)(1-t))^{q-1}(b-a)\,dt = (b-a)^{p+q-1}B(p,q).
\qquad\QED
$$

**例:** $\ds B(2,2)=\int_0^1 x(1-x)\,dx = \frac{1}{2}-\frac{1}{3}=\frac{1}{6}$ なので

$$
\int_a^b (x-a)(b-x)\,dx = (b-a)^3 B(2,2) = \frac{(b-a)^3}{6}.
\qquad \QED
$$


#### ガンマ函数を定義する積分の被積分函数のグラフ

**問題:** ガンマ函数を定義する積分の被積分函数のグラフを色々な $s>0$ について描いてみよ.

**解答例:** 次のセルを見よ. $\QED$

```julia
# ガンマ函数の積分の被積分函数のグラフ

f(s,x) = e^(-x)*x^(s-1)
x = 0.00:0.05:30.0
PP = []
for s in [1/2, 1, 2, 3, 6, 10]
    P = plot(x, f.(s,x), title="s = $s", titlefontsize=10)
    push!(PP, P)
end
for s in [15, 20, 30]
    x = 0:0.02:2.2s
    P = plot(x, f.(s,x), title="s = $s", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 200), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[4:6]..., size=(750, 200), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[7:9]..., size=(750, 200), legend=false, layout=@layout([a b c]))
```

$s$ を大きくすると, ガンマ函数の被積分函数(を函数で割ったもの)は正規分布の確率密度函数とほとんどぴったり一致するようになる. 次のセルを見よ.

```julia
# f(s,x) = e^{-x} x^{s-1} / Γ(s)
# g(s,x) = e^{-(x-s)^2/(2s)} / √(2πs)

f(s,x) = e^(-x+(s-1)*log(x)-lgamma(s))
g(s,x) = e^(-(x-s)^2/(2s)) / √(2π*s)
s = 100
x = 0:0.5:2s
plot(size=(400, 250))
plot!(title="y = e^(-x) x^(s-1)/Gamma(s),  s = $s", titlefontsize=11)
plot!(x, f.(s,x), label="Gamma dist", lw=2)
plot!(x, g.(s,x), label="normal dist", ls=:dash, lw=2)
```

#### ベータ函数を定義する積分の被積分函数のグラフ

**問題:** ベータ函数を定義する積分の被積分函数のグラフを色々な $p,q>0$ について描いてみよ.

**解答例:** 次のセルを見よ. $\QED$

```julia
# ベータ函数の積分の被積分函数のグラフ

f(p,q,x) = x^(p-1)*(1-x)^(q-1)
x = 0.002:0.002:0.998
PP = []
for (p,q) in [(1/2,1/2), (1,1), (1,2), (2,2), (2,3), (2,4), (4,6), (8, 12), (16, 24)]
    y = f.(p,q,x)
    P = plot(x, y, title="(p,q) = ($p,$q)", titlefontsize=10, xlims=(0,1), ylims=(0,1.05*maximum(y)))
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 200), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[4:6]..., size=(750, 200), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[7:9]..., size=(750, 200), legend=false, layout=@layout([a b c]))
```

$p,q$ がそれらの比を保ちながら大きくすると, ベータ函数の被積分函数をベータ函数で割ったものは正規分布の被積分函数にほとんどぴったり一致するようになる. 次のセルを見よ.

```julia
# f(p,q,x) = x^{p-1} (1-x)^{q-1} / B(p,q)
# μ = p/(p+q)
# σ² = pq/((p+q)^2(p+q+1))
# g(μ,σ²,x) = e^{-(x-μ)^2/(2σ²)} / √(2πσ²)

f(p,q,x) = x^(p-1)*(1-x)^(q-1)/beta(p,q)
g(μ,σ²,x) = e^(-(x-μ)^2/(2*σ²)) / √(2π*σ²)
p, q = 45,55
μ = p/(p+q)
σ² = p*q/((p+q)^2*(p+q+1))
x = 0.000:0.002:1.000
plot(size=(400, 250))
plot!(title="y = x^(p-1) (1-x)^(q-1) / B(p,q),  (p,q) = ($p,$q)", titlefontsize=10)
plot!(x, f.(p,q,x), label="Beta dist", lw=2)
plot!(x, g.(μ,σ²,x), label="normal dist", lw=2, ls=:dash)
```

### ガンマ函数の特殊値と函数等式

#### ガンマ函数の1と1/2での値.

**問題(ガンマ函数の最も簡単な特殊値):** $\Gamma(1)=1$ と $\Gamma(1/2)=\sqrt{\pi}$ を示せ.

**解答例:** 前者は

$$
\Gamma(1)=\int_0^\infty e^{-x}\,dx = [-e^{-x}]_0^\infty = 1.
$$

と容易に示される. 後者を示すためには $\Gamma(1/2)$ がGauss積分 $\int_{-\infty}^\infty e^{-y^2}\,dy=\sqrt{\pi}$ に等しいことを示せばよい.  $x=y^2$ で置換積分すると,

$$
\begin{aligned}
\Gamma(1/2) &= \int_0^\infty e^{-x}x^{1/2-1}\,dx =
\int_0^\infty e^{-y^2} \frac{1}{y} 2y\,dy 
\\ &= 
2\int_0^\infty e^{-y^2}\,dy =
\int_{-\infty}^\infty e^{-y^2}\,dy = \sqrt{\pi}. 
\qquad \QED
\end{aligned}
$$

**注意:** 上の問題の解答より, $\Gamma(1/2)$ は本質的にGauss積分に等しい. その意味でガンマ函数はGauss積分の一般化になっていると言える. $\QED$


#### ガンマ函数の函数等式

**問題(ガンマ函数の函数等式):** $s>0$ のとき $\Gamma(s+1)=s\Gamma(s)$ となることを示せ.

**解答例:** 部分積分を使う. $s>0$ と仮定する. このとき

$$
\begin{aligned}
\Gamma(s+1) &=
\int_0^\infty e^{-x}x^s\,dx =
\int_0^\infty (-e^{-x})'x^s\,dx
\\ &=
\int_0^\infty e^{-x} (x^s)'\,dx =
\int_0^\infty e^{-x} sx^{s-1}\,dx =
s\Gamma(s).
\end{aligned}
$$

3つ目の等号で部分積分を行った. そのとき, $x\searrow 0$ でも $x\to\infty$ でも $e^{-x}x^s\to 0$ となることを使った($s>0$ と仮定したことに注意せよ).  (積分以外の項が消える.) $\QED$

**注意:** 上の問題の結果を使えば, $s< 0$, $s\ne 0,-1,-2,\ldots$ のとき $s+n>0$ となる整数 $n$ を取れば, 

$$
\Gamma(s) = \frac{\Gamma(s+n)}{s(s+1)\cdots(s+n-1)}
$$

の右辺はwell-definedになるので, この公式によってガンマ函数を $s<0$, $s\ne 0,-1,-2,\ldots$ の場合に自然に拡張できる. $\QED$ 

**注意(ガンマ函数は階乗の一般化):** 以上の問題の結果より, 非負の整数 $n$ について

$$
\Gamma(n+1)=n\Gamma(n)=n(n-1)\Gamma(n-1)=\cdots=n(n-1)\cdots1\,\Gamma(1)=n!.
$$

すなわち, $\Gamma(s+1)$ は階乗 $n!$ の連続変数 $s$ への拡張になっていることがわかる. $\QED$


#### ガンマ函数の正の半整数での値

**問題(ガンマ函数の正の半整数での値):** 次を示せ: 非負の整数 $k$ に対して

$$
\Gamma((2k+1)/2) = \frac{1\cdot3\cdots(2k-1)}{2^k}\sqrt{\pi} =
\frac{(2k)!}{2^{2k}k!}\sqrt{\pi}
$$

**解答例1:** ガンマ函数の函数等式と $\Gamma(1/2)=\sqrt{\pi}$ より

$$
\begin{aligned}
\Gamma\left(\frac{2k+1}{2}\right) &=
\frac{2k-1}{2}\Gamma\left(\frac{2k-1}{2}\right) =
\frac{2k-1}{2}\frac{2k-3}{2}\Gamma\left(\frac{2k-3}{2}\right) = \cdots 
\\ &=
\frac{2k-1}{2}\frac{2k-3}{2}\cdots\frac{1}{2}\Gamma\left(\frac{1}{2}\right) = 
\frac{1\cdot3\cdots(2k-1)}{2^k}\sqrt{\pi}.
\end{aligned}
$$

これで示したい公式の1つ目の等号は示せた. 2つ目の等号は上の方のGauss積分の応用問題で使った方法を使えば同様に示される. $\QED$

**解答例2:** ガンマ函数の $\Gamma(s)=2\int_0^\infty e^{-y^2}y^{2s-1}\,dy$ という表示を使うと,

$$
\Gamma((2k+1)/2) = 2\int_0^\infty e^{-y^2} y^{2k}\,dy = \int_{-\infty}^\infty e^{-y^2} y^{2k}\,dy
$$

なので, 上の方のGauss積分の応用問題に関する結果から欲しい公式が得られる. $\QED$


#### ガンマ函数の Ramanujan's master theorem 型解析接続

**問題:** $N=0,1,2,\ldots$ とする. 次の公式を示せ:

$$
\Gamma(s) = \int_0^\infty \left(e^{-x}-\sum_{k=0}^N\frac{(-x)^k}{k!}\right)x^{s-1}\,dx
\qquad (-(N+1)<\real s<-N).
$$

**解答例:** $\real s > 0$ のとき

$$
\begin{aligned}
&
\int_0^1 e^{-x}x^{s-1}\,dx 
\\ &= 
\int_0^1 \left(e^{-x}-\sum_{k=0}^N\frac{(-x)^k}{k!}\right)x^{s-1}\,dx +
\sum_{k=0}^N\frac{(-1)^k}{k!}\int_0^1 x^{s+k-1}\,dx
\\ &=
\int_0^1 \left(e^{-x}-\sum_{k=0}^N\frac{(-x)^k}{k!}\right)x^{s-1}\,dx +
\sum_{k=0}^N\frac{(-1)^k}{k!}\frac{1}{s+k}.
\end{aligned}
$$

この計算の最後の結果中の積分は $\real s > -(N+1)$ で収束し, この計算結果は $\int_0^1 e^{-x}x^{s-1}\,dx$ の $\real s > -(N+1)$ への解析接続を与える.

$\int_1^\infty x^{s+k-1}\,dx = -1/(s+k)$ ($\real s < -k$) を使った上と同様の計算によって, $\real s < -N$ のとき

$$
\begin{aligned}
&
\int_1^\infty e^{-x}x^{s-1}\,dx
\\ &=
\int_1^\infty \left(e^{-x}-\sum_{k=0}^N\frac{(-x)^k}{k!}\right)x^{s-1}\,dx -
\sum_{k=0}^N\frac{(-1)^k}{k!}\frac{1}{s+k}
\end{aligned}
$$

となることがわかる.

$-(N+1)<\real s<-N$ のとき, 以上の2つの結果を足し合わせると, $\sum$ の項がキャンセルして消えて, 

$$
\Gamma(s) = \int_0^\infty \left(e^{-x}-\sum_{k=0}^N\frac{(-x)^k}{k!}\right)x^{s-1}\,dx
$$

が得られる. $\QED$

**注意:** この公式は [Ramanujan's master theorem](https://www.google.com/search?q=Ramanujan+master+theorem) の特別な場合になっている. 論文

* Tewodros Amdeberhen, Olivier Espinosa, Ivan Gonzalez, Marshall Harrison, Victor H. Moll, and Armin Straub. Ramanujan’s Master Theorem. The Ramanujan Journal 29(1-3). DOI: 10.1007/s11139-011-9333-y ([ResearchGate](https://www.researchgate.net/publication/257643116_Ramanujan's_Master_Theorem))

の Example 8.2 は上の公式の $N=0$ の場合になっている. $\QED$


### Riemannのゼータ函数の積分表示と函数等式と負の整数と正の偶数における特殊値

この節はこのノートを最初に読むときには飛ばして読んでも構わない. ガンマ函数の理論がRiemannのゼータ函数の理論と密接に関係していることを認識しておけば問題ない. 

Bernoulli数やBernoulli多項式に関してはノート「<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/13%20Euler-Maclaurin%20summation%20formula.ipynb">13 Euler-Maclaurinの和公式</a>」により詳しい解説がある.


#### Riemannのゼータ函数の積分表示1

**問題(Riemannのゼータ函数の積分表示1):** 次をが成立することを示せ.

$$
\zeta(s)=\sum_{n=1}^\infty \frac{1}{n^s} = 
\frac{1}{\Gamma(s)}\int_0^\infty \frac{x^{s-1}\,dx}{e^x-1}
\quad (s>1).
$$

**注意:** $x\to 0$ のとき $\ds \frac{e^x-1}{x}\to 1$ となるので,  $\ds \frac{x}{e^x-1}$ は $x=0$ まで連続的に拡張され, この公式の積分は

$$
\int_0^\infty \frac{x^{s-1}\,dx}{e^x-1} = 
\int_0^\infty \frac{x}{e^x-1} x^{s-2}\,dx
$$

と書けるので, $s-2 > -1$ すなわち $s>1$ ならば収束している. $\QED$

**解答例:** 上の問題の結果より, $\ds\frac{1}{n^s} = \frac{1}{\Gamma(s)}\int_0^\infty e^{-nx}x^{s-1}\,dx$ なので,

$$
\begin{aligned}
\zeta(s) &=\sum_{n=1}^\infty \frac{1}{n^s} =
\frac{1}{\Gamma(s)}\sum_{n=1}^\infty\int_0^\infty e^{-nx}x^{s-1}\,dx
\\ &=
\frac{1}{\Gamma(s)}\int_0^\infty\sum_{n=1}^\infty e^{-nx}x^{s-1}\,dx =
\frac{1}{\Gamma(s)}\int_0^\infty \frac{x^{s-1}\,dx}{e^x-1}.
\qquad \QED
\end{aligned}
$$


**定義:** Bernoulli数 $B_n$ ($n=0,1,2,\ldots$) を次の条件によって定める:

$$
\frac{z}{e^z-1} = \sum_{n=1}^\infty \frac{B_n}{n!}z^n.
\qquad\QED
$$


**問題:** $B_0=1$, $\ds B_1=-\frac{1}{2}$ であり, $n$ が3以上の奇数のとき $B_n=0$ となることを示せ.

**解答例:** $z\to 0$ のとき, $\ds \frac{z}{e^z-1}\to 1$ より $B_0=1$ となる. さらに, 

$$
\frac{z}{e^z-1}+\frac{z}{2} = \frac{z}{2}\frac{e^z+1}{e^z-1} = 
\frac{z}{2}\frac{e^{z/2}+e^{-z/2}}{e^{z/2}-e^{-z/2}}
$$

であることと, これが偶函数であることから, $\ds B_1=-\frac{1}{2}$ でかつ $n$ が3以上の奇数ならば $B_n=0$ となることがわかる. $\QED$


**問題(Riemannのゼータ函数の積分表示1'):** 非負の整数 $N$ に対して, 次を示せ:

$$
\zeta(s) = 
\frac{1}{\Gamma(s)}\left[
\int_1^\infty \frac{x^{s-1}\,dx}{e^x-1} +
\int_0^1 \left(\frac{x}{e^x-1} - \sum_{k=0}^N \frac{B_k}{k!}x^k\right)x^{s-2}\,dx +
\sum_{k=0}^N \frac{B_k}{k!}\frac{1}{s+k-1}
\right].
$$

さらに右辺の括弧の内側の2つ目の積分が $s>-N$ で絶対収束していることを示せ.

**解答例:** Riemannのゼータ函数の積分表示1の公式で積分を $\int_1^\infty$ と $\int_0^1$ に分けて, $k=0,1,\ldots,N$ に対する

$$
\ds\int_0^1\frac{B_k}{k!}x^{s+k-2}\,dx = \frac{B_k}{k!}\frac{1}{s+k-1}
$$

を足して引けば示したい公式が得られる. 

$$
\frac{x}{e^x-1} - \sum_{k=0}^N \frac{B_k}{k!}x^k = O(x^{N+1})
$$

であり, $\ds\int_0^1 x^{N+1}x^{s-2}\,dx=\int_0^1 x^{s+N-1}\,dx$ が $s>-N$ で絶対収束していることから, 右辺の括弧の内側の2つ目の積分もそこで収束している. $\QED$


#### Riemannのゼータ函数の0以下での整数での値

**問題:** Riemannのゼータ函数の積分表示1'の右辺で $\zeta(s)$ を $s>-N$ まで拡張しておくとき, 

$$
\zeta(0) = -\frac{1}{2}, \quad \zeta(-r) = -\frac{B_{r+1}}{r+1} \quad (r=1,2,3,\ldots)
$$

となることを示せ. ($r$ が2以上の偶数のとき $B_{r+1}=0$ となることに注意せよ.)

**解答例:** ガンマ函数の函数等式より,

$$
\begin{aligned}
\frac{1}{\Gamma(s)}\frac{B_k}{k!}\frac{1}{s+k-1} &=
\frac{s(s+1)\cdots(s+k-2)(s+k-1)}{\Gamma(s+k)}\frac{B_k}{k!}\frac{1}{s+k-1} 
\\ &=
\frac{s(s+1)\cdots(s+k-2)}{\Gamma(s+k)}\frac{B_k}{k!}
\end{aligned}
$$

なので, 非負の整数 $r$ に対して, $k=r+1$ とおいて $s\to -r$ とすると,

$$
\frac{1}{\Gamma(s)}\frac{B_k}{k!}\frac{1}{s+k-1}\to
(-1)^r \frac{B_{r+1}}{r+1} =
\begin{cases}
-\dfrac{1}{2} & (r=0) \\
-\dfrac{B_{r+1}}{r+1} & (r=1,2,3,\ldots)
\end{cases}.
$$

ただし, 等号で, $\ds B_1=-\frac{1}{2}$ と $r+1$ が3以上の奇数のとき $B_{r+1}=0$ となることを使った. これをRiemannのゼータ函数の積分表示1'

$$
\zeta(s) = 
\frac{1}{\Gamma(s)}\left[
\int_1^\infty \frac{x^{s-1}\,dx}{e^x-1} +
\int_0^1 \left(\frac{x}{e^x-1} - \sum_{k=0}^N \frac{B_k}{k!}x^k\right)x^{s-2}\,dx +
\sum_{k=0}^N \frac{B_k}{k!}\frac{1}{s+k-1}
\right]
$$

に適用すれば,

$$
\zeta(0) = -\frac{1}{2}, \quad \zeta(-r) = -\frac{B_{r+1}}{r+1} \quad (r=1,2,3,\ldots)
$$

が得られる.  $\QED$


#### 交代版のRiemannのゼータ函数の積分表示

**問題:** 次を示せ.

$$
(1-2^{1-s})\zeta(s)=\sum_{n=1}^\infty \frac{(-1)^{n-1}}{n^s} = \frac{1}{\Gamma(s)}\int_0^\infty \frac{x^{s-1}\,dx}{e^x+1} \quad (s>1).
$$

**注意:** この公式の積分は $\real s > 0$ で収束している. $\QED$

**解答例:** 1つ目の等号を示そう:

$$
\begin{aligned}
&
\zeta(s) = \frac{1}{1^s}+\frac{1}{2^s}+\frac{1}{3^s}+\frac{1}{4^s}+\cdots,
\\ &
2^{1-s}\zeta(s) = \frac{2}{2^s}+\frac{2}{4^s}+\frac{2}{6^s}+\frac{2}{8^s}+\cdots,
\\ &
(1-2^{1-s})\zeta(s) = \frac{1}{1^s}-\frac{1}{2^s}+\frac{1}{3^s}-\frac{1}{4^s}+\cdots =
\sum_{n=1}^\infty \frac{(-1)^{n-1}}{n^s}.
\end{aligned}
$$

2つ目の等号を示そう. 上の問題の解答例と同様にして, $\ds\frac{1}{n^s} = \frac{1}{\Gamma(s)}\int_0^\infty e^{-nx}x^{s-1}\,dx$ なので,

$$
\begin{aligned}
\sum_{n=1}^\infty \frac{(-1)^{n-1}}{n^s} &=
\frac{1}{\Gamma(s)}\sum_{n=1}^\infty(-1)^{n-1}\int_0^\infty e^{-nx}x^{s-1}\,dx
\\ &=
\frac{1}{\Gamma(s)}\int_0^\infty\sum_{n=1}^\infty (-1)^{n-1}e^{-nx}x^{s-1}\,dx =
\frac{1}{\Gamma(s)}\int_0^\infty \frac{x^{s-1}\,dx}{e^x+1}\,dx.
\qquad \QED
\end{aligned}
$$

**注意:** 以上の計算は統計力学における<a href="https://www.google.co.jp/search?q=Dirac-Fermi%E5%88%86%E5%B8%83+%CE%B6">Fermi-Dirac統計</a>に関する議論に登場する. ゼータ函数は数論の基本であるだけではなく, 統計力学的にも意味を持っている. $\QED$


#### Hurwitzのゼータ函数0以下の整数での特殊値がBernoulli多項式で書けること

**問題:** **Hurwitzのゼータ函数** $\zeta(s,x)$ と**Bernoulli多項式** $B_k(x)$ を

$$
\zeta(s,x) = \sum_{k=0}^\infty \frac{1}{(x+k)^s}\quad (x>0,\;\; s>1), \qquad
\frac{te^{xt}}{e^t-1} = \sum_{k=0}^\infty \frac{B_k(x)}{k!}t^k
$$

と定める. $\zeta(s)=\zeta(s,1)$ なのでHurwitzのゼータ函数はRiemannのゼータ函数の拡張になっている. 以下を示せ:

(1) $\quad\ds \zeta(s,x) = \frac{1}{\Gamma(s)}\int_0^\infty \frac{e^{(1-x)t}t^{s-1}}{e^t-1}\,dt$.

(2) $\quad\ds \zeta(s,x) = \frac{1}{\Gamma(s)}\left[
\int_1^\infty \frac{e^{(1-x)t}t^{s-1}}{e^t-1}\,dt +
\int_0^1\left(\frac{t e^{(1-x)t}}{e^t-1}-\sum_{k=0}^N\frac{B_k(1-x)}{k!}t^k\right)t^{s-2}\,dt +
\sum_{k=0}^N \frac{B_k(1-x)}{k!}\frac{1}{s+k-1}
\right].
$

(3) Hurwitzのゼータ函数を(2)によって $s<1$ に拡張すると, $0$ 以上の整数 $m$ について

$$
\zeta(-m,x) = \frac{(-1)^m B_{m+1}(1-x)}{m+1} = -\frac{B_{m+1}(x)}{m+1}.
$$

**解答例:** (1) $x,s>0$, $k\geqq 0$ に対して, $\ds \frac{1}{(x+k)^s}=\frac{1}{\Gamma(s)}\int_0^\infty e^{-(x+k)t}t^{s-1}\,dt$ を使うと, 

$$
\begin{aligned}
\zeta(s,x) &=
\sum_{k=0}^\infty \frac{1}{\Gamma(s)}\int_0^\infty e^{-(x+k)t}t^{s-1}\,dt =
\frac{1}{\Gamma(s)}\int_0^\infty \left(\sum_{k=0}^\infty e^{-kt}\right)e^{-xt}t^{s-1}\,dt 
\\ &=
\frac{1}{\Gamma(s)}\int_0^\infty \frac{e^{-xt}t^{s-1}}{1-e^{-t}}\,dt =
\frac{1}{\Gamma(s)}\int_0^\infty \frac{e^{(1-x)t}t^{s-1}}{e^t-1}\,dt.
\end{aligned}
$$

(2) 上の(1)の結果の右辺の積分を $0$ から $1$ への積分と $1$ から $\infty$ の積分に分けて, $0$ から $1$ への積分の被積分函数に $\ds\sum_{k=0}^N\frac{B_k(1-x)}{k!}t^k$ を足して引き, 引いた方の積分を計算すれば, (2)の公式が得られる.

(3) Bernoulli多項式の定義より, $\ds \left(\frac{t e^{(1-x)t}}{e^t-1} - \sum_{k=0}^N\frac{B_k(1-x)}{k!}t^k\right)t^{s-1} = O(t^{s+N-1})$ となるので, (2)の右辺の $0$ から $1$ への積分は $s>-N$ で絶対収束している. $N > m$ と仮定する. $s$ が $0$ 以下の整数に近付くと $\ds\frac{1}{\Gamma(s)}\to 0$ となり, 

$$
\frac{1}{\Gamma(s)}\frac{1}{s+(m+1)-1} = \frac{s(s+1)\cdots(s+m-1)}{\Gamma(s+m+1)} \to (-1)^m m! \quad (s\to -m)
$$

より, $s\to -m$ のとき,

$$
\frac{1}{\Gamma(s)}\frac{B_{m+1}(1-x)}{(m+1)!}\frac{1}{s+(m+1)-1} \to 
(-1)^m m! \frac{B_{m+1}(1-x)}{(m+1)!} =
\frac{(-1)^m B_{m+1}(1-x)}{m+1}
$$

となることから, $\ds\zeta(-m,x)=\frac{(-1)^m B_{m+1}(1-x)}{m+1}$ が得られる. さらに, $\ds\frac{te^{(1-x)t}}{e^t-1} = \frac{(-t)e^{a(-t)}}{e^{-t}-1}$ によって, $B_k(1-x)=(-1)^k B_k(x)$ となることがわかるので, $\zeta(-m,x) = \ds \frac{(-1)^m B_{m+1}(1-x)}{m+1}=-\frac{B_{m+1}(x)}{m+1}$ も得られる. $\QED$


#### べき乗和のBernoulli多項式による表示

**注意:** 上の結果より, 累乗和

$$
S_m(n) = 1^m + 2^m + \cdots + n^m
$$

を次のようにして, Bernoulli多項式を使って表すことができることがわかる:

$$
S_m(n) = \zeta(-m,1) - \zeta(-m,n+1) = \frac{B_{m+1}(n+1)-B_{m+1}(1)}{m+1}.
$$

ここで $\zeta(s,x)$ の解析接続を使った. 形式的には

$$
\begin{aligned}
\zeta(-m,1) - \zeta(-m,n+1) &= 
(1^m + \cdots + n^m + (n+1)^m + (n+1)^m + \cdots) - ((n+1)^m + (n+1)^m + \cdots)
\\ & = 1^m + \cdots + n^m = S_m(n)
\end{aligned}
$$

という計算が実行されたことになる. この計算は $m < -1$ ならばそのまま正しい. それ以外の場合には解析接続によって正当化される. $\QED$


#### Riemannのゼータ函数の積分表示2(テータ函数のMellin変換)と函数等式

**問題(Riemannのゼータ函数の積分表示2):** $\theta(t)$ を

$$
\theta(t) = \sum_{n=1}^\infty e^{-\pi n^2 t} \quad (t>0)
$$

とおくと, 次が成立することを示せ:

$$
\pi^{-s/2}\Gamma(s/2)\zeta(s) = \int_0^\infty \theta(t) t^{s/2-1}\,dt \quad (s>2).
$$

**解答例:** 

$$
\begin{aligned}
\pi^{-s/2}\Gamma(s/2)\zeta(s) &=\sum_{n=1}^\infty \frac{\Gamma(s/2)}{(\pi n^2)^{s/2}} =
\sum_{n=1}^\infty\int_0^\infty e^{-\pi n^2 t} t^{s/2-1}\,dx
\\ &=
\int_0^\infty\sum_{n=1}^\infty e^{-\pi n^2 t} t^{s/2-1}\,dx =
\int_0^\infty \theta(t) t^{s/2-1}\,dt .
\qquad \QED
\end{aligned}
$$


**問題(Riemannのゼータ函数の積分表示2'):** 上の問題の続き. $\theta(t)$ が

$$
1+2\theta(1/t)=t^{1/2}(1+2\theta(t)) \quad (t>0)
$$

すなわち

$$
\theta(1/t) =-\frac{1}{2} + \frac{1}{2}t^{1/2} + t^{1/2}\theta(t)
$$

を満たしていることを認めて, 次を示せ:

$$
\pi^{-s/2}\Gamma(s/2)\zeta(s) = 
-\frac{1}{s}-\frac{1}{1-s} +
\int_1^\infty \theta(t) (t^{s/2}+t^{(1-s)/2})\,\frac{dt}{t}.
$$

$\theta(t)$ に関する上の公式の証明についてはノート「<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/12%20Fourier%20analysis.ipynb">12 Fourier解析</a>」におけるPoissonの和公式の解説を見よ.

**注意:** 上の問題の公式の右辺の積分は $s$ が任意の複素数であってもしているので, 右辺は左辺の複素平面上への解析接続を与える. さらに, 右辺は $s$ を $1-s$ で置き換える操作で不変であるから,

$$
\hat{\zeta}(s) = \pi^{-s/2}\Gamma(s/2)\zeta(s)
$$

とおくと, 

$$
\hat{\zeta}(1-s) = \hat{\zeta}(s)
$$

が成立している. これを**ゼータ函数の函数等式**と呼ぶ.  $\QED$

**解答例:** 上の問題と以下の計算を合わせれば欲しい結果が得られる. 積分区間を $0$ から $1$ と $1$ から $\infty$ に分けて, $t=1/u$ とおくと, $t^{s/2-1}\,dt=-u^{-s/2+1}u^{-2}\,du=-u^{-s/2-1}\,du$ を使うと, 
$$
\begin{aligned}
&
\int_0^\infty \theta(t) t^{s/2-1}\,dt =
\int_0^1 \theta(t) t^{s/2-1}\,dt + 
\int_1^\infty \theta(t) t^{s/2}\,dt,
\\ &
\int_0^1 \theta(t) t^{s/2-1}\,dt =
\int_1^\infty \left(-\frac{1}{2} + \frac{1}{2}t^{1/2} + t^{1/2}\theta(t)\right)t^{-s/2-1}\,dt
\\ &\qquad =
\int_1^\infty\left(
-\frac{1}{2}t^{-s/2-1}+\frac{1}{2}t^{(1-s)/2-1} + \theta(t)t^{(1-s)/2-1}
\right)\,dt
\\ &\qquad =
-\frac{1}{s}-\frac{1}{s-1} + \int_1^\infty \theta(t)t^{(1-s)/2-1}\,dt.
\end{aligned}
$$

上の問題の結果と以上の計算をまとめると, 欲しい結果が得られる. $\QED$


**問題:** 上の問題の続き. ガンマ函数が Euler's reflection formula

$$
\Gamma(s)\Gamma(1-s) = \frac{\pi}{\sin(\pi s)}
$$

と Legendre's duplication formula

$$
\Gamma(s)\Gamma(s+1/2) = 2^{1-2s}\pi^{1/2}\Gamma(2s)
$$

を満たしていることを認めて, 上の問題の注意におけるゼータ函数の函数等式 $\hat\zeta(1-s)=\hat\zeta(s)$ が

$$
\zeta(s) = 2^s \pi^{s-1}\sin\frac{\pi s}{2}\,\Gamma(1-s)\,\zeta(1-s)
$$

と書き直されることを示せ.

Legendre's duplication formula と Euler's reflection formula はこのノートの下の方で初等的に証明される. Euler's reflection formulaの証明についてはノート「<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/12%20Fourier%20analysis.ipynb">12 Fourier解析</a>」のガンマ函数とsinの関係の節も参照せよ.

**解答例:** $\hat\zeta(s)=\pi^{-s/2}\Gamma(s/2)\zeta(s)$, $\hat\zeta(s)=\hat\zeta(1-s)$ より,

$$
\pi^{-s/2}\Gamma(s/2)\zeta(s) = \pi^{-(1-s)/2}\Gamma((1-s)/2)\zeta(1-s).
$$

これは以下のように書き直される:

$$
\zeta(s) = \pi^{s-1/2}\frac{\Gamma((1-s)/2)}{\Gamma(s/2)}\zeta(s).
$$

一方, Euler's reflection formula の $s$ に $s/2$ を代入すると,

$$
\Gamma(s/2)\Gamma(1-s/2)=\frac{\pi}{\sin(\pi s/2)},
\quad\text{i.e.}\quad
\frac{1}{\Gamma(s/2)} = \pi^{-1}\sin\frac{\pi s}{2}\Gamma(1-s/2)
$$

となり, Legendre's duplication formula の $s$ に $(1-s)/2$ を代入すると,

$$
\Gamma((1-s)/2)\Gamma(1-s/2)=2^s \pi^{1/2}\,\Gamma(1-s),
$$

となるので, それらを上の公式に代入すると,

$$
\zeta(s) = 2^s \pi^{s-1}\sin\frac{\pi s}{2}\,\Gamma(1-s)\,\zeta(1-s)
$$

が得られる. $\QED$


**問題:** $k$ が正の整数であるとき $\ds\zeta(-(2k-1)) = -\frac{B_{2k}}{2k}$ であるという事実と上の問題の結果から

$$
\zeta(2k) = \frac{2^{2k-1}(-1)^{k-1}B_{2k}}{(2k)!}\pi^{2k}
$$

が導かれることを示せ.

**解答例:** $\ds\zeta(-(2k-1)) = -\frac{B_{2k}}{2k}$ と上の問題の結果より, 

$$
-\frac{B_{2k}}{2k} = \zeta(-(2k-1)) = 2^{-(2k-1)}\pi^{-2k}(-1)^k(2k-1)!\zeta(2k).
$$

これより示したい公式が得られる. $\QED$


### ベータ函数とガンマ函数の関係

ベータ函数はガンマ函数によって

$$
B(p,q) = \frac{\Gamma(p)\Gamma(q)}{\Gamma(p+q)}
\tag{$*$}
$$

と表わされる. これを証明したい. そのためには

$$
\Gamma(p)\Gamma(q)=
\int_0^\infty
\left(
\int_0^\infty e^{-(x+y)} x^{p-1} y^{q-1}\,dy
\right)\,dx
$$

が

$$
\Gamma(p+q)B(p,q)=\int_0^\infty e^{-z}z^{p+q-1}\,dz
\,\int_0^1 t^{p-1}(1-t)^{q-1}\,dt
$$

に等しいことを示せばよい. ガンマ函数とベータ函数の別の表示を使えば右辺も別の形になることに注意せよ.


#### 方法1: 置換積分と積分の順序交換のみを使う方法

ガンマ函数とベータ函数のあいだの関係式は1変数の置換積分と積分の順序交換のみを使って証明可能である. 条件 $A$ に対して, $x,y$ が条件 $A$ をみたすとき値が $1$ になり, それ以外のときに値が $0$ になる $x,y$ の函数を $1_A(x,y)$ と書くことにすると,
$$
\begin{aligned}
\Gamma(p)\Gamma(q) &=
\int_0^\infty
\left(
\int_0^\infty e^{-(x+y)} x^{p-1} y^{q-1}\,dy
\right)\,dx
\\ &=
\int_0^\infty
\left(
\int_x^\infty e^{-z} x^{p-1} (z-x)^{q-1}\,dz
\right)\,dx
\\ &=
\int_0^\infty
\left(
\int_0^\infty 1_{x<z}(x,z) e^{-z} x^{p-1} (z-x)^{q-1}\,dz
\right)\,dx
\\ &=
\int_0^\infty
\left(
\int_0^\infty 1_{x<z}(x,z) e^{-z} x^{p-1} (z-x)^{q-1}\,dx
\right)\,dz
\\ &=
\int_0^\infty
\left(
\int_0^z e^{-z} x^{p-1} (z-x)^{q-1}\,dx
\right)\,dz
\\ &=
\int_0^\infty
\left(
\int_0^1 e^{-z} (zt)^{p-1} (z-zt)^{q-1}z\,dt
\right)\,dz
\\ &=
\int_0^\infty e^{-z}z^{p+q-1}\,dz
\,\int_0^1 t^{p-1}(1-t)^{q-1}\,dt =
\Gamma(p+q)B(p,q).
\end{aligned}
$$
2つ目の等号で $y=z-x$ と置換積分し, 4つ目の等号で積分の順序を交換し, 6つ目の等号で $x=zt$ と置換積分した.

以上の計算は

* 黒木玄, <a href="https://genkuroki.github.io/documents/20160501StirlingFormula.pdf">ガンマ分布の中心極限定理とStirlingの公式</a>

の第7.4節からの引き写しである.


#### 方法2: 極座標変換を使う方法

この方法は2重積分に関する知識が必要になる. 2重積分について知らない人は次の節の別の方法を参照せよ.

$x=X^2$, $y=Y^2$ と変数変換すると, 

$$
\Gamma(p)\Gamma(q) = 4\int_0^\infty\int_0^\infty e^{-(X^2+Y^2)} X^{2p-1} Y^{2q-1}\,dX\,dY.
$$

さらに $X=r\cos\theta$, $Y=r\sin\theta$ と変数変換すると,

$$
\begin{aligned}
\Gamma(p)\Gamma(q) &= 
4\int_0^{\pi/2}d\theta\int_0^\infty e^{-r^2} (r\cos\theta)^{2p-1} (r\sin\theta)^{2q-1} r\,dr
\\ &=
4\int_0^{\pi/2}(\cos\theta)^{2p-1} (\sin\theta)^{2q-1}\,d\theta
\int_0^\infty e^{-r^2} r^{2(p+q)-1}\,dr =
B(p,q)\Gamma(p+q).
\end{aligned}
$$

最後の等号でベータ函数の三角函数を用いた表示とガンマ函数のGauss積分に似た表示を用いた. $\QED$


#### 方法3: y = tx と変数変換する方法

$y=tx$ とおくと, $dy = x\,dt$ より, 

$$
\begin{aligned}
\Gamma(p)\Gamma(q) &=
\int_0^\infty\left(\int_0^\infty e^{-(x+y)}x^{p-1}y^{q-1}\,dy\right)\,dx =
\int_0^\infty\left(\int_0^\infty e^{-(1+t)x}x^{p+q-1}t^{q-1}\,dt\right)\,dx
\\ &=
\int_0^\infty\left(\int_0^\infty e^{-(1+t)x}x^{p+q-1}\,dx\right)t^{q-1}\,dt =
\int_0^\infty \frac{\Gamma(p+q)}{(1+t)^{p+q}} t^{q-1}\,dt
\\ &=
\Gamma(p+q)\int_0^\infty \frac{t^{q-1}}{(1+t)^{p+q}}\,dt =
\Gamma(p+q)B(p,q).
\end{aligned}
$$

3つ目の等号で積分順序を交換し, 4つ目の等号で $s,c>0$ についてよく使われる公式($x=y/c$ と置けば得られる公式)

$$
\int_0^\infty e^{-cx}x^{s-1}\,dx = \frac{\Gamma(s)}{c^s}
$$

を使い, 最後の等号でベータ函数の次の表示の仕方を用いた:

$$
B(p,q) = \int_0^1 x^{p-1}(1-x)^{q-1}\,dx =
\int_0^\infty \frac{t^{q-1}}{(1+t)^{p+q}}\,dt
$$

この公式は積分変数を $\ds x=\frac{1}{1+t}$ と置換すれば得られる. $\ds x = \frac{t}{1+t}$ と置換すれば $p,q$ を交換した公式が得られる. 

ベータ函数に関するその公式を知っていれば, ガンマ函数とベータ函数の関係を導くにはこの方法が簡単かもしれない.

$y=tx$ の $t$ は直線の傾きという意味を持っている. $xy$ 平面の第一象限の点を $(x,y)$ で指定していたのを, $(x,y)=(x,tx)$ と直線の傾き $t$ と $x$ で指定するようにしたことが, 上の計算で採用した方法である. この方法はJacobianが出て来る二重積分の積分変数の変換を避けたい場合に便利である.


#### ガンマ函数の1/2での値をベータ函数経由で計算

**問題:** ベータ函数とガンマ函数の関係を用いて $\Gamma(1/2)=\sqrt{\pi}$ を証明せよ.

**解答例:**
$$
\Gamma(1/2)^2 = \frac{\Gamma(1/2)\Gamma(1/2)}{\Gamma(1)} = B(1/2,1/2) =
2\int_0^{\pi/2}(\cos\theta)^{2\cdot1/2-1}(\sin\theta)^{2\cdot1/2-1}\,d\theta =
2\int_0^{\pi/2}d\theta = \pi.
$$

1つ目の等号で $\Gamma(1)=1$ を使い, 2つ目の等号でベータ函数とガンマ函数の関係を用い, 3つ目の等号でベータ函数の三角函数を用いた表示を使った. ゆえに $\Gamma(1/2)=\sqrt{\pi}$. $\QED$

**注意:** この問題の解答例はGauss積分の公式の別証明 $\int_{-\infty}^\infty e^{-x^2}\,dx=\Gamma(1/2)=\sqrt{\pi}$ を与える. $\QED$


#### ベータ函数の応用の雑多の例

**問題:** 次の積分を計算せよ:

$$
A = \int_0^1 x^5(1-x^2)^{3/2}\,dx.
$$

**解答例:** $x=t^{1/2}$ と置換すると $\ds dx=\frac{1}{2}t^{-1/2}\,dt$ なので,

$$
A = \int_0^1 t^{5/2}(1-t)^{3/2}\,\frac{1}{2}t^{-1/2}\,dt = 
\frac{1}{2}\int_0^1 t^2(1-t)^{3/2}\,dt = \frac{1}{2}B(3, 5/2) =
\frac{\Gamma(3)\Gamma(5/2)}{2\Gamma(3+5/2)}.
$$

3つ目の等号で $2=3-1$, $3/2=5/2-1$ とみなしてからベータ函数の表示を得ていることに注意せよ. このステップでよく間違う.

一般に非負の整数 $n$ について

$$
\Gamma(n+1) = n!, \quad
\frac{\Gamma(s)}{\Gamma(s+n)} = \frac{1}{s(s+1)\cdots(s+n-1)}
$$

なので, 

$$
\Gamma(3) = 2! = 2, \quad
\frac{\Gamma(5/2)}{\Gamma(3+5/2)} = \frac{1}{(5/2)(7/2)(9/2)} = \frac{2^3}{5\cdot 7\cdot 9}.
$$

したがって

$$
A = \frac{2}{2}\frac{2^3}{5\cdot7\cdot9} = \frac{8}{315}.
\qquad \QED
$$

```julia
x = symbols("x", real=true)
sol = integrate(x^5*(1-x^2)^(Sym(3)/2), (x,0,1))
latexstring(raw"\ds \int_0^1 x^5 (1-x^2)^{3/2} \,dx =", latex(sol))
```

#### B(s, 1/2)の級数展開

$\ds\binom{-1/2}{n}$ は次を満たしている:

$$
\binom{-1/2}{n}(-x)^n =
\frac{(1/2)(3/2)\cdots((2n-1)/2)}{n!}x^n =
\frac{1}{2^{2n}}\binom{2n}{n}x^n.
$$

ゆえに, $|x|<1$ のとき,

$$
(1-x)^{-1/2} = \sum_{n=0}^\infty \frac{1}{2^{2n}}\binom{2n}{n}x^n.
$$

したがって,

$$
B(s,1/2)=\int_0^1 x^{s-1}(1-x)^{-1/2}\,dx=
\sum_{n=0}^\infty \frac{1}{2^{2n}}\binom{2n}{n}\int_0^1 x^{s+n-1}\,dx =
\sum_{n=0}^\infty \frac{1}{2^{2n}}\binom{2n}{n}\frac{1}{s+n}.
$$

例えば, $s=1/2$ のとき, $B(1/2,1/2)=\Gamma(1/2)^2=\pi$ なので, 両辺を2で割ると,

$$
\sum_{n=0}^\infty \frac{1}{2^{2n}}\binom{2n}{n}\frac{1}{2n+1} =
\frac{1}{2}B(1/2,1/2) = \frac{\pi}{2}.
$$

このような公式はベータ函数について知らないと驚くべき公式に見えてしまうが, ベータ函数について知っていれば単に二項展開をベータ函数の被積分函数に適用しただけの公式に過ぎない.

<!-- #region -->
### ガンマ函数の無限積表示


#### ガンマ函数に関するGaussの公式

**問題(Gaussの公式):** ベータ函数とガンマ函数の関係を用いて, 次の公式を示せ.

$$
\Gamma(s) = \lim_{n\to\infty}\frac{n^s n!}{s(s+1)\cdots(s+n)}.
$$

**解答例:** 右辺をベータ函数と表示することを考える. 以下では $n$ は正の整数であるとし, $s>0$ と仮定する. ベータ函数とガンマ函数の函数等式および $\Gamma(n+1)=n!$ より,

$$
B(s,n+1) = \frac{\Gamma(s)\Gamma(n+1)}{\Gamma(s+n+1)} =
\frac{n!}{s(s+1)\cdots(s+n)}.
$$

ゆえに

$$
n^s B(s,n+1) = \frac{n^s n!}{s(s+1)\cdots(s+n)}.
$$

左辺を $n\to\infty$ での極限を取り易い形に変形しよう. $x=t/n$ と置換することによって, $n\to\infty$ のとき

$$
\begin{aligned}
n^s B(s,n+1) &= n^s \int_0^1 x^{s-1}(1-x)^n\,dx
\\ &=
\int_0^n t^{s-1}\left(1-\frac{t}{n}\right)^n\,dt \to \int_0^\infty t^{s-1}e^{-t}\,dt = \Gamma(s).
\end{aligned}
$$

以上をまとめると示したい結果が得られる. $\QED$
<!-- #endregion -->

#### 指数函数の上からと下からの評価を用いた厳密な議論

**問題:** 上の問題の解答例中で極限と積分の順序を交換した. その部分の議論を指数函数に関する不等式

$$
\left(1+\frac{t}{a}\right)^a \leqq e^t \leqq \left(1-\frac{t}{b}\right)^{-b}\qquad(-a<t<b,\;\; a,b>0)
\tag{1}
$$

と $\ds \left(1+\frac{t}{a}\right)^a$, $\ds \left(1-\frac{t}{b}\right)^{-b}$ がそれぞれ $a,b$ について単調増加, 単調減少することを用いて正当化せよ.

**解答例:** 問題文の中で与えられた不等式の全体の逆数を取り, $a=m$, $b=n$ とおくと, 

$$
\left(1-\frac{t}{n}\right)^n \leqq e^{-t} \leqq \left(1+\frac{t}{m}\right)^{-m} \qquad (-m<t<n)
\tag{2}
$$

となり, $\ds \left(1-\frac{t}{n}\right)^n$, $\ds \left(1+\frac{t}{m}\right)^{-m}$ はそれぞれ $n,m$ について単調増加, 単調減少する. ベータ函数の表式

$$
B(p,q) = 
\int_0^1 x^{p-1}(1-x)^{q-1}\,dx =
\int_0^\infty \frac{x^{p-1}}{(1+x)^{p+q}}\,dx
$$

において, それぞれ $(p,q,x)=(s,n+1,t/n)$, $(p,q,x)=(s,m-s,t/m)$ とおくことによって, $s,n>0$, $m>s$ のとき, 

$$
n^s B(s,n+1) = \int_0^n t^{s-1}\left(1-\frac{t}{n}\right)^n\,dt, \quad 
m^s B(s,m-s) = \int_0^\infty t^{s-1}\left(1+\frac{t}{m}\right)^{-m}\,dt
$$

が得られ, それぞれ, $n$, $m$ について単調増加, 単調減少することがわかる.  これらと $\ds\Gamma(s)=\int_0^\infty t^{s-1}e^{-t}\,dt$ を比較すると, 

$$
n^s B(s,n+1) \leqq \Gamma(s) \leqq m^s B(s,m-s).
\tag{$*$}
$$

$n^s B(s,n+1)$, $ m^s B(s,m-s)$ はそれぞれ $n$, $m$ について単調増加, 単調減少するので, どちらも $n,m\to\infty$ で収束する. そして, $m=n+s+1$ とおくと, 

$$
\frac{m^s B(s,m-s)}{n^s B(s,n+1)} = \frac{(n+s+1)^s B(s,n+1)}{n^s B(s,n+1)} =
\left(1+\frac{s+1}{n}\right)^s \to 1 \quad(n\to\infty)
$$

なので, $n^s B(s,n+1)$, $ m^s B(s,m-s)$ は $n,m\to\infty$ で同じ値に収束する. これと不等式($*$)を合わせると,  $n^s B(s,n+1)$, $ m^s B(s,m-s)$ は $n,m\to\infty$ で $\Gamma(s)$ に収束することがわかる. $\QED$

**注意:** 不等式(1),(2)と $a,b$, $m,n$ に関する単調性は極限で指数函数が現われる結果を初等的に正当化するために非常に便利である. $\QED$


#### ガンマ函数に関するWeierstrassの公式

**問題(Weierstrassの公式):** 上の問題の結果を用いて, 次の公式を示せ.

$$
\frac{1}{\Gamma(s)} = 
e^{\gamma s} s\prod_{n=1}^\infty\left[\left(1+\frac{s}{n}\right)e^{-s/n}\right].
\tag{$*$}
$$

ここで $\gamma$ はEuler定数である:

$$
\gamma = \lim_{n\to\infty}\left(\sum_{k=1}^n\frac{1}{k}-\log n\right) =
0.5772\cdots
$$

**解答例:**
$$
\begin{aligned}
&
\frac{s(s+1)\cdots(s+n)}{n^s n!}
\\ &=
s\left(1+s\right)\left(1+\frac{s}{2}\right)\cdots\left(1+\frac{s}{n}\right) e^{-s\log n}
\\ &=
s\left(1+s\right)e^{-s}
\left(1+\frac{s}{2}\right)e^{-s/2}
\cdots
\left(1+\frac{s}{n}\right)e^{-s/n}
e^{s\left(1+\frac{1}{2}+\cdots+\frac{1}{n}-\log n\right)}
\end{aligned}
$$

であるから, 公式($*$)を得る. $\QED$

**注意:** 
$$
\begin{aligned}
\log\left[\left(1+\frac{s}{n}\right)e^{-s/n}\right] &=
\log\left(1+\frac{s}{n}\right) - \frac{s}{n} 
\\ &=
\frac{s}{n} - \frac{s^2}{2n^2} + O\left(\frac{1}{n^3}\right) - \frac{s}{n} 
\\ &= -
\frac{s^2}{2n^2} + O\left(\frac{1}{n^3}\right)
\end{aligned}
$$

なので

$$
\prod_{n=1}^\infty\left[\left(1+\frac{s}{n}\right)e^{-s/n}\right] =
\prod_{n=1}^\infty\left[1 + O\left(\frac{1}{n^2}\right)\right]
$$

となり, この無限積は任意の複素数 $s$ について収束する. したがって, Weierstrassの公式は $1/\Gamma(s)$ のすべての複素数 $s$ への自然な拡張を与える.  $\QED$


### sinとガンマ函数の関係

sinの無限積表示と Euler's reflection formulaの証明についてはノート「<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/12%20Fourier%20analysis.ipynb">12 Fourier解析</a>」のガンマ函数とsinの関係の節も参照せよ. 以下ではsinの奇数倍角の公式を用いた証明を紹介する.


#### sinの奇数倍角の公式を用いたsinの無限積表示の導出

sinの無限積表示

$$
\frac{\sin(\pi s)}{\pi} =
s \prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right)
$$

を導出したい. この公式は正弦函数の奇数倍角の公式の極限としても導出されることを以下で説明しよう. (他にも様々な経路での証明がある.)

非負の整数 $n$ に関する $e^{inx} = (e^{ix})^n$ の右辺に $e^{ix} = \cos x + i\sin x$ を代入して二項定理を適用し, 両辺の虚部を取ると次が得られる:

$$
  \sin(nx) = \sum_{0\leqq k<n/2}
  (-1)^k\binom{n}{2k+1} (\cos x)^{n-2k-1}(\sin x)^{2k+1}.
$$

$(\cos x)^2=1-(\sin x)^2$ より, $n$ が奇数ならば $\sin(nx)$ は $\sin x$ の $n$ 次多項式で表わされることがわかる.

以下では, $m$ は非負の整数であるとし, $n=2m+1$ とおき, $\sin(nx)=\sin((2m+1)x)$ について考える. $\sin((2m+1)x)$ は $\sin x$ の $2m+1$ 次の多項式で表わされ, 周期 $\frac{2\pi}{2m+1}$ を持ち,

$$
 x_k = \frac{k\pi}{2m+1}, \quad k=0,\pm1,\ldots,\pm m
$$

で $0$ になり,

$$
  -\pi/2<x_{-m}<x_{-m+1}<\cdots<x_{m-1}<x_m<\pi/2
$$

なので, $\sin x_k$ は互いに異なる. これで $\sin((2m+1)x)$ の互いに異なる $(2m+1)$ 個の零点 $\sin x_k$ が判明したことになる. $\sin((2m+1)x)$ は $\sin x$ の $2m+1$ 次の多項式で表われるので, $0$ でないある定数 $C$ が存在して,

$$
\sin((2m+1)x) =
  C \sin x
  \prod_{k=1}^m
  \left[
  \left(\sin x - \sin\frac{k\pi}{2m+1}\right)
  \left(\sin x + \sin\frac{k\pi}{2m+1}\right)
  \right].
$$

両辺を $x$ で割って, $x\to 0$ の極限を取ると,

$$
  2m+1 = C
  \prod_{k=1}^m
  \left[
  \left(-\sin\frac{k\pi}{2m+1}\right)
  \left(\sin\frac{k\pi}{2m+1}\right)
  \right].
$$

したがって,

$$
\frac{\sin((2m+1)x)}{2m+1} =
  \sin x
  \prod_{k=1}^m
  \left[
  \left(1-\frac{\sin x}{\sin\frac{k\pi}{2m+1}}\right)
  \left(1+\frac{\sin x}{\sin\frac{k\pi}{2m+1}}\right)
  \right].
$$

これで, $\sin$ の奇数倍角の公式の積表示が得られた.  これに $x=\frac{\pi s}{2m+1}$ を代入し, 両辺に $\frac{2m+1}{\pi}$ をかけると

$$
  \frac{\sin(\pi s)}{\pi} =
  \frac{\sin\frac{\pi s}{2m+1}}{\frac{\pi}{2m+1}}
  \prod_{k=1}^m
  \left[
  \left(1-\frac{\sin\frac{\pi s}{2m+1}}{\sin\frac{\pi k}{2m+1}}\right)
  \left(1+\frac{\sin\frac{\pi s}{2m+1}}{\sin\frac{\pi k}{2m+1}}\right)
  \right].
$$

$m\to\infty$ の極限を取ると正弦函数の無限積表示

$$
\frac{\sin(\pi s)}{\pi}
=s \prod_{k=1}^\infty\left[
\left(1-\frac{s}{k}\right)\left(1+\frac{s}{k}\right)
\right]
= s \prod_{k=1}^\infty\left(1-\frac{s^2}{k^2}\right).
$$

が得られる.  ここで以下を使った: $t\to 0$ のとき

$$
\frac{\sin(at)}{t}\to a, \qquad
  \frac{\sin(at)}{\sin(bt)} \to \frac{a}{b}.
$$

以上のようにsinの無限積表示はsinの奇数倍角の公式の極限として導出される.


**問題:** 上の議論で現われた定数 $C$ を求めよ.

**解答例:** 上の議論中で得られた公式に $n=2m+1$ を代入すると

$$
  \sin((2m+1)x) = \sum_{k=0}^m
  (-1)^k\binom{2m+1}{2k+1} (\cos x)^{2(m-k)}(\sin x)^{2k+1}.
$$

さらに $(\cos x)^2 = 1 - (\sin x)^2$ を代入すると, 右辺の $\sin x$ の多項式としての最高次の係数 $C$ は

$$
\begin{aligned}
C &=
\sum_{k=0}^m (-1)^k \binom{2m+1}{2k+1} (-1)^{m-k} =
(-1)^m\sum_{k=0}^m \binom{2m+1}{2k+1} 
\\ &=
(-1)^m\frac{1}{2}\sum_{j=0}^{2m+1}\binom{2m+1}{j} =
(-1)^m\frac{1}{2}(1+1)^{2m+1}=(-4)^m
\end{aligned}
$$

となることがわかる. $\QED$

**注意:** 上の議論と問題を合わせると次の公式が証明されたことになる: 正の奇数 $n$ に対して,

$$
\frac{\sin(nx)}{\sin x} =
  (-4)^{(n-1)/2}
  \prod_{k=1}^{(n-1)/2}
  \left(\sin^2 x - \sin^2\frac{k\pi}{n}\right).
$$

さらに, 整数 $j$ について, $n/2 < 2j < n$ のとき, $n-2j$ は奇数でかつ $0 < n-2j < n/2$ を満たし,  

$$
\sin\frac{2j\pi}{n} = 
\sin\left(\pi-\frac{2j}{n}\right) =
\sin\frac{(n-2j)\pi}{n}
$$

なので次を得る:

$$
\frac{\sin(nx)}{\sin x} =
  (-4)^{(n-1)/2}
  \prod_{j=1}^{(n-1)/2}
  \left(\sin^2 x - \sin^2\frac{2j\pi}{n}\right).
$$

この公式を使って平方剰余の相互法則を証明することができる(Eisenstein (1845)). その詳細については

* J.-P. セール, 数論講義, 彌永健一訳, 岩波書店, 1979年, 188頁, 第1章補遺.

* G. Eisenstein, Crelle's Journal, Vo1.29, 1845. <a href="https://gdz.sub.uni-goettingen.de/id/PPN243919689_0029?tify={%22pages%22:[183],%22view%22:%22scan%22}">View Archive</a>

を参照せよ. $\QED$


#### Euler's reflection formula 

ガンマ函数の無限積表示より

$$
\begin{aligned}
&
\Gamma(s) = 
\lim_{n\to\infty}\frac{n^s n!}{s(1+s)(2+s)\cdots(n+s)} =
\lim_{n\to\infty}\frac{n^s}{s\left(1+s\right)\left(1+\frac{s}{2}\right)\cdots\left(1+\frac{s}{n}\right)},
\\ &
\Gamma(1-s) = 
\lim_{n\to\infty}\frac{n^{1-s} n!}{(1-s)(2-s)\cdots(n+1-s)} =
\lim_{n\to\infty}\frac{n^{-s}}{\left(1-s\right)\left(1-\frac{s}{2}\right)\cdots\left(1-\frac{s}{n}\right)\left(1+\frac{1-s}{n}\right)},
\end{aligned}
$$

ゆえに, $\sin$ の無限積表示も使うと, 

$$
\frac{1}{\Gamma(s)\Gamma(1-s)} =
\lim_{n\to\infty}s\prod_{k=1}^n\left(\left(1+\frac{s}{k}\right)\left(1-\frac{s}{k}\right)\right) =
s\prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right) =
\frac{\sin(\pi s)}{\pi}.
$$

これで次の公式が得られた:

$$
\Gamma(s)\Gamma(1-s) = \frac{\pi}{\sin(\pi s)}
$$

これは **Euler's reflection formula** と呼ばれる. 


#### cosの偶数倍角の公式を用いたcosの無限積表示の導出

この節の内容は

* https://twitter.com/genkuroki/status/1094856745485139971

の詳述になっている. 上における $\sin$ の無限積表示の導出と同じ方法で

$$
\cos\frac{\pi s}{2} = \prod_{k=1}^\infty\left(1-\frac{s^2}{(2k-1)^2}\right) =
\left(1-\frac{s^2}{1^2}\right)
\left(1-\frac{s^2}{3^2}\right)
\left(1-\frac{s^2}{5^2}\right)
\cdots
$$

を示そう. $e^{inx}=(e^{ix})^n$ の右辺に $e^{ix}=\cos x+i\sin x$ を代入して二項定理を適用し, 両辺の実部を取ることによって,

$$
\cos(nx) = \sum_{0\leqq k\leqq n/2}(-1)^k \binom{n}{2k}(\cos x)^{n-2k}(\sin x)^{2k}.
$$

$(\cos x)^2 = 1 - (\sin x)^2$ より, $n$ が偶数ならば $\cos(nx)$ は $\sin x$ の $n$ 次多項式で表わされることがわかる. 以下では $m$ は非負の整数であるとし, $n=2m$ とおき, $\cos(nx)=\cos(2mx)$ について考える. $\cos(2mx)$ は $\sin x$ の $2m$ 次の多項式で表わされ, 周期 $\ds\frac{2\pi}{2m}$ を持ち, 

$$
\pm x_k = \pm\frac{(2k-1)\pi}{4m}, \quad $k=1,2,\ldots,m$
$$

で $0$ になり,

$$
-\pi/2 < -x_m < \cdots < -x_1 < 0 < x_1 < \cdots < x_m < \pi/2
$$

なので, $2m$ 個の $\sin(\pm x_k)$ は互いに異なる. $0$ でないある定数 $C$ が存在して, 

$$
\cos(2mx) = 
C\prod_{k=1}^m\left[
\left(\sin x - \sin\frac{(2k-1)\pi}{4m}\right)
\left(\sin x + \sin\frac{(2k-1)\pi}{4m}\right)
\right].
$$

この等式の両辺に $x=0$ を代入すると,

$$
1 = C\prod_{k=1}{m}\left[
\left(-\sin\frac{(2k-1)\pi}{4m}\right)
\left(\sin\frac{(2k-1)\pi}{4m}\right)
\right].
$$

この等式の両辺で上の等式の両辺をそれぞれ割ると,

$$
\cos(2mx) = 
\prod_{k=1}^m\left[
\left(1-\frac{\sin x}{\sin\frac{(2k-1)\pi}{4m}}\right)
\left(1+\frac{\sin x}{\sin\frac{(2k-1)\pi}{4m}}\right)
\right].
$$

これに $x=\frac{\pi s}{4m}$ を代入すると,

$$
\cos\frac{\pi s}{2} =
\prod_{k=1}^m\left[
\left(1-\frac{\sin\frac{\pi s}{4m}}{\sin\frac{(2k-1)\pi}{4m}}\right)
\left(1+\frac{\sin\frac{\pi s}{4m}}{\sin\frac{(2k-1)\pi}{4m}}\right)
\right].
$$

これの両辺の $m\to\infty$ の極限を取ると余弦函数の無限積表示

$$
\cos\frac{\pi s}{2} =
\prod_{k=1}^\infty\left[
\left(1-\frac{s}{2k-1}\right)
\left(1+\frac{s}{2k-1}\right)
\right] =
\prod_{k=1}^\infty
\left(1-\frac{s^2}{(2k-1)^2}\right)
$$

を得る. この等式の両辺の $s^2$ の係数の $-1$ 倍を比較すると

$$
\frac{\pi^2}{8} = \sum_{k=1}^\infty \frac{1}{(2k-1)^2} =
\frac{1}{1^2}+\frac{1}{3^2}+\frac{1}{5^2}+\cdots
$$

ゆえに

$$
\zeta(2) = \sum_{n=1}^\infty\frac{1}{n^2} = 
\sum_{k=0}^\infty\frac{1}{(2k)^2} + \sum_{k=1}^\infty\frac{1}{(2k-1)^2} =
\frac{1}{4}\zeta(2) + \frac{\pi^2}{8}
$$

となるので

$$
\zeta(2) = \frac{\pi^2}{6}
$$

も得られる. この公式は

$$
\frac{\sin(\pi s)}{\pi s} =
\prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right)
$$

の両辺の $s^2$ の係数の $-1$ 倍を比較しても得られる.

**問題:** 上の議論で現われた定数 $C$ を求めよ.

**解答例:** 
$$
\cos(2mx) = \sum_{k=0}^m (-1)^k \binom{2m}{2k}(\cos x)^{2m-2k}(\sin x)^{2k}
$$

の右辺に $(\cos x)^2 = 1 - (\sin x)^2$ を代入すると, 右辺の $\sin x$ の多項式としての最高次の係数 $C$ は

$$
\begin{aligned}
C &=
\sum_{k=0}^m (-1)^k\binom{2m}{2k}(-1)^{m-k} = (-1)^m\sum_{k=0}^m\binom{2m}{2k}
\\ &=
(-1)^m\left(\sum_{k=1}^m\binom{2m-1}{2k-1} + \sum_{k=0}^{m-1}\binom{2m-1}{2k}\right) \\ &=
(-1)^m\sum_{j=0}^{2m-1}\binom{2m-1}{j} =
(-1)^m(1+1)^{2m-1} = \frac{(-4)^m}{2}
\end{aligned}
$$

となることがわかる. $\QED$

**注意:** これで次が示された: 非負の偶数 $n$ に対して,

$$
\cos(nx) = \frac{(-4)^{n/2}}{2}\prod_{k=1}^{n/2}\left(\sin^2 x - \sin^2\frac{(2k-1)\pi}{2n}\right).
\qquad \QED
$$


### Wallisの公式

#### 2種類のWallisの公式の同値性

互いに同じ深さにある次の2つの公式の両方を**Wallisの公式**と呼ぶ.

$$
\prod_{n=1}^\infty\frac{(2n)(2n)}{(2n-1)(2n+1)} = \frac{\pi}{2}, \qquad
\frac{1}{2^{2n}}\binom{2n}{n} = \frac{(2n)!}{2^{2n}(n!)^2} \sim \frac{1}{\sqrt{\pi n}}.
$$

**問題:** 前者のWallisの公式から後者のWallisの公式を導け.

**解答例:** 前者のWallisの公式と

$$
1\cdot3\cdots(2n-1) = \frac{(2n)!}{2^n n!}
$$

より,

$$
\frac{2}{\pi}\sim 
\frac
{1\cdot3\cdots(2n-1)\times 3\cdot5\cdots(2n+1)}
{2\cdot4\cdots(2n)\times 2\cdot4\cdots(2n)} =
\left(\frac{(2n)!}{2^{2n}(n!)^2}\right)^2 (2n+1) =
\left(\frac{1}{2^{2n}}\binom{2n}{n}\right)^2 (2n+1).
$$

ゆえに,

$$
\left(\frac{(2n)!}{2^{2n}(n!)^2}\right)^2 = \left(\frac{1}{2^{2n}}\binom{2n}{n}\right)^2 \sim \frac{1}{\pi n}
$$

全体の平方根を取れば, 後者のWallisの公式が得られる. $\QED$

**問題:** 後者のWallisの公式から前者のWallisの公式を導け.

**解答例:** 前者のWallisの公式の無限積の最初の $N$ 個の因子の積は

$$
\prod_{n=1}^N \frac{(2n)(2n)}{(2n-1)(2n+1)} =
\frac{2^{2N} (N!)^2}{(1\cdot3\cdots(2N-1))}\frac{1}{2N+1} =
\left(\frac{2^{2N}(N!)^2}{(2N)!}\right)\frac{1}{2N+1}
$$

であり, 後者のWallisの公式より,  $N\to\infty$ において,

$$
\left(\frac{2^{2N}(N!)^2}{(2N)!}\right)\frac{1}{2N+1} \sim
\frac{\pi N}{2N+1} \sim \frac{\pi}{2}
$$

となる. ゆえに 

$$
\lim_{N\to\infty} \prod_{n=1}^N \frac{(2n)(2n)}{(2n-1)(2n+1)} = \frac{\pi}{2}.
$$

これで後者のWallisの公式から前者のWallisの公式を導けた. $\QED$


#### sinの無限積表示を用いたWallisの公式の証明

**問題(Wallisの公式の証明1):** $\sin$ の無限積表示

$$
s \prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right) = \frac{\sin(\pi s)}{\pi s}
\tag{1}
$$

を用いて, 次の公式を証明せよ:

$$
\prod_{n=1}^\infty\frac{(2n)(2n)}{(2n-1)(2n+1)} = \frac{\pi}{2}.
\tag{2}
$$

**解答例:** (1)に $\ds s=\frac{\pi}{2}$ を代入すれば(2)がただちに得られる. $\QED$

```julia
n = symbols("n")
sol = 1/factor(1-1/(2n)^2)
latexstring(raw"\ds \frac{1}{1-1/(2n)^2} =", latex(sol))
```

#### ベータ函数の極限でガンマ函数が得らえることを用いWallisの公式の証明

**問題(Wallisの公式の証明2):** 前節のガンマ函数のGaussの公式の問題の解答例の中で次を示した:

$$
\lim_{n\to\infty}n^s B(s,n+1) = \Gamma(s)
\tag{1}
$$

これは $n$ が整数以外の実数を動いても成立している. この公式を用いて次の公式を証明せよ.

$$
\prod_{n=1}^\infty\frac{(2n)(2n)}{(2n-1)(2n+1)} = \frac{\pi}{2}.
\tag{2}
$$

**解答例:** ベータ函数とガンマ函数の関係とガンマ函数の特殊値に関する結果より,

$$
\begin{aligned}
&
B(1/2, n+1/2) = \frac{\Gamma(1/2)\Gamma(n+1/2)}{\Gamma(n+1)}=
\pi\frac{1\cdot3\cdots(2n-1)}{2\cdot4\cdots(2n)},
\\ &
B(1/2, n+1) = \frac{\Gamma(1/2)\Gamma(n+1)}{\Gamma(n+1+1/2)}=
2\frac{2\cdot4\cdots(2n)}{3\cdot5\cdots(2n+1)},
\end{aligned}
$$

ゆえに,

$$
\frac{B(1/2,n+1)}{B(1/2,n+1/2)} = 
\frac{2}{\pi}\frac{2\cdot4\cdots(2n)\times 2\cdot4\cdots(2n)}
{1\cdot3\cdots(2n-1)\times 3\cdot5\cdots(2n+1)} =
\frac{2}{\pi}\prod_{k=1}^n\frac{(2k)(2k)}{(2k-1)(2k+1)}
$$

一方, 

$$
\lim_{n\to\infty}\frac{(n+a-1)^s}{(n+b-1)^s} = 1
$$

と公式(1)より,

$$
\lim_{n\to\infty}\frac{B(s,n+a)}{B(s,n+b)} = 
\lim_{n\to\infty}\frac{(n+a-1)^s B(s,n+a)}{(n+b-1)^s B(s,n+b)} = 
\frac{\Gamma(s)}{\Gamma(s)} = 1
$$

この等式の $s=1/2$, $a=1/2$, $b=1$ の場合と上の結果を合わせると, (2)が得られる. $\QED$


**注意:** $\ds B(p,q) = 2\int_0^{\pi/2} (\cos\theta)^{2p-1}(\sin\theta)^{2q-1}\,d\theta$ より,

$$
B(1/2,n+1/2) = 2\int_0^{\pi/2} \sin^{2n}\theta\,d\theta, \quad
B(1/2,n+1) = 2\int_0^{\pi/2} \sin^{2n+1}\theta\,d\theta
$$

であることに注意せよ. 巷でよく見るWallisの公式の証明は右辺のsinのべきの積分を部分積分によって計算することによって行われている. その正体はベータ函数の特殊値であり, 結局のところ $\ds\lim_{n\to\infty}\frac{B(s,n+a+1)}{B(s,n+b+1)}=1$ の特別な場合として, Wallisの公式は得られているのである. $\QED$


#### Wallisの公式からGauss積分の値を得る方法

**問題(Wallisの公式からGauss積分の値を方法):** 問題(Wallisの公式の証明2)の解答例を続けることによって, Gauss積分 $\ds \int_{-\infty}^\infty e^{-x^2}\,dx=\Gamma\left(\frac{1}{2}\right)$ を計算してみよ.

**解答例:** 上の解答例より, 

$$
n^{1/2}B(1/2,n+1/2)\cdot n^{1/2}B(1/2,n+1)=\frac{2n\pi}{2n+1}.
$$

左辺は $n\to\infty$ で $\Gamma(1/2)^2$ に収束し, 右辺は $\pi$ に収束する. このことから $\Gamma(1/2)=\sqrt{\pi}$ であることがわかる. $\QED$


### Legendre's duplication formula

**問題(Legendre's duplication formula):** 次を示せ:

$$
\Gamma(s)\Gamma(s+1/2) = 2^{1-2s}\sqrt{\pi}\;\Gamma(2s).
$$


**解答例1:** 次の公式を示そう:

$$
\int_{-1}^1 (1-x^2)^{s-1}\,dx = 2^{2s-1}B(s,s) = B(1/2,s).
$$

まず, $(1-x^2)^{s-1}=(1-x)^{s-1}(1+x)^{s-1}$ であることに注意し, $x=1-2t$ とおくと,

$$
\int_{-1}^1 (1-x^2)^{s-1}\,dx = 2^{2s-1}\int_0^1 t^{s-1}(1-t)^{s-1}\,dt = 2^{2s-1}B(s,s). 
$$

以上とは別に, 偶函数の積分であることを使って, $x=\sqrt{t}$ とおくと,

$$
\int_{-1}^1 (1-x^2)^{s-1}\,dx = 2\int_0^1 (1-x^2)^{s-1}\,dx =
\int_0^1 (1-t)^{s-1} t^{-1/2}\,dt = B(1/2, s).
$$

これで上の公式が示された. $B(1/2,s) = 2^{2s-1}B(s,s)$ の両辺をガンマ函数を使って表すと,

$$
2^{2s-1}\frac{\Gamma(s)\Gamma(s)}{\Gamma(2s)} =
\frac{\Gamma(s)\Gamma(1/2)}{\Gamma(s+1/2)}
$$

ゆえに $\Gamma(1/2)=\sqrt{\pi}$ を使うと,

$$
\Gamma(s)\Gamma(s+1/2) = 2^{1-2s}\Gamma(1/2)\Gamma(2s) = 2^{1-2s}\sqrt{\pi}\;\Gamma(2s).
\QED
$$


**解答例2:** ガンマ函数の函数等式とこのノートの下の方で証明されているStirlingの近似公式を使っても証明できる. 実は Legendre's duplication formula を Gauss's multiplication formula に一般化しても証明の方針は完全に同様であるので, 一般化した場合の証明のみを書いておくことにする. このノートの下の方を見よ. $\QED$


### sinとガンマ函数の関係の再証明

#### Legendre's duplication formula を用いた Euler's reflection formula の再証明

Legendre's reflection formula 

$$
\Gamma(s)\Gamma(s+1/2) = 2^{1-2s} \sqrt{\pi}\;\Gamma(2s)
$$

で $s=t/2$ とおいて得られる

$$
\Gamma(t) = \frac{2^{t-1}}{\sqrt{\pi}}\Gamma\left(\frac{t}{2}\right)\Gamma\left(\frac{t+1}{2}\right)
$$

を用いた Euler's reflection formula の証明を紹介しよう. 以下の方法は

* E. Artin, <a href="https://www.google.co.jp/search?q=Artin+The+Gamma+Function">The Gamma Function</a>

の第4節に書いてある.

函数 $f(t)$ を

$$
f(t) = \Gamma(t)\Gamma(1-t)\frac{\sin(\pi t)}{\pi}
$$

と定める. $0<t<1$ において $f(t)$ が正値 $C^2$ 級(実際には解析的)であることがわかる. さらに $\Gamma(t)=\Gamma(1+t)/t$, $\Gamma(1-t)=\Gamma(2-t)/(1-t)$, $\sin(\pi t)=\sin(\pi(1-t))$ より,

$$
f(t) = 
\Gamma(1+t)\Gamma(1-t)\frac{\sin(\pi t)}{\pi t} =
\Gamma(t)\Gamma(2-t)\frac{\sin(\pi(1-t))}{\pi(1-t)}
$$

なので, この前者の表示より $t=0$ の十分近くで, 後者の表示より $t=1$ の近くで, $f(t)$ は正値でかつ $C^2$ 級(実際には解析的)であることがわかる. 特に, 前者より $f(0)=1$ であることがわかり, 後者より $f(1)=1$ であることがわかる. 

Legendre's duplication formula と

$$
\ds\sin(\pi t)=
2\sin\frac{\pi t}{2}\,\cos\frac{\pi t}{2}=
2\sin\frac{\pi t}{2}\,\sin\frac{\pi(t+1)}{2}
$$

を使うと, 

$$
\begin{aligned}
f(t) &= 
\frac{2^{t-1}}{\sqrt{\pi}}\Gamma\left(\frac{t}{2}\right)\Gamma\left(\frac{t+1}{2}\right)
\frac{2^{-t}}{\sqrt{\pi}}\Gamma\left(\frac{1-t}{2}\right)\Gamma\left(\frac{2-t}{2}\right)
\frac{2}{\pi}\sin\frac{\pi t}{2}\,\sin\frac{\pi(t+1)}{2}
\\ &= 
\Gamma\left(\frac{t}{2}\right)\Gamma\left(\frac{2-t}{2}\right)\frac{\sin(\pi t/2)}{\pi}
\Gamma\left(\frac{t+1}{2}\right)\Gamma\left(\frac{1-t}{2}\right)\frac{\sin(\pi (t+1)/2)}{\pi}
\\ &= 
\Gamma\left(\frac{t}{2}\right)\Gamma\left(1-\frac{t}{2}\right)\frac{\sin(\pi t/2)}{\pi}
\Gamma\left(\frac{t+1}{2}\right)\Gamma\left(1-\frac{t+1}{2}\right)\frac{\sin(\pi(t+1)/2)}{\pi} 
\\ &=
f\left(\frac{t}{2}\right)f\left(\frac{t+1}{2}\right).
\end{aligned}
$$

これより, $g(t)=\log f(t)$ とおくと, $g(0)=g(1)=\log 1=0$ でかつ,

$$
g(t) = g\left(\frac{t}{2}\right) + g\left(\frac{t+1}{2}\right).
$$

ゆえに, $0\leqq t\leqq 1$ において, 

$$
\begin{aligned}
&
g''(t) = \frac{1}{4}\left(g''\left(\frac{t}{2}\right) + g''\left(\frac{t+1}{2}\right)\right), 
\\ &
|g''(t)| \leqq \frac{1}{4}\left(\left|g''\left(\frac{t}{2}\right)\right| + \left|g''\left(\frac{t+1}{2}\right)\right|\right)
\end{aligned}
$$

となるので, $\ds M=\max_{0\leqq t\leqq 1}|g''(t)|$ とおくと,

$$
M \leqq \frac{1}{4}(M+M)=\frac{M}{2}
$$

となることがわかる. ゆえに $M=0$ である. これで $0\leqq t\leqq 1$ において $g''(t)=0$ となることがわかった. $g(0)=g(1)=0$ より, 恒等的に $g(t)=0$ となる. これで $f(t)=1$ すなわち 

$$
\Gamma(t)\Gamma(1-t) = \frac{\pi}{\sin(\pi t)}
$$

が成立することが示された. この公式は **Euler's reflection formula** と呼ばれているのであった.

#### sinの無限積表示の再証明

Euler's reflection formulaとガンマ函数の無限積表示

$$
\begin{aligned}
&
\Gamma(s) = 
\lim_{n\to\infty}\frac{n^s n!}{s(1+s)(2+s)\cdots(n+s)} =
\lim_{n\to\infty}\frac{n^s}{s\left(1+s\right)\left(1+\frac{s}{2}\right)\cdots\left(1+\frac{s}{n}\right)},
\\ &
\Gamma(1-s) = 
\lim_{n\to\infty}\frac{n^{1-s} n!}{(1-s)(2-s)\cdots(n+1-s)} =
\lim_{n\to\infty}\frac{n^{-s}}{\left(1-s\right)\left(1-\frac{s}{2}\right)\cdots\left(1-\frac{s}{n}\right)\left(1+\frac{1-s}{n}\right)},
\end{aligned}
$$

より,

$$
\frac{\sin(\pi t)}{\pi} =
\frac{1}{\Gamma(t)\Gamma(1-t)} =
\lim_{n\to\infty}s\prod_{k=1}^n\left(\left(1+\frac{s}{k}\right)\left(1-\frac{s}{k}\right)\right) =
s\prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right).
$$

**まとめ:** ガンマ函数とベータ函数をEuler型の積分表示式に基いて, ガンマ函数の函数等式やガンマ函数とベータ函数の関係を証明できる. $\ds\lim_{n\to\infty}n^s B(s,n+1)=\Gamma(s)$ からガンマ函数の無限積表示が得られ, 同じ積分の2通りの計算 $\ds \int_{-1}^1 (1-x^2)^{s-1}\,dx = 2^{2s-1}B(s,s) = B(1/2,s)$ から Legendre's duplication formula が得られる. これだけの準備のもとで Euler's reflection formula が得られ, sin の無限積表示も得られるというのが以上の筋道である. E. Artinの有名なガンマ函数の本ではガンマ函数は函数等式と対数凸性で定数倍を除いて一意に特徴付けられるという結果を全般的に用いてガンマ函数に関する様々な公式を証明している. そのような対数凸性に頼らなくても, Euler型積分表示式だけを使っても主要な公式を示せることが以上の議論を見ればわかる. $\QED$


ガンマ函数についてはさらに

* 黒木玄, <a href="https://genkuroki.github.io/documents/20160501StirlingFormula.pdf">ガンマ分布の中心極限定理とStirlingの公式</a>

の第8節を参照せよ.

<!-- #region -->
### Lerchの定理とゼータ正規化積

#### Lerchの定理 (Hurwitzのゼータ函数とガンマ函数の関係)

**Lerchの定理:** Hurwitzのゼータ函数 $\zeta(s,x)$ からガンマ函数が

$$
\zeta_s(0,x) = \log\frac{\Gamma(x)}{\sqrt{2\pi}}, \qquad
\Gamma(x) = \sqrt{2\pi}\;\exp(\zeta_s(0,x))
$$

によって得られる. ここで $\zeta_s(s,x)$ は $\zeta(s,x)$ の $s$ に関する偏導函数である.

**証明:** $F(x)=\zeta_s(0,x)-\log\Gamma(x)$ とおく. $F(x)=-\log\sqrt{2\pi}$ であることを示せば十分である. 

(1) $(\zeta_s(0,x))'' = (\log\Gamma(x))''$ を示そう. ここで $'$ は $x$ による微分を表わす. まずHurwitzのゼータ函数

$$
\zeta(s,x) = \sum_{k=0}^\infty \frac{1}{(x+k)^s}
$$

については

$$
\zeta_x(s,x) = -s\zeta(s+1,x)
$$

が成立しているので, 

$$
\zeta_{xx}(s,x) = s(s+1)\zeta(s+2,x).
$$

これより, 

$$
(\zeta_s(0,x))'' = \zeta_{xxs}(0,x) = \zeta(2,x).
$$

一方, ガンマ函数

$$
\Gamma(x) = \lim_{\to\infty}\frac{n!\,n^x}{x(x+1)\cdots(x+n)}
$$

については,

$$
\begin{aligned}
&
\log\Gamma(x) =
\lim_{n\to\infty}\left(
\log n! + x\log n - \log x - \log(x+1) - \cdots - \log(x+n)
\right),
\\ &
(\log\Gamma(x))' =
\lim_{n\to\infty}\left(
\log n - \frac{1}{x} - \frac{1}{x+1} - \cdots - \frac{1}{x+n}
\right),
\\ &
(\log\Gamma(x))'' =
\lim_{n\to\infty}\left(
\frac{1}{x^2} + \frac{1}{(x+1)^2} + \cdots + \frac{1}{(x+n)^2}
\right) = \zeta(2,x).
\end{aligned}
$$

これで $(\zeta_s(0,x))'' = (\log\Gamma(x))''$ が示された.

(2) 上の結果より, $F(x)=\zeta_s(0,x)-\log\Gamma(x)$ は $x$ の一次函数である.

(3) $\zeta_s(0,x)$ と $\log\Gamma(x)$ がどちらも同一の函数等式 $f(x+1)=f(x)+\log x$ を満たすことを示そう. 

$$
\begin{aligned}
&
\zeta(s,x+1) = \zeta(s,x) - \frac{1}{x^s},
\qquad\therefore\quad
\zeta_s(0,x+1) = \zeta_s(0,x) + \log x.
\\ &
\log\Gamma(x+1) = \log(x\Gamma(x)) = \log\Gamma(x) + \log x.
\end{aligned}
$$

(4) $F(x)=\zeta_s(0,x)-\log\Gamma(x)$ は $x$ の一次函数だったので, 上の結果より $F(x)$ は定数になる.

(5) $\zeta_s(0,1/2)=-\log\sqrt{2}$ を示そう.

$$
\begin{aligned}
\zeta(s) - 2^{-s}\zeta(s) &=
\left(\frac{1}{1^s}+\frac{1}{2^s}+\frac{1}{3^s}+\frac{1}{4^s}+\cdots\right) -
\left(\frac{1}{2^s}+\frac{1}{4^s}+\cdots\right) 
\\ &=
\frac{1}{1^s}+\frac{1}{3^s}+\cdots =
\sum_{k=0}^\infty\frac{1}{(2k+1)^s}
\end{aligned}
$$

なので

$$
\begin{aligned}
&
\zeta(s,1/2) = \sum_{k=0}^\infty\frac{1}{(k+1/2)^s} =
2^s\sum_{k=0}^\infty\frac{1}{(2k+1)^s} = 
2^s(\zeta(s) - 2^{-s}\zeta(s)) =
(2^s-1)\zeta(s),
\\ &\therefore\quad
\zeta_s(0,1/2) = \zeta(0)\log 2 = -\frac{1}{2}\log 2 = -\log\sqrt{2}.
\end{aligned}
$$

(6) $\log\Gamma(1/2)=\log\sqrt{\pi}$ なので, 上の結果より, $F(x)=-\log\sqrt{2\pi}$ であることがわかる. $\QED$


#### digamma, trigamma, polygamma 函数

上のLerchの定理の証明中に出て来た**対数ガンマ函数** $\log\Gamma(x)$ の導函数達を**polygamma函数**と呼ぶ.  例えば, 以下の $\psi(x)$, $\psi'(x)$ はそれぞれ**digamma函数**, **trigamma函数**と呼ばれる:

$$
\begin{aligned}
&
\log\Gamma(x) =
\lim_{n\to\infty}\left(
\log n! + x\log n - \log x - \log(x+1) - \cdots - \log(x+n)
\right),
\\ &
\psi(x) = (\log\Gamma(x))' =
\lim_{n\to\infty}\left(
\log n - \frac{1}{x} - \frac{1}{x+1} - \cdots - \frac{1}{x+n}
\right),
\\ &
\psi'(x) = (\log\Gamma(x))'' =
\lim_{n\to\infty}\left(
\frac{1}{x^2} + \frac{1}{(x+1)^2} + \cdots + \frac{1}{(x+n)^2}
\right) = \zeta(2,x).
\end{aligned}
$$

これらの式を見ればわかるように, $m\geqq 3$ に対するtrigamma函数以降の $m$-th polygamma函数 $\psi^{(m-2)}(x)=(\log\Gamma(x))^{(m-1)}$ はHurwitzのゼータ函数の特別な場合 $\zeta(m-1, x)$ に一致する.

調和級数 $1+1/2+\cdots+1/n$ と $\log n$ の差が $n\to\infty$ で収束し, その収束先はEuler定数と呼ばれ, $\gamma=0.5772\cdots$ と書かれる. その事実はdigamma函数を使うと

$$
-\psi(1) = \gamma = 0.5772\cdots
$$

と表わされる.  digamma函数の $-1$ 倍 $-\psi(x)$ は一般化された調和級数 $1/x+1/(x+1)+\cdots+1/(x+n)$ と $\log n$ の差の極限に等しいので, digamma函数の $-1$ 倍はEuler定数の一般化であるとも考えられる.

digamma函数 $\psi(\alpha)$ は

$$
\psi(\alpha) = \frac{\Gamma'(\alpha)}{\Gamma(\alpha)} = 
\frac{1}{\Gamma(\alpha)}\int_0^\infty e^{-x} x^{\alpha-1}\log x\,dx
$$

とも書ける. これはパラメーター ($\alpha$, $\theta=1$) のガンマ分布に従う確率変数 $X$ の対数 $\log X$ の期待値に等しい.  統計学におけるガンマ分布の取り扱いではdigamma函数などが必要になる.  Lerchの定理の証明を通して, 対数ガンマ函数の導函数の様子がよくわかるという感覚を身に付けておくと, その感覚はガンマ分布の統計学でも役に立つことになる.


#### ゼータ正規化積 

数列 $a_n$ に対して,

$$
f(s) = \sum_{n=1}^N \frac{1}{a_n^s}
$$

とおくとき, 

$$
f'(0) = -\sum_{n=1}^N \log a_n
$$

なので, 

$$
\exp(-f'(0)) = \prod_{n=1}^N a_n
$$

が成立している. もしも $N=\infty$ のときの $\ds\prod_{n=1}^\infty a_n$ が発散していても, $\ds f(s)=\sum_{n=1}^\infty \frac{1}{a_n^s}$ の解析接続によって, 左辺の $\exp(-f'(0))$ はwell-definedになる可能性がある. そのとき, $\exp(-f'(0))$ を

$$
\exp(-f'(0)) = \PROD_{n=1}^\infty a_n
$$

と書き, $a_n$ 達の**ゼータ正規化積**と呼ぶ. 

例えば $x,x+1,x+2,x+3,\ldots$ のゼータ正規化積はLerchの定理より, $\ds\frac{\sqrt{2\pi}}{\Gamma(x)}$ になる. 特に $x=1$ のときの $1,2,3,4,\ldots$ のゼータ正規化積は $\sqrt{2\pi}$ になる:

$$
"\! 1\times 2\times 3\times 4\times\cdots \!" \,= 
\PROD_{n=1}^\infty n = \exp(-\zeta'(0)) = \sqrt{2\pi}.
$$

これは

$$
"\! 1+2+3+4+\cdots \!"\, = \zeta(-1) = -\frac{1}{12}
$$

の積バージョンである. 
<!-- #endregion -->

#### Lerchの定理を用いたBinetの公式の証明

上でLerchの定理を示した.

**Lerchの定理:** $\ds \zeta_s(0,x) = \log\frac{\Gamma(x)}{\sqrt{2\pi}}$. $\QED$

これを用いて次のBinetの公式を示そう. 

**Binetの公式:** $x>0$ のとき, 

$$
\log\Gamma(x+1) = x\log x - x +\frac{1}{2}\log x + \log\sqrt{2\pi} + \varphi(x).
$$

ここで

$$
\varphi(x) = \int_0^\infty \left(\frac{1}{e^t-1}-\frac{1}{t}+\frac{1}{2}\right) e^{-xt}t^{-1}\,dt.
\qquad \QED
$$

**証明:** $x>0$ と仮定する.

$\real s > 1$ のとき,  $a>0$ について $\ds \frac{1}{a^s} =\frac{1}{\Gamma(s)}\int_0^\infty e^{-at}t^{s-1}\,dt$ より,

$$
\begin{aligned}
\zeta(s,x) &= \sum_{k=0}^\infty \frac{1}{(x+k)^s} =
\frac{1}{\Gamma(s)}\sum_{k=0}^\infty \int_0^\infty e^{-(x+k)t} t^{s-1}\,dt =
\frac{1}{\Gamma(s)}\int_0^\infty \frac{e^{-xt}}{1-e^{-t}} t^{s-1}\,dt.
\end{aligned}
$$

ゆえに

$$
\zeta(s,x+1) = \frac{1}{\Gamma(s)}\int_0^\infty \frac{e^{-xt}}{e^t-1}t^{s-1}\,dt.
$$

$F(s,x)$ を

$$
F(s,x) = \frac{1}{\Gamma(s)} \int_0^\infty \left(\frac{1}{e^t-1}-\frac{1}{t}+\frac{1}{2}\right) e^{-xt} t^{s-1}\,dt
$$

と定めると, 

$$
\frac{1}{e^t-1}-\frac{1}{t}+\frac{1}{2} = O(t) \quad (t\to 0)
$$

なので, $F(s,x)$ の定義式は $\real s > -1$ で収束している. $\real s > 1$ のとき

$$
\begin{aligned}
&
\frac{1}{\Gamma(s)}\int_0^\infty \frac{1}{t} e^{-xt} t^{s-1}\,dt = 
\frac{\Gamma(s-1)}{\Gamma(s)}\frac{1}{x^{s-1}} =
\frac{x^{1-s}}{s-1}, 
\\ &
\frac{1}{\Gamma(s)}\int_0^\infty \left(-\frac{1}{2}\right) e^{-xt} t^{s-1}\,dt = -\frac{x^{-s}}{2}
\end{aligned}
$$

であるから, 

$$
\zeta(s,x+1) = \frac{x^{1-s}}{s-1} - \frac{x^{-s}}{2} + F(s,x).
$$

この等式は $\real s > -1$ でも解析接続によって成立している. そこで以下では $\real s > -1$ と仮定する.

$\ds\lim_{s\to 0}\frac{1}{\Gamma(s)} = 0$ より, $F(0,x)=0$ となるので, 

$$
\begin{aligned}
&
\left.\frac{\d}{\d s}\right|_{s=0} \frac{x^{1-s}}{s-1} =
\left.\left(\frac{-x^{1-s}\log x}{s-1} - \frac{x^{1-s}}{(s-1)^2}\right)\right|_{s=0} =
x\log x - x,
\\ &
\left.\frac{\d}{\d s}\right|_{s=0} \left(-\frac{x^{-s}}{2}\right) = 
\left.\left(\frac{x^{-s}\log x}{2}\right)\right|_{s=0} =
\frac{1}{2}\log x,
\\ &
\left.\frac{\d}{\d s}\right|_{s=0}F(s,x) = 
\lim_{s\to 0}\frac{F(s,x)}{s} =
\lim_{s\to 0}\frac{1}{\Gamma(s+1)}
\int_0^\infty \left(\frac{1}{e^t-1}-\frac{1}{t}+\frac{1}{2}\right) e^{-xt} t^{s-1}\,dt 
\\ & \qquad =
\int_0^\infty \left(\frac{1}{e^t-1}-\frac{1}{t}+\frac{1}{2}\right) e^{-xt}t^{-1}\,dt =
\varphi(x).
\end{aligned}
$$

ゆえに,

$$
\zeta_s(0, x+1) = x\log x - x +\frac{1}{2}\log x + \varphi(x).
$$

したがって, Lerchの定理より,

$$
\log\Gamma(x+1) = \zeta_s(0,x+1) + \log\sqrt{2\pi} =
x\log x - x +\frac{1}{2}\log x + \log\sqrt{2\pi} + \varphi(x).
$$

これで, Lerchの定理からBinetの公式が導かれることがわかった. $\QED$

Binetの公式は本質的にHurwitzのゼータ函数 $\zeta(s,x)$ の $s$ に関する偏微分係数 $\zeta_s(0,x)$ に関する公式であるとみなせる. Binetの公式の右辺の積分表示はHurwitzのゼータ函数の積分表示式から得られる.


## Stirlingの公式とLaplaceの方法

一般に数列 $a_n,b_n$ について

$$
\lim_{n\to\infty}\frac{a_n}{b_n} = 1
$$

が成立するとき,

$$
a_n\sim b_n
$$

と書くことにする. 


### Stirlingの公式

**Stirlingの(近似)公式:** $n\to\infty$ のとき,

$$
n!\sim n^n e^{-n} \sqrt{2\pi n}.
$$

さらに, 両辺の対数を取ることによって, $n\to\infty$ のとき,

$$
\log n! = n\log n - n + \frac{1}{2}\log n + \log\sqrt{2\pi} + o(1).
$$


Stirlingの公式の「物理学的」もしくは「情報理論的」な応用については

* 黒木玄, <a href="https://genkuroki.github.io/documents/20160616KullbackLeibler.pdf">Kullback-Leibler情報量とSanovの定理</a>

の第1節を参照せよ.


**Stirlingの公式の証明:**

$$
n! = \Gamma(n+1) = \int_0^\infty e^{-x} x^n\,dx
$$

で $x = n+\sqrt{n}\;y = n(1+y/\sqrt{n})$ と置換すると, 

$$
n! = 
n^n e^{-n} \sqrt{n} \int_{-\sqrt{n}}^\infty e^{-\sqrt{n}\;y}\;\left(1+\frac{y}{\sqrt{n}}\right)^n\,dy =
n^n e^{-n} \sqrt{n} \int_{-\sqrt{n}}^\infty \;f_n(y)\,dy.
$$

ここで, 被積分函数を $f_n(y)$ と書いた. そのとき $n\to\infty$ で

$$
\begin{aligned}
\log f_n(y) &= -\sqrt{n}\;y + n\log\left(1+\frac{y}{\sqrt{n}}\right) =
-\sqrt{n}\;y + n\left(\frac{y}{\sqrt{n}} - \frac{y^2}{2n} + O\left(\frac{1}{n\sqrt{n}}\right)\right) 
\\ &=
-\frac{y^2}{2} + O\left(\frac{1}{\sqrt{n}}\right) \to -\frac{y^2}{2}.
\end{aligned}
$$

すなわち $f_n(y)\to e^{-y^2/2}$ となる. ゆえに

$$
\frac{n!}{n^n e^{-n} \sqrt{2\pi n}} =
\frac{1}{\sqrt{2\pi}}\int_{-\sqrt{n}}^\infty\;f_n(y)\,dy
\to \frac{1}{\sqrt{2\pi}}\int_{-\infty}^\infty e^{-y^2/2}\,dy = 1.
$$

最後の等号でGauss積分の公式 $\int_{-\infty}^\infty e^{-y^2/a}\,dy=\sqrt{a\pi}$ を用いた. $\QED$


**Stirlingの公式の証明の解説:** 上の証明のポイントは $x=n+\sqrt{n}\;y$ という積分変数変換である. この変数変換の「正体」は $\Gamma(n+1)=\int_0^\infty e^{-x} x^n\,dx$ の被積分函数 $f(x)=e^{-x}x^n$ のグラフを描いてみれば見当がつく.

$g(x)=\log f(x)=n\log x - x$ の導函数は $g'(x)=n/x-1$ は $x$ について単調減少であり, $x=n$ で $0$ になる. ゆえに $g(x)=\log f(x)$ は $x=n$ で最大になる.  そこで $x=n$ における $g(x)=\log f(x)$ のTaylor展開を求めてみよう. $g''(x)=-n/x^2$, $g'''(x)=2n/x^3$ なので, $g(n)=n\log n - n$, $g'(n)=0$, $g''(n)=-1/n$, $g'''(n)=2/n^2$ なので,

$$
g(x) = n \log n - n -\frac{(x-n)^2}{2n} + \frac{(x-n)^3}{3\,n^2} + \cdots
$$

これの2次の項が $-y^2/2$ になるような変数変換がちょうど $x=n+\sqrt{n}\;y$ になっている. これが上の証明で用いた変数変換の「正体」である. $\QED$

```julia
# y = f(x) = e^{-x} x^n / (n^n * e^{-n}) のグラフは n が大きなとき,
# Gauss近似 y = e^{-(x-n)^2/(2n)} のグラフにほぼ一致する.

f(n,x) = e^(-x + n*log(x) - (n*log(n) - n))
g(n,x) = e^(-(x-n)^2/(2n))
PP = []
for n in [10, 30, 100, 300]
    x = 0:2.5n/400:2.5n
    n ≤ 20 && (x = 0:3n/400:3n)
    P = plot()
    plot!(title="n = $n", titlefontsize=9)
    plot!(x, g.(n,x), label="Gaussian")
    plot!(x, f.(n,x), label="approx.", ls=:dash)
    push!(PP, P)
end

plot(PP[1:2]..., size=(700, 200))
```

```julia
plot(PP[3:4]..., size=(700, 200))
```

**注意(ガンマ函数のStirlingの近似公式):** 上の証明で $n$ が整数であることは使っていない. ゆえに正の実数 $s$ について

$$
\Gamma(s+1) \sim s^s e^{-s} \sqrt{2\pi s} \quad (s\to\infty)
$$

が証明されている. これの両辺を $s$ で割ると,

$$
\Gamma(s) \sim s^s e^{-s} s^{-1/2} \sqrt{2\pi} \quad (s\to\infty)
$$

が得られる. これらをも**Stirlingの近似公式**と呼ぶ. $\QED$


Stirlingの公式の重要な応用については

* 黒木玄, <a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/11%20Kullback-Leibler%20information.ipynb">11 Kullback-Leibler情報量</a>

も参照せよ. 「Stirlingの公式」とその応用としての「KL情報量に関するSanovの定理」についてはできるだけ早く理解しておいた方がよい. $\QED$


**問題:** $n=1,2,\ldots,10$ について Stirling の公式の相対誤差

$$
\frac{n^n e^{-n} \sqrt{2\pi n}}{n!}-1
$$

を求めよ.

**解答例:** 以下のセルを参照せよ. $n=5$ で相対誤差は2%を切っている. $\QED$

```julia
f(n) = factorial(n)
g(n) = n^n * exp(-n) * √(2π*n)
[(n, f(n), g(n), g(n)/f(n)-1) for n in 1:10]
```

**参考:** 上の計算を見れば, $n^n e^{-n} \sqrt{2\pi n}$ は $n!$ よりも微小に小さいことがわかる. その分を補正したより精密な近似式

$$
n! = n^n e^{-n} \sqrt{2\pi n}\left(1+\frac{1}{12n}+O\left(\frac{1}{n^2}\right)\right)
$$

が成立している. (実際には $O(1/n^2)$ の部分についてもっと詳しいことがわかる.)

$1/(12n)$ で補正した近似式の相対誤差は $n=1$ ですでに0.1%程度と非常に小さくなる. 次のセルを見よ. $\QED$

```julia
f(n) = factorial(n)
g1(n) = n^n * exp(-n) * √(2π*n) * (1+1/(12n))
[(n, f(n), g1(n), g1(n)/f(n)-1) for n in 1:10]
```

### Wallisの公式のStirlingの公式を使った証明

**問題(Wallisの公式):** Stirlingの公式を用いて次を示せ:

$$
\frac{1}{2^{2n}}\binom{2n}{n} \sim \frac{1}{\sqrt{\pi n}}.
$$

**解答例:**
$$
\frac{1}{2^{2n}}\binom{2n}{n} = \frac{(2n)!}{2^{2n}(n!)^2}
\sim \frac{(2n)^{2n}e^{-2n}\sqrt{4\pi n}}{2^{2n}n^{2n}e^{-2n}2\pi n} = \frac{1}{\sqrt{\pi n}}.
\qquad \QED
$$

**注意:** この形のWallisの公式は1次元の単純ランダムウォークの逆正弦法則に関係している.

* 黒木玄, <a href="https://genkuroki.github.io/documents/#2016-11-02">単純ランダムウォークの逆正弦法則</a> (<a href="https://genkuroki.github.io/documents/20161102ArcSineLawForSimpleRandomWalks.pdf">手描きのノートのPDF</a>)

を参照せよ. 特に手描きのノートのPDFファイルの12頁以降にまとまった解説がある. 1次元の単純ランダムウォークの場合には高校数学レベルの組み合わせ論的な議論とWallisの公式から逆正弦法則を導くことができる. 1次元の一般ランダムウォークの場合にはTauber型定理を使ってWallisの公式に対応する漸近挙動を証明することになる. $\QED$

```julia
# Wallisの公式より
#
# [ 2^{2n} (n!)^2 / ((2n)! √n) ]^2 ---→ π
#
# 以下はこれの数値的確認
#
# log n! を log lgamma(n+1) で計算している. ここで lgamma(x) = log(Γ(x)).
# lgamma(x) は対数ガンマ函数を巨大な x についても計算してくれる.

f(n) = exp((2n)*log(typeof(n)(2)) + 2lgamma(n+1) - lgamma(2n+1) - log(n)/2)^2
Wallis_pi = f(big"10.0"^40)
Exact__pi = big(π)
@show Wallis_pi
@show Exact__pi
Wallis_pi - Exact__pi
```

### Gauss's multiplication formula

**問題(Gauss's multiplication formula):** 次を示せ: 正の整数 $n$ に対して,

$$
\Gamma(s)\Gamma\left(s+\frac{1}{n}\right)\cdots\Gamma\left(s+\frac{n-1}{n}\right) =
n^{1/2-ns}(2\pi)^{(n-1)/2}\Gamma(ns).
$$

**解答例:** 函数 $f(s)$ を次のように定める:

$$
f(s) = \frac{\Gamma(s)\Gamma\left(s+\frac{1}{n}\right)\cdots\Gamma\left(s+\frac{n-1}{n}\right)}{n^{-ns}\Gamma(ns)}
$$

$f(s)=n^{1/2}(2\pi)^{(n-1)/2}$ を示せばよい.

ガンマ函数の函数等式だけを使って, $f(s+1)=f(s)$ を示せる:

$$
f(s+1) = 
f(s)\frac
{s\left(s+\frac{1}{n}\right)\cdots\left(s+\frac{n-1}{n}\right)}
{n^{-n}(ns+n-1)\cdots(ns+1)(ns)} = f(s).
$$

上で証明されているStirlingの近似公式

$$
\Gamma(s) \sim s^s e^{-s} s^{-1/2}\sqrt{2\pi} \quad (s\to\infty)
$$

を使って, $s\to\infty$ のときの $f(s)$ の極限を求めよう. $s\to\infty$ のとき, $\ds \left(1+\frac{a}{s}\right)^s\to e^a$ なので, $s\to\infty$ において, 

$$
\begin{aligned}
\Gamma(s+a) &\sim (s+a)^{s+a-1/2} e^{-s-a} \sqrt{2\pi} 
\\ &=
s^{s+a-1/2}e^{-s}\sqrt{2\pi}\;\left(1+\frac{a}{s}\right)^{s+a-1/2} e^{-a} 
\\ &\sim
s^{s+a-1/2}e^{-s}\sqrt{2\pi}.
\end{aligned}
$$

となる. ゆえに, $\frac{1}{n}+\frac{2}{n}+\cdots+\frac{n-1}{n}=\frac{n-1}{2}$ なので, $s\to\infty$ において, 

$$
\begin{aligned}
&
\Gamma(s) \sim
s^{s-1/2} e^{-s}\sqrt{2\pi},
\\ &
\Gamma\left(s+\frac{1}{n}\right)\sim
s^{s+1/n-1/2} e^{-s} \sqrt{2\pi},
\\ &
\qquad\qquad\cdots\cdots\cdots\cdots\cdots
\\ &
\Gamma\left(s+\frac{n-1}{n}\right)\sim
s^{s+(n-1)/n-1/2} e^{-s} \sqrt{2\pi}
\\ &
\therefore\quad
\Gamma(s)\Gamma\left(s+\frac{1}{n}\right)\cdots\Gamma\left(s+\frac{n-1}{n}\right)\sim
s^{ns-1/2}e^{-ns}(2\pi)^{n/2}
\\ &
n^{-ns}\Gamma(ns)\sim
n^{-ns}(ns)^{ns-1/2}e^{-ns}\sqrt{2\pi} =
n^{-1/2}s^{ns-1/2}e^{-ns}(2\pi)^{1/2}
\end{aligned}
$$

となり, 

$$
f(s)\sim\frac{s^{ns-1/2}e^{-ns}(2\pi)^{n/2}}{n^{-1/2}s^{ns-1/2}e^{-ns}(2\pi)^{1/2}}=
n^{1/2}(2\pi)^{(n-1)/2}.
$$

ゆえに整数 $N$ について, $f(s+N)=f(s)$ なので, $N\to\infty$ のとき $f(s)=f(s+N)\to n^{1/2}(2\pi)^{(n-1)/2}$ となる. これで $f(s)=2^{1/2}(2\pi)^{(n-1)/2}$ が示された. $\QED$


**問題:** Gauss's multiplication formula の $n=2$ の場合である Legendre's duplication formula は定積分の計算だけで証明できるのであった. 上の解答例は本質的にStirlingの近似公式を使っている. Gauss's multiplication formula にも定積分の計算だけで証明する方法がないだろうか. 以下の方針で Gauss's multiplication formula を証明せよ. ただし, (3)の証明には Euler's reflection formula は使ってよいことにする. 

$t>0$ に対する $n-1$ 重積分 $I(t)$ を次のように定める:

$$
I(t) = \int_0^\infty\cdots\int_0^\infty 
e^{-(t^n/(x_2\cdots x_n)+x_2+\cdots+x_n)}
x_2^{-(n-1)/n}x_3^{-(n-2)/n}\cdots x_n^{-1/n} \,dx_2\cdots dx_n.
$$

以下を示せ:

(1) $\ds I(t) = \Gamma\left(\frac{1}{n}\right)\Gamma\left(\frac{2}{n}\right)\cdots\Gamma\left(\frac{n-1}{n}\right)e^{-nt}$.

(2) $\ds \Gamma(s)\Gamma\left(s+\frac{1}{n}\right)\cdots\Gamma\left(s+\frac{n-1}{n}\right) = 
n^{1-ns}I(0)\Gamma(ns) =
n^{1-ns}\Gamma\left(\frac{1}{n}\right)\Gamma\left(\frac{2}{n}\right)\cdots\Gamma\left(\frac{n-1}{n}\right)\Gamma(ns)$.

(3) $\ds I(0) = 
\Gamma\left(\frac{1}{n}\right)\Gamma\left(\frac{2}{n}\right)\cdots\Gamma\left(\frac{n-1}{n}\right) =
(2\pi)^{(n-1)/2} n^{-1/2}$.


**注意:** (1), (2) の方針の証明は

* Andrews, G.E., Askey,R., and Roy, R. Special functions. Encyclopedia of Mathematics and its Applications, Vol. 71, Cambridge University Press, 1999, 2000, 681 pages.

のpp.24-25で解説されている. その方法は

* Liouville, J. Sur un théorème relatif à l’intégrale eulérienne de seconde espèce. Journal de mathématiques pures et appliquées 1re série, tome 20 (1855), p. 157-160. <a href="http://sites.mathdoc.fr/JMPA/PDF/JMPA_1855_1_20_A15_0.pdf">PDF</a>

の方法の再構成ということらしい.


**解答例:** (1) $t=0$ のとき, $I(0)$ の積分は変数分離形になって, 

$$
I(0) = \Gamma\left(\frac{1}{n}\right)\Gamma\left(\frac{2}{n}\right)\cdots\Gamma\left(\frac{n-1}{n}\right)
$$

となることがすぐにわかる. $I(t)$ を $t$ で微分して, $\ds x_2 = \frac{t^n}{x_3\cdots x_n x_1}$ によって積分変数 $x_2$ を積分変数 $x_1$ に変換すると, 

$$
\begin{aligned}
I'(t) &=
\int_0^\infty\cdots\int_0^\infty 
e^{-(t^n/(x_2\cdots x_n)+x_2+\cdots+x_n)}
\frac{-nt^{n-1}}{x_2\cdots x_n}
x_2^{-(n-1)/n}x_3^{-(n-2)/n}\cdots x_n^{-1/n} \,dx_2\cdots dx_n
\\ &=
-n\int_0^\infty\cdots\int_0^\infty 
e^{-(t^n/(x_2\cdots x_n)+x_2+\cdots+x_n)}
t^{n-1}
x_2^{-(n-1)/n-1}x_3^{-(n-2)/n-1}\cdots x_n^{-1/n-1} \,dx_2\cdots dx_n
\\ &=
-n\int_0^\infty\cdots\int_0^\infty 
e^{-(x_1+t^n/(x_3\cdots x_n x_1)+x_3+\cdots+x_n)}
\\ & \qquad\qquad\quad\times
t^{n-1}
\left(\frac{t^n}{x_3\cdots x_n x_1}\right)^{-(2n-1)/n}
x_3^{-(2n-2)/n}\cdots x_n^{-(n+1)/n} \frac{t^n}{x_3\cdots x_n x_1^2}\,dx_3\cdots dx_n\,dx_1
\\ &=
-n\int_0^\infty\cdots\int_0^\infty 
e^{-(x_1+t^n/(x_3\cdots x_n x_1)+x_3+\cdots+x_n)}
x_3^{-(n-1)/n}\cdots x_n^{-2/n} x_1^{-1/n}\,dx_3\cdots dx_n\,dx_1
\\ &=
-n I(t)
\end{aligned}
$$

ゆえに $I(t)=I(0)e^{-nt}$.  これで(1)が示された.


(2) 左辺をLHSと書き, $\ds x_1=\frac{t^n}{x_2\cdots x_n}$ とおくと, 

$$
\begin{aligned}
\text{LHS} &=
\int_0^\infty\cdots\int_0^\infty e^{-(x_1+\cdots+x_n)} x_1^{s-1}x_2^{s-(n-1)/n}\cdots x_n^{s-1/n}\,dx_1\cdots dx_n
\\ &=
\int_0^\infty\cdots\int_0^\infty e^{-(t^n/(x_2\cdots x_n)+x_2\cdots+x_n)}
\left(\frac{t^n}{x_2\cdots x_n}\right)^{s-1}
x_2^{s-(n-1)/n}\cdots x_n^{s-1/n}
\frac{nt^{n-1}}{x_2\cdots x_n}
\,dt\,dx_2\cdots dx_n
\\ &=
n\int_0^\infty\cdots\int_0^\infty e^{-(t^n/(x_2\cdots x_n)+x_2\cdots+x_n)}
x_2^{-(n-1)/n}\cdots x_n^{-1/n} t^{ns-1}
\,dx_2\cdots dx_n\,dt
\\ &=
n\int_0^\infty I(t) t^{ns-1}\,dt =
nI(0)\int_0^\infty e^{-nt} t^{ns-1}\,dt =
n^{1-ns}I(0)\Gamma(ns).
\end{aligned}
$$

これで(2)が示された.


(3) $I(0)=(2\pi)^{(n-1)/2}n^{-1/2}$ を示したい. そのためには $I(0)^2 = (2\pi)^{n-1} n^{-1}$ を示せばよい. Euler's reflection formula より, $\ds \Gamma\left(\frac{k}{n}\right)\Gamma\left(\frac{n-k}{n}\right) = \frac{\pi}{\sin(k\pi/n)}$ なので

$$
I(0)^2 = \prod_{k=1}^{n-1}\frac{\pi}{\sin(k\pi/n)} =
\frac{\pi^{n-1}}{\ds\prod_{k=1}^{n-1}\sin\frac{k\pi}{n}}.
$$

そして, 

$$
\prod_{k=1}^{n-1}\sin\frac{k\pi}{n} = 
\prod_{k=1}^{n-1}\frac{e^{\pi ik/n}-e^{-\pi ik/n}}{2i} =
\frac{e^{\pi i(1+2+\cdots+(n-1))/n}}{2^{n-1}i^{n-1}}\prod_{k=1}^{n-1}(1-e^{-2\pi ik/n}) =
\frac{1}{2^{n-1}}\prod_{k=1}^{n-1}(1-e^{-2\pi ik/n})
$$

であり,  

$$
\frac{x^n-1}{x-1} = \prod_{k=1}^{n-1}(x-e^{-2\pi ik/n})
$$

において $x\to 1$ とすると $\ds \prod_{k=1}^{n-1}(1-e^{-2\pi ik/n})=n$ が得られる. 以上を合わせると

$$
I(0)^2 = \frac{(2\pi)^{n-1}}{n}.
$$

両辺の平方根を取れば(3)が得られる. $\QED$


### Laplaceの方法

#### Laplaceの方法の解説

**Laplaceの方法:** Stirlingの公式の証明の解説のようにして見付かる変数変換はより一般の場合に非常に有用である. 以下では $\int_{-\infty}^\infty$ や $\int_0^\infty$ を単に $\int$ と書くことにし, 

$$
Z_n = \int e^{-nf(x)}g(x)\,dx
$$

とおく. ただし, $f(x)$ は実数値函数で唯一つの最小値 $f(x_0)$ を持ち, $x=x_0$ において, 

$$
f(x) = f(x_0) + \frac{a}{2}(x-x_0)^2 + O((x-x_0)^3), \quad a=f''(x_0) > 0
$$

とTaylor展開されていると仮定するし, さらに, $0$ 以上の値を持つ実数値函数 $g(x)$ は積分 $Z_n$ がうまく定義されるような適当な条件を満たしていると仮定し, $x_0$ の近傍で $g(x)>0$ を満たしていると仮定する. (ここで, $x_0$ の近傍で $g(x)>0$ が成立しているとは, ある $\delta>0$ が存在して, $|x-x_0|<\delta$ ならば $g(x)>0$ となることである.) このとき, 

$$
Z_n = e^{-nf(x_0)} \int \exp\left(-n\left(\frac{a}{2}(x-x_0)^2+O((x-x_0)^3)\right)\right)\;g(x)\,dx.
$$

$x=x_0+y/\sqrt{n}$ と変数変換すると

$$
Z_n = \frac{e^{-nf(x_0)}}{\sqrt{n}}
\int \exp\left(-\frac{a}{2}y^2+O\left(\frac{1}{\sqrt{n}}\right)\right)\;
g\left(x_0+\frac{y}{\sqrt{n}}\right)\,dy.
$$

そして, $n\to\infty$ で

$$
\int \exp\left(-\frac{a}{2}y^2+O\left(\frac{1}{\sqrt{n}}\right)\right)\;
g\left(x_0+\frac{y}{\sqrt{n}}\right)\,dy \to
\int \exp\left(-\frac{a}{2}y^2\right)g(x_0)\,dy =
\sqrt{\frac{2\pi}{a}}\;g(x_0).
$$

$a=f''(x_0)$ とおいたことを思い出しながら, 以上をまとめると, $n\to\infty$ で

$$
Z_n \sim  \frac{e^{-nf(x_0)}}{\sqrt{n}} \sqrt{\frac{2\pi}{f''(x_0)}}\;g(x_0).
$$

すなわち, 

$$
-\log Z_n = nf(x_0) + \frac{1}{2}\log n - \log\left(\sqrt{\frac{2\pi}{f''(x_0)}}\;g(x_0)\right) + o(1).
$$

$Z_n$ の $n\to\infty$ における漸近挙動を調べるための以上の方法を**Laplaceの方法**(Laplace's method)と呼ぶ. $\QED$


#### Laplaceの方法によるStirlingの公式の導出

**問題(Stirlingの公式):**  $\ds n! = \int_0^\infty e^{-t}t^n\,dt$ にLapalceの方法を適用して, Stirlingの公式を導出せよ.

**解答例:** 積分変数を $t=nx$ で置換すると,

$$
n! = \int_0^\infty e^{-t+n\log t}\,dt = n^{n+1} \int_0^\infty e^{-n(x-\log x)}\,dx.
$$

$f(x)=x-\log x$, $g(x)=1$ とおく. $f'(x)=1-1/x$, $f''(x)=1/x^2$ なので $f(x)$ は $x_0=1$ で最小になり, $f(1)=f''(1)=1$ となる. ゆえに, それらにLaplaceの方法を適用すると,

$$
n! \sim n^{n+1}\frac{e^{-n}}{\sqrt{n}}\sqrt{2\pi} = n^n e^{-n}\sqrt{2\pi n}.
\qquad \QED
$$


Laplaceの方法は本質的にGauss積分の応用である.

Gauss積分をガンマ函数に置き換えることによって得られる一般化されたLaplaceの方法の素描については

* 黒木玄, <a href="https://genkuroki.github.io/documents/20161014GeneralizedLaplace.pdf">一般化されたLaplaceの方法</a>

を参照せよ. 一般化されたLaplaceの方法は

* 渡辺澄夫, <a href="https://www.amazon.co.jp/dp/4339024627">ベイズ統計の理論と方法</a>, 2012

の第4章の主結果であるベイズ統計における自由エネルギーの

$$
F_n = -\log Z_n = nS + \lambda \log n - (m-1)\log\log n + O(1)
$$

の形の漸近挙動を導く議論を初等化するために役に立つ. 特異点解消は本質的に不可避だが, この形の漸近挙動だけが欲しいのであればゼータ函数を用いた精密な議論は必要ない.


### Laplaceの方法の弱形

**Laplaceの方法の弱形:** Laplaceの方法が使える状況では, 

$$
Z_n = \int e^{-nf(x)}g(x)\,dx
$$

について, 特に, $n\to\infty$ のとき, 

$$
-\frac{1}{n}\log Z_n \to f(x_0) = \min f(x), \quad\text{i.e.}\quad
Z_n = \int e^{-nf(x)}g(x)\,dx = \exp\left(-n\min f(x)+o(n)\right)
$$

が成立している. この結論を**Laplaceの方法の弱形**と呼ぶことにする. Laplaceの方法のような精密な形でなくても, こちらの弱形だけで用が足りることは結構多い. $\QED$


**問題(Laplaceの方法の弱形が明瞭に成立する場合):** 閉区間 $[a,b]$ 上の実数値連続函数 $f(x)$ と $0$ 以上の値を持つ実数値函数 $g(x)$ は, $\ds f(x_0) = \min_{a\leqq x\leqq b} f(x)$ を満たすある $x_0\in [a,b]$ の近傍で $g(x)>0$ を満たしていると仮定する. このとき, $n\to\infty$ において, 

$$
\int_a^b e^{-nf(x)}g(x)\,dx = \exp\left(-n\min_{a\leqq x\leqq b} f(x) + o(n)\right)
$$

が成立していることを示せ. すなわち, $n\to\infty$ のとき, 

$$
-\frac{1}{n}\log\int_a^b e^{-nf(x)}g(x)\,dx \to \min_{a\leqq x\leqq b} f(x)
$$

が成立していることを示せ.


**解答例:** $\ds f_0(x) = f(x)-\min_{a\leqq \xi\leqq b}f(\xi)$ とおくと, $f_0(x)$ の最小値は $0$ になり, 

$$
-\frac{1}{n}\log\int_a^b e^{-nf(x)}g(x)\,dx = 
\min_{a\leqq x\leqq b}f(x) - \frac{1}{n}\log\int_a^b e^{-nf_0(x)}g(x)\,dx
$$

なので, $n\to\infty$ のとき

$$
-\frac{1}{n}\log\int_a^b e^{-nf_0(x)}g(x)\,dx \to 0
$$

となることを示せばよい. 

$\eps > 0$ を任意に取って固定し, $A = \{\, x\in[a,b]\mid f_0(x)\leqq\eps\,\}$ とおき, その $[a,b]$ での補集合を $A^c$ と書き, 

$$
Z_{0,n} = \int_a^b e^{-nf_0(x)}g(x)\,dx = I_n + J_n, \quad
I_n = \int_A e^{-nf_0(x)}g(x)\,dx, \quad
J_n = \int_{A^c} e^{-nf_0(x)}g(x)\,dx.
$$

とおく. $n\to\infty$ のとき $-\frac{1}{n}\log Z_{0,n} \to 0$ となることを示したい.

$x\in A$ について $\eps\geqq f_0(x)\geqq 0$ なので, $e^{-n\eps}\leqq e^{-nf_0(x)}\leqq 1$ となるので, 

$$
e^{-n\eps}\int_A g(x)\,dx\leqq I_n = \int_A e^{-nf_0(x)}g(x)\,dx \leqq \int_A g(x)\,dx.
$$

$\ds f(x_0) = \min_{a\leqq x\leqq b} f(x)$ を満たすある $x_0\in [a,b]$ の近傍で $g(x)>0$ となっていると仮定したことより, $\ds \int_A g(x)\,dx > 0$ となることにも注意せよ.

$x\in A^c$ について $f_0(x)>\eps$ なので, $0 < e^{-nf_0(x)}<e^{-n\eps}$ となるので,

$$
0\leqq J_n = \int_{A^c} e^{-nf_0(x)}g(x)\,dx \leqq e^{-n\eps}\int_{A^c} g(x)\,dx.
$$

以上をまとめると,

$$
e^{-n\eps}\int_A g(x)\,dx\leqq Z_{0,n} \leqq \int_A g(x)\,dx + e^{-n\eps}\int_{A^c} g(x)\,dx.
$$

これの全体の対数を取って $-1/n$ 倍すると,

$$
\eps - \frac{1}{n}\log\int_A g(x)\,dx \geqq
-\frac{1}{n}\log Z_{0,n}\geqq
-\frac{1}{n}\log\left(\int_A g(x)\,dx + e^{-n\eps}\int_{A^c} g(x)\,dx\right).
$$

したがって, $n\to 0$ とすると, 

$$
\eps\geqq
\limsup_{n\to\infty}\left(-\frac{1}{n}\log Z_{0,n}\right)\geqq
\liminf_{n\to\infty}\left(-\frac{1}{n}\log Z_{0,n}\right)\geqq
0.
$$

$\eps>0$ は幾らでも小さくできるので, 下極限と上極限が等しくなることがわかり, 

$$
\lim_{n\to\infty}\left(-\frac{1}{n}\log Z_{0,n}\right) = 0
$$

が得られる. $\QED$
