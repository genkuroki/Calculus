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
    display_name: Julia 1.9.4
    language: julia
    name: julia-1.9
---

# 12 Fourier解析

黒木玄

2018-06-28～2019-06-16, 2023-06-22

* Copyright 2018,2019,2023 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

* http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/12%20Fourier%20analysis.ipynb

* https://genkuroki.github.io/documents/Calculus/12%20Fourier%20analysis.pdf

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
<div class="toc"><ul class="toc-item"><li><span><a href="#Riemann-Lebesgueの定理" data-toc-modified-id="Riemann-Lebesgueの定理-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Riemann-Lebesgueの定理</a></span><ul class="toc-item"><li><span><a href="#階段函数" data-toc-modified-id="階段函数-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>階段函数</a></span></li><li><span><a href="#Riemann-Lebesgueの定理とその証明" data-toc-modified-id="Riemann-Lebesgueの定理とその証明-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>Riemann-Lebesgueの定理とその証明</a></span></li></ul></li><li><span><a href="#Fourier変換の逆変換の収束" data-toc-modified-id="Fourier変換の逆変換の収束-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Fourier変換の逆変換の収束</a></span><ul class="toc-item"><li><span><a href="#Fourier変換とFourier逆変換の定義" data-toc-modified-id="Fourier変換とFourier逆変換の定義-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>Fourier変換とFourier逆変換の定義</a></span></li><li><span><a href="#Dirichlet核" data-toc-modified-id="Dirichlet核-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>Dirichlet核</a></span></li><li><span><a href="#Dirichlet積分の公式" data-toc-modified-id="Dirichlet積分の公式-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Dirichlet積分の公式</a></span></li><li><span><a href="#Riemannの局所性定理" data-toc-modified-id="Riemannの局所性定理-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>Riemannの局所性定理</a></span></li><li><span><a href="#Fourier変換の逆変換の収束性-(Diniの条件)" data-toc-modified-id="Fourier変換の逆変換の収束性-(Diniの条件)-2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>Fourier変換の逆変換の収束性 (Diniの条件)</a></span></li></ul></li><li><span><a href="#たたみ込み積と総和核" data-toc-modified-id="たたみ込み積と総和核-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>たたみ込み積と総和核</a></span><ul class="toc-item"><li><span><a href="#たたみ込み積" data-toc-modified-id="たたみ込み積-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>たたみ込み積</a></span></li><li><span><a href="#微分やたたみ込み積とFourier変換の関係" data-toc-modified-id="微分やたたみ込み積とFourier変換の関係-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>微分やたたみ込み積とFourier変換の関係</a></span></li><li><span><a href="#総和核" data-toc-modified-id="総和核-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>総和核</a></span></li><li><span><a href="#総和核とFourier展開の関係" data-toc-modified-id="総和核とFourier展開の関係-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>総和核とFourier展開の関係</a></span></li><li><span><a href="#総和核の例" data-toc-modified-id="総和核の例-3.5"><span class="toc-item-num">3.5&nbsp;&nbsp;</span>総和核の例</a></span><ul class="toc-item"><li><span><a href="#Dirichlet核は総和核ではない" data-toc-modified-id="Dirichlet核は総和核ではない-3.5.1"><span class="toc-item-num">3.5.1&nbsp;&nbsp;</span>Dirichlet核は総和核ではない</a></span></li><li><span><a href="#Fejér核" data-toc-modified-id="Fejér核-3.5.2"><span class="toc-item-num">3.5.2&nbsp;&nbsp;</span>Fejér核</a></span></li><li><span><a href="#Poisson核" data-toc-modified-id="Poisson核-3.5.3"><span class="toc-item-num">3.5.3&nbsp;&nbsp;</span>Poisson核</a></span></li><li><span><a href="#Gauss核" data-toc-modified-id="Gauss核-3.5.4"><span class="toc-item-num">3.5.4&nbsp;&nbsp;</span>Gauss核</a></span></li></ul></li></ul></li><li><span><a href="#Fourier級数の収束" data-toc-modified-id="Fourier級数の収束-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Fourier級数の収束</a></span><ul class="toc-item"><li><span><a href="#Fourier級数の定義" data-toc-modified-id="Fourier級数の定義-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>Fourier級数の定義</a></span></li><li><span><a href="#Fourier級数展開のDirichlet核" data-toc-modified-id="Fourier級数展開のDirichlet核-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Fourier級数展開のDirichlet核</a></span></li><li><span><a href="#Fourier級数に関するRiemannの局所性定理" data-toc-modified-id="Fourier級数に関するRiemannの局所性定理-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>Fourier級数に関するRiemannの局所性定理</a></span></li><li><span><a href="#Fourier級数の収束-(Diniの条件)" data-toc-modified-id="Fourier級数の収束-(Diniの条件)-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>Fourier級数の収束 (Diniの条件)</a></span></li><li><span><a href="#Besselの不等式" data-toc-modified-id="Besselの不等式-4.5"><span class="toc-item-num">4.5&nbsp;&nbsp;</span>Besselの不等式</a></span></li><li><span><a href="#区分的に連続微分可能な連続函数の場合" data-toc-modified-id="区分的に連続微分可能な連続函数の場合-4.6"><span class="toc-item-num">4.6&nbsp;&nbsp;</span>区分的に連続微分可能な連続函数の場合</a></span></li><li><span><a href="#一点でLipschitz条件を満たす函数の場合" data-toc-modified-id="一点でLipschitz条件を満たす函数の場合-4.7"><span class="toc-item-num">4.7&nbsp;&nbsp;</span>一点でLipschitz条件を満たす函数の場合</a></span></li><li><span><a href="#局所的に一様Lipschitz条件を満たす函数の場合" data-toc-modified-id="局所的に一様Lipschitz条件を満たす函数の場合-4.8"><span class="toc-item-num">4.8&nbsp;&nbsp;</span>局所的に一様Lipschitz条件を満たす函数の場合</a></span></li><li><span><a href="#Gibbs現象" data-toc-modified-id="Gibbs現象-4.9"><span class="toc-item-num">4.9&nbsp;&nbsp;</span>Gibbs現象</a></span></li><li><span><a href="#Fourier級数の総和核" data-toc-modified-id="Fourier級数の総和核-4.10"><span class="toc-item-num">4.10&nbsp;&nbsp;</span>Fourier級数の総和核</a></span><ul class="toc-item"><li><span><a href="#Fourier級数の総和核の定義" data-toc-modified-id="Fourier級数の総和核の定義-4.10.1"><span class="toc-item-num">4.10.1&nbsp;&nbsp;</span>Fourier級数の総和核の定義</a></span></li><li><span><a href="#Fourier級数のDirichlet核は総和核ではない" data-toc-modified-id="Fourier級数のDirichlet核は総和核ではない-4.10.2"><span class="toc-item-num">4.10.2&nbsp;&nbsp;</span>Fourier級数のDirichlet核は総和核ではない</a></span></li><li><span><a href="#Fourier級数のFejér核" data-toc-modified-id="Fourier級数のFejér核-4.10.3"><span class="toc-item-num">4.10.3&nbsp;&nbsp;</span>Fourier級数のFejér核</a></span></li><li><span><a href="#Fourier級数のPoisson核" data-toc-modified-id="Fourier級数のPoisson核-4.10.4"><span class="toc-item-num">4.10.4&nbsp;&nbsp;</span>Fourier級数のPoisson核</a></span></li><li><span><a href="#Fourier級数のGauss核" data-toc-modified-id="Fourier級数のGauss核-4.10.5"><span class="toc-item-num">4.10.5&nbsp;&nbsp;</span>Fourier級数のGauss核</a></span></li></ul></li></ul></li><li><span><a href="#三角函数へのFourier級数論の応用" data-toc-modified-id="三角函数へのFourier級数論の応用-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>三角函数へのFourier級数論の応用</a></span><ul class="toc-item"><li><span><a href="#cosecとcotの部分分数展開" data-toc-modified-id="cosecとcotの部分分数展開-5.1"><span class="toc-item-num">5.1&nbsp;&nbsp;</span>cosecとcotの部分分数展開</a></span></li><li><span><a href="#sinの無限積表示" data-toc-modified-id="sinの無限積表示-5.2"><span class="toc-item-num">5.2&nbsp;&nbsp;</span>sinの無限積表示</a></span></li><li><span><a href="#ガンマ函数とsinの関係" data-toc-modified-id="ガンマ函数とsinの関係-5.3"><span class="toc-item-num">5.3&nbsp;&nbsp;</span>ガンマ函数とsinの関係</a></span></li><li><span><a href="#Wallisの公式" data-toc-modified-id="Wallisの公式-5.4"><span class="toc-item-num">5.4&nbsp;&nbsp;</span>Wallisの公式</a></span></li><li><span><a href="#Stirlingの近似公式" data-toc-modified-id="Stirlingの近似公式-5.5"><span class="toc-item-num">5.5&nbsp;&nbsp;</span>Stirlingの近似公式</a></span></li><li><span><a href="#ゼータ函数の正の偶数での特殊値" data-toc-modified-id="ゼータ函数の正の偶数での特殊値-5.6"><span class="toc-item-num">5.6&nbsp;&nbsp;</span>ゼータ函数の正の偶数での特殊値</a></span></li><li><span><a href="#多重ゼータ値-ζ(2,2,...,2)" data-toc-modified-id="多重ゼータ値-ζ(2,2,...,2)-5.7"><span class="toc-item-num">5.7&nbsp;&nbsp;</span>多重ゼータ値 ζ(2,2,...,2)</a></span></li><li><span><a href="#Lobachevskyの公式-(Dirichlet積分の公式の一般化の1つ)" data-toc-modified-id="Lobachevskyの公式-(Dirichlet積分の公式の一般化の1つ)-5.8"><span class="toc-item-num">5.8&nbsp;&nbsp;</span>Lobachevskyの公式 (Dirichlet積分の公式の一般化の1つ)</a></span></li></ul></li><li><span><a href="#Poissonの和公式" data-toc-modified-id="Poissonの和公式-6"><span class="toc-item-num">6&nbsp;&nbsp;</span>Poissonの和公式</a></span><ul class="toc-item"><li><span><a href="#Poissonの和公式とその証明" data-toc-modified-id="Poissonの和公式とその証明-6.1"><span class="toc-item-num">6.1&nbsp;&nbsp;</span>Poissonの和公式とその証明</a></span></li><li><span><a href="#Theta-zero-value-Θ(t)-のモジュラー変換性" data-toc-modified-id="Theta-zero-value-Θ(t)-のモジュラー変換性-6.2"><span class="toc-item-num">6.2&nbsp;&nbsp;</span>Theta zero value Θ(t) のモジュラー変換性</a></span></li><li><span><a href="#Lerchの超越函数へのPoissonの和公式の応用" data-toc-modified-id="Lerchの超越函数へのPoissonの和公式の応用-6.3"><span class="toc-item-num">6.3&nbsp;&nbsp;</span>Lerchの超越函数へのPoissonの和公式の応用</a></span><ul class="toc-item"><li><span><a href="#Lerchの超越函数" data-toc-modified-id="Lerchの超越函数-6.3.1"><span class="toc-item-num">6.3.1&nbsp;&nbsp;</span>Lerchの超越函数</a></span></li><li><span><a href="#Lipschitzの和公式＝Lerchの函数等式" data-toc-modified-id="Lipschitzの和公式＝Lerchの函数等式-6.3.2"><span class="toc-item-num">6.3.2&nbsp;&nbsp;</span>Lipschitzの和公式＝Lerchの函数等式</a></span></li><li><span><a href="#Hurwitzの函数等式" data-toc-modified-id="Hurwitzの函数等式-6.3.3"><span class="toc-item-num">6.3.3&nbsp;&nbsp;</span>Hurwitzの函数等式</a></span></li><li><span><a href="#Riemannのゼータ函数の函数等式" data-toc-modified-id="Riemannのゼータ函数の函数等式-6.3.4"><span class="toc-item-num">6.3.4&nbsp;&nbsp;</span>Riemannのゼータ函数の函数等式</a></span></li></ul></li></ul></li></ul></div>
<!-- #endregion -->

```julia
using Base.MathConstants
using Base64
using Printf
using Statistics
const e = ℯ
endof(a) = lastindex(a)
linspace(start, stop, length) = range(start, stop=stop, length=length)

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
#sympy.init_printing(order="lex") # default
#sympy.init_printing(order="rev-lex")

using SpecialFunctions
using QuadGK
```

```julia
# Override the Base.show definition of SymPy.jl:
# https://github.com/JuliaPy/SymPy.jl/blob/29c5bfd1d10ac53014fa7fef468bc8deccadc2fc/src/types.jl#L87-L105

@eval SymPy function Base.show(io::IO, ::MIME"text/latex", x::SymbolicObject)
    print(io, as_markdown("\\displaystyle " * sympy.latex(x, mode="plain", fold_short_frac=false)))
end
@eval SymPy function Base.show(io::IO, ::MIME"text/latex", x::AbstractArray{Sym})
    function toeqnarray(x::Vector{Sym})
        a = join(["\\displaystyle " * sympy.latex(x[i]) for i in 1:length(x)], "\\\\")
        """\\left[ \\begin{array}{r}$a\\end{array} \\right]"""
    end
    function toeqnarray(x::AbstractArray{Sym,2})
        sz = size(x)
        a = join([join("\\displaystyle " .* map(sympy.latex, x[i,:]), "&") for i in 1:sz[1]], "\\\\")
        "\\left[ \\begin{array}{" * repeat("r",sz[2]) * "}" * a * "\\end{array}\\right]"
    end
    print(io, as_markdown(toeqnarray(x)))
end
```

## Riemann-Lebesgueの定理


### 階段函数

空でない区間 $I$ 上の函数で部分集合 $A$ 上で $1$, その外で $0$ になる函数を $\chi_A(x)$ と書く:

$$
\chi_A(x) = \begin{cases}
1 & (x\in A) \\
0 & (x\not\in A).
\end{cases}
$$

区間 $I$ に含まれる**長さが有限**の区間 $I_1,\ldots,I_n$ と数 $\alpha_1,\ldots,\alpha_n$ によって定まる函数

$$
s(x) = \sum_{i=1}^n \alpha_i\chi_A(x) \quad (x\in I)
$$

を区間 $I$ 上の**階段函数**と呼ぶことにする.

$f$ は区間 $I$ 上の函数であるとする. 任意の $\eps>0$ に対してある階段函数 $s(x)$ で

$$
\|f-s\|_1 = \int_I |f(x)-s(x)|\,dx < \eps
$$

を満たすものが存在するとき, $f$ は**階段函数で $L^1$ 近似可能**であるということにする. ここで $I=[a,b], [a,b), (a,b], (a,b)$ のとき, $\int_I$ の意味は $\int_I=\int_a^b$ であるとする. $a=-\infty$ であっても $b=\infty$ であっても構わないものとする.


**問題:** $\R=(-\infty,\infty)$ 上の連続函数 $f(x)$ で $\ds\int_{-\infty}^\infty |f(x)|\,dx < \infty$ を満たすものは階段函数で $L^1$ 近似可能であることを示せ.

**解答例:** 任意に $\eps>0$ を取る $\int_{-\infty}^\infty |f(x)|\,dx < \infty$ より, 十分に $R>0$ を大きくすると, 

$$
\int_{-\infty}^{-R}|f(x)|\,dx + \int_R^\infty |f(x)|\,dx =
\int_{-\infty}^\infty |f(x)|\,dx - \int_{-R}^R |f(x)|\,dx < \frac{\eps}{2}.
$$

$f(x)$ は $[-R,R]$ 上で一様連続なので, 正の整数 $N$ を十分に大きくして, 区間 $[-R,R]$ を $N$ 等分したものを $I_1,\ldots,I_N$ として, $x_i\in I_i$ を任意に取ると,  

$$
|f(x)-f(x_i)|< \frac{\eps}{4R} \quad (x\in I_i,\ i=1,\ldots,N).
$$

ここで, 例えば $\Delta = 2R/N$, $a_i=R+i\Delta$, $I_1=[a_0,a_1)$, $\ldots$, $I_{N-1}=[a_{N-2},a_{N-1}])$, $I_N=[a_{N-1},a_N]$ とおいたと考えてよい. このとき階段函数 $s(x)$ を

$$
s(x)=\sum_{i=1}^N f(x_i)\chi_{I_i}(x)
$$

と定めると,

$$
|f(x)-f(x_i)| < \frac{\eps}{4R} \quad (-R\leqq x\leqq R).
$$

このとき

$$
\int_{-R}^R|f(x)-s(x)|\,dx \leqq \frac{\eps}{4R}2R = \frac{\eps}{2}.
$$

したがって,

$$
\begin{aligned}
\|f-s\|_1 &= \int_{-\infty}^\infty|f(x)-s(x)|\,dx 
\\ &=
\int_{-\infty}^{-R} |f(x)-s(x)|\,dx + \int_{-R}^R|f(x)-s(x)|\,dx + \int_R^\infty|f(x)-s(x)|\,dx 
\\ &=
\int_{-\infty}^{-R} |f(x)|\,dx + \int_{-R}^R|f(x)-s(x)|\,dx + \int_R^\infty|f(x)|\,dx 
\\ &< 
\frac{\eps}{2}+\frac{\eps}{2}=\eps.
\end{aligned}
$$

これで示すべきことが示された. $\QED$


**注意:** この注意はLebesgue積分論の知識を前提とする. 一般に区間上の可測函数 $f(x)$ で

$$
\int_I |f(x)|\,dx < \infty
$$

を満たすものを, $I$ 上の **$L^1$ 函数** または**可積分函数**(もしくは**積分可能函数**)と呼ぶ. $I$ 上 $L^1$ 函数は $I$ 上の階段函数で $L^1$ 近似可能であることが知られている. $\QED$


**補題(階段函数に関するRiemann-Lebesgueの定理):** 区間 $I$ 上の階段函数 $s(x)$ について

$$
\int_I s(x) e^{ipx}\,dx \to 0 \quad (|p|\to\infty).
$$

**証明:** $-\infty<a_j\leqq b_j<\infty$, $I_j=[a_j,b_j], [a_j,b_j), (a_j,b_j], (a_j,b_j)$ だとすると, $|p|\to\infty$ のとき,

$$
\int_I \chi_{I_j}(x)e^{ips}\,dx = 
\int_{a_j}^{b_j} e^{ipx}\,dx = 
\left[\frac{e^{ipx}}{ip}\right]_{x=a_j}^{x=b_j} =
\frac{e^{ipb_j} - e^{ipa_j}}{ip} \to 0.
$$

階段函数は $\chi_{I_j}(x)$ の有限一次結合なので補題の結果が成立することもわかる.  $\QED$


**問題** $p\ne 0$ のとき, $-\infty<a\leqq b<\infty$ を満たす実数 $a,b$ の取り方によらずに不等式

$$
\left|\int_a^b e^{ipx}\,dx\right|\leqq \frac{2}{|p|}
$$

が成立することを示し, このようなことが起こる理由について考えよ.

**解答例:**
$$
\left|\int_a^b e^{ipx}\,dx\right| = \left|\frac{e^{ipb}-e^{ipa}}{ip}\right| \leqq 
\frac{|e^{ipb}|+|e^{ipa}|}{|p|} = \frac{2}{|p|}.
$$

ただし, $\leqq$ では三角不等式を使った.

$e^{ipx}$ は周期 $2\pi/|p|$ を持つ函数であり, ちょうど周期分だけ積分すると $0$ になる性質を持っている:

$$
\int_c^{c+2\pi/|p|} e^{ipx}\,dx = 0.
$$

ゆえに, 整数 $n$ で $a+2\pi n/|p| \leqq |b|$ となる最大のものを $N$ と書くと, 

$$
\int_a^b e^{ipx}\,dx = \int_{a+2\pi N/|p|}^b e^{ipx}\,dx, \quad
b - \left(a+\frac{2\pi N}{|p|}\right) < \frac{2\pi}{|p|}.
$$

これより, 

$$
\left|\int_a^b e^{ipx}\,dx\right| =
\left|\int_{a+2\pi n/|p|}^b  e^{ipx}\,dx\right| \leqq
\int_{a+2\pi n/|p|}^b  |e^{ipx}|\,dx =
\int_{a+2\pi n/|p|}^b \,dx < 
\frac{2\pi}{|p|}.
$$

これは上の問題の結果よりは荒い結果だが, この方法ではどのような仕組みで積分の絶対値の値が小さくなるかはよくわかる.  振動によってほとんど打ち消しあって消えて, 積分区間の端の部分で打ち消し合いが起こらない分だけ積分の値が生き残る. 打ち消し合いが不完全な積分区間の幅は小さくなり, 打ち消し合わずに生き残った積分の値の絶対値も小さくなる.  

$\ds\frac{2}{|p|}$ と $\ds\frac{2\pi}{|p|}$ の違いは $\ds \left|\int_{a+2\pi n/|p|}^b  e^{ipx}\,dx\right|$ の評価を上よりも繊細に行えば解消できる.  次数 $\alpha,\beta$ に対して, $e^{i\alpha}$ と $e^{i\beta}$ の距離が最大で $2$ にしかならないことが, $\ds\frac{2}{|p|}$ の分子の $2$ が出て来る理由になっている. $\QED$

```julia
# e^{ipx} を sin(px) に置き換えた場合の図

a = 0.0
b = 10.0
f(p,x) = sin(p*x)
maxa(a,b,p) = a + 2π/abs(p)*fld(b-a, 2π/abs(p))
x = a:0.01:b
PP = []
for p in [5, 20]
    xx = maxa(a,b,p):0.001:b
    P = plot(title="p = $p", titlefontsize=10)
    plot!(legend=false, ylims=(-1.05,1.05))
    plot!(x, f.(p,x))
    hline!([0], color=:cyan, ls=:dot)
    plot!(xx, f.(p,xx), color=:red, fill=(0, 0.5, :red))
    push!(PP, P)
end
plot(PP..., size=(700, 250))
```

**問題:** $p\geqq 1$ であるとし, $f$ は $\R$ 上の連続函数で $\ds\int_{-\infty}^\infty |f(x)|^p\,dx<\infty$ を満たすものであるとする. このとき, $\R$ 上の階段函数 $s$ で $\ds \|f-s\|_p = \left(\int_{-\infty}^\infty |f(x)-s(x)|^p\,dx\right)^{1/p}<\eps$ を満たすものが存在することを示せ. 

**解答例:** 任意に $\eps>0$ を取る. 

$R>0$ に対して, $f_R(x)$ を $|x|\leqq R$ のとき $f(x)$ と定め, $|x|>R$ のとき $0$ と定める. このとき, 

$$
(\|f-f_R\|_p)^p =
\int_{-\infty}^\infty |f(x)-f_R(x)|^p\,dx =
\int_{|x|>R} |f(x)|^p\,dx
$$

より, $R\to\infty$ で $\|f-f_R\|_p\to 0$ となることがわかる.  ゆえに $R>0$ を十分に大きくすると, $\ds\|f-f_R\|_p<\frac{\eps}{2}$ となる.

区間 $[R,-R]$ 上の連続函数 $f_R(x)$ は階段函数 $s$ で一様近似される. すなわち, ある階段函数 $s$ が存在して, $\ds\sup_{|x|\leqq R}|f(x)-s(x)|<\frac{\eps}{2(2R)^{1/p}}$ となる. このとき, $s$ を $\R$ 全体に $[-R,R]$ の外で $0$ になるように拡張したものも $s$ と書くと, 

$$
\|f_R-s\|_p = 
\left(\int_{-R}^R |f(x)-s(x)|^p\right)^{1/p} \leqq
\left(\frac{\eps^p}{2^p(2R)}2R\right)^{1/p} = \frac{\eps}{2}.
$$

Minkowskiの不等式より,

$$
\|f-s\|_p \leqq 
\|f-f_R\|_p + \|f_R-s\|_p <
\frac{\eps}{2}+\frac{\eps}{2} = \eps.
\qquad \QED
$$

**注意:** 上の問題は連続な $L^p$ 函数が階段函数で $L^p$ 近似されるということを意味する. 実際には任意の $L^p$ 函数が階段函数で $L^p$ 近似されることが示される. $\QED$


**問題:** $f$ が $L^p$ 函数であるとき, $y\to 0$ ならば $\|f(\cdot-y)-f\|_p\to 0$ となることを示せ.

**略解例:** $f$ は階段函数で $L^p$ 近似され, $f$ が階段函数ならばこの問題の結論が成立することは容易に示されることから, $f$ が一般の $L^p$ 函数の場合に関するこの問題の結論が成立することが示される. $\QED$


### Riemann-Lebesgueの定理とその証明

**Riemann-Lebesgueの定理:** $f$ は区間 $I$ 上の階段函数で $L^1$ 近似可能な函数であるとする. (Lebesgue積分論を知っている人は, $f(x)$ は区間 $I$ 上の $L^1$ 函数であると仮定してもよい.) このとき,

$$
\int_I f(x)e^{ipx}\,dx \to 0 \quad (|p|\to\infty).
$$

**証明:** 任意に $\eps>0$ を取る. $f(x)$ は階段函数で $L^1$ 近似可能なので, ある階段函数 $\ds s(x)=\sum_{i=1}^n a_i\chi_{I_i}(x)$ で

$$
\|f-s\|_1=\int_I |f(x)-s(x)|\,dx < \frac{\eps}{2}
$$

を満たすものが存在する. 階段函数に関する前節の補題より, ある $R>0$ が存在して, $|p|\geqq R$ ならば

$$
\left|\int_I s(x)e^{ipx}\,dx\right|<\frac{\eps}{2}.
$$

そのとき, 

$$
\begin{aligned}
\left|\int_I f(x)e^{ipx}\,dx\right| &=
\left|\int_I (f(x)-s(x))e^{ipx}\,dx  + \int_I s(x)e^{ipx}\,dx\right| 
\\ &\leqq
\left|\int_I (f(x)-s(x))e^{ipx}\,dx\right| + \left|\int_I s(x)e^{ipx}\,dx\right| 
\\ &\leqq
\int_I |f(x)-s(x)|\,dx + \left|\int_I s(x)e^{ipx}\,dx\right| 
\\ &=
\|f-s\|_1 + \left|\int_I s(x)e^{ipx}\,dx\right| <
\frac{\eps}{2}+\frac{\eps}{2} = \eps.
\end{aligned}
$$

これで $|p|\to\infty$ で $\ds \int_I f(x)e^{ipx}\,dx\to 0$ となることがわかった. $\QED$


**解説:** Riemann-Lebesgueの定理(リーマン・ルベーグの定理)は $|p|$ を大きくすると, $e^{ipx}$ が $x$ についてより細かく振動するようになるので, $e^{ipx}$ と積を取ってから積分すると, 実部と虚部それぞれの正の成分と負の成分が互いに打ち消され易くなって, 積分の絶対値の値が小さくなることを意味している. $\QED$


## Fourier変換の逆変換の収束


### Fourier変換とFourier逆変換の定義

$\R$ 上の函数 $f(x)$ の**Fourier変換** $\hat{f}(p)$ を次のように定義する:

$$
\hat{f}(p) = \int_\R f(x)e^{-2\pi ipx}\,dx.
$$

さらに $\R$ 上の函数 $g(p)$ の**Fourier逆変換** $\check{g}(x)$ を次のように定義する:

$$
\check{g}(x) = \int_\R g(p)e^{2\pi ipx}\,dp.
$$

$f(x)$ が $\R$ 上の $L^1$ 函数ならば, Lebesgueの収束定理より $\hat{f}(p)$ は $p$ の連続函数になり, Riemann-Lebegueの定理より $|p|\to\infty$ で $\hat{f}(p)\to 0$ となる. 

**注意:** Fourier変換とFourier逆変換を, 上の公式における $p$ を $p/(2\pi)$ で置き換えることによって, 

$$
\hat{f}(p) = \int_\R f(x)e^{-ipx}\,dx, \quad
\check{g}(p) = \frac{1}{2\pi}\int_\R g(p)e^{ipx}\,dp
$$

と定義するスタイルもよく使われている. 他にも

$$
\hat{f}(p) = \frac{1}{\sqrt{2\pi}}\int_\R f(x)e^{-ipx}\,dx, \quad
\check{g}(p) = \frac{1}{\sqrt{2\pi}}\int_\R g(p)e^{ipx}\,dp
$$

と定義するスタイルも使われることがある. $\QED$

以下の節では, $L^1$ 函数 $f(x)$ に対して, 

$$
f(x) = \lim_{N\to\infty} \int_{-N}^N \hat{f}(p)e^{ipx}\,dp 
$$

がいつ成立しているかを調べる. 


### Dirichlet核

**Dirichlet核** $D_N(x)$ を

$$
D_N(x) = \int_{-N}^N e^{2\pi ipx}\,dp = \left[\frac{e^{2\pi ipx}}{2\pi ix}\right]_{p=-N}^{p=N} =
\frac{e^{e\pi iNx}-e^{-2\pi iNx}}{2\pi ix} = \frac{\sin(2\pi Nx)}{\pi x}
$$

と定める. ただし, $D_N(0)=2N$ と定めておく. $D_N(x)$ は有界な偶函数になる.

このとき

$$
\begin{aligned}
\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp &=
\int_{-N}^N \left(\int_\R f(y)e^{-2\pi ipy}\,dy\right)e^{2\pi ipx}\,dp =
\int_\R f(y) \left(\int_{-N}^N e^{2\pi ip(x-y)}\,dp\right)\,dy
\\ &=
\int_\R f(y) D_N(x-y)\,dy =
\int_\R f(x+y) D_N(y)\,dy.
\end{aligned}
$$

最後の等号で $y$ を $x+y$ で置き換えて, $D_N(-y)=D_N(y)$ を使った.


**問題:** Dirichlet核のグラフを描け. $\QED$

**解説:** $N$ が大きくなると, Dirichelt核 $D_N(x)$ のグラフの振動の仕方は細かくなり, $x=0$ の近くでの値も大きくなる.

以下のセルを見よ. $\QED$

```julia
# ディリクレ核 D_N(x) のグラフ

D(N,x) = iszero(x) ? 2N : sin(2π*N*x)/(π*x)
PP = []
for N in [1,2,3,4,5,6]
    x = -6:0.01:6
    P = plot(x, D.(N,x), title="N=$N", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[4:6]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

### Dirichlet積分の公式

条件収束する広義積分に関する公式

$$
\ds\int_0^\infty\frac{\sin t}{t}\,dt=\frac{\pi}{2}
$$

より

$$
\int_{-\infty}^\infty D_N(x)\,dx =
\frac{2}{\pi}\int_0^\infty \frac{\sin(2\pi Nx)}{x}\,dx =
\frac{2}{\pi}\int_0^\infty \frac{\sin t}{t}\,dt =
\frac{2}{\pi}\frac{\pi}{2} = 1.
$$

2番目の等号で $x=t/(2\pi N)$ と置換して計算した.  この公式は条件収束する積分の公式であることに注意せよ.

上の公式(**Dirichlet積分の公式**と呼ばれる)については

* <a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/09%20integration.ipynb">09 積分</a>

の<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/09%20integration.ipynb#Dirichlet積分とその一般化">Dirichlet積分とその一般化</a>の解説を参照せよ.


**Dirichlet積分の公式補足:** 条件収束する広義積分に関する次の公式が成立している:

$$
\int_{-\infty}^\infty \frac{\sin(ax)}{\pi x}\,dx = \sign(a) =
\begin{cases}
1 & (a<0) \\
0 & (a=0) \\
-1 & (a>0).
\end{cases}
$$

**証明:** $a=0$ のとき, 被積分函数が恒等的に $0$ になるので, 積分も $0$ になる. $a\ne 0$ と仮定する.

$$
\int_{-\infty}^\infty \frac{\sin(ax)}{\pi x}\,dx = \frac{2}{\pi}\int_0^\infty \frac{\sin(ax)}{x}\,dx
$$

であるから, $x = t/|a|$ と置換すると, $a/|a|=\sign(a)$ なので,

$$
\frac{2}{\pi}\int_0^\infty \frac{\sin(ax)}{x}\,dx =
\frac{2}{\pi}\int_0^\infty \frac{\sign(a)\sin(t)}{t}\,dt = \sign(a)\frac{2}{\pi}\frac{\pi}{2}=\sign(a).
\qquad\QED
$$


### Riemannの局所性定理

**Riemannの局所性定理:** $f$ は $\R$ 上の $L^1$ 函数であるとし, $\delta>0$ であるとする. このとき,

$$
\lim_{N\to\infty}\int_{-\infty}^{-\delta} f(x+y)D_N(y)\,dy = 0, \quad
\lim_{N\to\infty}\int_\delta^\infty f(x+y)D_N(y)\,dy = 0
$$

となる. これより, $\ds\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp = \int_\R f(x+y) D_N(y)\,dy$ が $N\to\infty$ で収束することと, $\ds\int_{-\delta}^\delta f(x+y) D_N(y)\,dy$ が $N\to\infty$ で収束することは同値であり, 収束する場合には同じ値に収束することがわかる.  後者が収束するか否かおよび収束するとしたらどこに収束するかは $x$ の近くでの函数 $f$ の様子だけで決まることに注意せよ. 

**証明:**
$$
\begin{aligned}
&
\int_{-\infty}^{-\delta} f(x+y)D_N(y)\,dy = \int_{-\infty}^{-\delta} \frac{f(x+y)}{y} \sin(2\pi Ny)\,dy,
\\ &
\int_\delta^\infty f(x+y)D_N(y)\,dy = \int_{-\infty}^{-\delta} \frac{f(x+y)}{y} \sin(2\pi Ny)\,dy.
\end{aligned}
$$

であり, 区間 $(-\infty,-\delta)$ と $(\delta,\infty)$ のそれぞれの上で函数 $f(x+y)/y$ は $L^1$ 函数なので, Riemann-Lebesgueの定理より, これらは $N\to\infty$ で $0$ に収束する. $\QED$


### Fourier変換の逆変換の収束性 (Diniの条件)


**補題:** $\delta>0$ のとき,

$$
\lim_{N\to\infty}\int_{-\infty}^{-\delta} D_N(y)dy = 0, \quad
\lim_{N\to\infty}\int_\delta^\infty D_N(y)dy = 0. 
$$

**注意:** 条件収束する広義積分の極限に関する結果なのでRiemann-Lebesgueの定理を直接使用できない. $\QED$

**証明:** $D_N(y)$ は偶函数なので後者のみを示せば十分である. Dirichlet積分の公式より $\ds\lim_{N\to\infty}\int_0^{2\pi N\delta} \frac{\sin t}{t}\,dt=\frac{\pi}{2}$ なので, $\ds\lim_{N\to\infty}\int_{2\pi N\delta}^\infty \frac{\sin t}{\pi t}\,dt=0$ となることがわかる. ゆえに, $y=t/(2\pi N)$ とおくと, 

$$
\int_\delta^\infty D_N(y)dy = \int_\delta^\infty \frac{\sin(2\pi Ny)}{\pi y}\,dy =
\int_{2\pi N\delta}^\infty \frac{\sin t}{\pi t}\,dt\to 0 \quad (N\to\infty).
\qquad\QED
$$


**定理(Diniの条件):** $f$ は $\R$ 上の $L^1$ 函数であり, $x\in \R$ であるとする.とする. さらに, ある $\delta>0$ が存在して, 

$$
\int_0^\delta \frac{|(f(x+y) + f(x-y) - 2f(x)|}{y}\,dy <\infty
\tag{$*$}
$$

が成立していると仮定する. この条件($*$)を**Diniの条件**と呼ぶ. このとき, 

$$
\lim_{N\to\infty}\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp = \lim_{N\to\infty}\int_\R f(x+y) D_N(y)\,dy = f(x).
$$

**証明:** $\ds \int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp - f(x)$ が $N\to\infty$ で $0$ に収束することを示せばよい. 

$$
\begin{aligned}
\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp - f(x) &=
\int_\R f(x+y) D_N(y)\,dy - f(x) \int_{-\infty}^\infty D_N(y)\,dy
\\ &=
\int_{-\infty}^\infty (f(x+y)-f(x)) D_N(y)\,dy
\\ &=
\int_0^\infty (f(x+y)-f(x)) D_N(y)\,dy +
\int_{-\infty}^0 (f(x+y)-f(x)) D_N(y)\,dy
\\ &=
\int_0^\infty (f(x+y)-f(x)) D_N(y)\,dy +
\int_0^\infty (f(x-y)-f(x)) D_N(y)\,dy
\\ &=
\int_0^\infty (f(x+y)+f(x-y)-2f(x)) D_N(y)\,dy
\\ &=
\int_0^\infty \frac{f(x+y)+f(x-y)-2f(x)}{y} \frac{\sin(2\pi N y)}{\pi}\,dy = I_N + J_N.
\end{aligned}
$$

ここで, $I_N,J_N$ を次のように定義した:

$$
\begin{aligned}
&
I_N = \int_0^\delta \frac{f(x+y)+f(x-y)-2f(x)}{y} \frac{\sin(2\pi N y)}{\pi}\,dy, 
\\ &
J_N = \int_\delta^\infty \frac{f(x+y)+f(x-y)-2f(x)}{y} \frac{\sin(2\pi N y)}{\pi}\,dy.
\end{aligned}
$$

$N\to\infty$ のとき, Riemannの局所性定理とすぐ上の補題より $J_N\to 0$ となり, Diniの条件の仮定とRiemann-Lebesgueの定理より $I_N\to 0$ となる. これで示したいことが示せた. $\QED$

**補足:** もしも $f(x)$ とそのFourier変換 $\hat{f}(p)$ が両方可積分でかつ, $f(x)$ が $x$ におけるDiniの条件を満たしているならば,

$$
\int_{-\infty}^\infty \hat{f}(p)e^{2\pi ipx}\,dp = f(x)
$$

は絶対収束する積分の公式として成立している. $\QED$


**例:** $f$ は $\R$ 上の $L^1$ 函数であるとする. $y\searrow 0$ のとき $f(x+y), f(x-y)$ は収束していると仮定し, それぞれの収束先を $f(x+0), f(x-0)$ と書くことにする. さらに $y\searrow 0$ で $\ds\frac{f(x+y)-f(x+0)}{y}$ と $\ds\frac{f(x-y)-f(x-0)}{y}$ が収束していると仮定する.  特に $f$ が $x$ で微分可能ならばそれらの条件が成立している. 

このとき, 十分小さな $\delta>0$ を取ると, $\ds\frac{f(x+y)+f(x-y)-(f(x+0)+f(x-0))}{y}$ は $0<y<\delta$ で有界になり, 特にそこで可積分になる. ゆえに

$$
f(x)=\frac{f(x+0)+f(x-0)}{2}
$$

ならば, Diniの条件が満たされており, 上の定理より,

$$
\lim_{N\to\infty}\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp = \lim_{N\to\infty}\int_\R f(x+y) D_N(y)\,dy = f(x)
$$

となる. さらにもしも $\hat{f}(p)$ が可積分ならば

$$
\int_\R \hat{f}(p)e^{2\pi ipx}\,dp = f(x)
$$

となる. $\QED$


**例:** $a>0$ であるとし, $f(x)$ を

$$
f(x) = \begin{cases}
1 & (-a<x<a) \\
1/2 & (a=\pm a) \\
0 & (\text{otherwise})
\end{cases}
$$

と定める. このとき, Diniの条件が満たされているので, 

$$
\lim_{N\to\infty}\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp = f(x)
\tag{$*$}
$$

が成立している. これを直接の計算で確認してみよう.

$$
\hat{f}(p) = \int_{-a}^a e^{-2\pi ipx}\,dx = \frac{e^{-2\pi ipa}-e^{2\pi ipa}}{-2\pi ip} =
\frac{\sin(2\pi a p)}{\pi p}.
$$

ゆえに, 

$$
\begin{aligned}
\int_{-N}^N \hat{f}(p)e^{2\pi ipx}\,dp &=
\int_{-N}^N \frac{\sin(2\pi ap)}{\pi p} e^{2\pi ipx}\,dp =
\int_{-N}^N \frac{\sin(2\pi ap)\cos(2\pi xp)}{\pi p}\,dp
\\ &=
\frac{1}{2}\int_{-N}^N \frac{\sin(2\pi(a+x)p)+\sin(2\pi(a-x)p)}{\pi p}\,dp.
\end{aligned}
$$

2つ目の等号で $\ds e^{2\pi ipx}=\cos(2\pi xp)+i\sin(2\pi xp)$ と $\ds\frac{\sin(2\pi a p)\sin(2\pi xp)}{\pi p}$ が $p$ の奇函数であることを用いた. 3つ目の等号では三角函数の加法公式を使って示される $2\sin\alpha\,\cos\beta = \sin(\alpha+\beta) + \sin(\alpha-\beta)$ を用いた.

そして, Dirichlet積分の公式より, 

$$
\begin{aligned}
&
\lim_{N\to\infty}\frac{1}{2}\int_{-N}^N \frac{\sin(2\pi(a+x)p)}{\pi p}\,dp =
\begin{cases}
1/2 & (x>-a) \\
0 & (x=a) \\
-1/2 & (x<-a),
\end{cases}
\\ &
\lim_{N\to\infty}\frac{1}{2}\int_{-N}^N \frac{\sin(2\pi(a-x)p)}{\pi p}\,dp =
\begin{cases}
1/2 & (x<a) \\
0 & (x=a) \\
-1/2 & (x>a).
\end{cases}
\end{aligned}
$$

以上を合わせると, 式($*$)が成立していることがわかる. $\QED$


## たたみ込み積と総和核


### たたみ込み積

$\R$ 上の函数 $f$, $g$ の**たたみ込み積**(convolution) $f*g$ が次のように定義される:

$$
f*g(x) = \int_{-\infty}^\infty f(x-y)g(y)\,dy = \int_{-\infty}^\infty f(y)g(x-y)\,dy.
$$

たたみ込み積は函数と函数から函数を作る操作であることに注意せよ.


**問題:** $f$ と $g$ が $\R$ 上の可積分函数ならば $\ds\int_\R |f*g(x)|\,dx < \infty$ となることを示せ.

**解答例:**
$$
\begin{aligned}
\int_\R |f*g(x)|\,dx &= 
\int_\R\left|\int_\R f(x-y)g(y)\,dy\right|\,dx \leqq
\int_\R\left(\int_\R |f(x-y)||g(y)|\,dy\right)\,dx 
\\ &\leqq
\int_\R\left(\int_\R |f(x-y)|\,dx\right)|g(y)|\,dy =
\int_\R\left(\int_\R |f(x)|\,dx\right)|g(y)|\,dy
\\ &=
\int_\R |f(x)|\,dx\;\int_\R|g(y)|\,dy <
\infty. \qquad \QED
\end{aligned}
$$

**注意:** $\|f*g\|_1\leqq \|f\|_1\|g\|_1$ を示せた. $\QED$


**問題:** $p,p',q,q',r\geqq 1$ が

$$
\frac{1}{p}+\frac{1}{p'}=1, \quad
\frac{1}{q}+\frac{1}{q'}=1, \quad
\frac{1}{r}+\frac{1}{p'}+\frac{1}{q'} = 1
$$

を満たしているならば, 

$$
\|f*g\|_r \leqq \|f\|_p \|g\|_q
$$

となることを示せ.

**解答例:** $f$, $g$ のそれぞれを $\ds\frac{f}{\|f\|_p}$, $\ds\frac{g}{\|g\|_q}$ で置き換えることによって, $\|f\|_p=\|g\|_q=1$ と仮定できるので, そのように仮定する. このとき,

$$
\begin{aligned}
&
\left(1-\frac{p}{r}\right)q' = 
p\left(\frac{1}{p}-\frac{1}{r}\right)q' =
p\left(1-\frac{1}{q}\right)q' =
p,
\\ &
\left(1-\frac{q}{r}\right)p' = 
q\left(\frac{1}{q}-\frac{1}{r}\right)p' =
q\left(1-\frac{1}{p}\right)p' =
q
\end{aligned}
$$

なので, Hölderの不等式と $\|f\|_p=\|g\|_q=1$ より,

$$
\begin{aligned}
|f*g(x)| &= 
\left|\int_\R f(x-y)g(y)\,dy\right| \leqq
\int_\R |f(x-y)||g(y)|\,dy
\\ &\leqq
\int_\R |f(x-y)|^{p/r}|g(y)|^{q/r}\cdot |f(x-y)|^{1-p/r}\cdot |g(y)|^{1-q/r}\,dy
\\ &=
\int_\R |f(x-y)|^{p/r}|g(y)|^{q/r}\cdot |f(x-y)|^{p/q'}\cdot |g(y)|^{q/p'}\,dy
\\ &\leqq
\left(\int_\R |f(x-y)|^p |g(y)|^q\,dy\right)^{1/r}
\left(\int_\R|f(x-y)|^p\,dy\right)^{1/q'}
\left(\int_\R|g(y)|^q\,dy\right)^{1/p'}
\\ &=
\left(\int_\R |f(x-y)|^p |g(y)|^q\,dy\right)^{1/r}.
\end{aligned}
$$

したがって,

$$
\begin{aligned}
(\|f*g\|_r)^r &=
\int_\R |f*g(x)|^r\,dx \leqq
\int_\R\left(\int_\R |f(x-y)|^p |g(y)|^q\,dy\right)\,dx
\\ &=
\int_\R\left(\int_\R |f(x-y)|^p\,dx\right)|g(y)|^q\,dy =
(\|f\|_p)^p(\|g\|_q)^q = 1.
\end{aligned}
$$

これで示すべきことがすべて示された. $\QED$


**問題:** たたみ込み積が結合法則を満たしていることを積分の形式的計算で確認せよ.

**解答例:** $\ds\varphi*\psi(\cdot)=\int_\R\varphi(\xi)\psi(\cdot-\xi)\,d\xi$ を繰り返し使うと,

$$
\begin{aligned}
(f*g)*h(x) &= 
\int_\R\left(\int_\R f(y)g(z-y)h(x-z)\,dy\right)\,dz 
\\ &=
\int_\R\left(\int_\R f(y)g(z-y)h(x-z)\,dz\right)\,dy 
\\ &=
\int_\R\left(\int_\R f(y)g(z)h(x-y-z)\,dz\right)\,dy = 
f*(g*h)(x).
\end{aligned}
$$

2つ目の等号で積分順序を交換し, 3つ目の等号で $z$ を $y+z$ で置換した. $\QED$


### 微分やたたみ込み積とFourier変換の関係

以下に登場するFourier変換と逆変換

$$
\Fourier[f](p) = \int_{-\infty}^\infty e^{-2\pi ipx}f(x)\,dx, 
\quad
\Fourier^{-1}[f](x) = \int_{-\infty}^\infty e^{2\pi ipx}f(p)\,dp 
$$

は適切な意味で収束していると仮定する. すなわち, $f(x),g(x),\ldots$ はその導函数とともに絶対値が $x\to\pm\infty$ で十分速く $0$ に収束していると仮定する.

**定理:** 以下の公式が成立している:

$$
\begin{alignedat}{2}
&
\Fourier[f'](p) = 2\pi ip\Fourier[f](p),
& \quad &
\Fourier[xf](p) = -\frac{1}{2\pi i}\frac{d}{dp}\Fourier[f](p),
\\ &
\Fourier[f*g] = \Fourier[f]\Fourier[g],
& \quad &
\Fourier[fg] = \Fourier[f]*\Fourier[g].
\end{alignedat}
$$

Fourier逆変換についても同様の公式が成立している. (これらの公式も空気のごとくよく使われる.)

**証明:** 以下, 記号の簡単のため, $\int_\R$ を $\int$ と書く.  部分積分によって,

$$
\begin{aligned}
&
\int e^{-2\pi ipx} f'(x)\,dx =
-\int \frac{d(e^{-2\pi ipx})}{dx} f(x)\,dx =
2\pi i p\int e^{-2\pi ipx} f(x)\,dx,
\\ & \therefore \quad
\Fourier[f'](p) = 2\pi ip\Fourier[f](p),
\\ &
\frac{d}{dp}\Fourier[f](p) =
-2\pi i\int x e^{-2\pi ipx}f(x)\,dx =
-2\pi i\Fourier[xf](p),
\\ & \therefore\quad
\Fourier[xf](p) = -\frac{1}{2\pi i}\frac{d}{dp}\Fourier[f](p).
\end{aligned}
$$

たたみ込み積とFourier変換の定義より,

$$
\begin{aligned}
\Fourier[f*g](p) &=
\int e^{-2\pi ipx}\left(\int f(y)g(x-y)\,dy\right)\,dx
\\ &=
\int e^{-2\pi ipy}e^{-2\pi ip(x-y)}\left(\int f(y)g(x-y)\,dy\right)\,dx
\\ &=
\int e^{-2\pi ipy} f(y)\left(\int e^{-2\pi ip(x-y)}g(x-y)\,dx\right)\,dy
\\ &=
\int e^{-2\pi ipy} f(y)\left(\int e^{-2\pi ipx}g(x)\,dx\right)\,dy
\\ &=
\Fourier[f](p)\Fourier[g](p).
\end{aligned}
$$

すなわち $\Fourier[f*g] = \Fourier[f]\Fourier[g]$. 同様にして $\Fourier^{-1}[f*g] = \Fourier^{-1}[f]\Fourier^{-1}[g]$ も成立していることを示せる. これを $f,g$ がそれぞれ $\Fourier[f]$, $\Fourier[g]$ の場合に適用すると

$$
\Fourier^{-1}[\Fourier[f]*\Fourier[g]] = 
\Fourier^{-1}[\Fourier[f]]\Fourier^{-1}[\Fourier[g]] =
fg.
$$

両辺に $\Fourier$ を作用させれば $\Fourier[f]*\Fourier[g] = \Fourier[fg]$ を得る. もしくは

$$
\int e^{2\pi i px}\left(\int e^{-2\pi ipy}g(y)\,dy\right)\,dp = g(x)
$$

を使うと, 途中の計算で $e^{-2\pi iqx}=e^{-2\pi ipx}e^{-2\pi i(p-q)x}$ を使うことによって,

$$
\begin{aligned}
(\Fourier[f]*\Fourier[g])(p) &=
\int \Fourier[f](q)\Fourier[g](p-q)\,dq
\\ &=
\int
\left(\int e^{-2\pi iqx}f(x)\,dx\right)
\left(\int e^{-2\pi i(p-q)y}g(y)\,dy\right)
\,dq
\\ &=
\int e^{-2\pi ipx}f(x)
\left(\int e^{2\pi i(p-q)x}\left(\int e^{-2\pi i(p-q)y}g(y)\,dy\right)\,dq\right)
\,dx
\\ &=
\int e^{-2\pi ipx}f(x)g(x)\,dx =
\Fourier[fg](p).
\end{aligned}
$$

このようにして $\Fourier[f]*\Fourier[g] = \Fourier[fg]$ を得ることもできる. $\QED$


### 総和核

$\delta > 0$ に対して, $\ds\int_{|x|>\delta} f(x)\,dx$ を

$$
\int_{|x|>\delta} f(x)\,dx = 
\int_{-\infty}^{-\delta} f(x)\,dx +
\int_\delta^\infty f(x)\,dx
$$

と定義しておく.  この記号法を使うと, 次の定義の記述を少し簡潔にできる.

**定義:** 以下の条件を満たす満たす $\R$ 乗の可積分函数の族 $\rho_t(x)$, $t>0$ を**総和核**(summability kernel)と呼ぶ:

(S1) $\ds\int_\R \rho_t(x)\,dx = 1$.

(S2) ある正の定数 $C$ が存在して, 任意の $t>0$ に対して $\ds\int_\R |\rho_t(x)|\,dx \leqq C$.

(S3) 任意の $\delta>0$ に対して, $t\searrow0$ のとき $\ds\int_{|x|>\delta}|\rho_t(x)|\,dx\to 0$. $\QED$

この定義は, 猪狩惺著『実解析入門』岩波書店(1996年)の§6.5に書いてあるものに等しい. 


**注意:** 大雑把に言えば, 総和核とは**Diracのデルタ函数**に「収束」するような函数族のことである.  Diracのデルタ函数 $\delta(x)$ とは $f*\delta=f$ すなわち

$$
\int_\R f(x-y)\delta(y)\,dy = \int_\R f(y)\delta(x-y)\,dy = f(x)
$$

を満たす「函数」のことである. 実際にはこのような「函数」は通常の函数としては存在しない. 所謂超函数として定義される. しかし, $\rho_t$ を総和核とすれば, $t\searrow0$ のとき適当な意味で $f*\rho_t\to f$ となることを示せる. より正確な内容については以下を参照せよ. $\QED$


**例:** $\rho(x)$ が $\R$ 上の可積分函数で $\ds\int_\R\rho(x)\,dx=1$ を満たしているならば, 

$$
\rho_t(x) = \frac{1}{t}\rho\left(\frac{x}{t}\right), \qquad t>0
$$

によって総和核 $\rho_t(x)$ を構成できる. (S1)は $x=ty$ と置換すれば得られる:

$$
\int_\R \rho_t(x)\,dx =
\int_\R \rho\left(\frac{x}{t}\right)\frac{dx}{t} =
\int_\R \rho(y)\,dy = 1.
$$

(S2)は $x=ty$, $\ds C=\int_\R|\rho(y)|\,dy$ とおくと得られる:

$$
\int_\R|\rho_t(x)|\,dx =
\int_\R\left|\rho\left(\frac{x}{t}\right)\right|\,\frac{dx}{t} =
\int_\R|\rho(y)|\,dy = C.
$$

(S3)を示そう. この場合も $x=ty$ と変換する. $R\to\infty$ のとき $\ds\int_{-R}^R|\rho(y)|\,dy\to C$ なので, $t\searrow0$ のとき, 

$$
\int_{|x|>\delta}|\rho_t(x)|\,dx =
\int_{|y|>\delta/t}|\rho(y)|\,dy =
C - \int_{-\delta/t}^{\delta/t} |\rho(y)|\,dy \to 0.
$$

これで $\rho_t(x)$ が総和核であることがわかった. $\QED$


**問題:** $\rho_t(x)$ は総和核であると仮定し, $f$ は $\R$ 上の連続函数であり、$|x|\to\infty$ のとき $f(x)\to 0$ となるものであると仮定する. このとき, $t\searrow0$ で $f$ と総和核 $\rho_t$ たたみ込み積 $f*\rho_t$ が $f$ に一様収束することを示せ.

**解答例:** $\eps>0$ であるとする. 

$$
|f*\rho_t(x)-f(x)| =
\left|\int_\R (f(x-y)-f(x)) \rho_t(y)\,dy\right| \leqq
\int_\R|f(x-y)-f(x)||\rho_t(y)|\,dy = I + J.
$$

1つ目の等号で(S1)を使った. 最後の等号では

$$
I = \int_{|y|\leqq\delta} |f(x-y)-f(x)||\rho_t(y)|\,dy, \quad
J = \int_{|y|>\delta} |f(x-y)-f(x)||\rho_t(y)|\,dy
$$

とおいた. $f$ は $\R$ 上の一様連続函数になるので, (S2)を満たす正の定数 $C$ を取ると, ある $\delta>0$ が存在して, $|y-x|\leqq\delta$ ならば $\ds |f(y)-f(x)|<\frac{\eps}{2C}$ となる. このとき,

$$
I \leqq 
\frac{\eps}{2C}\int_{|y|<\delta}|\rho_t(y)|\,dy \leqq
\frac{\eps}{2C} C \leqq 
\frac{\eps}{2}.
$$

$f$ は有界な函数になるので, ある正の定数 $M$ で $|f(x)|\leqq M$ ($x\in\R$) を満たすものが存在する. (S3)より, 
ある $\tau>0$ が存在して, $0<t<\tau$ ならば $\ds\int_{|x|>\delta}|\rho_t(x)|\,dx<\frac{\eps}{4M}$ となる. このとき, $|f(x-y)-f(x)|\leqq 2M$ となるので, 

$$
J \leqq
2M \int_{|y|>\delta}|\rho_t(y)|\,dy <
2M\frac{\eps}{4M} = \frac{\eps}{2}.
$$

したがって, そのとき, 

$$
|f*\rho_t(x)-f(x)| = I + J < \frac{\eps}{2}+\frac{\eps}{2} = \eps.
$$

これで $t\searrow 0$ のとき, $f*\rho_t$ が $f$ に一様収束することがわかった. $\QED$ 


**問題:** $p\geqq 1$ であるとし, $f$ は $L^p$ 函数であるとする. このとき, $t\searrow 0$ のとき $\|f*\rho_t - f\|_p\to 0$ となることを示せ.

**解答例:** $t\searrow0$ で $(\|f*\rho_t-f\|_p)^p\to 0$ となることを示せばよい. $p'$ を $\ds \frac{1}{p}+\frac{1}{p'}=1$ という条件で定める.  Hölderの不等式より, 

$$
\begin{aligned}
\left|f*\rho_t(x)-f(x)\right|^p &=
\left|\int_\R (f(x-y)-f(y))\rho_t(y)\,dy\right|^p \leqq
\left(\int_\R |f(x-y)-f(y)||\rho_t(y)|\,dy\right)^p 
\\ &=
\left(\int_\R |f(x-y)-f(y)||\rho_t(y)|^{1/p}\cdot|\rho_t(y)|^{1/p'}\,dy\right)^p 
\\ &\leqq
\int_\R |f(x-y)-f(y)|^p |\rho_t(y)|\,dy\;
\left(\int_\R |\rho_t(y)|\,dy\right)^{p/p'}
\\ &\leqq
C^{p/p'} \int_\R |f(x-y)-f(y)|^p|\rho_t(y)|\,dy.
\end{aligned}
$$

これを $x$ で積分すると,

$$
(\|f*\rho_t-f\|_p)^p \leqq
C^{p/p'} \int_\R\left(\int_\R |f(x-y)-f(x)|^p\,dx\right)|\rho_t(y)|\,dy =
I+J.
$$

ここで, $I,J$ はそれぞれ $|y|>\delta$ と $|y|\leqq \delta$ における積分である. $y\to 0$ のとき $\|f(\cdot-y)-f\|_p\to 0$ となることより, $C$ を条件(S2)の $C$ であるとすると, ある $\delta>0$ が存在して, $|y|\leqq\delta$ のとき $\ds\int_\R|f(x-y)-f(x)|\,dx\leqq\frac{\eps}{2C}$ となり,

$$
J = \int_{-\delta}^\delta\left(\int_\R |f(x-y)-f(x)|^p\,dx\right)|\rho_t(y)|\,dy \leqq
\frac{\eps}{2C}C\leqq\frac{\eps}{2}.
$$

$M=\|f\|_p$ とおくと, $\|f(\cdot-y)-f\|_p\leqq 2M$ となる. (S3)より, 
ある $\tau>0$ が存在して, $0<t<\tau$ のとき, $\ds\int_{|y|>\delta}|\rho_t(y)\,dy<\frac{\eps}{2(2M)^p}$ となるので,

$$
I = \int_{|y|>\delta}\left(\int_\R |f(x-y)-f(x)|^p\,dx\right)|\rho_t(y)|\,dy \leqq
(2M)^p \frac{\eps}{2(2M)^p} = \frac{\eps}{2}.
$$

このとき, 

$$
(\|f*\rho_t-f\|_p)^p \leqq I+J\leqq\frac{\eps}{2}+\frac{\eps}{2}=\eps.
$$

これで, $t\searrow0$ のとき $\|f*\rho_t-f\|_p\to0$ となることが示された. $\QED$


### 総和核とFourier展開の関係

仮に総和核 $\rho_t(x)$ が

$$
\rho_t(x) = \int_\R W_t(p) e^{2\pi ipx}\,dp
$$

と重み $W_t(p)$ によって表示されていたとする.  このとき, 重み $W_t(p)$ によって制限された函数 $f(x)$ のFourier展開 $\ds\int_\R W(p) \hat{f}(p)e^{2\pi ipx}\,dp$ は次のように表わされる:

$$
\begin{aligned}
\int_\R \hat{f}(p) e^{2\pi ipx} W_t(p)\,dp &=
\int_\R \left(\int_\R f(y)e^{-2\pi ipy}\,dy\right)\,e^{2\pi ipx} W_t(p)\,dp 
\\ &=
\int_\R f(y)\left(\int_\R W_t(p) e^{2\pi ip(x-y)}\,dp\right)\,dy 
\\ &=
\int_\R f(y)\rho_t(x+y)\,dy =
f*\rho_t(x)
\end{aligned}
$$

このように, 「重み $W_t(p)$ によってFourier展開された総和核と函数 $f$ のたたみ込み積」は「重み $W_t(p)$ で制限された函数 $f$ のFourier展開」になっている.


### 総和核の例


#### Dirichlet核は総和核ではない

Dirichlet核 $D_N(x)$ は区間 $[-N,N]$ 上で $1$ でそれ以外の場所で $0$ になるような重み函数によるFourier展開で定義されていた:

$$
D_N(x) = \int_{-N}^N e^{2\pi ipx}\,dp = \left[\frac{e^{2\pi ipx}}{2\pi ix}\right]_{p=-N}^{p=N} =
\frac{e^{e\pi iNx}-e^{-2\pi iNx}}{2\pi ix} = \frac{\sin(2\pi Nx)}{\pi x}.
$$

$N=1/t$ とおく. Dirichlet核 $\ds D_{1/t}(x) = \frac{\sin(2\pi x/t)}{\pi x}$ はDirichlet積分の公式より, 任意の $\delta>0$ について, 

$$
\int_{-\infty}^\infty D_{1/t}(x)\,dx = 1, \qquad
\int_\delta^\infty D_{1/t}(x)\,dx\to 0, \quad
\int_{-\infty}^{-\delta} D_{1/t}(x)\,dx \to 0
\quad (t\searrow0)
$$

を満たしているが, これらは条件収束する広義積分であり,

$$
\int_\R |D_{1/t}(x)|\,dx = \infty
$$

となっているので, このノートの意味での総和核ではない. Dirichlet核は足し上げる $p$ の範囲を区間 $[-N,N]$ に単純に制限するという素朴で分かり易い考え方に基いて得られるが, 可積分函数にならない. このことがDirichlet核を用いたFourier解析を難しくしていると考えられる.  足し上げる $p$ の範囲を単純に制限するのではなく, 適当な重みを付けて制限するという一般化を考えると, 様々な総和核が得られる.


#### Fejér核

重み函数 $\Delta_N(p)$ を

$$
\Delta_N(p) = \max\left\{1-\frac{|p|}{N}, 0\right\}
$$

と定め, $N>0$ に対して, **Fejér核** $F_N(x)$ を次のように定める:

$$
\begin{aligned}
F_N(x) &= 
\int_\R \Delta_N(p)e^{2\pi ipx}\,dp =
\int_{-N}^N \left(1-\frac{|p|}{N}\right)e^{2\pi ipx}\,dp 
\\ &=
2\int_0^N \left(1-\frac{p}{N}\right)\cos(2\pi px)\,dp =
2N\int_0^1 \left(1-t\right)\cos(2\pi Ntx)\,dt
\\ &=
2N\left[(1-t)\frac{\sin(2\pi Ntx)}{2\pi Nx}\right]_{t=0}^{t=1} +
2N\int_0^1 \frac{\sin(2\pi Ntx)}{2\pi Nx}\,dt
\\ &=
2N\left[ -\frac{\cos(2\pi Ntx)}{(2\pi Nx)^2}\right]_{t=0}^{t=1} =
\frac{1-\cos(2\pi Nx)}{2N(\pi x)^2} =
\frac{1}{N}\left(\frac{\sin(\pi Nx)}{\pi x}\right)^2.
\end{aligned}
$$

5つ目の等号で部分積分を行い, 最後の等号で倍角の公式 $\cos(2X)=1-2\sin^2 X$ を使った. 特にFejér核は正値函数である. そして, Dirichlet積分の公式より,

$$
\begin{aligned}
\int_0^\infty\left(\frac{\sin x}{x}\right)^2\,dx &=
\int_0^\infty\frac{1-\cos 2x}{2x^2}\,dx 
\\ &=
\left[\frac{-1}{2x}(1-\cos 2x)\right]_0^\infty -
\int_0^\infty\frac{-1}{2x}(2\sin 2x)\,dx 
\\ &=
\int_0^\infty \frac{\sin 2x}{x}\,dx =
\int_0^\infty \frac{\sin t}{t}\,dt =
\frac{\pi}{2}.
\end{aligned}
$$

(最後から2番目の等号で $x=t/2$ とおいた)を使えば, $\ds\int_\R F_N(x)\,dx = 1$ であることも容易に確認できる. 

Fejér核 $F_{1/t}(x)$ は総和核である.

```julia
# Δ_N(p) のグラフ
#
# N を大きくすると足し上げる p の範囲が拡大する.

Δ(N,p) = max(1-abs(p)/N, zero(p))
PP = []
for N in [1,3,8]
    p = -10:0.01:10
    P = plot(p, Δ.(N,p), title="N=$N", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

(最後から2番目の等号で $x=2t$ とおいた)を使えば, $\ds\int_\R F_N(x)\,dx = 1$ であることも容易に確認できる. 

Fejér核 $F_{1/t}(x)$ は総和核である.

```julia
# Fejér核のグラフ
#
# Dirichlet核よりも x=0 から離れた部分の値が急速に 0 に収束することがわかる.

F(N,x) = iszero(x) ? N : (sin(π*N*x)/(π*x))^2/N
PP = []
for N in [1,2,3,4,5,6]
    x = -6:0.01:6
    P = plot(x, F.(N,x), title="N=$N", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[4:6]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

#### Poisson核

$t>0$ に対して, Poisson核 $P_t(x)$ を次のように定める:

$$
P_t(x) = \frac{1}{\pi}\frac{t}{t^2+x^2} = \frac{1}{\pi t}\frac{1}{1+(x/t)^2}.
$$

これも総和核であり, 次のようにFourier展開される:

$$
P_t(x) = \int_\R \hat{P}_t(p)e^{2\pi ipx}\,dp, \qquad
\hat{P}_t(p) = e^{-2\pi t|p|}.
$$

この公式を証明するためには, 

$$
\int_0^\infty e^{-2\pi tp}\cos(2\pi px)\,dp = \frac{1}{2\pi}\frac{t}{t^2+x^2}
$$

を示せばよい. この公式は次の結果の実部に等しい:

$$
\int_0^\infty  e^{-2\pi tp} e^{2\pi ipx}\,dp =
\int_0^\infty  e^{-2\pi(t-ix)p}\,dp =
\left[\frac{e^{-2\pi(t-ix)p}}{-2\pi(t-ix)}\right]_{p=0}^{p=\infty} = \frac{1}{2\pi(t-ix)}.
$$

**注意:** Poisson核はCauchy分布の確率密度函数に等しい. 重み函数 $\hat{P}_t(p)$ はCauchy分布の特性函数とも呼ばれる. 確率論では確率分布の特性函数の形でFourier解析が利用されている. $\QED$

```julia
# Phat_t(p) のグラフ
#
# t > 0 を小さくすると足し上げる p の範囲が拡大する.

Phat(t,p) = exp(-2π*t*abs(p))
PP = []
for t in [0.5, 0.1, 0.05]
    p = -10:0.01:10
    P = plot(p, Phat.(t,p), title="t=$t", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

```julia
# Poisson核のグラフ
#
# t > 0 を小さくすると x 軸上での広がりの範囲が狭くなる.

Poisson(t,x) = t/(t^2+x^2)/π
PP = []
for t in [0.5, 0.1, 0.05]
    x = -1:0.001:1
    P = plot(x, Poisson.(t,x), title="t=$t", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

#### Gauss核

**Gauss核** $G_t(x)$ を次のように定める:

$$
G_t(x) = \frac{1}{\sqrt{2\pi t}}e^{-x^2/(2t)}
$$

これは総和核であり, 次のようにFourier展開される:

$$
G_t(x) = \int_\R \hat{G}_t(p)\,e^{2\pi ipx}\,dp, \qquad
\hat{G}_t(p) = e^{-t (2\pi p)^2/2} = e^{-2t\pi^2 p^2}
$$

**注意:** Gauss核は期待値0の分散 $t$ の正規分布の確率密度函数に等しい. $\QED$


**問題:** Gauss核 $u = u(t,x) = G_t(x)$ が熱方程式 $\ds u_t = \frac{1}{2}u_{xx}$ の解になっていることを示せ.

**解答例:** $u$ を $x$ で2回偏微分すると,

$$
u_x = -\frac{x}{t}, \qquad
u_{xx} = \left(-\frac{1}{t} + \frac{x^2}{t^2}\right)u.
$$

$\ds\frac{\d}{\d t}t^{-1/2} = -\frac{1}{2t}t^{-1/2}$ などを使うと, 

$$
u_t = \left(-\frac{1}{2t} + \frac{x^2}{2t^2}\right)u.
$$

以上を比較すれば $\ds u_t = \frac{1}{2}u_{xx}$ が得られる. $\QED$

**注意:** $t\searrow 0$ のとき $G_t(x)\to\delta(x)$ なので, Gauss核は<a href="https://www.google.co.jp/search?q=%E7%86%B1%E6%96%B9%E7%A8%8B%E5%BC%8F%E3%81%AE%E5%9F%BA%E6%9C%AC%E8%A7%A3">熱方程式の基本解</a>になっている. $\QED$

```julia
x = symbols("x", real=true)
p = symbols("p", real=true)
t = symbols("t", positive=true)
W(t,p) = simplify(integrate(exp(-x^2/(2t))*exp(-2*Sym(π)*im*p*x), (x,-oo,oo)))/sqrt(2*Sym(π)*t)
W(t,p)
```

```julia
G(t,x) = simplify(integrate(W(t,p)*exp(2*Sym(π)*im*p*x), (p,-oo,oo)))
G(t,x)
```

```julia
integrate(G(t,x), (x,-oo,oo))
```

```julia
[
    factor(diff(G(t,x), x, x))/2
    factor(diff(G(t,x), t))
]
```

```julia
# Ghat_t(p) のグラフ
#
# t > 0 を小さくすると足し上げる p の範囲が拡大する.

Ghat(t,p) = exp(-2t*π^2*p^2)
PP = []
for t in [0.1, 0.01, 0.004]
    p = -10:0.01:10
    P = plot(p, Ghat.(t,p), title="t=$t", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

```julia
# Gauss核のグラフ
#
# t > 0 を小さくすると x 軸上での広がりの範囲が狭くなる.

G(t,x) = exp(-x^2/(2t))/√(2π*t)
PP = []
for t in [0.1, 0.01, 0.004]
    x = -1:0.001:1
    P = plot(x, G.(t,x), title="t=$t", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

## Fourier級数の収束

Fourier変換の逆変換の収束の議論とほぼ同じ議論を以下では行う.

以下の内容を超えるFourier級数論については, 猪狩惺著『実解析入門』岩波書店(1996)の第8.5節以降などを参照せよ. 


### Fourier級数の定義

$f(x)$ は周期 $1$ を持つ $\R$ 上の函数であり($f(x+1)=f(x)$), $\ds\int_0^1|f(x)|\,dx<\infty$ を満たしていると仮定する.  このとき, 整数 $k\in\Z$ に対して, $f(x)$ の**Fourier係数** $a_k(f)$ を

$$
a_k(f) = \int_0^1 f(x)e^{-2\pi i kx}\,dx
$$

と定める. そして, 

$$
\sum_{k\in\Z} a_k(f) e^{2\pi ikx}
$$

を $f(x)$ の**Fourier級数展開**と呼ぶ. 


### Fourier級数展開のDirichlet核

**Dirichlet核** $D_N(x)$ を

$$
D_N(x) = \sum_{k=-N}^N e^{2\pi ikx} = e^{-2\pi iNx}\frac{e^{2\pi i(2N+1)x}-1}{e^{2\pi i x}-1} =
\frac{\sin(\pi(2N+1)x)}{\sin(\pi x)}
$$

と定める. ただし, $x\in\Z$ のときには $D_N(x)=2N+1$ と定めておく.  $D_N(x)$ は有界な偶函数で周期 $1$ を持つ.

このとき, 

$$
\begin{aligned}
\sum_{k=-N}^N a_k(f)e^{2\pi ikx} &=
\sum_{k=-N}^N \left(\int_0^1 f(y)e^{2\pi i ky}\,dy\right)e^{2\pi ikx} =
\int_0^1 f(y)\left(\sum_{k=-N}^N e^{2\pi i k(x-y)}\,dx\right)\,dy 
\\ &=
\int_0^1 f(y)D_N(x-y)\,dy =
\int_{-1/2}^{1/2} f(x+y)D_N(y)\,dy.
\end{aligned}
$$

最後の等号で $y$ を $x+y$ で置換し, $D_N(-y)=D_N(y)$ と $f(x+y)D_N(y)$ が $y$ について周期 $1$ を持つ函数になることを用いた.

$$
\int_0^1 D_N(y)\,dy = \sum_{k=-N}^N\int_0^1 e^{2\pi iky} \,dy = \int_0^1 1 \,dy = 1.
$$


**問題:** Fourier級数のDirichlet核のグラフを描け. $\QED$

**解説:** 上の $D_N(x)$ は $D_N(x+1)=D_N(x)$ を満たしているので, $-1/2\leqq x\leqq 1/2$ でグラフを描けば十分である. $D_N(x)$ のグラフは $N$ 個の山を持ち, 振動が細かくなり, $N$ が大きくなると, $x=0$ の近くでは(より正確には整数の近くの $x$ においては) $D_N(x)$ の値が大きくなり, 他での値は小さくなる.

次のセルを見よ. $\QED$

```julia
# フーリエ級数のディリクレ核 D_N(x) のグラフ

D(N,x) = iszero(x) ? 2N+1 : sin(π*(2N+1)*x)/(sin(π*x))
PP = []
for N in [1:6; 10; 20; 30]
    x = -0.5:0.001:0.5
    P = plot(x, D.(N,x), title="N=$N", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[4:6]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

```julia
plot(PP[7:9]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

**問題:** Fourier級数のDirichlet核 $\ds D_N(x)=\frac{\sin(\pi(2N+1)x)}{\sin(\pi x)}$ は上の方で示したように, $\int_0^1 D_N(x)\,dx=1$ を満たしている. しかし, その絶対値 $|D_N(x)|$ の積分については,

$$
\int_0^1 |D_N(x)|\,dx \to \infty \quad (N\to\infty)
$$

が成立していることを示せ.

**解答例:** $|\sin(\pi x)| \leqq \pi |x|$ なので,

$$
\int_0^1 |D_N(x)|\,dx = 2\int_0^{1/2} |D_N(x)|\,dx \geqq
2\int_0^{1/2} \frac{|\sin(\pi(2N+1)x)|}{\pi x}\,dx.
$$

$x=t/(π(2N+1))$ と置換すると, 

$$
\begin{aligned}
&
2\int_0^{1/2} \frac{|\sin(\pi(2N+1)x)|}{\pi x}\,dx =
\frac{2}{\pi}\int_0^{\pi(2N+1)/2} \frac{|\sin t|}{t}\,dt =
\frac{2}{\pi}\sum_{k=1}^{2N+1}\int_{(k-1)\pi/2}^{k\pi/2} \frac{|\sin t|}{t}\,dt
\\ &\qquad\geqq
\frac{2}{\pi}\sum_{k=1}^{2N+1}\int_{(k-1)\pi/2}^{k\pi/2}\frac{|\sin t|}{k\pi/2}\,dt =
\left(\frac{2}{\pi}\right)^2\int_0^{\pi/2}\sin t\,dt \sum_{k=1}^{2N+1}\frac{1}{k} =
\left(\frac{2}{\pi}\right)^2\sum_{k=1}^{2N+1}\frac{1}{k}.
\end{aligned}
$$

そして, $\ds\sum_{k=1}^{2N+1}\frac{1}{k}\sim\log(2N+1)$. 以上をまとめると,

$$
\int_0^1 |D_N(x)|\,dx \geqq \left(\frac{2}{\pi}\right)^2\sum_{k=1}^{2N+1}\frac{1}{k} \sim \log(2N+1).
$$

これより, 左辺は $N\to\infty$ のとき $\log N$ 以上のオーダーで無限大に発散することがわかる. $\QED$

**解説:** $N\to \infty$ のとき, $\ds \int_0^1 D_N(x)\,dx=1$ の左辺の積分はそれぞれ $\infty,-\infty$ に発散する正と負の成分が互いにキャンセルすることによって有限の値が残るという計算になる. これが, Fourier級数の収束の理論がややこしくなる理由の1つであると考えられる. $\QED$

```julia
# |sin πx| ≤ π|x| の確認

f(x) = abs(sin(π*x))
g(x) = π*abs(x)
x = -1/2:0.002:1/2
plot(size=(400, 250), legend=:top)
plot!(x, f.(x), label="|sin(pi x)|")
plot!(x, g.(x), label="pi |x|")
```

```julia
# |D_N(x)| の積分の漸近挙動の確認

D(N,x) = iszero(x) ? π*(2N+1) : sin(π*(2N+1)*x)/sin(π*x)
F(N) = quadgk(x-> abs(D(N,x)), 0, 1)[1]
G(N) = (2/π)^2*log(2N+1)+1
n = [1,2,3,6,10,20,30,60,100,200,300,600]
plot(size=(400,250), legend=:topleft, xlabel="N", xscale=:log)
plot!(n, F.(n), label="integal of |D_N(x)| from x=0 to 1", lw=2)
plot!(n, G.(n), label="(2/pi)^2*log(2N+1)+1", lw=2, ls=:dash)
```

### Fourier級数に関するRiemannの局所性定理

**Riemannの局所性定理:** $f$ は $\R$ 上の周期 $1$ を持つ函数であり, $[0,1)$ 上で可積分であると仮定し, $0<\delta<1/2$ であるとする. このとき, 

$$
\lim_{N\to\infty}\int_{-1/2}^{-\delta} f(x+y)D_N(y)\,dy = 0, \quad
\lim_{N\to\infty}\int_\delta^{1/2} f(x+y)D_N(y)\,dy = 0.
$$

これより, $\ds\sum_{k=-N}^N a_n(f)e^{2\pi ikx} = \int_{-1/2}^{1/2} f(x+y) D_N(y)\,dy$ が $N\to\infty$ で収束することと, $\ds\int_{-\delta}^\delta f(x+y) D_N(y)\,dy$ が $N\to\infty$ で収束することは同値であり, 収束する場合には同じ値に収束することがわかる.  後者が収束するか否かは $x$ の近くでの函数 $f$ の様子だけで決まることに注意せよ. 

**証明:**
$$
\begin{aligned}
&
\int_{-1/2}^{-\delta} f(x+y)D_N(y)\,dy = \int_{-1/2}^{-\delta} \frac{f(x+y)}{\sin(\pi y)}\sin(\pi(2N+1)y\,dy,
\\ &
\int_\delta^{1/2} f(x+y)D_N(y)\,dy = \int_\delta^{1/2} \frac{f(x+y)}{\sin(\pi y)}\sin(\pi(2N+1)y\,dy.
\end{aligned}
$$

であり, 区間 $(-1/2,-\delta)$, $(\delta,1/2)$ で $\ds \frac{f(x+y)}{\sin(\pi y)}$ は可積分なので, Riemann-Lebesgueの定理より, これらは $N\to\infty$ で $0$ に収束する. $\QED$


### Fourier級数の収束 (Diniの条件)

**定理(Diniの条件):** $f$ は $\R$ 上の周期 $1$ を持つ函数であり, 区間 $[0,1)$ 上で可積分であると仮定し, $x\in \R$ であるとする. さらに, $0<\delta<1/2$ を満たすある実数 $\delta$ が存在して,  

$$
\int_0^\delta \frac{|(f(x+y) + f(x-y) - 2f(x)|}{y}\,dy <\infty
\tag{$*$}
$$

が成立していると仮定する. この条件($*$)を**Diniの条件**と呼ぶ. このとき, 

$$
\lim_{N\to\infty}\sum_{k=-N}^N a_n(f)e^{2\pi ikx} = \lim_{N\to\infty}\int_{-1/2}^{1/2} f(x+y) D_N(y)\,dy = f(x).
$$

**証明:** Fourier変換の場合と同様に計算すると, 

$$
\begin{aligned}
\sum_{k=-N}^N a_n(f)e^{2\pi inx} - f(x) &=
\int_{-1/2}^{1/2} f(x+y) D_N(y)\,dy - f(x)\int_{-1/2}^{1/2}D_N(y)\,dy
\\ &=
\int_{-1/2}^{1/2} (f(x+y)-f(x)) D_N(y)\, dy
\\ &=
\int_0^{1/2} (f(x+y)-f(x)) D_N(y)\, dy +
\int_{-1/2}^0 (f(x+y)-f(x)) D_N(y)\, dy
\\ &=
\int_0^{1/2} (f(x+y)-f(x)) D_N(y)\, dy +
\int_0^{1/2} (f(x-y)-f(x)) D_N(y)\, dy
\\ &=
\int_0^{1/2} \frac{f(x+y)+f(x-y)-2f(x)}{y} \frac{y}{\sin(\pi y)} \sin(\pi(2N+1)y)\,dy.
\end{aligned}
$$

Diniの条件より, Riemann-Lebesgueの定理を使用できるので, これは $N\to\infty$ で $0$ に収束することがわかる. $\QED$


**例:** $f(x)$ は周期 $1$ を持つ $\R$ 上の函数であり, $\ds\int_0^1|f(x)|\,dx<\infty$ を満たしていると仮定し, 実数 $x$ を任意に取る. $y\searrow 0$ のとき $f(x+y), f(x-y)$ は収束していると仮定し, それぞれの収束先を $f(x+0), f(x-0)$ と書くことにする. さらに $y\searrow 0$ で $\ds\frac{f(x+y)-f(x+0)}{y}$ と $\ds\frac{f(x-y)-f(x-0)}{y}$ が収束していると仮定する.  特に $f$ が $x$ で微分可能ならばそれらの条件が成立している. 

このとき, 十分小さな $\delta>0$ を取ると, $\ds\frac{f(x+y)+f(x-y)-(f(x+0)+f(x-0))}{y}$ は $0<y<\delta$ で有界になり, 特にそこで可積分になる. ゆえに

$$
f(x)=\frac{f(x+0)+f(x-0)}{2}
$$

ならば, Diniの条件が満たされており, 上の定理より,

$$
\lim_{N\to\infty}\sum_{k=-N}^N a_n(f)e^{2\pi ikx} = \lim_{N\to\infty}\int_{-1/2}^{1/2} f(x+y) D_N(y)\,dy = f(x).
$$

となる. $\QED$


### Besselの不等式

**定理:** $\ds a_n=\int_0^1 f(x)e^{-2\pi inx}\,dx$ とおくとき, 整数 $a<b$ に対して,

$$
\int_0^1 |f(x)|^2\,dx \geqq \sum_{n=a}^b |a_n|^2.
$$

**証明:** 複素数値函数の内積 $(f,g)$ を $\ds (f,g)=\int_0^1 \overline{f(x)}g(x)\,dx$ と定め, $\ds \|f\|=\sqrt{(f,f)}$ とおくと, 整数 $m,n$ に対して,

$$
(e^{2\pi imx}, e^{2\pi inx}) = \int_0^1 e^{2\pi i(n-m)x}\,dx = \delta_{m,n} =
\begin{cases}
1 & (m=n) \\
0 & (m\ne n)
\end{cases}
$$

なので, $\ds s(x)=\sum_{n=a}^b a_n e^{2\pi inx}$ とおくと,

$$
\begin{aligned}
&
(f,f) = \int_0^1 |f(x)|^2\,dx
\\ &
(s,f) = \sum_{n=a}^b \overline{a_n}\int_0^1 e^{-2\pi inx}f(x)\,dx = 
\sum_{n=a}^b \overline{a_n}a_n = \sum_{n=a}^b |a_n|^2,
\\ &
(s,s) = \sum_{m,n=a}^b\overline{a_m}a_n(e^{2\pi imx}, e^{2\pi inx}) = 
\sum_{m,n=a}^b\overline{a_m}a_n\delta_{m,n} = \sum_{n=a}^b |a_n|^2,
\\ &
0\leqq \|f-s\|^2=
\|f\|^2-(s,f)-\overline{(f,s)}+\|s\|^2 =
\int_0^1 |f(x)|^2\,dx - \sum_{n=a}^b |a_n|^2.
\end{aligned}
$$

これより, 示したい不等式が得られる. $\QED$

**注意:** Besselの不等式は内積に関する平易な線形代数に過ぎない. $\QED$


### 区分的に連続微分可能な連続函数の場合

$f(x)$ は周期1を持つ $\R$ 上の連続函数であり, $0\leqq x_1<x_1<\cdots<x_r<1$, $x_0=x_r-1$ でかつ $x_j$ と整数の和になっていない $x$ で $f$ は $C^1$ 級(すなわち微分可能で導函数が連続)になっていると仮定する. さらに, $h\searrow0$ のとき, 

$$
\frac{f(x_j+h)-f(x_j)}{h}\to f'(x_j+0) \leftarrow f'(x_j+h), \qquad
\frac{f(x_j-h)-f(x_j)}{-h}\to f'(x_j-0) \leftarrow f'(x_j-h).
$$

と収束していると仮定する. この仮定は $f$ の導函数の開区間 $(x_{j-1},x_j)$ への制限が自然に閉区間 $[x_{j-1},x_j]$ 上に連続に拡張されることを意味している. このとき, $f(x)$ は**区分的に連続微分可能**もしくは区分的に $C^1$ 級であるということにする. 

$\ds a_n=\int_0^1 f(x)e^{-2\pi inx}\,dx$ とおく.

このとき, $f(x)$ はすべての実数 $x$ についてDiniの条件を満たしているので, 

$$
\lim_{N\to\infty}\sum_{n=-N}^N a_n e^{2\pi inx} = f(x)
$$

となる. そして, この場合にはより強く次が成立している.

**定理:** $f(x)$ が上の条件を満たしているとき, Fourier級数 $\ds\sum_{n=-\infty}^\infty a_n e^{2\pi inx}$ は $f$ に一様絶対収束している. 

**証明:** $\ds b_n = \int_0^1 f'(x) e^{-2\pi inx}\,dx$ とおく. ここで, $x$ が $x_j$ と整数の和のとき $f'(x)$ の値は定義されていないが, 区間に分けて積分することによって $b_n$ がwell-definedであることに注意せよ. このとき, 次のように部分積分を実行できることがわかる: $n\ne 0$ のとき, 

$$
a_n = 
\left[f(x)\frac{e^{-2\pi inx}}{-2\pi in}\right]_0^1 -
\int_0^1 f'(x) \frac{e^{-2\pi inx}}{-2\pi in}\,dx = \frac{b_n}{2\pi in}, \qquad
|a_n| = \frac{|b_n|}{2\pi|n|}
$$

Besselの不等式より,

$$
\sum_{n=-\infty}^\infty |b_n|^2 \leqq \int_0^1 |f'(x)|^2\,dx < \infty.
$$

一般に非負の実数 $\alpha,\beta$ について $\ds\alpha\beta\leqq \frac{\alpha^2+\beta^2}{2}$ なので, $n\ne 0$ のとき, 

$$
|a_n| = |n||a_n|\cdot\frac{1}{|n|} \leqq \frac{1}{2}\left(|n|^2|a_n|^2 + \frac{1}{|n|^2}\right).
$$

したがって, 

$$
\begin{aligned}
\sum_{n=-\infty}^\infty |a_n| &=
|a_0| + \sum_{n\ne 0}^\infty|a_n| \leqq
|a_0| + \frac{1}{2}\sum_{n\ne 0}^\infty\left(|n|^2|a_n|^2 + \frac{1}{|n|^2}\right)
\\ &\leqq
|a_0| + \frac{1}{2}\sum_{n\ne 0}^\infty\left(\frac{|b_n|^2}{(2\pi)^2} + \frac{1}{|n|^2}\right)
< \infty.
\end{aligned}
$$

これで, $\sum_{n=-\infty}^\infty a_n e^{2\pi inx}$ が一様絶対収束していることがわかった. この級数は $f$ に各点収束しているので, $f$ に一様絶対収束していることが示されたことになる. $\QED$


### 一点でLipschitz条件を満たす函数の場合

**定義:** 函数 $f(x)$ が $x=x_0$ で指数 $\alpha>0$ の**Lipschitz条件を満たす**とは, ある $C>$ と $\delta>0$ が存在して, $|x-x_0|<\delta$ ならば $|f(x)-f(x_0)|\leqq C|x-x_0|^\alpha$ が成立することであると定める.  $\QED$

$f(x)$ が $x=x_0$ でLipschitz条件を満たしているならば, $f(x)$ は $x=x_0$ で連続になる. 

函数 $f(x)$ が $x=x_0$ で連続でかつ $h\searrow0$ における極限達

$$
\frac{f(x_0+h)-f(x_0)}{h}\to f'(x_0+0), \qquad
\frac{f(x_0-h)-f(x_0)}{-h}\to f'(x_0-0)
$$

を持つならば, $x=x_0$ において指数 $\alpha=1$ のLipschitz条件を満たす. 特に, $f(x)$ が $x=x_0$ で微分可能ならば指数 $\alpha=1$ のLipschitz条件を満たす.

$f(x)$ は周期1を持つ $\R$ 上の函数で $\int_0^1|f(x)|\,dx<\infty$ を満たしており, $x=x_0$ で指数 $\alpha>0$ のLipschitz条件を満たしていると仮定し, $\ds a_n=\int_0^1 f(x)\,e^{-2\pi inx}\,dx$ とおく.

このとき, ある $C>0$ と $\delta > 0$ が存在して, $|x-x_0|<\delta$ ならば $|f(x)-f(x_0)|\leqq C|x-x_0|^\alpha$ となるので, $0<y<\delta$ ならば 

$$
\frac{|f(x_0+y)+f(x_0-y)-2f(x_0)|}{y}\leqq 2C y^{\alpha-1}
$$

となる. ゆえに

$$
\int_0^\delta \frac{|f(x_0+y)+f(x_0-y)-2f(x_0)|}{y}\,dy \leqq 
2C \int_0^\delta y^{\alpha-1}\,dy = \frac{2C\delta^\alpha}{\alpha} < \infty
$$

となって, $x=x_0$ におけるDiniの条件を満たしている. ゆえに

$$
\lim_{N\to\infty}\sum_{n=-N}^N a_n e^{2\pi inx_0} = f(x_0)
$$

が成立する.


### 局所的に一様Lipschitz条件を満たす函数の場合

**補題:** $f$ は $\R$ 上の周期 $1$ を持つ函数で $\int_0^1 |f(x)|\,dx<\infty$ を満たすものであり, $g$ は区間 $[a,b]$ 上の $C^1$ 級函数であるとする. このとき, $|p|\to \infty$ のとき, 次の積分 $J$ は $x$ について一様に $0$ に収束する:

$$
J = \int_a^b f(x+y)g(y)e^{ipy}\,dy.
$$

**証明:** 任意に $\eps > 0$ を取る. 

$\ds M = \sup_{a\leqq y\leqq b}|g(y)|$ とおく. $f(x)$ を周期 $1$ を持つ $C^1$ 級函数 $\tilde{f}$ で近似することによって, $\ds \int_0^1 |f(x)-\tilde{f}(x)|\,dx < \frac{\eps}{2M}$ となるようにできる. このとき, 

$$
\left|\int_a^b (f(x+y)-\tilde{f}(x+y))g(y)e^{ipx}\right| \leqq
M\int_0^1|f(t)-\tilde{f}(t)|\,dt < \frac{\eps}{2}.
$$

$\tilde{f}(x)$ は $C^1$ 級の周期函数なので有界函数になる. $\ds K = \sup_{x\in\R}|\tilde{f}(x)|$ とおく. $\ds\frac{\d}{\d y}(\tilde{f}(x+y)g(y))$ は連続函数でかつ $x$ について周期的なので $(x,y)\in\R\times[a,b]$ において, その絶対値は最大値 $L$ を持つ.

$\tilde{f}(x+y)g(y)$ は $y$ に関して $C^1$ 級なので, 部分積分を行うと,

$$
\int_a^b \tilde{f}(x+y)g(y)e^{ipx}\,dx =
\left[\tilde{f}(x+y)g(y)\frac{e^{ipx}}{ip}\right]_{y=a}^{y=b} -
\int_a^b \frac{\d}{\d y}(\tilde{f}(x+y)g(y))\frac{e^{ipx}}{ip}\,dx
$$

なので, $\ds |p|>\frac{\eps/2}{2KM+L(b-a)}$ とすると, 

$$
\left|\int_a^b \tilde{f}(x+y)g(y)e^{ipx}\,dx\right| \leqq
\frac{2KM}{p} + \frac{L(b-a)}{p} =
\frac{2KM+L(b-a)}{|p|} <
\frac{\eps}{2}.
$$

そのとき, 

$$
|J| \leqq 
\left|\int_a^b (f(x+y)-\tilde{f}(x+y))g(y)e^{ipx}\right| +
\left|\int_a^b \tilde{f}(x+y)g(y)e^{ipx}\,dx\right| < 
\frac{\eps}{2} + \frac{\eps}{2} = \eps.
$$

これで $J$ が $|p|\to 0$ で($x$ ) $0$ に一様収束することがわかった.


**定義:** 函数 $f(x)$ が区間 $I$ において指数 $\alpha>0$ の**一様Lipschitz条件を満たす**とは, ある $C>$ と $\delta>0$ が存在して, $x,x'\in I$ かつ $|x-x'|<\delta$ ならば $|f(x)-f(x')|\leqq C|x-x'|^\alpha$ が成立することであると定める.  $\QED$

$C^1$ 函数は閉区間上で指数 $\alpha=1$ の一様Lipschitz条件を満たす.

函数 $f(x)$ が区間 $(a,x_0)$ と $(x_0,b)$ 上で $C^1$ 級でかつ $x=x_0$ で連続であり, $h\searrow0$ における極限達

$$
\begin{aligned}
&
\frac{f(x_0+h)-f(x_0)}{h}\to f'(x_0+0)\leftarrow f'(x_0+h), 
\\ &
\frac{f(x_0-h)-f(x_0)}{-h}\to f'(x_0-0)\leftarrow f'(x_0-h)
\end{aligned}
$$

が存在するならば, $f(x)$ は区間 $(a,b)$ に含まれる任意の閉区間上で一様Lipschitz条件を満たす.


**定理:** $f(x)$ は周期1を持つ $\R$ 上の函数で $\int_0^1|f(x)|\,dx<\infty$ を満たしてるとし, $\ds a_n=\int_0^1 f(x)e^{-2\pi inx}\,dx$ とおく. さらに $f(x)$ は開区間 $I$ 上で一様Liptschitz条件を満たしていると仮定する. このとき, $N\to\infty$ とすると $\ds\sum_{n=-N}^N a_n e^{2\pi inx}$ は開区間 $I$ 上で $f(x)$ に一様収束する.

**証明:** $f$ の $I$ 上における一様Lipschitz性より, ある $\alpha>0$ と $C>0$ と $\eta>0$ が存在して, $x,x'\in I$ かつ $|x-x'|<\eta$ ならば $|f(x)-f(x')|\leqq C|x-x'|^\alpha$ となる.

一般に $\ds 0<\delta<\frac{1}{2}$ のとき, 

$$
\begin{aligned}
&
\sum_{k=-N}^N a_n(f)e^{2\pi inx} - f(x) = I+J,
\\ &
I = \int_0^\delta \frac{f(x+y)+f(x-y)-2f(x)}{y} \frac{y}{\sin(\pi y)} \sin(\pi(2N+1)y)\,dy,
\\ &
J = \int_\delta^{1/2} (f(x+y)+f(x-y)-2f(x)) \frac{1}{\sin(\pi y)} \sin(\pi(2N+1)y)\,dy.
\end{aligned}
$$

が成立しているのであった. 

$\ds 0<y<\frac{1}{2}$ のとき, $N$ によらずに

$$
\frac{1}{\pi} < \frac{y}{\sin(\pi y)} < \frac{1}{2}, \qquad
|\sin(\pi(2N+1)y)| \leqq 1
$$

が成立しているので, $x\in I$ のとき, $x$ から $I$ の両端までの距離の小さい方を $d$ とし,  $\ds 0<\delta<\min\left\{\frac{1}{2}, d, \eta\right\}$ とすると, $f$ の $I$ 上における一様Lipschitz性より, 

$$
\frac{|f(x+y)+f(x-y)-2f(x)|}{y} \leqq 2Cy^{\alpha-1}\quad (0<y<\delta)
$$

となるので, 

$$
\begin{aligned}
|I|\leqq C\int_0^\delta y^{\alpha-1}\,dy = \frac{C\delta^\alpha}{\alpha}.
\end{aligned}
$$

ゆえに, 必要なら $\delta>0$ をさらに小さくすることによって, $\ds |I|<\frac{\eps}{2}$ となるようにできる. 上の補題によって, $J$ は $N\to\infty$ で $0$ に一様収束することがわかる. ゆえに($x$ と無関係の)ある $N_0$ が存在して $N\geqq N_0$ ならば $\ds |J|<\frac{\eps}{2}$ となるようにできる. そのとき, 

$$
\left|\sum_{k=-N}^N a_n(f)e^{2\pi inx} - f(x)\right| = |I|+|J| < \eps.
$$

これで $\ds\sum_{n=-N}^N a_n e^{2\pi inx}$ は開区間 $I$ 上で $f(x)$ に一様収束することを示せた. $\QED$


### Gibbs現象

周期 $1$ を持つ函数 $b(x)$ を次のように定める:

$$
b(x) = \frac{1}{2} - (x - \lfloor x\rfloor).
$$

ここで $\lfloor x\rfloor$ は $x$ 以下の最大の整数を表わす.  $b(x)$ のグラフの形は**のこぎり波**(sawtooth wave)と呼ばれることがある. 

**注意:** Bernoulli多項式 $\ds B_1(x)=x-\frac{1}{2}$ を用いると, $b(x)=-B_1(x-\lfloor x\rfloor)$ と書ける. $B_1(x-\lfloor x\rfloor)$ はEulerMaclaurinの和公式の $n=1$ の場合に出て来る. この意味でのこぎり波はEuler-Maclaurinの和公式とも関係がある. $\QED$

$\ds a_n=\int_0^1 b(x)e^{-2\pi inx}\,dx$ とおくと, $a_0=0$ であり, $n\ne 0$ のとき, 部分積分によって, 

$$
a_n = -\int_0^1 x e^{-2\pi inx}\,dx = 
-\left[x\frac{e^{-2\pi inx}}{-2\pi in}\right]_0^1 + \int_0^1 \frac{e^{-2\pi inx}}{-2\pi in}\,dx =
\frac{1}{2\pi in}.
$$

部分積分後の積分項は $n\ne 0$ なので消える. $b(x)$ のFourier級数の部分和は

$$
S_N(x) = \sum_{n=-N}^N a_n e^{2\pi inx} =
\sum_{n=-N}^N \frac{e^{2\pi inx}}{2\pi in} =
\sum_{n=1}^N\frac{\sin(2\pi nx)}{\pi n} =
2x\sum_{n=1}^N\sinc(2\pi nx)
$$

と書ける. ここで $\ds\sinc x = \frac{\sin x}{x}$ という記号法を用いた.

$x$ が $b(x)$ の不連続点である整数に近いところのどこかで $b(x)$ の値との違いがある値以上になってしまう(overshootしてしまう)という現象を**Gibbs現象**と呼ぶ.

実際, $S_N(x)$ の $\ds x=\frac{1}{2N}$ における値は $N\to\infty$ のとき $\ds b(x+0)=\frac{1}{2}$ の $1.178979744\cdots$ 倍に収束する. (同様に $S_N(x)$ の $\ds x=\frac{1}{2N}$ における値は $N\to\infty$ のとき $\ds b(x-0)=-\frac{1}{2}$ の $1.178979744\cdots$ 倍に収束する.) このことは以下のようにして示される. $N\to\infty$ のとき,

$$
\begin{aligned}
S_N\left(\frac{1}{2N}\right) &=
\frac{1}{N} \sum_{n=1}^N\sinc\left(\frac{\pi n}{N}\right)=
\frac{1}{\pi}\sum_{n=1}^N\sinc\left(\frac{\pi n}{N}\right)\,\frac{\pi}{N}
\\ &\to
\frac{1}{\pi}\int_0^\pi\sinc(x)\,dx =
\frac{1}{\pi}\int_0^\pi\frac{\sin x}{x}\,dx 
\\ &=
0.5894898722\cdots
=1.178979744\cdots\times\frac{1}{2}.
\end{aligned}
$$

**注意:** $0$ に近い正の実数として $\ds x=\frac{1}{2N}$ を選んだのは以下のような理由からである. 上と同様にして, 任意の $a>0$ に対して $\ds x=\frac{a}{2\pi N}$ とおいた場合を考えると, $N\to\infty$ のとき, 

$$
\begin{aligned}
S_N\left(\frac{a}{2N}\right) &=
\frac{a}{N} \sum_{n=1}^N\sinc\left(\frac{a\pi n}{N}\right)=
\frac{1}{\pi}\sum_{n=1}^N\sinc\left(\frac{a\pi n}{N}\right)\,\frac{a\pi}{N}
\\ &\to
\frac{1}{\pi}\int_0^{a\pi}\sinc(x)\,dx =
\frac{1}{\pi}\int_0^{a\pi}\frac{\sin x}{x}\,dx
\end{aligned}
$$

となる. この積分は $a=1$ のときに最大になる. $\QED$


**注意(一般の場合):** $f$ は $\R$ 上の周期1を持つ函数で $\ds\int_0^1|f(x)|\,dx<\infty$ を満たすものであるとする. 区間 $[a,x_0)$ と $(x_0,b]$ 上で $f$ は $C^1$ 級であり, $h\searrow0$ における極限達

$$
\begin{aligned}
&
f(x_0+h)\to f(x_0+0), \qquad
f(x_0-h)\to f(x_0-0), 
\\ &
\frac{f(x_0+h)-f(x_0+0)}{h}\to f'(x_0+0)\leftarrow f'(x_0+h)
\\ &
\frac{f(x_0-h)-f(x_0-0)}{-h}\to f'(x_0-0)\leftarrow f'(x_0-h)
\end{aligned}
$$

が存在すると仮定する. さらに $f(x_0+0)>f(x_0-0)$ であり, $\ds f(x_0) = \frac{f(x_0+0)+f(x_0-0)}{2}$ であると仮定する.

このとき, $f$ のFourier級数の部分和も $x=x_0$ の近くでGibbs現象を引き起こす. 

なぜならば, 函数 $g(x)$ を

$$
g(x) = f(x) - (f(x_0+0)-f(x_0-0))s(x-x_0)
$$

とおくと, 

$$
\begin{aligned}
&
g(x_0+0) = f(x_0+0) - \frac{f(x_0+0)-f(x_0-0)}{2} = \frac{f(x_0+0)+f(x_0-0)}{2} = f(x_0), 
\\ &
g(x_0-0) = f(x_0-0) + \frac{f(x_0+0)-f(x_0-0)}{2} = \frac{f(x_0+0)+f(x_0-0)}{2} = f(x_0)
\end{aligned}
$$

となり, $g$ は $x_0$ を含むある開区間 $I$ で指数 $1$ の一様Lipschitz条件を満たすことがわかる. ゆえに, $g$ のFourier級数の部分和は $I$ 上で $g$ に一様収束する. そして,

$$
f(x) = (f(x_0+0)-f(x_0-0))s(x-x_0) + g(x)
$$

であり, $(f(x_0+0)-f(x_0-0))s(x-x_0)$ のFourier級数の部分和が $x=x_0$ の近くでGibbs現象を引き起こすことが上の方の具体的な計算でわかっているので, $f(x)$ の Fourier級数の部分和も $x=x_0$ の近くでGibbs現象を引き起こすことがわかる. $\QED$

```julia
# (1/π)∫_0^{aπ} sinc(x) dx のグラフ

f(a) = quadgk(x->sin(x)/x, eps(), a*π)[1]/π
a = 0.2:0.01:6
plot(size=(400,250), legend=false, xlims=(0,maximum(a)))
plot!(title="(1/pi) int_0^{a pi} sin(x)/x dx", titlefontsize=11)
plot!(xlabel="a")
plot!(a, f.(a))
```

```julia
# のこぎり波のグラフ

sawtooth(x) = 1/2 - (x - floor(x))
x = -2:0.01:1.99
plot(x, sawtooth.(x), legend=false, size=(400, 150))
hline!([0], ls=:dot, color=:black)
```

```julia
# のこぎり波のGibbs現象
#
# Fourier級数の部分和が不連続点で上下にオーバーシュートしている.

sawtooth(x) = 1/2 - (x - floor(x))
S(N,x) = sum(n->sin(2π*n*x)/(π*n), 1:N)
PP = []
for N in [10, 20, 50, 100]
    x = -1:0.0005:0.99
    P = plot(legend=false, ylims=(-0.61, 0.61))
    plot!(x, sawtooth.(x))
    plot!(x, S.(N, x))
    plot!(title="N=$N", titlefontsize=10)
    push!(PP, P)
    
    x = -0.01:0.0005:0.2
    P = plot(legend=false, ylims=(0.28, 0.61))
    plot!(x, sawtooth.(x))
    plot!(x, S.(N, x))
    plot!(title="N=$N", titlefontsize=10)
    push!(PP, P)
end

plot(PP[1:2]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

```julia
plot(PP[3:4]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

```julia
plot(PP[5:6]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

```julia
plot(PP[7:8]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

**例(矩形波):** 矩形波 $f(x)$ を

$$
f(x) = 
\begin{cases}
 0 & (x-\lfloor x\rfloor = 0) \\
 1 & (0 < x-\lfloor x\rfloor < 1/2) \\
 0 & (x-\lfloor x\rfloor = 1/2) \\
-1 & (1/2 < x-\lfloor x\rfloor < 1) \\
\end{cases}
$$

と定めると,

$$
4\sum_{k=1}^\infty \frac{\sin(2\pi(2k-1)x)}{\pi(2k-1)} = f(x).
$$

以下のセルにおけるプロットを見よ. $\QED$

```julia
# 矩形波のGibbs現象
#
# Fourier級数の部分和が不連続点で上下にオーバーシュートしている.

squarewave(x) = (x - floor(x) ≤ 1/2) ? one(x) : -one(x)
S(K,x) = 4*sum(k->sin(2π*(2k-1)*x)/(π*(2k-1)), 1:K)
PP = []
for K in [5, 10, 25, 50]
    x = -1:0.0005:0.99
    P = plot(legend=false, ylims=(-1.22, 1.22))
    plot!(x, squarewave.(x))
    plot!(x, S.(K, x))
    plot!(title="K=$K", titlefontsize=10)
    push!(PP, P)
    
    x = -0.01:0.0005:0.2
    P = plot(legend=false, ylims=(0.7, 1.22))
    plot!(x, squarewave.(x))
    plot!(x, S.(K, x))
    plot!(title="K=$K", titlefontsize=10)
    push!(PP, P)
end

plot(PP[1:2]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

```julia
plot(PP[3:4]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

```julia
plot(PP[5:6]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

```julia
plot(PP[7:8]..., size=(500, 200), layout=@layout([a b{0.3w}]))
```

**例:** 周期1を持つ函数 $f(x)$ を次のように定める:

$$
f(x) =
\begin{cases}
 1/4 & (x-\lfloor x\rfloor=0) \\
 1/2 & (0  < x-\lfloor x\rfloor < 1/4) \\
  0  & (x-\lfloor x\rfloor=1/4) \\
-1/2 & (1/4< x-\lfloor x\rfloor < 1/2) \\
-1/4 & (x-\lfloor x\rfloor=1/2) \\
  0  & (1/2< x-\lfloor x\rfloor < 1). \\
\end{cases}
$$

このとき,

$$
\sum_{k=1}^\infty(-1)^{k-1}\frac{\cos(2\pi(2k-1)x)}{\pi(2k-1)} +
\sum_{l=1}^\infty\frac{\sin(2\pi(4l-2)x)}{\pi(2l-1)} =
f(x).
$$

部分和 $S_{M,N}(x)$ を

$$
S_{M,N}(x) =
\sum_{k=1}^M (-1)^{k-1}\frac{\cos(2\pi(2k-1)x)}{\pi(2k-1)} +
\sum_{l=1}^N \frac{\sin(2\pi(4l-2)x)}{\pi(2l-1)}
$$

と定める. このとき, $S_{N,N}(x)$ として, $\cos$ と $\sin$ の和の個数を同じにすると, $x-\lfloor x\rfloor=3/4$ にノイズが出てしまう.  正しい部分和は $S_{2N, N}(x)$ の方である. 以下のセルのプロットを見よ. $\QED$

```julia
function f(x)
    xx = x - floor(x)
    xx == 0 && return 1/4
    0 < xx < 1/4 && return 1/2
    xx == 1/2 && return 0.0
    1/4 < xx < 1/2 && return -1/2
    xx == 1/2 && return -1/4
    1/2 < xx && return 0.0
end
S(M,N,x) = sum(m->(-1)^(m-1)*cos(2π*(2m-1)x)/(π*(2m-1)), 1:M) + sum(n->sin(2π*(4n-2)*x)/(π*(2n-1)), 1:N)

PP = []
for N in [15, 45]
    M = N
    x = -1.0:0.0005:0.9995
    P = plot(legend=false)#, ylims=(-0.61, 0.61))
    plot!(x, f.(x))
    plot!(x, S.(M, N, x))
    plot!(title="(M,N)=($M,$N)", titlefontsize=10)
    push!(PP, P)

    x = 0.6:0.0005:0.9
    P = plot(legend=false, ylims=(-0.2, 0.2))
    plot!(x, f.(x))
    plot!(x, S.(M, N, x))
    plot!(title="(M,N)=($M,$N)", titlefontsize=10)
    push!(PP, P)

    x = -0.01:0.0005:0.26
    P = plot(legend=false, ylims=(0.35, 0.61))
    plot!(x, f.(x))
    plot!(x, S.(M, N, x))
    plot!(title="(M,N)=($M,$N)", titlefontsize=10)
    push!(PP, P)
end

plot(PP[1:3]..., size=(640, 200), layout=@layout([a b{0.2w} c{0.3w}]))
```

```julia
plot(PP[4:6]..., size=(640, 200), layout=@layout([a b{0.2w} c{0.3w}]))
```

```julia
PP = []
for N in [10, 30]
    M = 2N
    x = -1.0:0.0005:0.9995
    P = plot(legend=false)#, ylims=(-0.61, 0.61))
    plot!(x, f.(x))
    plot!(x, S.(M, N, x))
    plot!(title="(M,N)=($M,$N)", titlefontsize=10)
    push!(PP, P)

    x = 0.6:0.0005:0.9
    P = plot(legend=false, ylims=(-0.2, 0.2))
    plot!(x, f.(x))
    plot!(x, S.(M, N, x))
    plot!(title="(M,N)=($M,$N)", titlefontsize=10)
    push!(PP, P)

    x = -0.01:0.0005:0.26
    P = plot(legend=false, ylims=(0.35, 0.61))
    plot!(x, f.(x))
    plot!(x, S.(M, N, x))
    plot!(title="(M,N)=($M,$N)", titlefontsize=10)
    push!(PP, P)
end

plot(PP[1:3]..., size=(640, 200), layout=@layout([a b{0.2w} c{0.3w}]))
```

```julia
plot(PP[4:6]..., size=(640, 200), layout=@layout([a b{0.2w} c{0.3w}]))
```

### Fourier級数の総和核

#### Fourier級数の総和核の定義

$\R$ 上の周期 $1$ を持つ周期的函数(正確には周期的可測函数) $f(x)$ でその絶対値の周期分の定積分 $\int_0^1 |f(x)|\,dx$ が有限の値になるものを1次元トーラス(もしくは円周) $\T$ 上の可積分函数もしくは $L^1$ 函数と呼び, それら全体の集合を $L^1(\T)$ と書くことにする.  同様に, 絶対値の $p$ 上の周期分の積分が有限の値になるものを $L^p$ 函数と呼び, それら全体の集合を $L^p(\T)$ と書く. $f\in L^p(\T)$ の $L^p$ ノルムが

$$
\|f\|_p = \left(\int_0^1 |f(x)|^p\,dx\right)^{1/p}
$$

と定義される.

**定義:** $\rho_t\in L^1(\T)$ ($t>0$) で以下の条件を満たすものをFourier級数に関する**総和核**(summability kernel)と呼ぶ:

(s1) $\ds \int_{-1/2}^{1/2} \rho_t(x)\,dx = 1$.

(s2) ある $C>0$ が存在して, すべての $t>0$ について $\ds \int_{-1/2}^{1/2}|\rho_t(x)|\,dx \leqq C$.

(s3) $0<\delta<1/2$ ならば, $\ds\lim_{t\searrow 0}\int_{\delta<|x|<1/2} |\rho_t(x)|\,dx = 0$. $\QED$

この定義は, 猪狩惺著『実解析入門』岩波書店(1996年)の§8.5(b)に書いてあるものに等しい.

大雑把に言えば, Fourier級数の総和核は周期 $1$ を持つデルタ函数 $\ds\sum_{m\in\Z}\delta(x-m)$ に「収束」するような函数族のことである.

**総和核の作り方:** $\rho_t\in L^1(\T)$ ($t>0$) が(s1)と以下の条件を満たしていればFourier級数の総和核である.

(s2') ある $C_1>0$ が存在して, すべての $t>0$ について $\ds\sup_{|x|<1/2}\ds|\rho_t(x)|\leqq\frac{C_1}{t}$.

(s3') ある $C_2>0$ と $\eta > 0$ が存在して, すべての $t>0$ と $|x|<1/2$ について $\ds|\rho_t(x)|\leqq C_2\frac{t^\eta}{|x|^{\eta+1}}$.

**証明:** (s2)と(s3)を示せばよい. (s2)の積分を $|x|<t$ と $|x|>t$ に分割して, (s2'), (s3')を使うと, 

$$
\begin{aligned}
&
\int_{-1/2}|\rho_t(x)|\,dx \leqq I + J,
\\ &
I = 2\frac{C_1}{t}\int_0^{\min\{t,1/2\}}dx \leqq \frac{C_1}{t}2t = 2C_1,
\\ &
J = 2C_2\int_t^{1/2}\frac{t^\eta}{x^{\eta+1}}\,dx \leqq
2C_2\int_t^\infty \frac{t^\eta}{x^{\eta+1}}\,dx =
2C_2\int_1^\infty \frac{dy}{y^{\,\eta+1}} =
\frac{2C_2}{\eta}.
\end{aligned}
$$

これで(s2)が示された. $0<\delta<1/2$ であるとする. このとき, (s3') を使うと, 上と同様にして, 

$$
\int_{\delta<|x|<1/2} |\rho_t(x)|\,dx \leqq
2C_2\int_\delta^\infty \frac{t^\eta}{x^{\eta+1}}\,dx =
2C_2\int_{\delta/t}^\infty \frac{dy}{y^{\,\eta+1}} =
\frac{2C_2}{\eta}\left(\frac{\delta}{t}\right)^{-\eta} \to 0 \qquad (t\searrow 0).
$$

これで(s3)も示された. $\QED$

$f,g\in L^1(\T)$ の畳み込み積を次のように定義しておく:

$$
f*g(x) = \int_0^1 f(y)g(x-y)\,dy = \int_0^1 f(x-y)g(y)\,dy.
$$

このとき, Fourier変換の場合と同様にして以下を示すことができる.

**定理:** $\rho_t(x)$ はFourier級数の総和核であり, $f$ は周期1を持つ $\R$ 上の連続函数であるとする. このとき, $t\searrow 0$ のとき $\rho_t*f$ は $f$ に一様収束する. $\QED$

**定理:** $\rho_t(x)$ はFourier級数の総和核であり, $p\geqq 1$ であるとし, $f\in L^p(\T)$ であるとする. このとき, $t\searrow 0$ のとき $\|\rho_t*f - f\|_p\to 0$ となる. $\QED$

**問題:** 以上を示せ. $\QED$


#### Fourier級数のDirichlet核は総和核ではない

上の方で示したようにFourier級数のDirichlet核 $\ds D_N(x) = \sum_{n=-N}^N e^{2\pi inx} = \frac{\sin(\pi(2N+1)x)}{\sin(\pi x)}$ は

$$
\int_0^1 |D_N(x)|\,dx \sim \log(2N+1) \qquad (N\to\infty)
$$

を満たしているので, $D_{1/t}(x)$ は条件(s2)を満たしていない. 


#### Fourier級数のFejér核

Dirichlet核の加法平均

$$
F_N(x) = \frac{1}{N+1}\sum_{n=0}^N D_n(x) = \sum_{n=-N}^N \left(1-\frac{|n|}{N+1}\right)e^{2\pi inx}
$$

をFourier級数の**Fejér核**と呼ぶ. 

このとき, $\ds a_n = \int_0^1 f(y)e^{-2\pi iny}\,dy$ とおくと, 

$$
F_N*f(x) = 
\frac{1}{N+1}\sum_{n=0}^N D_n*f(x) =
\frac{1}{N+1}\sum_{n=0}^N \sum_{m=-n}^n a_m e^{2\pi imx}.
$$

ゆえに, $F_N*f(x)$ が $N\to\infty$ で収束することと, $D_n*f(x)$ が $n\to\infty$ でCesàro総和可能(すなわちFourier級数がCesàro総和可能)であることは同値であり, その収束先はCesàro和になる.  $D_n*f(x)$ が $n\to\infty$ で収束するならばCesàro総和可能になるが, 逆は成立しない.  Cesàro和は通常の無限和よりも収束し易い.

Fourier級数のFejér核は

$$
F_N(x) = \frac{1}{N+1}\left(\frac{\sin(\pi(N+1)x)}{\sin(\pi x)}\right)^2
$$

と書ける. この表示は一般に

$$
\left(\frac{z^{(N+1)/2}-z^{-(N+1)/2}}{z^{1/2}-z^{-1/2}}\right)^2 =
\left(z^{-N/2}+z^{-N/2+1}+\cdots+z^{N/2-1}+z^{N/2}\right)^2 =
\sum_{n=-N}^N(N+1-|n|)z^n
$$

であることを, $z=e^{2\pi ix}$ に適用すれば得られる. 例えば,

$$
\left(\frac{z^{4/2}-z^{-4/2}}{z^{1/2}-z^{-1/2}}\right)^2 =
\left(z^{-3/2}+z^{-1/2}+z^{1/2}+z^{3/2}\right)^2 =
z^{-3} + 2z^{-2}+3z^{-1}+4+3z+2z^2+z^3.
$$

上の表示より, Fourier級数のFejér核は $N+1=t^{-1/2}$ とおくことによってFourier級数の総和核とみなせることがわかる. 

```julia
# Fourier級数のFejer核のプロット

Fejer(N,x) = 1/(N+1)*(sin(π*(N+1)*x)/sin(π*x))^2
PP = []
for N in [10; 20; 30]
    x = -0.5:0.001:0.5
    P = plot(x, Fejer.(N,x), title="N=$N", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

#### Fourier級数のPoisson核

$0\leqq r<1$ に対する

$$
P_r(x) = \sum_{n\in\Z} r^{|n|} e^{2\pi inx} =
\frac{1-r^2}{1-2r\cos(2\pi x)+r^2}
$$

はFourier級数の**Poisson核**と呼ばれる. このとき, $P_r(x) > 0$ でかつ

$$
\int_0^1 P_r(x)\,dx = \sum_{n\in\Z}r^{|n|}\int_0^1 e^{2\pi in}\,dx = 1.
$$

$P_r(x)$ の最大値は $x=0$ のときの $\ds\frac{1+r}{1-r}$ であり, $\ds\frac{1+r}{1-r}\leqq\frac{2}{1-r}$.

さらに, $-1/2\leqq x\leqq 1/2$ のとき, $\cos(2\pi x)\leqq 1-8x^2$ となることより, 

$$
P_r(x) \leqq 
\frac{2(1-r)}{1-2r(1-8x^2)+r^2} =
\frac{2(1-r)}{(1-r)^2+16x^2} \leqq
\frac{1}{8}\frac{1-r}{x^2}.
$$

ゆえにFourier級数のPoisson核は $t=1-r$ とおくことによってFourier級数の総和核とみなされる.



```julia
x = -1/2:0.005:1/2
f(x) = 1 - 8*x^2
P = plot(size=(400,230), legend=:bottom)
plot!(x, cos.(2π.*x), label="cos(2pi x)")
plot!(x, f.(x), label="1 - 8x^2")
```

```julia
Poisson(r,x) = (1-r^2)/(1-2r*cos(2π*x)+r^2)
g(r,x) = 1/8*(1-r)/x^2
x = -1/2:0.005:1/2
r = 0.8
plot(size=(400,230), ylim=(0, 1.01*(1+r)/(1-r)))
plot!(title="r = $r", titlefontsize=10)
plot!(x, Poisson.(r, x), label="P_r(x)")
plot!(x, g.(r,x), label="(1/8)(1-r)/x^2")
```

**注意:** Fourier級数のPoisson核は本質的に絶対値が $1$ 未満の複素数 $z = re^{2\pi ix}$ に関する次の等比級数の実部に等しい(正確に言えば実部の2倍から1を引いたものに等しい):

$$
1+z+z^2+z^3+\cdots = \frac{1}{1-z}.
$$

実際, 

$$
\frac{1}{1-z} = \frac{1}{1-re^{2\pi ix}} =
\frac{1-re^{-2\pi ix}}{(1-re^{2\pi ix})(1-re^{-2\pi ix})} =
\frac{1-r\cos(2\pi x) + r\sin(2\pi x)}{1-2r\cos(2\pi x)+r^2}
$$

でかつ

$$
2\frac{1-r\cos(2\pi x)}{1-2r\cos(2\pi x)+r^2}-1 = 
\frac{1-r^2}{1-2r\cos(2\pi x)+r^2} = P_r(x).
$$

このようにFourier級数のPoisson核の理論は本質的に複素数の等比級数の理論である. $\QED$


**注意:** $-1/2\leqq x\leqq 1/2$ のとき, $\cos(2\pi x)\leqq 1-8x^2$ となることは以下のようにして証明される.

$f(x)=\cos(2\pi x)$, $f_1(x)=-4x+1$, $g(x)=1-8x^2$, $g_1(x)=-6x+2$ とおく. $|x|\leqq 1/2$ において $f(x)\leqq g(x)$ となることを示したい. $f(x)$ も $g(x)$ も偶函数なので, $0\leqq x\leqq 1/2$ の範囲でその不等式を示せば十分である.

$f'(x)=-2\pi\sin(2\pi x)$, $g'(x)=-16x$ であり, $0\leqq x\leqq 1/2$ において $f(x)$ は下に凸であり, $f'(0)=g'(0)=0$, $f'(1/4)=-2\pi<-4=g'(1/4)$ なので, $0\leqq x\leqq 1/4$ において $f'(x)\leqq g'(x)$ となる. そして, $f(0)=g(0)=1$ なので

$$
0\leqq x\leqq 1/4 \implies f(x)\leqq g(x).
$$

$1/4\leqq x\leqq 1/2$ において, $f(x)$ は下に凸であり, $g(x)$ は上に凸なので, $f(1/4)=f_1(1/4)=0<1/2=g_1(1/4)=g(1/4)$ と $f(1/2)=f_1(1/2)=g_1(1/2)=g(1/2)=-1$ より,

$$
1/4\leqq x\leqq 1/2 \implies f(x)\leqq f_1(x)\leqq g_1(x)\leqq g(x).
$$

以上を合わせると目的の結果が得られる. 

以上のような証明は以下のような図を描けば容易に発見できる. $\QED$

```julia
x = 0:0.005:1/2
x_1 = 1/4:0.005:1/2
f(x) = cos(2π*x)
f_1(x) = -4x + 1
g(x) = 1 - 8*x^2
g_1(x) = -6x + 2

P1 = plot(size=(400,240), legend=:bottomleft)
plot!(x,   g.(x),     label="g(x)=1-8x^2")
plot!(x_1, g_1.(x_1), label="g_1(x)=-6x+2", ls=:dash)
plot!(x_1, f_1.(x_1), label="f_1(x)=-4x+1", ls=:dash)
plot!(x,   f.(x),     label="f(x)=cos(2pi x)")
```

```julia
x = 0:0.005:1/2
df(x) = -2π*sin(2π*x)
dg(x) = -16x

P2 = plot(size=(400,240), legend=:topright)
plot!(x, dg.(x), label="g'(x)=-16x")
plot!(x, df.(x), label="f'(x)=-2pi cos(2pi x)")
```

```julia
# Fourier級数のPoisson核のプロット

Poisson(r,x) = (1-r^2)/(1-2r*cos(2π*x)+r^2)
PP = []
for r in [0.3; 0.8; 0.9]
    x = -0.5:0.001:0.5
    P = plot(x, Poisson.(r,x), title="r = $r", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

#### Fourier級数のGauss核

Fourier級数のGauss核を, Fourier変換のGauss核を周期的に足し上げて,

$$
G_t(x) = \sum_{m\in\Z} \frac{e^{-\pi(x-m)^2/t}}{\sqrt{t}} = \sum_{n\in\Z} e^{-\pi n^2 t + 2\pi inx}.
$$

と定める. 後者の等号は後で示すPoissonの和公式より示される. Gauss核もFourier級数の総和核である.

```julia
# Fourier級数のGauss核のプロット

Gauss(t,x; N=100) = (1/√t)*sum(m->e^(-π*(x-m)^2/t), -N:N)
PP = []
for t in [0.1; 0.01; 0.004]
    x = -0.5:0.001:0.5
    P = plot(x, Gauss.(t,x), title="t = $t", titlefontsize=10)
    push!(PP, P)
end
plot(PP[1:3]..., size=(750, 150), legend=false, layout=@layout([a b c]))
```

**注意:** Fourier級数のDirichlet核 $D_N(x)$, Fejér核 $F_N(x)$, Poisson核 $P_r(x)$, Gauss核 $G_t(x)$ はそれぞれ以下のようなFourier級数で表わされる:

$$
\begin{alignedat}{3}
D_N(x) &= \sum_{n=-N}^N e^{2\pi inx}
& &=
\frac{\sin(\pi(2N+1)x)}{\sin(\pi x)},
& &
\\
F_N(x) &= \sum_{n=-N}^N \left(1 - \frac{|n|}{N+1}\right) e^{2\pi inx}
& &=
\frac{1}{N+1}\left(\frac{\sin(\pi(N+1)x)}{\sin(\pi x)}\right)^2,
& &
\\
P_r(x) &= \sum_{n\in\Z} e^{-\alpha |n|} e^{2\pi inx}
& &=
\frac{1-r^2}{1-2r\cos(2\pi x)+r^2}
& &
\quad (r=e^{-\alpha}<1),
\\
G_t(x) &= \sum_{n\in\Z} e^{-\pi tn^2} e^{2\pi inx}
& &=
\sum_{m\in\Z} \frac{e^{-\pi(x-m)^2/t}}{\sqrt{t}}
& &
\quad (t>0).
\end{alignedat}
$$

$n=0$ を中心に上下対象に適当に重みを付けて足し上げている. 足し上げる範囲で同じ重みにするとDirichlet核になり, $n$ の絶対値に比例して重みを減少させるとFejér核になり, $n$ の絶対値の負の定数倍の指数函数で重みを減少させるとPoisson核になり, $n$ の2乗の負の定数倍の指数函数で重みを減少させるとGauss核になる. もちろん, これら以外にもたくさんの変種を考えることができる. $\QED$


## 三角函数へのFourier級数論の応用


### cosecとcotの部分分数展開

整数ではない実数 $t$ に対して, $-1/2\leqq x < 1/2$ 上の函数 $f(x)$ を

$$
f(x) = e^{2\pi itx} \qquad (-1/2\leqq x < 1/2)
$$

と定め, 周期 $1$ で $\R$ 上の函数に拡張したものを $f(x)$ と書くことにする.

このとき, $f(x)$ のFourier係数は次のように計算される:

$$
\begin{aligned}
a_n(f) = \int_{-1/2}^{1/2} e^{2\pi itx}e^{-2\pi nx}\,dx = 
\left[\frac{e^{2\pi i(t-n)x}}{2\pi i(t-n)}\right]_{x=-1/2}^{x=1/2} =
\frac{\sin(\pi(t-n))}{\pi(t-n)} =
(-1)^n\frac{\sin(\pi t)}{\pi(t-n)}.
\end{aligned}
$$

したがって, $-1/2<x<1/2$ のとき, 次の等式が成立している:

$$
\begin{aligned}
e^{2\pi itx} &= 
\lim_{N\to\infty}\sum_{n=-N}^N a_n(f)e^{2\pi i nx} =
\frac{\sin(\pi t)}{\pi}\lim_{N\to\infty}\sum_{n=-N}^N \frac{(-1)^ne^{2\pi inx}}{t-n}
\\ &=
\frac{\sin(\pi t)}{\pi}
\left[\frac{1}{t} + \sum_{n=1}^\infty(-1)^n\left(\frac{e^{2\pi inx}}{t-n} + \frac{e^{-2\pi inx}}{t+n}\right)\right].
\end{aligned}
$$

この等式の両辺の実部と虚部を比較することによって, $-1/2<x<1/2$ のとき,

$$
\begin{aligned}
&
\cos(2\pi tx) =
\frac{\sin(\pi t)}{\pi}
\left[\frac{1}{t} + \sum_{n=1}^\infty(-1)^n\left(\frac{\cos(2\pi nx)}{t-n} + \frac{\cos(2\pi nx)}{t+n}\right)\right],
\\ &
\sin(2\pi tx) =
\frac{\sin(\pi t)}{\pi}
\sum_{n=1}^\infty(-1)^n\left(\frac{\sin(2\pi nx)}{t-n} - \frac{\sin(2\pi nx)}{t+n}\right).
\end{aligned}
$$

前者で $x=0$, $x\to 1/2$ とすると, 以下の結果が得られる:

$$
\begin{aligned}
&
\pi\cosec(\pi t) = \frac{\pi}{\sin(\pi t)} =
\frac{1}{t} + \sum_{n=1}^\infty(-1)^n\left(\frac{1}{t-n} + \frac{1}{t+n}\right),
\\ &
\pi\cot(\pi t) = \frac{\pi\cos(\pi t)}{\sin(\pi t)} =
\frac{1}{t} + \sum_{n=1}^\infty\left(\frac{1}{t-n} + \frac{1}{t+n}\right).
\end{aligned}
$$


### sinの無限積表示

$\ds \frac{d}{dt}\log\sin(\pi t) = \pi\cot(\pi t)$ と $\ds \frac{d}{dt}\log(\pi t) = \frac{1}{t}$ と $\pi\cot(\pi t)$ の部分分数展開の公式より,

$$
\frac{d}{dt}\log\frac{\sin(\pi t)}{\pi t} =
\sum_{n=1}^\infty\left(\frac{1}{t-n}+\frac{1}{t+n}\right) =
\sum_{n=1}^\infty\left(\frac{-1/n}{1-t/n}+\frac{1/n}{1+t/n}\right).
$$

両辺を $t=0$ から $t=s$ まで積分すると, $t\searrow 0$ で $\ds\log\frac{\sin(\pi t)}{\pi t}\to \log 1=0$ となることより, 

$$
\log\frac{\sin(\pi s)}{\pi s} =
\sum_{n=1}^\infty\left(\log\left(1-\frac{s}{n}\right)+\log\left(1+\frac{s}{n}\right)\right)=
\sum_{n=1}^\infty\log\left(1-\frac{s^2}{n^2}\right).
$$

すなわち, 

$$
\frac{\sin(\pi s)}{\pi s} =
\prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right).
$$

この公式を**sinの無限積表示**, **sinの無限乗積展開**などと呼ぶことにする.


### ガンマ函数とsinの関係

ガンマ函数は次の表示を持つのであった: 

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

さらに $\ds
\Gamma(s)\Gamma(1-s)=
B(s,1-s)=
\int_0^\infty\frac{t^{s-1}\,dt}{1+t}=
\frac{1}{s}\int_0^\infty\frac{du}{1+u^{1/s}}
$ なので以下の総まとめ的な公式が得られる:

$$
\Gamma(s)\Gamma(1-s)=
B(s,1-s)=
\int_0^\infty\frac{t^{s-1}\,dt}{1+t}=
\frac{1}{s}\int_0^\infty\frac{du}{1+u^{1/s}} =
\frac{\pi}{\sin(\pi s)}
\quad(0<s<1).
$$

$\ds \Gamma(s)\Gamma(1-s) = \frac{\pi}{\sin(\pi s)}$ は **Euler's reflection formula** と呼ばれる. これより, 例えば, 

$$
\begin{aligned}
&
\Gamma(1/2)^2 = B(1/2,1/2) = 2\int_0^\infty\frac{du}{1+u^2} = \pi,
\\ &
\Gamma(1/3)\Gamma(2/3) = B(1/3,2/3) = 3\int_0^\infty\frac{du}{1+u^3} = \frac{2\pi}{\sqrt{3}},
\\ &
\Gamma(1/4)\Gamma(3/4) = B(1/4,3/4) = 4\int_0^\infty\frac{du}{1+u^4} = \sqrt{2}\,\pi,
\\ &
\Gamma(1/6)\Gamma(5/6) = B(1/6,5/6) = 6\int_0^\infty\frac{du}{1+u^6} = 2\pi.
\end{aligned}
$$


### Wallisの公式

$\sin$ の無限乗積展開

$$
\frac{\sin(\pi s)}{\pi s} =
\prod_{n=1}^\infty\left(1-\frac{s^2}{n^2}\right).
$$

で $s=1/2$ とおくと, 

$$
\frac{2}{\pi} = \prod_{n=1}^\infty\left(1-\frac{1}{(2n)^2}\right) =
\prod_{n=1}^\infty \frac{(2n-1)(2n+1)}{(2n)(2n)}.
$$

すなわち,

$$
\lim_{n\to\infty}\frac{2\cdot2\cdot4\cdot4\cdots(2n)(2n)}{1\cdot3\cdot3\cdot5\cdots(2n-1)(2n+1)} =
\prod_{n=1}^\infty\frac{(2n)(2n)}{(2n-1)(2n+1)} = \frac{\pi}{2}.
$$

これを**Wallisの公式**と呼ばれる. さらに, これ

$$
2\cdot4\cdots(2n) = 2^n n!, \quad
1\cdot3\cdots(2n-1) = \frac{(2n)!}{2^n n!}, \quad
3\cdot5\cdots(2n+1) = (2n+1)\frac{(2n)!}{2^n n!}
$$

より

$$
\frac{2\cdot2\cdot4\cdot4\cdots(2n)(2n)}{1\cdot3\cdot3\cdot5\cdots(2n-1)(2n+1)} =
\frac{1}{2n+1}\left(\frac{2^{2n}(n!)^2}{(2n)!}\right)
$$

これが $\ds\frac{\pi}{2}$ に収束することとWallisの公式は同値である. そして, これは

$$
\frac{(2n)!}{2^{2n}(n!)^2} \sim \frac{1}{\sqrt{n\pi}}
$$

と同値であることもわかる. これもWallisの公式と呼ばれる.


### Stirlingの近似公式

$\log n! = \log 1 + \log 2 + \cdots + \log n$ の $\log x$ の積分による近似を精密に実行すればStirlingの公式

$$
\log n! = n\log n - n + \frac{1}{2}\log n + \log\sqrt{2\pi} + o(1)
\tag{$*$}
$$

が得られることを説明しよう. $\log\sqrt{2\pi}$ の定数項を得るためにWallisの公式を使うことになる.

正の整数 $k$ に対して, 

$$
a_{2k-1} = \frac{1}{2}\log k - \int_{k-1/2}^k \log x\,dx, \quad
a_{2k} = \int_k^{k+1/2} \log x\,dx - \frac{1}{2}\log k
$$

とおく. $\log x$ が単調増加函数であることより, $a_n>0$ となり, さらに $\log x$ が上に凸な函数であることより, $a_n$ が単調減少することがわかる. $\log x$ の導函数 $1/x$ は $x\to\infty$ で $0$ に収束することから $a_n\to 0$ となることもわかる. ゆえに $a_n$ 達から作られる交代級数は収束する. そして, そのとき,

$$
\log k = \int_{k-1/2}^{k+1/2} \log x\,dx + a_{2k-1} - a_{2k}
$$

なので

$$
\begin{aligned}
\log n! &= \sum_{k=1}^n \log k =
\frac{1}{2}\log 1 + \int_1^n \log x\,dx + \frac{1}{2}\log n+ \sum_{j=2}^{2n-1} (-1)^{j-1}a_j 
\\ &=
n\log n - n + 1 + \frac{1}{2}\log n + \sum_{j=2}^{2n-1} (-1)^{j-1}a_j.
\end{aligned}
$$

ゆえに $\ds c_n = 1 + \sum_{j=2}^{2n-1} (-1)^{j-1}a_j$ とおくと, 

$$
\log n! = n\log n - n + \frac{1}{2}\log n + c_n.
$$

$c_n$ は上で述べたことより, 収束する交代級数として, $n\to\infty$ で収束する. その収束先 $c$ を求めよう. Wallisの公式

$$
\frac{(2n)!}{2^{2n}(n!)^2} \sim \frac{1}{\sqrt{n\pi}}
$$

に $(2n)!=(2n)^{2n}e^{-2n}\sqrt{2n}\;e^{c_{2n}}$, $n!=n^n e^{-n}\sqrt{n}\;e^{c_n}$ を代入すると

$$
\frac{1}{\sqrt{n\pi}} \sim
\frac{(2n)!}{2^{2n}(n!)^2} = 
\frac
{(2n)^{2n}e^{-2n}\sqrt{2n}\;e^{c_{2n}}}
{2^{2n}n^{2n} e^{-2n} n\,e^{2c_n}} =
\sqrt{\frac{2}{n}}\;e^{c_{2n}-2c_n}
$$

なので, 両辺を $\ds \sqrt{\frac{2}{n}}$ で割って, $n\to\infty$ とすると,

$$
e^{-c} = \frac{1}{\sqrt{2\pi}}.
$$

すなわち, $c=\log\sqrt{2\pi}$ であることがわかった. これで, Stirlingの公式($*$)が証明された. 

```julia
n = big"1000000"
Float64(factorial(n)/(n^n*e^(-n)*√n)), √(2π)
```

```julia
u = symbols("u", positive=true)
[integrate(k/(1+u^k), (u, 0, oo)) for k in 2:6]
```

### ゼータ函数の正の偶数での特殊値


**問題:** $\sin$ の無限乗積展開を用いて, $\ds\zeta(2)=\sum_{n=1}^\infty \frac{1}{n^2}$ を求めよ.

**解答例:** $\sin$ のMaclaurin展開を使うと, 

$$
\frac{\sin(\pi x)}{\pi x} = 1 - \frac{\pi^2}{6}x^2 + O(x^4).
$$

$\sin$ の無限乗積展開を使うと, 

$$
\frac{\sin(\pi x)}{\pi x} = 1 - \left(\sum_{n=1}^\infty \frac{1}{n^2}\right)x^2 + O(x^4).
$$

これらを比較すると,

$$
\zeta(2) = \sum_{n=1}^\infty \frac{1}{n^2} = \frac{\pi^2}{6}.
\qquad\QED
$$


**問題:** $\sin$ の無限乗積展開を用いて, $\ds\zeta(4)=\sum_{n=1}^\infty \frac{1}{n^4}$ を求めよ.

**解答例:** $\sin$ のMaclaurin展開を使うと,

$$
\begin{aligned}
\frac{\sin(\pi x)}{\pi x}\frac{\sin(\pi i x)}{\pi(ix)} &=
\left(1 - \frac{\pi^2}{6}x^2 + \frac{\pi^4}{120}x^4 + O(x^6)\right)
\left(1 + \frac{\pi^2}{6}x^2 + \frac{\pi^4}{120}x^4 + O(x^6)\right) 
\\ &=
1 - \frac{\pi^4}{90}x^4 + O(x^6).
\end{aligned}
$$

$\sin$ の無限乗積展開を使うと,

$$
\begin{aligned}
\frac{\sin(\pi x)}{\pi x}\frac{\sin(\pi i x)}{\pi(ix)} &=
\prod_{n=1}^\infty\left(1-\frac{x^2}{n^2}\right)
\prod_{n=1}^\infty\left(1+\frac{x^2}{n^2}\right) =
\prod_{n=1}^\infty\left(1-\frac{x^4}{n^4}\right)
\\ &=
1 - \left(\sum_{n=1}^\infty\frac{1}{n^4}\right)x^4 + O(x^8).
\end{aligned}
$$

これらを比較すると,

$$
\zeta(4) = \sum_{n=1}^\infty \frac{1}{n^4} = \frac{\pi^4}{90}.
\qquad\QED
$$

```julia
x = symbols("x")
f(x) = sin(π*x)/(π*x)
series(f(x), x, n=10)
```

**注意:** 上のセルの計算結果を使うと, $\ds\zeta(2)=\frac{\pi^2}{6}$ を示せる. $\QED$

```julia
x = symbols("x")
f(x) = sin(π*x)/(π*x)
series(f(x)*f(im*x), x, n=12)
```

**注意:** 上のセルの計算結果を使うと, $\ds\zeta(4)=\frac{\pi^4}{90}$ を示せる. $\QED$

```julia
x = symbols("x")
f(x) = sin(π*x)/(π*x)
ω = exp(im*Sym(π)/4)
series(f(x)*f(ω*x)*f(ω^2*x)*f(ω^3*x), x, n=24)
```

**注意:** 上のセルの計算結果を使うと, $\ds\zeta(8)=\frac{\pi^8}{9450}$ を示せる. $\QED$


**注意:** より一般に

$$
\zeta(2k) = \sum_{n=1}^\infty \frac{1}{n^{2k}} = \frac{2^{2k-1}(-1)^{k-1}B_{2k}}{(2k)!}\pi^{2k}
$$

を示せる. 実は $\sin$ の無限乗積展開を用いるよりも, $\cot$ の部分分数展開を用いた方がこの公式を得ることは易しい(少し下の方にある問題の解答例を見よ). ここで, $B_n$ は次のように定義されるBernoulli数である:

$$
\frac{z}{e^z-1} = \sum_{n=0}^\infty B_n\frac{z^n}{n!}.
$$

$n$ が $3$ 以上の奇数ならば $B_n=0$ となり, 例えば, 

$$
\begin{aligned}
&
B_0 = 1, \ 
B_1 = -1/2, \\ & 
B_2 = \frac{1}{6}, \ 
B_4 = -\frac{1}{30}, \ 
B_6 = \frac{1}{42}, \ 
B_8 = -\frac{1}{30}, \\ &
B_{10} = \frac{5}{66}, \
B_{12} = -\frac{691}{2730}, \ 
B_{14} = \frac{7}{6}, \ 
B_{16} = -\frac{3617}{510}, \ 
\ldots
\end{aligned}
$$

これより, 

$$
\begin{aligned}
&
\zeta(2) = \frac{2(1/6)}{2!}\pi^2 = \frac{\pi^2}{6}, 
\\ &
\zeta(4) = \frac{2^2(1/30)}{4!}\pi^4 = \frac{\pi^4}{90}, 
\\ &
\zeta(6) = \frac{2^3(1/42)}{6!}\pi^6 = \frac{\pi^6}{945}, 
\\ &
\zeta(8) = \frac{2^4(1/30)}{8!}\pi^8 = \frac{\pi^8}{9450}, 
\\ &
\zeta(10) = \frac{2^5(5/66)}{10!}\pi^8 = \frac{\pi^{10}}{93555}, 
\\ &
\zeta(12) = \frac{2^6(691/2730)}{12!}\pi^8 = \frac{691 \pi^{12}}{638512875}, 
\\ &
\zeta(14) = \frac{2^7(7/6)}{14!}\pi^8 = \frac{2 \pi^{14}}{18243225}, 
\\ &
\zeta(16) = \frac{2^8(3617/510)}{16!}\pi^{16} = \frac{3617\pi^{16}}{325641566250}, \ \ldots
\end{aligned}
$$

```julia
BernoulliNumber(n) = sympy.bernoulli(n)
[BernoulliNumber(n) for n in 0:8]
```

```julia
BernoulliNumber(n) = sympy.bernoulli(n)
B = [BernoulliNumber(2k) for k in 1:8]
```

```julia
BernoulliNumber(n) = sympy.bernoulli(n)
Z = [2^(2k-1)*(-1)^(k-1)*BernoulliNumber(2k)*PI^(2k)/factorial(2k) for k in 1:8]
```

```julia
[zeta(Sym(2k)) for k in 1:8]
```

**問題:** $\pi z\cot(\pi z)$ の部分分数展開と $\pi z\cot(\pi z)$ のMaclaurine展開を比較して, $k=1,2,\ldots$ に対する $\zeta(2k)$ の値をBernoulli数で表す公式を得よ.

**解答例:** $\pi\cot(\pi t)$ の部分分数展開の公式より,

$$
\pi z\cot(\pi z) = 
1 + z\sum_{n=1}^\infty\left(\frac{z}{z-n} + \frac{z}{z+n}\right) =
1 + 2\sum_{n=1}^\infty\frac{z^2}{z^2-n^2}.
$$

そして,

$$
\frac{z^2}{z^2-n^2} = -\frac{z^2/n^2}{1-z^2/n^2} = -\sum_{k=1}^\infty \frac{z^{2k}}{n^{2k}}
$$

なので,

$$
\pi z\cot(\pi z) = 
1 - 2\sum_{k=1}^\infty\left(\sum_{n=1}^\infty\frac{1}{n^{2k}}\right)z^{2k} =
1 - 2\sum_{k=1}^\infty \zeta(2k)z^{2k}.
$$

一方, $x\cot x$ のMaclaurin展開が

$$
x\cot x = \sum_{k=0}^\infty (-1)^k\frac{2^{2k}B_{2k}}{(2k)!}x^{2k} =
1 - 2\sum_{k=0}^\infty \frac{2^{2k-1}(-1)^{k-1} B_{2k}}{(2k)!}x^{2k}
$$

であることより(これを $B_{2k}$ の定義だと思ってもよい), $x=\pi z$ を代入すると, 

$$
\pi z\cot(\pi z) = 1 - 2\sum_{k=0}^\infty \frac{2^{2k-1}(-1)^{k-1} B_{2k}}{(2k)!}\pi^{2k}z^{2k}.
$$

以上を比較すると, 

$$
\zeta(2k) = \frac{2^{2k-1}(-1)^{k-1} B_{2k}}{(2k)!}\pi^{2k}
\qquad(k=1,2,3,\ldots)
$$

が得られる. $\QED$


### 多重ゼータ値 ζ(2,2,...,2)

$\sin$ の無限積表示

$$
\frac{\sin(\pi x)}{\pi x} =
\prod_{k=1}^\infty \left(1-\frac{x^2}{k^2}\right)
$$

の両辺を別々に展開すると,

$$
\begin{aligned}
&
\frac{\sin(\pi x)}{\pi x} =
\sum_{n=0}^\infty \frac{\pi^{2n}}{(2n+1)!}\frac{\pi^{2n}}{(2n+1)!}(-x^2)^n,
\\ &
\prod_{m=1}^\infty \left(1-\frac{x^2}{m^2}\right) =
\sum_{n=1}^\infty\left(
\sum_{m_1>m_2>\cdots>m_n\geqq 1} \frac{1}{m_1^2 m_2^2\cdots m_n^2}
\right)(-x^2)^n.
\end{aligned}
$$

ゆえに係数を比較して,

$$
\sum_{m_1>m_2>\cdots>m_n\geqq 1} \frac{1}{m_1^2 m_2^2\cdots m_n^2} =
\frac{\pi^{2n}}{(2n+1)!}
$$

を得る.  より一般に多重ゼータ値 $\zeta(k_1,k_2,\ldots,k_n)$ が

$$
\zeta(k_1,k_2,\ldots,k_n) =
\sum_{m_1>m_2>\cdots>m_n\geqq 1} \frac{1}{m_1^{k_1} m_2^{k_2}\cdots m_n^{k_n}}
$$

によって定義される. 上の結果は

$$
\zeta(\underbrace{2,2,\ldots,2}_{\text{$n$ times}}) = \frac{\pi^{2n}}{(2n+1)!}
$$

を意味している.

多重ゼータ値については例えば次の文献を参照せよ:

* 荒川恒男(立教大), 金子昌信(九州大), 多重ゼータ値入門, 2010, 2016. [PDF](http://www2.math.kyushu-u.ac.jp/~mkaneko/papers/MZV_LectureNotes.pdf)


### Lobachevskyの公式 (Dirichlet積分の公式の一般化の1つ)

この節の内容は

* Hassan Jolany, <a href="https://hal.archives-ouvertes.fr/hal-01539895v3">An extension of Lobachevsky formula</a>, 2017 (<a href="https://arxiv.org/abs/1004.2653">arXiv版</a>)

の第2節の引き写しである. 


$\pi\cosec(\pi t) = \pi/\sin(\pi t)$ の部分分数展開の公式に $t=x/\pi$ を代入すると,

$$
\frac{1}{\sin x} = 
\frac{1}{x} + 
\sum_{k=1}^\infty(-1)^k\left(\frac{1}{x-k\pi}+\frac{1}{x+k\pi}\right).
$$

$\pi\cot(\pi t)$ の部分分数展開の公式に $t=x/\pi$ を代入すると,

$$
\cot(x) =
\frac{1}{x} + 
\sum_{k=1}^\infty\left(\frac{1}{x-k\pi} + \frac{1}{x+k\pi}\right).
$$

これの両辺に $-d/dx$ を作用させると,  

$$
\frac{1}{\sin^2 x} =
\frac{1}{x^2} +
\sum_{k=1}^\infty\left(\frac{1}{(x-k\pi)^2} + \frac{1}{(x+k\pi)^2}\right).
$$


**Lobachevskyの公式:** $f$ は

$$
f(x+\pi) = f(x)\, \quad f(\pi-x)=f(x)
$$

を満たす $\R$ 上の連続函数であると仮定する. このとき,

$$
\int_0^\infty \frac{\sin^2 x}{x^2} f(x)\,dx =
\int_0^\infty \frac{\sin x}{x} f(x)\,dx =
\int_0^{\pi/2} f(x)\,dx.
$$

**注意:** 特に $f(x)=1$ のとき,

$$
\int_0^\infty \frac{\sin^2 x}{x^2}\,dx =
\int_0^\infty \frac{\sin x}{x}\,dx =
\frac{\pi}{2}
$$

なので, Lobachevskyの公式はDirichlet積分の公式の一般化になっている.

**証明:** $0$ から $\infty$ のあいだを長さ $\pi/2$ の区間で区切って整理し直して, $1/\sin x$ と $1/\sin^2 x$ の部分分数展開を使えばこの公式が得られる. 詳しい計算の手順は以下の通り.

整数 $k$ に対して, $f(x+\pi)=f(x)$, $f(\pi-x)=f(x)$ という仮定より,

$$
\begin{aligned}
&
\int_{(2k-1)\pi/2}^{2k\pi/2} \frac{\sin x}{x}f(x)\,dx =
(-1)^k \int_0^{\pi/2} \frac{\sin(-x)}{-x+k\pi}f(x)\,dx =
(-1)^k \int_0^{\pi/2} \frac{\sin x}{x-k\pi}f(x)\,dx,
\\ &
\int_{2k\pi/2}^{(2k+1)\pi/2} \frac{\sin x}{x}f(x)\,dx =
(-1)^k \int_0^{\pi/2} \frac{\sin x}{x+k\pi}f(x)\,dx.
\end{aligned}
$$

ゆえに,

$$
\begin{aligned}
\int_0^\infty \frac{\sin x}{x} f(x)\,dx &=
\sum_{n=0}^\infty\int_{n\pi/2}^{(n+1)\pi/2} \frac{\sin x}{x} f(x)\,dx
\\ &=
\int_0^{\pi/2}\frac{\sin x}{x} f(x)\,dx +
\sum_{k=1}^\infty(-1)^k\int_0^{\pi/2}
\left(\frac{\sin x}{x-k\pi}+\frac{\sin x}{x+k\pi}\right)f(x)\,dx
\\ &=
\int_0^{\pi/2}\sin x
\left[
\frac{1}{x} +
\sum_{k=1}^\infty(-1)^k\left(\frac{1}{x-k\pi}+\frac{1}{x+k\pi}\right)
\right]
f(x)\,dx
\\ &=
\int_0^{\pi/2} \sin x \frac{1}{\sin x} f(x)\,dx =
\int_0^{\pi/2} f(x)\,dx.
\end{aligned}
$$

終わりから2番目の等号で $1/\sin x$ の部分分数展開を使った.

整数 $k$ に対して, $f(x+\pi)=f(x)$, $f(\pi-x)=f(x)$ という仮定より,

$$
\begin{aligned}
&
\int_{(2k-1)\pi/2}^{2k\pi/2} \frac{\sin^2 x}{x^2}f(x)\,dx =
\int_0^{\pi/2} \frac{\sin^2(-x)}{(-x+k\pi)^2}f(x)\,dx =
\int_0^{\pi/2} \frac{\sin^2 x}{(x-k\pi)^2}f(x)\,dx,
\\ &
\int_{2k\pi/2}^{(2k+1)\pi/2} \frac{\sin^2 x}{x^2}f(x)\,dx =
\int_0^{\pi/2} \frac{\sin^2 x}{(x+k\pi)^2}f(x)\,dx.
\end{aligned}
$$

ゆえに,

$$
\begin{aligned}
\int_0^\infty \frac{\sin^2 x}{x^2} f(x)\,dx &=
\sum_{n=0}^\infty\int_{n\pi/2}^{(n+1)\pi/2} \frac{\sin^2 x}{x^2} f(x)\,dx
\\ &=
\int_0^{\pi/2}\frac{\sin x^2}{x^2} f(x)\,dx +
\sum_{k=1}^\infty \int_0^{\pi/2}
\left(\frac{\sin^2 x}{(x-k\pi)^2}+\frac{\sin^2 x}{(x+k\pi)^2}\right)f(x)\,dx
\\ &=
\int_0^{\pi/2}\sin^2 x
\left[
\frac{1}{x^2} +
\sum_{k=1}^\infty\left(\frac{1}{(x-k\pi)^2}+\frac{1}{(x+k\pi)^2}\right)
\right]
f(x)\,dx
\\ &=
\int_0^{\pi/2} \sin^2 x \frac{1}{\sin^2 x} f(x)\,dx =
\int_0^{\pi/2} f(x)\,dx.
\end{aligned}
$$

終わりから2番目の等号で $1/\sin^2 x$ の部分分数展開を使った. $\QED$


## Poissonの和公式


### Poissonの和公式とその証明

Fourier級数の収束とFourier変換の定義から, **Poissonの和公式**が得られることを説明しよう.


**定義(急減少函数):** $\R$ 上の $C^\infty$ 函数 $f$ で, 任意の非負の整数 $m,n$ に対して, $|x|\to\infty$ のとき $f^{(m)}(x)x^n\to 0$ となるものを**急減少函数**と呼ぶ. $\QED$


**定理(Poissonの和公式)** $f$ は $\R$ 上の急減少函数であると仮定する. このとき, 次の公式が成立している:

$$
\sum_{m\in\Z} f(m) = \sum_{n\in\Z} \hat{f}(n)
$$

**略証:** $\ds g(x) = \sum_{m\in\Z} f(x+m)$ とおくと, $g(x)$ は周期 $1$ を持つ $C^\infty$ 函数になる. ゆえに,

$$
g(x) = \sum_{n\in\Z} a_n(g)e^{2\pi inx} \quad (x\in \R).
$$

そして, 

$$
\begin{aligned}
a_n(g) &= \int_0^1 \sum_{m\in\Z}f(x+m)e^{-2\pi inx}\,dx =
\sum_{m\in\Z} \int_0^1 f(x+m)e^{-2\pi inx}\,dx 
\\ &=
\sum_{m\in\Z} \int_m^{m+1} f(x)e^{-2\pi inx}\,dx =
\int_{-\infty}^\infty f(x)e^{-2\pi inx}\,dx = \hat{f}(n).
\end{aligned}
$$

以上をまとめると,

$$
g(x) = \sum_{m\in\Z} f(x+m) = \sum_{n\in\Z} \hat{f}(n)e^{2\pi inx} \quad (x\in\R).
$$

この等式で $x=0$ をおけば欲しい公式が得られる. $\QED$


### Theta zero value Θ(t) のモジュラー変換性

**問題:** $t>0$ に対して, 次の公式を示せ:

$$
\sum_{m\in\Z} \frac{e^{-\pi(x-m)^2/t}}{\sqrt{t}} = \sum_{n\in\Z} e^{-\pi n^2 t + 2\pi inx}.
$$

**注意:** この問題の左辺と右辺は楕円テータ函数と呼ばれる特殊函数の特別な場合になっている. 左辺は熱方程式の基本解を周期函数になるように足し上げたものになっている. そのことから, この公式の両辺は周期境界条件のもとでの熱方程式の基本解になっていることがわかる. 楕円テータ函数については

* David Mumford, <a href="http://www.dam.brown.edu/people/mumford/alg_geom/papers/Tata1.pdf">Tata Lectures on Theta I</a>, Reprint of the 1983 Edition

が非常に面白くてかつ読み易い教科書であり, 論文などでもよく引用されている. $\QED$

**解答例:** $\ds f(x)=\frac{e^{-\pi x^2/t}}{\sqrt{t}}$ とおくと, $f(x)$ が偶函数であることと, Poissonの和公式の証明より,

$$
\sum_{m\in\Z} \frac{e^{-\pi(x-m)^2/t}}{\sqrt{t}} =
\sum_{m\in\Z}f(x-m)=\sum_{m\in\Z}f(x+m)=\sum_{n\in\Z}\hat{f}(n)e^{2\pi inx}.
$$

そして, 公式

$$
\int_{-\infty}^\infty e^{-y^2/a}e^{-ipy}\,dy = \sqrt{\pi a}e^{-ap^2/4}
$$

より, $x=y/(2\pi)$ とおくと, 

$$
\hat{f}(n) = \int_{-\infty}^\infty \frac{e^{-\pi x^2/t}}{\sqrt{t}} e^{-2\pi inx}\,dx =
\int_{-\infty}^\infty \frac{e^{-y^2/(4\pi t)}}{\sqrt{t}} e^{-iny}\,\frac{dy}{2\pi} =
\frac{\sqrt{4\pi t\,\pi}}{2\pi\sqrt{t}}e^{-4\pi t n^2/4} =
e^{-\pi n^2 t}.
$$

以上をまとめると示したい公式が得られる. $\QED$


**問題(Theta zero value のモジュラー変換性):** $t>0$ に対して, $\ds \Theta(t)=\sum_{n\in\Z} e^{-\pi n^2 t} = 1 + 2\sum_{n=1}^\infty e^{-\pi n^2 t}$ とおくと, 

$$
\Theta(t) = \frac{1}{\sqrt{t}}\Theta\left(\frac{1}{t}\right)
\quad\text{すなわち}\quad
\Theta\left(\frac{1}{t}\right) = \sqrt{t}\;\Theta(t)
$$

が成立することを示せ.  この結果を $\Theta(t)$ の**モジュラー変換性**と呼ぶ.

**注意:** $\Theta(t)$ の定義式を見ると, $t>0$ が大きなときに $\Theta(t)$ がほぼ $1$ になることはすぐに確認できるが, $t>0$ が小さいときにはどのような様子になっているかはよくわからない.  しかし, モジュラー変換性を使うと, $\Theta(t)$ は, $t>0$ が小さいときにはほぼ $\ds\frac{1}{\sqrt{t}}$ に等しいことがわかる.  このようにモジュラー変換性は「よくわからない量」を「よくわかる量」に関係付ける公式である. $\QED$

**注意:** <a hred="https://www.google.co.jp/search?q=%E8%B6%85%E5%BC%A6%E7%90%86%E8%AB%96+%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%A9%E3%83%BC%E5%A4%89%E6%8F%9B%E6%80%A7">超弦理論</a>や<a href="https://www.google.co.jp/search?q=%E5%85%B1%E5%BD%A2%E5%A0%B4%E7%90%86%E8%AB%96+%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%A9%E3%83%BC%E5%A4%89%E6%8F%9B%E6%80%A7">共形場理論</a>ではモジュラー変換性が重要な意味を持つ. $\QED$

**注意:** 上の問題の公式はRiemannのゼータ函数の函数等式を証明するときに使われる. 上の問題の結果は楕円テータ函数のモジュラー変換性の特別な場合になっている. モジュラー変換性を持つ函数は数論的にも極めて重要な数学的対象である. $\QED$

**解答例1:**  $\ds f(x)=\frac{e^{-\pi x^2/t}}{\sqrt{t}}$ にPoissonの和公式を適用すればこの公式が得られる. すなわち, 上の問題の結果で $x=0$ とおけば示したい公式が得られる. $\QED$

**解答例2:** $f(x)=e^{-\pi tx^2}$ について $\ds\hat{f}(p) = \frac{e^{-\pi p^2/t}}{\sqrt{t}}$ なので, Poissonの和公式より, 示したい公式が得られる. $\QED$


**問題:** 上の問題の結論を $\Theta(t)$ と $\ds\frac{1}{\sqrt{t}}\Theta\left(\frac{1}{t}\right)$ のグラフを重ねてプロットすることによって確認せよ. 2つのグラフはぴったり重なるはずである. $\QED$

次のセルを見よ.

```julia
Theta(t; N=20) = iszero(N) ? one(t) : 1 + 2*sum(n->e^(-π*n^2*t), 1:N)
t = 0.2:0.05:2.0
A = Theta.(t)
B = @. 1/√(t)*Theta(1/t)
plot(size=(500, 350))
plot!(t, B, label="Theta(t)",               lw=2)
plot!(t, A, label="(1/sqrt(t)) Theta(1/t)", lw=2, ls=:dash)
```

**問題:** $t=0.1$ のとき, まず $\ds\frac{1}{\sqrt{t}}\Theta\left(\frac{1}{t}\right)$ の近似値を

$$
\frac{1}{\sqrt{t}}\left(1 + 2\sum_{n=1}^N e^{-\pi n^2/t}\right)
$$

の $N=1$ の場合を用いて計算せよ. そして, $\Theta(t)$ を

$$
1 + 2\sum_{n=1}^N e^{-\pi n^2 t}
$$

で近似するとき, $N\geqq 10$ としなければ, 上と同じ精度が得られないことを確認せよ. $t>0$ が小さいときには $\Theta(t)$ の定義に基いて直接的に数値計算するより, モジュラー変換を経由して数値計算した方が効率的である. $\QED$

次のセルを見よ.

```julia
Theta(t; N=20) = iszero(N) ? one(t) : 1 + 2*sum(n->e^(-π*n^2*t), 1:N)
t = 0.1
@show 1/√t*Theta(1/t; N=0)
@show 1/√t*Theta(1/t; N=1)
@show 1/√t*Theta(1/t; N=2)
@show Theta(t; N=9);
@show Theta(t; N=10);
@show Theta(t; N=11);
```

<!-- #region -->
### Lerchの超越函数へのPoissonの和公式の応用

#### Lerchの超越函数

以下では $a\ne0,-1,-2,\ldots$ であると仮定する.

**定義:** **Lerchの超越函数** $\Phi(z,s,a)$ を

$$
\Phi(z,s,a) = 
\sum_{k=0}^\infty \frac{z^k}{(a+k)^s}.
$$

と定義する. 次の記号法も使われる:

$$
L(\tau, a, s) = \Phi(e^{2\pi i\tau}, s ,a) =
\sum_{k=0}^\infty \frac{e^{2\pi ik\tau}}{(a+k)^s}, 
\qquad z=e^{2\pi i\tau}.
$$

これは**Lerchのゼータ函数**(レルヒのゼータ函数)と呼ばれる. **引数の順序が変わっていることに注意せよ.**  $|z|<1$ ならば右辺の級数は絶対収束し, $|z|=1$ であっても右辺の級数は $\real s > 1$ ならば絶対収束する.  さらに,

$$
\Phi(z,s,a) = \left(z\frac{\d}{\d z} + a\right)^m \Phi(z,s+m,a)
$$

を使って, $|z|=1$ であっても, $\real s>1-m$ まで $\Phi(z,s,a)$ を解析接続できる.

Lerchのゼータ函数は, $\tau=0$ ($z=1$) とするとHurwitzのゼータ函数になり, $a=1$ としてから $z$ 倍すると($s$ が正の整数の場合には)polylogarithmになり, $\tau=0$ ($z=1$), $a=1$ とするとRiemannのゼータ函数になる.  $z=e^{2\pi i\tau}$ のとき,

$$
\begin{aligned}
&
L(0,a,s) = \Phi(1,s,a)=\zeta(s,z) = \sum_{k=0}^\infty\frac{1}{(k+a)^s},
\\ &
e^{2\pi i\tau}L(\tau,1,s) = z\Phi(z,s,1)=\Li_s(z) = \sum_{n=1}^\infty\frac{z^n}{n^s},
\\ &
L(0,1,s) = \Phi(1,s,1)=\zeta(s) = \sum_{n=1}^\infty \frac{1}{n^s}.
\end{aligned}
$$

Lerchの超越函数の基本性質については

* Jesus Guillera and Jonathan Sondow. Double integrals and infinite products for some classical constants via analytic continuations of Lerch's transcendent. <a href="https://arxiv.org/abs/math/0506319">arXiv:math/0506319</a>

* Jeffrey C. Lagarias and Wen-Ching Winnie Li. Mathematics > Number Theory
The Lerch Zeta Function I, II, III, IV. <a href="https://arxiv.org/abs/1005.4712">arXiv:1005.4712</a>, <a href="https://arxiv.org/abs/1005.4967">arXiv:1005.4967</a>, <a href="https://arxiv.org/abs/1506.06161">arXiv:1506.06161</a>, <a href="https://arxiv.org/abs/1511.08116">arXiv:1511.08116</a>

を参照せよ.

#### Lipschitzの和公式＝Lerchの函数等式


**Lipschitzの和公式:** $\real s>1$, $\imag\tau > 0$, $0<a<1$ のとき,

$$
\sum_{k=0}^\infty\frac{e^{2\pi i\tau(k+a)}}{(k+a)^{1-s}} = 
\frac{\Gamma(s)}{(-2\pi i)^s}\sum_{n\in\Z}\frac{e^{-2\pi ina}}{(n+\tau)^s}.
$$

すなわち

$$
e^{2\pi i\tau a}L(\tau, a, 1-s) =
\frac{\Gamma(s)}{(2\pi)^s}\left(
e^{-\pi is/2}e^{2\pi ia}L(a, 1-\tau, s) +
e^{\pi is/2}L(-a, \tau, s)
\right),
\\
e^{2\pi i\tau a}\Phi(e^{2\pi i\tau},1-s,a) =
\frac{\Gamma(s)}{(2\pi)^s}\left(
e^{-\pi is/2}e^{2\pi ia}\Phi(e^{2\pi ia}, s, 1-\tau) +
e^{\pi is/2}\Phi(e^{-2\pi ia},s,\tau)
\right).
$$

**注意:** この結果は**Lerchの函数等式**とも呼ばれる.

**証明:** 函数 $f(x)$ を次のように定める:

$$
f(x) =
\begin{cases}
e^{2\pi i\tau(x+a)}(x+a)^{s-1} & (x > -a)\\
0 & (x < -a).
\end{cases}
$$

このとき,

$$
\sum_{k\in\Z}f(k) = \sum_{k=0}^\infty\frac{e^{2\pi i\tau(k+a)}}{(k+a)^{1-s}} =
e^{2\pi i\tau a}\Phi(e^{2\pi i\tau}, 1-s, a).
$$

一方, $p\in\R$ に対して, 

$$
\begin{aligned}
\hat{f}(p) &= 
\int_{-a}^\infty e^{2\pi i\tau(x+a)} (x+a)^{s-1} e^{-2\pi ipx}\,dx =
\int_0^\infty e^{2\pi i\tau x}x^{s-1}e^{-2\pi ip(x-a)}\,dx
\\ &=
e^{2\pi ipa}\int_0^\infty e^{2\pi i(\tau-p)x} x^{s-1}\,dx =
e^{2\pi ipa}\frac{\Gamma(s)}{(-2\pi i(\tau-p))^s} =
\frac{\Gamma(s)}{(-2\pi i)^s} \frac{e^{2\pi ipa}}{(\tau-p)^s}.
\end{aligned}
$$

4番目の等号で, Cauchyの積分定理より, $y=-2\pi i(\tau-p)x$ とおいて $y$ に関する積分に書き換えた後に, 積分経路を $(0,\infty)$ に置き換えても値が変わらないことを用いた.

したがって, Poissonの和公式 $\ds\sum_{m\in\Z}f(m)=\sum_{n\in\Z}\hat{f}(n)=\sum_{n\in\Z}\hat{f}(-n)$ より, 示したい前者の公式

$$
\sum_{k=0}^\infty\frac{e^{2\pi i\tau(k+a)}}{(k+a)^{1-s}} = 
\frac{\Gamma(s)}{(-2\pi i)^s}\sum_{n\in\Z}\frac{e^{-2\pi ina}}{(n+\tau)^s}
$$

が得られる. 後者の公式は以下のように右辺の和を $n<0$ ($n=-(k+1)$, $k=0,1,2,\ldots$) と $n\geqq 0$ に分けることによって得られる.

$$
\begin{aligned}
e^{2\pi i\tau a}L(\tau, a, 1-s) &=
\sum_{k=0}^\infty\frac{e^{2\pi i\tau(k+a)}}{(k+a)^{1-s}} = 
\frac{\Gamma(s)}{(-2\pi i)^s}\sum_{n\in\Z}\frac{e^{-2\pi ina}}{(n+\tau)^s}
\\ &=
\frac{\Gamma(s)}{(-2\pi)^s}\left(
\sum_{k=0}^\infty\frac{e^{2\pi i(k+1)a}}{(-(k+1-\tau))^s} +
\sum_{n=0}^\infty\frac{e^{-2\pi ina}}{(n+\tau)^s}
\right)
\\ &=
\frac{\Gamma(s)}{(2\pi)^s}e^{-\pi is/2}\left(
e^{\pi i s}e^{2\pi ia}\sum_{k=0}^\infty\frac{e^{2\pi ika}}{(k+1-\tau)^s} +
\sum_{n=0}^\infty\frac{e^{-2\pi ina}}{(n+\tau)^s}
\right)
\\ &=
\frac{\Gamma(s)}{(2\pi)^s}\left(
e^{-\pi is/2}e^{2\pi ia}L(a, 1-\tau, s) +
e^{\pi is/2}L(-a, \tau, s)
\right).
\qquad \QED
\end{aligned}
$$

**注意:** 上のPoissonの和公式を使う証明の方針は

* Knopp, Marvin and Robins, Sinai. Easy proofs of Riemann's functional equation for $\zeta(s)$ and Lipschitz summation.  <a href="https://www.ams.org/journals/proc/2001-129-07/S0002-9939-01-06033-6/S0002-9939-01-06033-6.pdf">Proc. Amer. Math. Soc., Vol.129 (2001), No.7, 1915-1922</a>

による. この論文の説明の方がこのノートの説明より詳しい. Lerchの函数等式の別証明については

* Berndt, Bruce C.  Two new proofs of Lerch's functional equation. <a href="https://www.ams.org/journals/proc/1972-032-02/S0002-9939-1972-0297721-3/">Proc. Amer. Math. Soc., Vol.32 (1972), No.2, 403-408 </a>

を参照せよ. $\QED$

#### Hurwitzの函数等式

$z=e^{2\pi i\tau}$ とおくと,

$$
\begin{aligned}
L(\tau,a,s) &= \Phi(z,s,a) = 
\sum_{k=0}^\infty\frac{z^k}{(k+a)^s} =
\frac{1}{a^s} + \sum_{m=0}^\infty\frac{z^{m+1}}{(m+1+a)^s} 
\\ &=
\frac{1}{a^s} + z\,\Phi(z, s, 1+a) =
\frac{1}{a^s} + e^{2\pi i\tau}L(\tau, 1+a, s).
\end{aligned}
$$

なので, Lipschitzの和公式＝Lerchの函数等式は次のように書き直される:

$$
e^{2\pi i\tau a}L(\tau, a, 1-s) =
\frac{\Gamma(s)}{(2\pi)^s}\left(
e^{-\pi is/2}e^{2\pi ia}L(a, 1-\tau, s) +
e^{\pi is/2}e^{-2\pi ia}L(a, 1+\tau, s) +
\frac{e^{\pi is/2}}{\tau^s}
\right).
$$

したがって, $\real s < 0$ として $\tau \to 0$ とすると $1/\tau^s\to 0$ となり, 一般に $L(0,a,s)=\zeta(s,a)$, $e^{2\pi i\tau}L(\tau,1,s)=\Li_s(e^{2\pi i\tau})$ が成立しているので,

$$
\zeta(1-s, a) = 
\frac{\Gamma(s)}{(2\pi)^s}\left(
e^{-\pi is/2} \Li_s(e^{2\pi ia}) + e^{\pi is/2}\Li_s(e^{-2\pi ia})
\right)
\qquad (0<a<1).
$$

これを**Hurwitzの函数等式**と呼ぶ.  この公式は解析接続によって $\real s < 0$ でなくても成立している.  

Hurwitzの函数等式で, $s=m+1$, $m\in\Z_{\geqq 0}$ とおくと,

$$
\zeta(-m,a) = m!\sum_{0\ne n\in\Z}\frac{e^{2\pi ina}}{(2\pi in)^{m+1}} \qquad(0<a<1).
$$

Hurwitzのゼータ函数 $\zeta(s,a)$ とBernoulli多項式 $B_k(x)$ については

$$
\zeta(-m,a) = -\frac{B_{m+1}(a)}{m+1}
$$

という公式がよく知られている(ノート「10 Gauss積分, ガンマ函数, ベータ函数」で示した). 以上を比較すると,

$$
B_{m+1}(a) = -(m+1)!\sum_{0\ne n\in\Z}\frac{e^{2\pi ina}}{(2\pi in)^{m+1}} \qquad(0<a<1).
$$

この結果はノート「13 Euler-Maclaurinの和公式」で示すことになる周期的Bernoulli多項式のFourier級数展開の結果と一致している. $\QED$

#### Riemannのゼータ函数の函数等式

Hurwitzの函数等式でさらに $a\to 1$ とすると, $\zeta(s,1)=\zeta(s)$ と $\Li_s(1)=\zeta(s)$ より,

$$
\zeta(1-s) = \frac{\Gamma(s)}{(2\pi)^s}2\cos\left(\frac{\pi s}{2}\right)\,\zeta(s).
$$

さらに $s$ を $1-s$ で置き換えると,

$$
\zeta(s) = \frac{\Gamma(1-s)}{(2\pi)^{1-s}}2\sin\left(\frac{\pi s}{2}\right)\,\zeta(1-s) =
2^s \pi^{s-1} \sin\left(\frac{\pi s}{2}\right)\Gamma(1-s)\,\zeta(1-s).
$$

Riemannのゼータ函数の有名な函数等式が得られた. $\QED$
<!-- #endregion -->
