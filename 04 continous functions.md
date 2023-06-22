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

# 04 連続函数

黒木玄

2018-04-20～2019-04-03

* Copyright 2018, 2019 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

* http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/04%20continous%20functions.ipynb

* https://genkuroki.github.io/documents/Calculus/04%20continous%20functions.pdf

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
$

<!-- #region toc=true -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#連続函数" data-toc-modified-id="連続函数-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>連続函数</a></span><ul class="toc-item"><li><span><a href="#連続函数の定義" data-toc-modified-id="連続函数の定義-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>連続函数の定義</a></span></li><li><span><a href="#中間値の定理" data-toc-modified-id="中間値の定理-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>中間値の定理</a></span></li><li><span><a href="#不動点定理" data-toc-modified-id="不動点定理-1.3"><span class="toc-item-num">1.3&nbsp;&nbsp;</span>不動点定理</a></span></li><li><span><a href="#閉区間上の実数値連続函数が最大最小を持つこと" data-toc-modified-id="閉区間上の実数値連続函数が最大最小を持つこと-1.4"><span class="toc-item-num">1.4&nbsp;&nbsp;</span>閉区間上の実数値連続函数が最大最小を持つこと</a></span></li><li><span><a href="#例" data-toc-modified-id="例-1.5"><span class="toc-item-num">1.5&nbsp;&nbsp;</span>例</a></span><ul class="toc-item"><li><span><a href="#例1:-コンパクト台を持つ滑らかな連続函数" data-toc-modified-id="例1:-コンパクト台を持つ滑らかな連続函数-1.5.1"><span class="toc-item-num">1.5.1&nbsp;&nbsp;</span>例1: コンパクト台を持つ滑らかな連続函数</a></span></li><li><span><a href="#例2:-無限に振動する不連続函数" data-toc-modified-id="例2:-無限に振動する不連続函数-1.5.2"><span class="toc-item-num">1.5.2&nbsp;&nbsp;</span>例2: 無限に振動する不連続函数</a></span></li><li><span><a href="#例3:-無限に振動する連続函数" data-toc-modified-id="例3:-無限に振動する連続函数-1.5.3"><span class="toc-item-num">1.5.3&nbsp;&nbsp;</span>例3: 無限に振動する連続函数</a></span></li><li><span><a href="#例4:-Weierstrass函数" data-toc-modified-id="例4:-Weierstrass函数-1.5.4"><span class="toc-item-num">1.5.4&nbsp;&nbsp;</span>例4: Weierstrass函数</a></span></li><li><span><a href="#例5:-高木函数" data-toc-modified-id="例5:-高木函数-1.5.5"><span class="toc-item-num">1.5.5&nbsp;&nbsp;</span>例5: 高木函数</a></span></li></ul></li></ul></li><li><span><a href="#一様連続性" data-toc-modified-id="一様連続性-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>一様連続性</a></span><ul class="toc-item"><li><span><a href="#閉区間上の連続函数が一様連続であること" data-toc-modified-id="閉区間上の連続函数が一様連続であること-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>閉区間上の連続函数が一様連続であること</a></span></li><li><span><a href="#連続だが一様連続ではない函数の例" data-toc-modified-id="連続だが一様連続ではない函数の例-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>連続だが一様連続ではない函数の例</a></span></li></ul></li><li><span><a href="#一様収束" data-toc-modified-id="一様収束-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>一様収束</a></span><ul class="toc-item"><li><span><a href="#函数列の各点収束の定義" data-toc-modified-id="函数列の各点収束の定義-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>函数列の各点収束の定義</a></span></li><li><span><a href="#函数列の一様収束の定義" data-toc-modified-id="函数列の一様収束の定義-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>函数列の一様収束の定義</a></span></li><li><span><a href="#連続函数列の一様収束先も連続になること" data-toc-modified-id="連続函数列の一様収束先も連続になること-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>連続函数列の一様収束先も連続になること</a></span></li><li><span><a href="#Peano曲線" data-toc-modified-id="Peano曲線-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>Peano曲線</a></span></li></ul></li></ul></div>
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
#clibrary(:colorcet)
#clibrary(:misc)
default(fmt=:png)

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
using SpecialFunctions
using QuadGK
```

```julia
# Override the Base.show definition of SymPy.jl:
# https://github.com/JuliaPy/SymPy.jl/blob/29c5bfd1d10ac53014fa7fef468bc8deccadc2fc/src/types.jl#L87-L105

@eval SymPy function Base.show(io::IO, ::MIME"text/latex", x::SymbolicObject)
    print(io, as_markdown("\displaystyle " * sympy.latex(x, mode="plain", fold_short_frac=false)))
end
@eval SymPy function Base.show(io::IO, ::MIME"text/latex", x::AbstractArray{Sym})
    function toeqnarray(x::Vector{Sym})
        a = join(["\displaystyle " * sympy.latex(x[i]) for i in 1:length(x)], "\\")
        """\left[ \begin{array}{r}$a\end{array} \right]"""
    end
    function toeqnarray(x::AbstractArray{Sym,2})
        sz = size(x)
        a = join([join("\displaystyle " .* map(sympy.latex, x[i,:]), "&") for i in 1:sz[1]], "\\")
        "\left[ \begin{array}{" * repeat("r",sz[2]) * "}" * a * "\end{array}\right]"
    end
    print(io, as_markdown(toeqnarray(x)))
end
```

## 連続函数


### 連続函数の定義

**定義:** 函数 $f(x)$ が $x=a$ で**連続**であるとは, $x\to\alpha$ のとき $f(x)\to f(a)$ が成立することであると定める.

これは $\eps$-$\delta$ のスタイルでは次のように言い直される.

函数 $f(x)$ が $x=a$ で連続であるとは次が成立していることである: 任意の $\eps>0$ に対して, ある $\delta>0$ が存在して, $|x-a|<\delta$ ならば $|f(x)-f(a)|<\eps$ となる. $\QED$ 


**定理:** 以下の2つの条件は互いに同値である:

(1) $f(x)$ は $x=a$ で連続である. (すなわち $x\to a$ のとき $f(x)\to f(a)$ となる.)

(2) 数列 $x_n$ が $a$ に収束するならば数列 $f(x_n)$ は $f(a)$ に収束する. (すなわち $x_n\to a$ のとき $f(x_n)\to f(a)$ となる.)

**証明:** (1)ならば(2)を示そう. (1)を仮定し, $x_n$ は $a$ に収束する任意の数列であるとし, $\eps>0$ を任意に取る. (1)より, ある $\delta>0$ が存在して, $|x-a|<\delta$ ならば $|f(x)-f(a)|<\eps$ となる. $x_n\to a$ なのである番号 $N$ で $n\geqq N$ ならば $|x_n-a|<\delta$ となり, そのとき $f(x_n)-f(a)|<\eps$ となる. これで(1)から(2)が導かれることがわかった.

(2)ならば(1)の対偶を示すために, (1)の否定を仮定する. すなわち, ある $\eps>0$ で次の条件を満たすものが存在すると仮定する: 任意の $\delta>0$ に対して, ある $x$ で $|x-a|<\delta$ と $f(x)-f(a)|\geqq\eps$ を満たすものが存在する. そのとき, 任意の正の整数 $n$ に対して, ある $x_n$ で $|x_n-a|<1/n$ と $|f(x_n)-f(a)|\geqq\eps$ を満たすものが存在する. そのとき, $x_n$ は $a$ に収束しているが, $f(x_n)$ は $f(a)$ に収束していない. これで, (1)の否定から(2)の否定が導かれることがわかった. 

これで示すべきことがすべて示された. $\QED$


### 中間値の定理


**中間値の定理:** $f$ は閉区間 $[a,b]$ 上の実数値連続函数であるとする. このとき $f(a)$ と $f(b)$ のあいだにある任意の実数 $\alpha$ に対して, ある $\xi\in[a,b]$ で $f(\xi)=\alpha$ を満たすものが存在する. $\QED$

**補足:** $x$ が $a$ と $b$ のあいだにあるとは $a\leqq x\leqq b$ または $a\geqq x\geqq b$ が成立していることだと定める. $\QED$

**注意:** 中間値の定理において, $f(a)<\alpha<f(b)$ または $f(a)>\alpha>f(b)$ ならば $f(\xi)=\alpha$ をみたす $\xi\in[a,b]$ は $a<\xi<b$ となるように取れる. $\QED$

**中間値の定理の証明:** $f(x)$ の代わりに $f(x)-\alpha$ を考えることによって, $\alpha=0$ の場合に証明すれば十分である. 必要なら $f(x)$ の代わりに $-f(x)$ を考えることによって, $f(a)\leqq 0$, $f(b)\geqq 0$ の場合に証明すれば十分である. 以下, そのように仮定する.

$(a_1, b_1)=(a,b)$ とおく. $n$ について帰納的に $f(a_n)\leqq 0\leqq f(b_n)$ を満たす $(a_n,b_n)$ を次のように定めることができる.

* $f((a_n+b_n)/2)\geqq 0$ ならば $(a_{n+1}, b_{n+1}) = (a_{n+1}, (a_n+b_n)/2)$.

* $f((a_n+b_n)/2)< 0$ ならば, $(a_{n+1}, b_{n+1}) = ((a_n+b_n)/2, b_n)$.

このとき, $a_1\leqq a_2\leqq\cdots$, $\cdots\leqq b_2\leqq b_1$ でかつ $|a_n-b_n|=|a-b|/2^{n-1}$ であることから, $a_n$ と $b_n$ は同一の値 $\xi$ に収束する. そのとき, $f(x)$ の連続性より $f(a_n)$ と $f(b_n)$ も同一の値 $f(\xi)$ に収束し, $f(a_n)\leqq 0\leqq f(b_n)$ なのでその収束先は $0$ でなければいけない.  ゆえに $f(\xi)=0$. $\QED$

**注意:** 上の証明法の**二分法 (bisection method)**と呼ぶことがある. 実際の数値計算でも利用可能な方法である. $\QED$

```julia
function bisection(f, a, b; atol=eps(), maxiter=10^3)
    f(a)*f(b) > 0 && return NaN
    aₙ, bₙ = a, b
    k = 0
    while abs(aₙ - bₙ) > atol
        k > maxiter && return NaN
        c = (aₙ+bₙ)/2
        if f(bₙ)*f(c) ≥ 0
            aₙ, bₙ = aₙ, c
        else
            aₙ, bₙ = c, bₙ
        end
        k += 1
    end
    return (aₙ+bₙ)/2
end
```

```julia
f(x) = x^2 - 2
@show ξ = bisection(f, 1, 2)
@show √2
@show round(f(ξ), digits=15)

x = 1:0.005:2
plot(size=(400,250), legend=:topleft)
plot!(x, f.(x), label="y = x^2 - 2")
hline!([0], label="y = 0")
vline!([ξ], label="x = $(round(ξ, digits=5))", ls=:dash)
```

```julia
f(x) = cos(x) - x
@show ξ = bisection(f, 0, 1)
@show round(f(ξ), digits=15)

x = 0:0.005:1
plot(size=(400,250))
plot!(x, f.(x), label="y = cos x - x")
hline!([0], label="y = 0")
vline!([ξ], label="x = $(round(ξ, digits=5))", ls=:dash)
```

### 不動点定理


**不動点定理:** 閉区間 $[a,b]$ からそれ自身への連続写像 $f$ に対して, ある $\xi\in[a,b]$ で $f(\xi)=\xi$ を満たすものが存在する. (そのような $\xi$ を $f$ の不動点と呼ぶ.)

**証明:** $g(x)=f(x)-x$ とおく. このとき $g$ は閉区間 $[a,b]$ 上の実数値連続函数であり, $g(a)=f(a)-a\geqq 0$ かつ $g(b)=f(b)-b\leqq 0$ となり, $0$ は $g(a)$ と $g(b)$ のあいだにある実数になる.  ゆえに中間値の定理より, ある $\xi\in[a,b]$ で $g(\xi)=0$ すなわち $f(\xi)=\xi$ を満たすものが存在する.  $\QED$ 


**補足:** 以下の形の $\R$ の部分集合を**区間**と呼ぶ:

$$
\begin{aligned}
& [a,b] = \{\,x\in\R\mid a\leqq x\leqq b\,\}, \\
& [a,b) = \{\,x\in\R\mid a\leqq x< b\,\}, \\
& (a,b] = \{\,x\in\R\mid a< x\leqq b\,\}, \\
& (a,b) = \{\,x\in\R\mid a< x< b\,\}. \\
\end{aligned}
$$

$[a,b]$ は**閉区間**と呼ばれ, $(a,b)$ は**開区間**と呼ばれる. $(-\infty,b]$ のように無限に長い区間を扱うこともある. $\QED$


**補足:** 閉区間 $[a,b]$ からそれ自身への写像とは $x\in[a,b]$ に $f(x)\in[a,b]$ を対応させる函数のことである. そのグラフ $G=\{\,(x,f(x))\mid a\leqq x\leqq b\,\}$ は正方形 $[a,b]\times[a,b]=\{\,(x,y)\in\R^2\mid a\leqq x,y\leqq b\,\}$ に含まれる. $\QED$


**注意:** 上の不動点定理は以下のように説明することもできる. 簡単のため $[a,b]=[-1,1]$ の場合を考える. もしも $f$ の不動点が存在しなかったならば, すべての $x\in [-1,1]$ について $f(x)\ne x$ となる. ゆえに $x\in[-1,1]$ の連続函数 $g(x)$ を

$$
g(x) = \frac{x-f(x)}{|x-f(x)|}
$$

と定めることができる. すべての $x\in[-1,1]$ について $g(x)$ は $\pm1$ のどちらかになり, $-1<f(-1)$ なので $g(-1)=-1$ となり, $1>f(1)$ なので $g(1)=1$ となる. しかし, そのような $[-1,1]$ 上の連続函数は存在するはずがないので, 不動点が存在するはずである. $\QED$

```julia
f(x) = 0.6 + 0.4x - 0.8x^2 + 0.3sin(10x-0.5)
ξ = bisection(x->f(x)-x, 0, 1)
x = 0:0.01:1
plot(size=(400,400), aspect_ration=1, legend=false)
plot!(xlim=(0,1), ylim=(0,1))
plot!(x, f.(x), lw=1.5)
plot!([0,1], [0,1], color=:black)
hline!([ξ], ls=:dash, color=:red)
vline!([ξ], ls=:dash, color=:red)
```

**注意:** 上の易しい不動点定理はBrouwerの不動点定理の次元1の特別な場合である. $\QED$

**Brouwerの不動点定理:** $n$ 次元閉円盤

$$
D = \{\,(x_1,\ldots,x_n)\in\R^n\mid x_1^2+\cdots+x_n^2\leqq 1\,\}
$$

からそれ自身への連続写像 $f$ は不動点を持つ. すなわち, ある $\xi\in D$ で $f(\xi)=\xi$ を満たすものが存在する. $\QED$

このノート群の内容を逸脱する話題なので<a href="https://www.google.co.jp/search?q=Brouwer%E3%81%AE%E4%B8%8D%E5%8B%95%E7%82%B9%E5%AE%9A%E7%90%86+%E8%A8%BC%E6%98%8E">証明</a>には触れない. 証明する前に直観的にどこまでこの定理が「明らか」であるかについて考えてみた方がよい.

**研究課題:** $n=2$ の場合にBrouwerの不動点定理が直観的には当然成立するべき結果であることがわかるような説明を見付けよ. $\QED$

**ヒント:** 上にある注意の方法をこの場合に適用してみよ. $\QED$


### 閉区間上の実数値連続函数が最大最小を持つこと

**定理:** 実数の閉区間上の実数値連続函数は最大値と最小値を持つ.

**証明:** $f$ は閉区間 $I=[a,b]$ 上の実数値連続函数であると仮定する. $f$ を $-f$ で置き換えることによって, $f$ が最大値を持つことを示せば十分である. 

まず, $f$ が閉区間 $I$ 上で有界であることを示そう. もしも $f$ が有界でないならばある $a_n\in I$ で $f(a_n)\geqq n$ を満たすものが存在する. Bolzano-Weierstrassの定理より, $a_n$ の部分列 $a_{k_n}$ である実数 $\alpha$ に収束するものが存在する. $a_{k_n}\in I$ すなわち $a\leqq a_{k_n}\leqq b$ より $a\leqq \alpha\leqq b$ すなわち $\alpha\in I$ であることもわかる.  $f$ の連続性より, $n\to\infty$ で $f(a_{k_n})\to f(\alpha)$ でなればいけないが, $a_n$ の取り方より, $f(a_{k_n})\to\infty$ となる. これは矛盾である. ゆえに, $f$ は閉区間 $I$ 上で有界でなければいけない.

次に, $f$ が閉区間 $I$ 上で最大値を持つことを示そう. $\{\,f(x)\mid x\in I\,\}$ は上に有界なので, 実数の連続性より, それは最小の上界を持つ. その最小上界を $M$ と書くことにする. $M$ が $\{\,f(x)\mid x\in I\,\}$ の最小上界であることより, $n=1,2,\ldots$ に対して, $M-\frac{1}{n}\leqq f(x_n)\leqq M$ を満たす $x_n\in I$ が存在する. そのとき, $f(x_n)$ は $M$ に収束する. Bolzano-Weierstrassの定理より, $x_n$ のある部分列 $x_{l_n}$ である $\beta\in I$ に収束するものが存在する.  このとき, 

$$
f(\beta)=\lim_{n\to\infty}f(x_{l_n})=M.
$$

これで $f$ が $I$ 上で最大値 $M$ を持つことがわかった. $\QED$.


**定義(supノルム):** 集合 $X$ 上の函数で $|f(x)|$ が上に有界なものに対して, 

$$
\|f\|_\infty = \sup_{x\in X}|f(x)|
$$

を $f$ の**supノルム**と呼ぶ. $\QED$

閉区間 $[a,b]$ 上の連続函数 $f(x)$ に対して, $|f(x)|$ も閉区間 $[a,b]$ 上の連続函数になるので, $|f(x)|$ は最大値を持つ. ゆえに $f(x)$ のsupノルムも定義され,

$$
\|f\|_\infty = \sup_{a\leqq x\leqq b}|f(x)| = \max_{a\leqq x\leqq b}|f(x)|
$$

が成立する. 

函数の差のsupノルム $\|f-g\|_\infty$ は函数 $f,g$ の違いの大きさの指標としてよく使われる.


**問題:** 適当に $\eps>0$ と函数 $\varphi$ を与え, $\|f-\varphi\|_\infty<\eps$ を満たす函数 $f$ のグラフがどのような領域に含まれるかを図示せよ. $\QED$

次のセルを見よ.

```julia
φ(x) = sin(x) - 0.5x + 0.1x^2
x = -2.5:0.02:3.5
ε = 0.3
plot(size=(500, 350))
plot!(x, φ.(x), label="y = phi(x)", color=:black, lw=2)
plot!(x, φ.(x).+ε, label="phi(x) - eps < y < phi(x)+eps", color=:red, ls=:dash, fill=(:pink, 0.2, φ.(x)))
plot!(x, φ.(x).-ε, label="", color=:red, ls=:dash, fill=(:pink, 0.2, φ.(x)))
```

**問題:** その絶対値が上に有界な集合 $X$ 上の函数達 $f,g$ について以下が成立することを示せ:

(1) $\|f+g\|_\infty \leqq \|f\|_\infty + \|g\|_\infty$.

(2) 数 $\alpha$ について $\|\alpha f\|_\infty = |\alpha|\|f\|_\infty$.

(3) $\|f\|_\infty = 0$ ならば $f$ は恒等的に $0$.

**証明:** (1) $A,B$ をそれぞれ $|f(x)|$, $|g(x)|$ の上界だとすると, $|f(x)+g(x)|\leqq |f(x)|+|g(x)|\leqq A+B$. そして上限は上界の最小値だったので, $\|f+g\|_\infty\leqq\sup_{x\in X}|f(x)|+\sup_{x\in X}|g(x)|=\|f\|_\infty+\|g\|_\infty$. 

(2) $|\alpha f(x)| = |\alpha||f(x)|$ より, それらの上限は等しい. すなわち, $\|\alpha f\|_\infty=|\alpha|\|f\|_\infty$.

(3) $\|f\|_\infty = \sup_{x\in X}|f(x)|=0$ であると仮定する. このとき, $|f(x)|$ の上界の最小値は $0$ である. これは任意の $x\in X$ について $|f(x)|\leqq 0$ が成立することを意味する. ゆえに $f$ は恒等的に $0$ になる. $\QED$


**参考($L^1$ノルム):** 閉区間 $[a,b]$ 上の $L^1$ノルム $\|f\|_1$ は次のように定義される:

$$
\|f\|_1 = \int_a^b |f(x)|\,dx.
$$

これも函数の違いを測るためによく使われる. 次のセルを見よ. 塗りつぶされている部分の面積が $\|f-g\|_1$ に等しい. $\QED$

```julia
f(x) = sin(x) - 0.5x + 0.1x^2
g(x) = cos(x) - 0.5 + 0.1x^3
plot(size=(450, 280), legend=:top)
x = -2.5:0.02:3.5
plot!(x, f.(x), color=:blue, label="f(x)")
plot!(x, g.(x), color=:red,  label="g(x)")
x = -2.0:0.02:3.0
plot!(x, g.(x), color=:red, fill=(0.1, f.(x)), label="")
vline!([minimum(x)], color=:black, ls=:dash,    label="x=a")
vline!([maximum(x)], color=:black, ls=:dashdot, label="x=b")
```

**問題:** 閉区間 $[a,b]$ 上の連続函数 $f,g$ について以下が成立することを示せ:

(1) $\|f+g\|_\infty \leqq \|f\|_1 + \|g\|_\infty$.

(2) 数 $\alpha$ について $\|\alpha f\|_1 = |\alpha|\|f\|_1$.

(3) $\|f\|_1 = 0$ ならば $f$ は恒等的に $0$.

**解答例:** (1) $|f(x)+g(x)|\leqq |f(x)|+|g(x)|$ なので両辺を積分することによって $\|f+g\|_1\leqq\|f\|_1+\|g\|_1$ を得る.

(2) $|\alpha f(x)|=|\alpha||f(x)|$ の両辺を積分することによって $\|\alpha f\|_1=|\alpha|\|f\|_1$ を得る.

(3) 対偶を示すために, ある $x_0\in[a,b]$ で $f(x_0)\ne 0$ を満たすものが存在すると仮定する. $f(x)$ は連続なので, ある $\delta >0$ が存在して, $x\in[a,b]$ かつ $|x-x_0|\leqq\delta$ ならば $\leqq |f(x)-f(x_0)|\leqq|f(x_0)|/2$ となり, $|f(x)|=|f(x_0)-(f(x_0)-f(x))|\geqq|f(x_0)|-|f(x_0)-f(x)|\geqq|f(x_0)|-|f(x_0)|/2=|f(x_0)|/2$ となる. ゆえに, $A=\max\{a,x_0-\delta\}$, $B=\min\{a,x_0+\delta\}$ とおくと, 

$$
\|f\|_1 = \int_a^b |f(x)|\,dx = 
\int_A^B |f(x)|\,dx \geqq
\int_A^B \frac{|f(x_0)|}{2}\,dx \geqq \frac{|f(x_0)|(B-A)}{2} > 0.
$$

これで(3)の対偶が証明された. $\QED$

**注意:** 上の解答例の(3)の証明は, 単に $f(x_0)\ne 0$ ならば $x_0$ の近くの $x$ について $|f(x)|>0$ となるので, $|f(x)|$ を積分すると $0$ より大きくなることを示しているに過ぎない. 実際に書かれた証明はややこしいが, やっていることはシンプルである. 数学的に詳しく書かれた証明はこういうことになっている場合が実に多い. 直観レベルでは非常にシンプルなことしかやっていなくても, 順番に丁寧に書いたせいで複雑に見えてしまうことがよくある. そのような証明を読むときには, 「シンプルだが雑な直観をもとにして, どのようにして隙の無い証明を書きあげればよいか」という発想で証明を自力で再構成してみるのがよい. 結局のところ他人が書いた証明は他人にとってわかりやすいだけで, 自分にとってはわかりにくい.  自分にとってわかり易い証明は自分で書くしかない. $\QED$


**まとめ:** 閉区間 $[a,b]$ 上の連続函数 $f,g$ に対して, 差のsupノルム $\|f-g\|$ は $f,g$ の同一の $x$ における値の差の最大値に等しく, 差の$L^1$ノルム $\|f-g\|_1$ は $f,g$ のグラフに挟まれている部分の面積に等しい. このように函数の違いの大きさを測る方法は複数存在する. $\QED$


### 例


#### 例1: コンパクト台を持つ滑らかな連続函数

函数

$$
f(x) = \begin{cases}
0 & (x\leqq 0) \\
e^{-1/x} & (x>0) \\
\end{cases}
$$

は $x\searrow 0$ のとき $e^{-1/x} \to 0$ なので連続函数である. ここで $x\searrow 0$ は $x$ が上から(正の値のまま) $0$ に近付くことを意味している($x\to +0$ と書くこともある).  $f(x)$ は任意有限回微分可能である(そのとき $C^\infty$ 函数であるという). 


**問題:** $n$ が非負の整数のとき, $x\searrow 0$ のとき $\ds \frac{e^{-1/x}}{x^n}\to 0$ となることを示せ. $\QED$

**証明:** $t = 1/x$ とおくと, 示すべきことは, $t\to\infty$ のとき $\ds\frac{t^n}{e^t}\to 0$ となることと同値である. しかし, それは指数函数が多項式函数より速く増加することを意味しているので, 成立することがすでにわかっている. $\QED$

**注意:** $e^{-1/x}$ の $n$ 階の導函数は $\ds\frac{e^{-1/x}}{x^n}$ に $x$ の多項式をかけた形になる. そのことより, $x\searrow 0$ のとき $e^{-1/x}$ の $n$ 階の導函数も $0$ に収束することがわかる. $\QED$

```julia
x = symbols("x")
diffexpneginvx(n) = diff(exp(-1/x), x, n)
[diffexpneginvx(n) for n in 0:3]
```

```julia
f(x) = x > 0 ? exp(-1/x) : zero(x)
g(x) = x > 0 ? exp(-1/x)/x^2 : zero(x)

p1 = plot(ylims=(-0.1, 1.0), legend=:topleft)
plot!(f, -10, 20, label="f(x)")

p2 = plot(ylims=(-0.1, 0.6), legend=:topright)
plot!(g, -10, 20, label="df(x)/dx")

plot(p1, p2, size=(700,200))
```

**問題:** $x\leqq 0$ のとき最小値 $0$ になり, $x\geqq 1$ のとき最大値 $1$ になる函数で任意有限回微分可能なものが存在することを示せ.

**解答例:** 函数

$$
f(x) = \begin{cases}
0 & (x\leqq 0\;\text{or}\; 1\geqq x) \\
\exp\left(-\frac{1}{x}+\frac{1}{x-1}\right) & (0<x<1) \\
\end{cases}
$$

は任意有限回微分可能である. $Z=\int_0^1 f(x)\,dx$ とおき, $g(x)=f(x)/Z$ とおくと, $\int_0^1 g(x)\,dx=1$ となる. このとき, 函数

$$
h(x) = \int_{-\infty}^x g(t)\,dt
$$

は問題の条件を満たしている. $\QED$

```julia
f(x) = 0 < x < 1 ? exp(-1/x+1/(x-1)) : zero(x)
@show Z, err = quadgk(f, 0, 1) # = ∫_0^1 f(x) dx, error
g(x) = f(x)/Z
h(x) = quadgk(g, -1, x)[1] # = ∫_{-1}^x g(t) dt

p1 = plot(legend=:topleft, ylims=(-0.1,2.7))
plot!(g, -1, 2, label="g(x)")

p2 = plot(legend=:topleft, ylims=(-0.1, 1.1))
plot!(h, -1, 2, label="h(x)")

plot(p1, p2, size=(700,200))
```

**問題:** $x\leqq 0$ または $4\leqq x$ のとき最小値 $0$ になり, $1\leqq x\leqq 3$ のとき最大値 $1$ になる函数で任意有限回微分可能なものが存在することを示せ.

**解答例:** 上の問題の函数 $h(x)$ を用いて, 函数 $\phi(x)$ を

$$
\phi(x) = h(x) h(4-x)
$$

と定めると条件を満たしている. $\QED$

```julia
φ(x) = h(x)*h(4-x)

plot(size=(400, 200), ylims=(-0.1, 1.1))
plot!(φ, -1, 5, label="phi(x)")
```

#### 例2: 無限に振動する不連続函数

函数

$$
f(x) = \begin{cases}
\sin(1/x) & (x\ne 0) \\
0         & (x=0)
\end{cases}
$$

は $x=0$ で不連続である. なぜならば $x\to 0$ のとき $f(x)$ は振幅を保ったまま無限に振動するからである.

```julia
f(x) = x == 0 ? 0 : sin(1/x)

p1 = plot(size=(400, 250), xlabel="x", ylabel="y")
plot!(f, -1.0, 1.0, ylims=(-1.2, 2.0), label="y = sin(1/x)")

p2 = plot(size=(400, 250), xlabel="x", ylabel="y")
plot!(f, -0.1, 0.1, ylims=(-1.2, 2.0), label="y = sin(1/x)")

plot(p1, p2, size=(700, 250))
```

#### 例3: 無限に振動する連続函数

函数

$$
g(x) = \begin{cases}
x\sin(1/x) & (x\ne 0) \\
0         & (x=0)
\end{cases}
$$

は $x\to 0$ のとき無限に振動しながら $0$ に収束するので連続である.

```julia
f(x) = x == 0 ? 0 : x*sin(1/x)

p1 = plot(size=(400, 250), xlabel="x", ylabel="y")
plot!(f, -3, 3, ylims=(-0.4, 1.5), label="y = x sin(1/x)")

p2 = plot(size=(400, 250), xlabel="x", ylabel="y")
plot!(f, -0.3, 0.3, ylims=(-0.25, 0.3), label="y = x sin(1/x)")

plot(p1, p2, size=(700, 250))
```

#### 例4: Weierstrass函数

$0<a<1$ について, 函数

$$
F(x) = \sum_{n=0}^\infty a^n\cos(b^n\pi x)
$$

は連続函数であり, Weierstrass 函数と呼ばれる.  $b$ が正の奇数であり, $ab>1+3\pi/2$ のとき至るところ微分不可能なことが知られている.

* <a href="https://ja.wikipedia.org/wiki/%E3%83%AF%E3%82%A4%E3%82%A8%E3%83%AB%E3%82%B7%E3%83%A5%E3%83%88%E3%83%A9%E3%82%B9%E9%96%A2%E6%95%B0">ワイエルシュトラス関数 - Wikipedia</a>


**注意:** 一般に, 区間 $I$ 上の連続函数列 $f_n(x)$ について, $x$ によらない非負実数列 $M_n$ で

$$
|f_n(x)|\leqq M_n \quad (x\in I), \qquad \sum_{n=1}^\infty M_n < \infty
$$

を満たしているならば, 

$$
F(x) = \sum_{n=1}^\infty f_n(x)\quad (x\in I)
$$

で定義される函数 $F(x)$ は $I$ 上の連続函数になる. このノートの一様種速の節を参照せよ. $\QED$

```julia
struct WeierstrassFunction{R<:Real, S<:Integer, T<:Integer} <: Function
    a::R;  b::S;  N::T
end
(F::WeierstrassFunction)(x) = sum(n->F.a^n*cos(F.b^n*π*x), 0:F.N)
```

```julia
F = WeierstrassFunction(0.6, 13, 100)
@show F.a*F.b, 1+3π/2

p1 = plot(legend=false, titlefontsize=11)
plot!(title="Weierstrass function: a = $(F.a), b = $(F.b)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
plot!(F, -2, 2)

p2 = plot(legend=false, titlefontsize=11)
plot!(title="Weierstrass function: a = $(F.a), b = $(F.b)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
plot!(F, -2/F.b, 2/F.b)

plot(p1, p2, size=(700, 250))
```

```julia
F = WeierstrassFunction(0.85, 7, 100)
@show F.a*F.b, 1+3π/2

p1 = plot(legend=false, titlefontsize=11)
plot!(title="Weierstrass function: a = $(F.a), b = $(F.b)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
plot!(F, -2, 2)

p2 = plot(legend=false, titlefontsize=11)
plot!(title="Weierstrass function: a = $(F.a), b = $(F.b)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
plot!(F, -2/F.b, 2/F.b)

plot(p1, p2, size=(700, 250))
```

```julia
F = WeierstrassFunction(1/4, 3, 100)
@show F.a*F.b, 1+3π/2

p1 = plot(legend=false, titlefontsize=11)
plot!(title="Weierstrass function: a = $(F.a), b = $(F.b)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
plot!(F, -2, 2)

p2 = plot(legend=false, titlefontsize=11)
plot!(title="Weierstrass function: a = $(F.a), b = $(F.b)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
plot!(F, -2/F.b, 2/F.b)

plot(p1, p2, size=(700, 250))
```

#### 例5: 高木函数

$0<w<1$ について

$$
F(x) = \sum_{n=0}^\infty w^n s(2^n x), \quad s(x)=\min_{k\in\Z}|x-k|
$$

で定義される函数 $F(x)$ は連続函数である. $w=1/2$ のとき $F(x)$ は高木函数と呼ばれる.

```julia
seesaw(x) = abs(x - round(x))
plot(seesaw, size=(400, 250), ylims=(-0.1, 0.7), label="seesaw(x)")
```

```julia
struct TakagiFunction{R<:Real, T<:Integer} <: Function
    w::R;  N::T
end
(F::TakagiFunction)(x) = sum(n->F.w^n*seesaw(2^n*x), 0:F.N)
```

```julia
F = TakagiFunction(0.5, 100)

p1 = plot(legend=false, titlefontsize=11)
plot!(title="Takagi function: w = $(F.w)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
x = 0:0.001:1
plot!(x, F.(x))

p2 = plot(legend=false, titlefontsize=11)
plot!(title="Takagi function: w = $(F.w)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
x = 0.25:0.00025:0.50
plot!(x, F.(x))

plot(p1, p2, size=(700, 250))
```

```julia
F = TakagiFunction(0.7, 100)

p1 = plot(legend=false, titlefontsize=11)
plot!(title="Takagi function: w = $(F.w)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
x = 0:0.001:1
plot!(x, F.(x))

p2 = plot(legend=false, titlefontsize=11)
plot!(title="Takagi function: w = $(F.w)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
x = 0.250:0.00025:0.50
plot!(x, F.(x))

plot(p1, p2, size=(700, 250))
```

```julia
F = TakagiFunction(1/4, 100)

p1 = plot(legend=false, titlefontsize=11)
plot!(title="Takagi function: w = $(F.w)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
x = 0:0.001:1
plot!(x, F.(x))

p2 = plot(legend=false, titlefontsize=11)
plot!(title="y = x(1-x)")
plot!(size=(600, 420), xlabel="x", ylabel="y")
x = 0:0.001:1
plot!(x, @.(x*(1-x)))

plot(p1, p2, size=(700, 250))
```

## 一様連続性


$f(x)$ が $x=a$ で**連続**であるとは, $x\to a$ のとき $f(x)\to f(a)$ となることであった.  区間 $I$ 上で定義された函数 $f(x)$ が**連続**であるとはすべての $x=a\in I$ で $f(x)$ が連続なことであると定める.


しかし, 一般に, 連続な $f(x)$ について, 各 $a\in I$ ごとに $x\to a$ のときの $f(x)\to f(a)$ の「収束の速さ」は一般に異なる.  $x\to a$ のときの $f(x)\to f(a)$ の「収束の速さ」がすべての $a\in I$ について同時に上からおさえられるとき, $f(x)$ は $I$ 上で**一様連続**であるという.

正確には一様連続性は次のように定義される. 

**定義:** $I$ 上の函数 $f$ は $I$ 上で**一様連続**であるとは, 任意に $\eps>0$ を与えたとき, ある $\delta>0$ が存在して, 任意の $a,x\in I$ について $|x-a|<\delta$ ならば $|f(x)-f(a)|<\eps$ が成立することである. $\QED$

$x\to a$ のとき $f(x)\to f(a)$ が成立するということは, $x$ を $a$ に近付けることによって $f(a)$ を $f(x)$ でいくらでも近似できるということである.  $x\to a$ のとき $f(x)\to f(a)$ であるということは, 許される誤差 $\eps > 0$ を与えたとき, ある $\delta > 0$ が存在して, $f(x)$ と $f(a)$ の違いを $\eps$ 未満におさえるためには $x$ と $a$ の違いを $\delta$ 未満におさえればよいということである. 

単なる連続性の定義では $\delta$ の取り方は各 $a$ ごとに違っていてもよい.  $\delta$ を小さく取らなければいけない $a$ においては $x\to a$ のときの $f(x)\to f(a)$ の「収束の速さ」は「遅い」ということである.

一様連続の定義は, すべての $a$ について共通の $\delta>0$ を選べることを意味している(すなわち $\delta>0$ は $\eps>0$ に依存するが, $a\in I$ には依存しないようにできることを意味している).


### 閉区間上の連続函数が一様連続であること

**定理:** 閉区間上の連続函数は一様連続である. $\QED$

**証明:** $f$ は閉区間 $I$ 上の連続函数であるとし, $\eps>0$ であるとする. このとき, 任意の $x\in I$ に対して, ある $\delta_x>0$ が存在して, $y\in I$ かつ $|y-x|<\delta_x$ ならば $\ds|f(y)-f(x)|<\frac{\eps}{2}$ となる. $\ds U_x = \left(x-\frac{\delta_x}{2},x+\frac{\delta_x}{2}\right)$ とおくと, $U_x$ 達は $I$ を覆う. ゆえに, <a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/01%20convergence.ipynb#閉区間に関するHeine-Borelの定理">閉区間に関するHeine-Borelの定理</a>より, 有限個の $x_1,\ldots,x_r\in I$ で $U_{x_1},\ldots,U_{x_r}$ で $I$ を覆うものが存在する. $\ds\delta=\min\{\frac{\delta_1}{2},\ldots,\frac{\delta_r}{2}\}$ とおき, $a,x\in I$ かつ $|x-a|<\delta$ と仮定する. $a$ はある $U_{x_i}$ に含まれるので, $\ds |a-x_i|<\frac{\delta_{x_i}}{2}<\delta_{x_i}$ となり, 

$$
|x-x_i|\leqq|x-a|+|a-x_i|<\delta+\frac{\delta_{x_i}}{2}\leqq \frac{\delta_{x_i}}{2}+\frac{\delta_{x_i}}{2}=\delta_{x_i}
$$

となるので, 

$$
|f(x)-f(a)|\leqq|f(x)-f(x_i)|+|f(x_i)-f(a)|<\frac{\eps}{2}+\frac{\eps}{2}=\eps.
$$

これで $f$ が一様連続であることが示された. $\QED$

**注意:** 閉区間上の函数については, 連続函数と一様連続函数を区別しなくてもよい. $\QED$


**問題:** $\R$ 上の連続函数 $f(x)$ で $|x|\to\infty$ のとき $f(x)\to 0$ となるものが一様連続になることを示せ.

**解答例:** $\eps>0$ を任意に取る.  $|x|\to\infty$ のとき $f(x)\to 0$ となるで, ある $R>0$ が存在して, $|x|\geqq R$ ならば $\ds |f(x)|<\frac{\eps}{2}$ となる. 閉区間 $[-3R, 3R]$ 上で連続函数 $f(x)$ は一様連続になるので, ある $\delta>0$ が存在して, $|x|,|y|\leqq 3R$ かつ $|x-y|<\delta$ ならば $\ds|f(x)-f(y)|<\eps$ となる. このとき, 実数 $x,y$ が $|x-y|<\min\{\delta,R\}$ を満たしているならば, $|x|\leqq 2R$ のとき, $|y|\leqq R$, $|x-y|<\delta$ となるので, $|f(x)-f(y)|<\eps$ となり, $|x|> 2R$ のとき, $|y|\geqq R$ となるので, 

$$
|f(x)-f(y)|\leqq|f(x)|+|f(y)|<\frac{\eps}{2}+\frac{\eps}{2}=\eps
$$

となる.  これで, $f(x)$ が $\R$ 全体で一様連続であることがわかった. $\QED$


### 連続だが一様連続ではない函数の例


**問題:** $x>0$ の連続函数 $f(x)=1/x$ は区間 $(0,1]$ 上では一様連続ではないことを示せ.

**解答例:** 図を描けば, $\eps>0$ と $a>0$ について, 

$$
x>0,\; |x-a|<\delta \implies |f(x)-f(a)|<\eps
\tag{$*$}
$$

すなわち

$$
x>0,\; a-\delta < x < a+\delta
\implies
\frac{1}{a}-\eps < \frac{1}{x} < \frac{1}{a}+\eps
$$

かつ $\delta>0$ となるためには, 

$$
\delta\leqq a - \frac{1}{1/a + \eps} = \frac{a^2\eps}{1+a\eps} < a^2\eps
$$

でなければいけないことがわかる. したがって $a>0$ を小さくすれば $\delta>0$ はいくらでも小さくなる. ゆえに $a$ と無関係に($*$)を満たす $\delta > 0$ を取ることは不可能である. つまり $f(x)$ は $(0,1]$ 上で一様連続ではない. $\QED$


**問題:** 任意に与えられた $r>0$ について, $f(x)=1/x$ は区間 $[r,\infty)$ 上で一様連続であることを示せ.

**解答例:** $\eps > 0$ であるとする. $a^2\eps/(1+a\eps)$ は $a$ について単調増加函数なので, $a$ が $[r,\infty)$ 内を動くとき, その最小値は $r^2\eps/(1+r\eps)$ になる. それを $\delta$ とおけばすべての $a\in[r,\infty)$ について同時に ($*$) が成立する. これで $f(x)=1/x$ が $[r,\infty)$ 上一様連続であることがわかった. $\QED$


**注意:** 微分可能函数については, 区間 $I$ 上でのグラフの傾きが幾らでも大きくなる函数は $I$ 上で一様連続ではなくなるが, 区間 $I$ 上でのグラフの傾きに上限がある場合には一様連続になる. $\QED$

```julia
f(x) = 1/x
x = 0.1:0.001:1

ε = 0.7

plot(xlims=(0,1), ylims=(0,10), size=(500, 350))
plot!(x, f.(x), label="y = 1/x", lw=2)

a = 0.15
δ₀ = a - 1/(1/a+ε)
δ₁ = 1/(1/a-ε) - a
c = :red
plot!([a-δ₀,a-δ₀], [0,f(a-δ₀)], color=c, label="")
plot!([a,a], [0,f(a)], color=c, lw=2, label="x = $a")
plot!([a+δ₁,a+δ₁], [0,f(a+δ₁)], color=c, label="")
plot!([0,a-δ₀], [f(a-δ₀),f(a-δ₀)], ls=:dash, color=c, label="y = 1/$a+$ε")
plot!([0,a], [f(a),f(a)], color=c, ls=:dash, lw=2, label="y = 1/$a")
plot!([0,a+δ₁], [f(a+δ₁),f(a+δ₁)], ls=:dashdot, color=c, label="y = 1/$a-$ε")

a = 0.5
δ₀ = a - 1/(1/a+ε)
δ₁ = 1/(1/a-ε) - a
c = :orange
plot!([a-δ₀,a-δ₀], [0,f(a-δ₀)], color=c, label="")
plot!([a,a], [0,f(a)], color=c, lw=2, label="x = $a")
plot!([a+δ₁,a+δ₁], [0,f(a+δ₁)], color=c, label="")
plot!([0,a-δ₀], [f(a-δ₀),f(a-δ₀)], ls=:dash, color=c, label="y = 1/$a+$ε")
plot!([0,a], [f(a),f(a)], color=c, ls=:dash, lw=2, label="y = 1/$a")
plot!([0,a+δ₁], [f(a+δ₁),f(a+δ₁)], ls=:dashdot, color=c, label="y = 1/$a-$ε")
```

## 一様収束


### 函数列の各点収束の定義


**各点収束の定義:** 区間 $I$ 上の函数の列 $f_1(x), f_2(x), f_3(x), \ldots$ が函数 $\varphi(x)$ に**各点収束**するとは, **各 $x\in I$ ごとに,** 任意の $\eps>0$ に対して, ある番号 $N$ が存在して,

$$
|f_n(x) - f(x)| < \eps \quad (n\geqq N)
$$

が成立することだと定める.

$$
|f_n(x) - \varphi(x)| < \eps \quad (n\geqq N,\; x\in I)
$$

が成立することだと定める. すなわち, 各 $x\in I$ ごとに, 

$$
f_n(x)\to \varphi(x) \quad (n\to\infty)
$$

が成立するとき, 函数列 $f_n(x)$ は $I$ 上で $\varphi(x)$ に各点収束するという. 各点収束においては, 各点 $x\in I$ ごとに $f_n(x)$ の $\varphi(x)$ への収束の速さは全く違っていてもよい. $\QED$


**問題:** 閉区間 $I = [0,1]$ 上で函数列 $f_n(x)=x^n$ は函数

$$
\varphi(x) = \begin{cases}
0 & (0\leqq x < 1) \\
1 & (x = 1) \\
\end{cases}
$$

に各点収束することを示せ.  $0\leqq x<1$ が $1$ に近ければ近いほど, $f_n(x)=x^n$ の $0$ に収束する速さは遅くなることに注意せよ. $\QED$

```julia
f(n,x) = x^n

x = 0:0.005:1
p1 = plot(xlims=(0,1), legend=:topleft)
cycls = [:solid, :dash, :dashdot, :dashdotdot]
for n in 1:12
    plot!(x, f.(n,x), label="n = $n", ls=cycls[mod1(n, lastindex(cycls))])
end
plot(p1, size=(360,370), aspect_ratio=1, title="y = x^n", lw=1.5)
```

### 函数列の一様収束の定義


**一様収束の定理:** 区間 $I$ 上の函数の列 $f_1(x), f_2(x), f_3(x), \ldots$ が函数 $\varphi(x)$ に**一様収束**するとは, 任意の $\eps>0$ に対して, ある番号 $N$ が存在して, $x\geqq N$ ならば, **すべての $x\in I$ について一様に**

$$
|f_n(x) - \varphi(x)| < \eps
$$

が成立することだと定める. $\QED$


**注意:** 函数列 $f_n(x)$ の数値計算では, 区間 $I$ 内の無限個の点から有限個の点 $x_1<\cdots<x_L$ ($x_i\in I$)を選んで, 函数 $f_n(x)$ の代わりに配列 $[f_n(x_1),\ldots,f_n(x_L)]$ を扱うことが普通である. そのとき収束するかどうかの判定を $f_n(x_i)$ と $f_{n+1}(x_i)$ の違いの大きさがすべての $i=1,\ldots,L$ について一斉に最初に決めておいた「許される誤差」 $\eps>0$ より小さくなったかどうかで判定することは, 一様収束に近い考え方で収束を判定していることになる. $\QED$


**問題:** 区間 $I$ 上の函数 $\varphi(x)$ と函数の列 $f_n(x)$ について以下の条件が互いに同値であることを示せ:

(1) $f_n$ は $\varphi$ に $I$ 上一様収束する.

(2) $I$ 上の函数のsupノルム $\|\ \|_\infty$ について, $n\to\infty$ のとき $\|f_n-\varphi\|_\infty\to 0$ が成立する.

(3) $0$ に収束する非負実数の列 $c_n$ で $n\to\infty$ のとき $|f_n(x)-\varphi(x)|\leqq c_n$ ($x\in I$)を満たすものが存在する. 

**解答例:** (1)ならば(2)を示そう. 条件(1)を仮定し, $\eps>0$ とする. (1)より, ある番号 $N$ が存在して, $n\geqq N$ ならばすべての $x\in I$ について $|f_n(x)-\varphi(x)|<\eps/2$ となる. そのとき, $n\geqq N$ ならば

$$
\|f_n-\varphi\|_\infty = \sup_{x\in I}|f_n(x)-\varphi(x)| \leqq \frac{\eps}{2}<\eps.
$$

これで, $\|f_n-\varphi\|_\infty\to 0$ となることが示された. (1)ならば(2)であることがわかった.

(2)ならば(1)を示そう. (2)を仮定し, $\eps>0$ とする. (2)より, ある番号 $N$ が存在して, $n\geqq N$ ならば $\ds\|f_n-\varphi\|_\infty<\eps$ となる. そのとき, $n\geqq N$ かつ $x\in I$ ならば

$$
|f_n(x)-\varphi(x)|\leqq \sup_{x\in I}|f_n(x)-\varphi(x)| = \|f_n-\varphi\|_\infty < \eps.
$$

これで, $f_n$ が $\varphi$ に $I$ 上一様収束することが示された. (2)ならば(1)であることがわかった.

(2)ならば(3)を示そう. (2)の仮定のもとで, $c_n=\|f_n-\varphi\|_\infty$ とおくと, $c_n$ は $0$ に収束する非負実数列であり, $x\in I$ のとき

$$
|f_n(x)-\varphi(x)| \leqq \sup_{x\in I}|f_n(x)-\varphi(x)| = \|f_n-\varphi\|_\infty = c_n.
$$

これで(2)ならば(3)であることがわかった.

(3)ならば(2)を示そう. (3)の仮定のもとで, $n\to\infty$ のとき

$$
\|f_n-\varphi\|_\infty = \sup_{x\in I}|f_n(x)-\varphi(x)| \leqq c_n \to 0.
$$

これで(3)ならば(2)であることがわかった. $\QED$

**注意:** 特に, supノルムに関する収束と一様収束は同じ意味であることがわかった. 一様収束を扱うときにはsupノルムを使うと議論がシンプルになりやすい. $\QED$


**問題:** 閉区間 $I = [0,1]$ 上で函数列 $f_n(x)=x^n$ は函数

$$
\varphi(x) = \begin{cases}
0 & (0\leqq x < 1) \\
1 & (x = 1) \\
\end{cases}
$$

に一様収束**しない**ことを示せ. 

**解答例:** $0<x<1$ のとき $|x| \geqq 1/2^{1/n}$ とすると, $f_n(x)\geqq 1/2$ となる. これで, $f_n(x)$ と $\varphi(x)$ の差の絶対値の大きさをすべての $x\in I$ について一斉に $1/2$ より小さくできないことがわかった. $\QED$

```julia
f(n,x) = x^n

x = 0:0.005:1

n = 3
p1 = plot(xlims=(-0.01,1.01), legend=:topleft, aspect_ratio=1)
plot!(x, f.(n,x), label="y = x^$n")
plot!(x[1:end-1], 1/2*fill(1, size(x[1:end-1])), label="", color=:red, ls=:dash, fill=(0.1, 0))
plot!([1,1], [1/2,1], label="", color=:red, alpha=0.1, lw=3)
plot!([1/2^(1/n), 1/2^(1/n)], [0, 1/2], label="x = 1/2^{1/$n}", color=:black, ls=:dash)

n = 9
p2 = plot(xlims=(-0.01,1.01), legend=:topleft, aspect_ratio=1)
plot!(x, f.(n,x), label="y = x^$n")
plot!(x[1:end-1], 1/2*fill(1, size(x[1:end-1])), label="", color=:red, ls=:dash, fill=(0.1, 0))
plot!([1,1], [1/2,1], label="", color=:red, alpha=0.1, lw=3)
plot!([1/2^(1/n), 1/2^(1/n)], [0, 1/2], label="x = 1/2^{1/$n}", color=:black, ls=:dash)

plot(p1, p2, size=(600, 250))
```

### 連続函数列の一様収束先も連続になること

**定理:** 区間 $I$ 上の連続な函数の列 $f_n$ が函数 $\varphi$ に一様収束するとき, 収束先の $\varphi(x)$ も $I$ 上の連続函数になる. 

**証明:** 区間 $I$ 上の連続な函数の列 $f_n$ が函数 $\varphi$ に一様収束すると仮定し, $\eps>0$ であるとする. $f_n$ が $\varphi$ に一様収束することより, ある番号 $N$ が存在して, $n\geqq N$ かつ $x\in I$ ならば $\ds|f_n(x)-\varphi(x)|<\frac{\eps}{3}$ となる. $x_0\in I$ を任意に取る. 函数 $f_N$ の $x_0$ における連続性より, ある $\delta>0$ が存在して, $x\in I$ かつ $|x-x_0|<\delta$ ならば $\ds|f_N(x)-f_N(x_0)|<\frac{\eps}{3}$ となる. ゆえに, $x\in I$ かつ $|x-x_0|<\delta$ ならば

$$
|\varphi(x)-\varphi(x_0)| \leqq
|\varphi(x)-f_n(x)| +
|f_N(x)-f_N(x_0)| +
|f_N(x_0)-\varphi(x_0)| < 
\frac{\eps}{3}+\frac{\eps}{3}+\frac{\eps}{3} = \eps.
$$

これで, $\varphi(x)$ が任意の $x_0\in I$ で連続なこと, すなわち, $\varphi$ が $I$ 上の連続函数であることが示された. $\QED$


**定理:** 区間 $I$ 上の函数の列 $f_n(x)$ について, 函数列

$$
F_n(x) = \sum_{k=1}^n |f_k(x)|
$$

が有限の値を持つ函数に $I$ 上一様収束するとき, 函数項級数 $\ds\sum_{n=1}^\infty f_n(x)$ も一様収束する. このとき, その函数項級数は**一様絶対収束**するという. $\QED$

**証明:** $\ds F_n(x) = \sum_{k=1}^n|f_k(x)|$ は有限の値を持つ函数 $\Phi(x)$ に一様収束すると仮定する. $F_n$ は $\Phi$ に一様収束しているので, $\|F_n - \Phi\|_\infty\to 0$ かつ $\ds\Phi(x)=\sum_{k=1}^\infty |f_k(x)|$ ($x\in I$) となる. このとき, $x\in I$ かつ $m\geqq n$ とすると,

$$
\begin{aligned}
\left|\sum_{k=1}^n f_k(x) - \sum_{k=1}^m f_k(x)\right| &=
\left|-\sum_{k=n+1}^m f_k(x)\right| 
\\ &\leqq
\sum_{k=n+1}^m |f_k(x)| =
\sum_{k=1}^m |f_k(x)| - \sum_{k=1}^n |f_k(x)| 
\\ &\leqq
\sum_{k=1}^\infty |f_k(x)| - \sum_{k=n+1}^m |f_k(x)| =
\Phi(x) - F_n(x) = |F_n(x) - \Phi(x)|
\\ & \leqq
\sup_{x\in I}|F_n(x)-\Phi(x)|\leqq
\|F_n - \Phi\|_\infty.
\end{aligned}
$$

各 $x\in I$ ごとに $\ds F_n(x)=\sum_{k=1}^n |f_k(x)|$ が有限の値に収束することから, $\ds\sum_{k=1}^n f_k(x)$ も収束するので(絶対収束), その収束先を $\varphi(x)$ と書く. すぐ上で示した不等式で $m\to\infty$ とすると, 任意の番号 $n$ と $x\in I$ について,

$$
\left|\sum_{k=1}^n f_k(x) - \varphi(x)\right| \leqq \|F_n - \Phi\|_\infty
$$

が得られる. 右辺は $x$ に依存しない数列であり $0$ に収束するので, $\ds\sum_{k=1}^n f_k(x)$ が $\varphi(x)$ に一様収束していることがわかる. $\QED$


次の定理は以上の2つの定理からただちに得られる.

**定理:** すべての $f_n(x)$ が区間 $I$ 上の連続函数であり, $\ds\sum_{n=1}^\infty f_n(x)$ が $I$ 上一様絶対収束するならば, その収束先も連続函数になる. $\QED$


**定理:** 区間 $I$ 上の函数の列 $f_n(x)$ に対して, 非負実数列 $c_n$ で

$$
|f_n(x)| \leqq c_n \quad (x\in I), \qquad \sum_{n=1}^\infty c_n < \infty
$$

を満たすものが存在するならば, 函数列 $\ds\sum_{n=1}^\infty f_n(x)$ は $I$ 上一様絶対収束する. 

**証明:** 非負実数列 $c_n$ で

$$
|f_n(x)| \leqq c_n \quad (x\in I), \qquad C = \sum_{n=1}^\infty c_n < \infty
$$

を満たすものが存在すると仮定する. そのとき, 

$$
\sum_{n=1}^\infty |f_n(x)|\leqq \sum_{n=1}^\infty c_n < \infty \qquad (x\in I)
$$

となるので, $\ds\sum_{n=1}^\infty |f_n(x)|$ は有限の値に収束する. その値を $\Phi(x)$ と書く. このとき, $x\in I$ ならば

$$
0\leqq
\Phi(x) - \sum_{k=1}^n |f_k(x)| = 
\sum_{k=n+1}^\infty |f_k(x)| \leqq
\sum_{k=n+1}^\infty c_k =
C - \sum_{k=1}^n c_k.
$$

$\ds C - \sum_{k=1}^n c_k$ は $n\to\infty$ で $0$ に収束し, $x$ に依存しないので, $\ds \sum_{k=1}^n |f_k(x)|$ が $\Phi(x)$ に一様収束することもわかった. これで, 函数列 $\ds\sum_{n=1}^\infty f_n(x)$ は $I$ 上一様絶対収束することがわかった. $\QED$

**注意:** 上の定理を使えば, 収束先の函数が不明であっても収束することがわかる場合がある. $\QED$


**問題:** $s>1$ の函数 $f_n(s)$ を

$$
f_n(s) = \frac{1}{n^s}
$$

と定める. $1$ より真に大きい実数 $a$ を任意に取る. このとき $s\geqq a$ において, 函数項級数

$$
\sum_{n=1}^\infty f_n(s) = \sum_{n=1}^\infty \frac{1}{n^s}
$$

が一様絶対収束することを示せ.


**解答例:** 各 $n\geqq 1$ ごとに $f_n(s)=1/n^s$ は $s$ に関する単調減少函数である. ゆえに

$$
|f_n(s)| =\frac{1}{n^s} \leqq \frac{1}{n^a} \quad (s\geqq a).
$$

そして, $a>1$ より $\ds\sum_{n=1}^\infty\frac{1}{n^a}$ は有限の値に収束する. 以上によって, $\ds\sum_{n=1}^\infty f_n(s) = \sum_{n=1}^\infty\frac{1}{n^s}$ は $s\geqq a$ で絶対収束する. $\QED$


**注意:** 上の問題の $a>1$ はいくらでも $1$ に近付けることができるので, 

$$
\zeta(s) = \sum_{n=1}^\infty \frac{1}{n^s} \quad (s>1)
$$

は $s>1$ における連続函数になることがわかる. $\QED$

**注意:** 実際にはもっとよいことが言える. $\zeta(s)$ は $s>1$ でいくらでも項別微分可能であり, $s=1$ を除いた複素平面全体に解析接続される. $\QED$

```julia
g(N, s) = sum(n->1/n^s, 1:N)
s = 1.01:0.01:5.00

p1 = plot(title="sum of 1/n^s for n=1,...,N")
plot!(ylims=(0.8, 5), xlabel="s")
plot!(s, zeta.(s), label="zeta(s)", lw=2)
for (ls, N) in [(:dot,3), (:dash, 10), (:dashdot, 30), (:dashdotdot, 100), (:dashdotdot, 300), (:dashdotdot, 1000)]
    plot!(s, g.(N,s), label="N = $N", ls=ls, lw=1.5)
end
plot(p1, size=(500, 350))
```

**問題:** $f(x)$ は $\R$ 上の連続函数であり, $|x|\to\infty$ のとき $f(x)\to 0$ となっていると仮定する. このとき, 函数列 $\ds f_n(x)=f\left(x+\frac{1}{n}\right)$ が $f(x)$ に $\R$ 上一様収束することを示せ.

**解答例:** 任意に $\eps>0$ を取る. 上の方の問題の解答例で示したように, $f(x)$ は $\R$ 上一様連続になる. 
ゆえに, ある $\delta>0$ が存在して, $x,y\in\R$ かつ $|y-x|<\delta$ ならば $|f(y)-f(x)|<\eps$ となる. そのとき $\ds n>\frac{1}{\delta}$ とすると,

$$
\left|f_n(x)-f(x)\right|=
\left|f\left(x+\frac{1}{n}\right)-f(x)\right|<
\eps
$$

となる. これで, $f_n$ が $f$ に $\R$ 上一様収束することがわかった. $\QED$


### Peano曲線

以下のプログラムのように $n$ について帰納的に定義された $\leqq t\leqq 1$ の連続函数 $x_n(t)=\mathrm{Peano}\_x(n,t)$ と $y_n(t)=\mathrm{Peano}\_y(n,t)$ は $n\to\infty$ で一様収束し, その収束先 $x(t)$, $y(t)$ は平面上の連続曲線 $(x(t),y(t))$ ($0\leqq t\leqq 1$) を定める. その曲線は**Peano曲線**と呼ばれており, 正方形 $[0,1]\times [0,1]$ の内側を埋め尽くす.


**問題:** $x_n(t)$, $y_n(t)$ がともに閉区間 $[0,1]$ 上で一様収束することを示せ.

**解答例:** $f_n(t)$ は $x_n(t)$ または $y_n(t)$ であるとする. そのとき

$$
|f_n(t) - f_{n-1}(t)| \leqq \frac{1}{2^n} \quad (0\leqq t\leqq 1)
$$

となっているので(下の方のプロットを参照せよ), $0\leqq t\leqq 1$ のとき

$$
\sum_{n=1}^\infty |f_n(t)-f_{n-1}(t)| \leqq \sum_{n=1}^\infty \frac{1}{2^n} = 1 < \infty
$$

となる. ゆえに函数項級数

$$
f_0(t) + (f_1(t)-f_0(t)) + (f_2(t)-f_1(t)) + (f_3(t) - f_2(t)) + \cdots
$$

は閉区間 $[0,1]$ 上で一様絶対収束する. これは $f_n(t)$ が一様収束することを意味する. $\QED$

**注意:** $0\leqq t\leqq 1$ のとき

$$
f(t) = f_n(t) + \sum_{k=n}^\infty (f_{k+1}(t)-f_k(t)), \quad
\sum_{k=n}^\infty |f_{k+1}(t)-f_k(t)| \leqq \sum_{k=n}^\infty\frac{1}{2^{k+1}} = \frac{1}{2^n}
$$

なので, 

$$
|f_n(t)-f(t)|\leqq \frac{1}{2^n} \quad (0\leqq t\leqq 1)
$$

であることもわかる. $\QED$


**問題:** $x_n(t)$, $y_n(t)$ の極限をそれぞれ $x(t)$, $y(t)$ と書く. そのとき, 任意の正方形上の点 $(a,b)\in[0,1]\times[0,1]$ に対して, ある実数 $t\in[0,1]$ で $(x(t),y(t))=(a,b)$ を満たすものが存在することを示せ.

**解答例:** $x_n(t)$, $y_n(t)$ の作り方より(下の方のプロットを参照せよ), 任意の正の整数 $n$ に対して, ある $t_n\in[0,1]$ で

$$
|x_n(t_n)-a|\leqq \frac{1}{2^n}, \quad
|y_n(t_n)-b|\leqq \frac{1}{2^n}
$$

を満たすものが存在する.  数列 $t_n$ は閉区間 $[0,1]$ 上の数列なので, そのある部分列 $t_{n_k}$ ($1\leqq n_1<n_2<\cdots$) で収束するものが存在する. その収束先を $t\in[0,1]$ と書く. 任意に $\eps > 0$ を取る. $x(t)$, $y(t)$ の連続性より, 十分に $k$ を大きくすると

$$
|x(t)-x(t_{n_k})|\leqq\eps, \quad
|y(t)-y(t_{n_k})|\leqq\eps.
$$

ゆえに, 以上と上の注意の内容を合わせると, $k\to\infty$ で,

$$
\begin{aligned}
&
|x(t)-a|
\leqq |x(t)-x(t_{n_k})| + |x(t_{n_k})-x_{n_k}(t_{n_k})| + |x_{n_k}(t_{n_k})-a| 
\leqq \eps + \frac{1}{2^{n_k}} + \frac{1}{2^{n_k}} \to \eps,
\\&
|y(t)-b|
\leqq |y(t)-y(t_{n_k})| + |y(t_{n_k})-y_{n_k}(t_{n_k})| + |y_{n_k}(t_{n_k})-b| 
\leqq \eps + \frac{1}{2^{n_k}} + \frac{1}{2^{n_k}} \to \eps.
\end{aligned}
$$

$\eps>0$ はいくらでも小さくできるので, $(x(t),y(t))=(a,b)$. $\QED$

```julia
peano_x₀(t) = t < 1/2 ? 1/2 : t
peano_y₀(t) = t < 1/2 ? t : 1/2

n = 0
t = 0:1/(2*4^n):1
x = peano_x₀.(t)
y = peano_y₀.(t)
plot(size=(200,200))
plot!(title="Peano($n)", titlefontsize=10)
plot!(xlims=(0,1), ylims=(0,1))
plot!(aspect_ratio=1, legend=:false)
plot!(x, y)
```

```julia
function peano_x(n, t)
    if n == 0
        peano_x₀(t)
    else
        if t < 1/(2*4^n)
            1/(2*2^n)
        elseif t < 1/4
            peano_y(n-1, 4t)/2
        elseif t < 2/4
            peano_x(n-1, 4t-1)/2
        elseif t < 3/4
            (1-peano_x(n-1, 3-4t))/2 + 1/2
        else
            (1-peano_y(n-1, 4-4t))/2 + 1/2
        end
    end
end

function peano_y(n, t)
    if n == 0
        peano_y₀(t)
    else
        if t < 1/(2*4^n)
            2^n*t
        elseif t < 1/4
            peano_x(n-1, 4t)/2
        elseif t < 2/4
            peano_y(n-1, 4t-1)/2 + 1/2
        elseif t < 3/4
            peano_y(n-1, 3-4t)/2 + 1/2
        else
            peano_x(n-1, 4-4t)/2
        end
    end
end

P = Plots.Plot[]
for n in 0:8
    t = 0:1/(2*4^n):1
    x = peano_x.(n,t)
    y = peano_y.(n,t)
    p = plot()
    plot!(title="Peano($n)", titlefontsize=10)
    plot!(xlims=(0,1), ylims=(0,1))
    plot!(aspect_ratio=1, legend=:false)
    plot!(x, y)
    push!(P, p)
end
plot(P..., size=(800,800), layout=@layout(grid(3,3)))
pngplot()
```

```julia
P = Plots.Plot[]
for n in 0:8
    t = 0:1/(2*4^n):1
    x = peano_x.(n,t)
    y = peano_y.(n,t)
    p = plot()
    plot!(title="Peano($n)", titlefontsize=10)
    plot!(xlims=(0,1), ylims=(0,1))
    plot!(aspect_ratio=1, legend=:false)
    plot!(t, x)
    plot!(t, y)
    push!(P, p)
end
plot(P..., size=(800,800), layout=@layout(grid(3,3)))
pngplot()
```

```julia
n = 8
px = plot()
py = plot()
t = 0:1/(2*4^n):1
x = peano_x.(n,t)
y = peano_y.(n,t)
plot!(px, title="Peano_x($n)", titlefontsize=10)
plot!(py, title="Peano_y($n)", titlefontsize=10)
plot!(px, xlims=(0,1), ylims=(0,1))
plot!(py, xlims=(0,1), ylims=(0,1))
plot!(px, legend=:topleft)
plot!(py, legend=:topleft)
plot!(px, t, x, label="$n", lw=0.3)
plot!(py, t, y, label="$n", lw=0.3)
plot(px, py, size=(600,500), layout=@layout(grid(2,1)), legend=false)
pngplot()
```

```julia
P = Plots.Plot[]
for n in 1:9
    t = 0:1/(2*4^n):1
    dx = peano_x.(n,t) - peano_x.(n-1,t)
    dy = peano_y.(n,t) - peano_y.(n-1,t)
    p = plot()
    plot!(title="Peano($n) - Peano($(n-1))", titlefontsize=10)
    plot!(xlims=(0,1), ylims=(-1/2,1/2))
    plot!(legend=:false)
    plot!(t, dx, lw=0.7)
    plot!(t, dy, lw=0.7)
    push!(P, p)
end
plot(P..., size=(800,600))
pngplot()
```
