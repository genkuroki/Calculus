---
jupyter:
  jupytext:
    cell_metadata_json: true
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

# ディリクレ級数の滑らかなカットオフ

黒木玄

2018-08-12～2019-04-03, 2020-04-25, 2023-06-22

* Copyright 2018,2019,2020,2023 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/A01%20Smooth%20cutoff%20of%20Dirichlet%20series.ipynb

このノートの元ネタは次のリンク先の式(26)以降の解説である:

* Terence Tao, <a href="https://terrytao.wordpress.com/2010/04/10/the-euler-maclaurin-formula-bernoulli-numbers-the-zeta-function-and-real-variable-analytic-continuation/">The Euler-Maclaurin formula, Bernoulli numbers, the zeta function, and real-variable analytic continuation</a>, What's new, 2010-04-10

自分のパソコンに<a href="https://julialang.org/">Julia言語</a>をインストールしたい場合には

* [WindowsへのJulia言語のインストール](http://nbviewer.jupyter.org/gist/genkuroki/81de23edcae631a995e19a2ecf946a4f)

* [Julia v1.1.0 の Windows 8.1 へのインストール](https://nbviewer.jupyter.org/github/genkuroki/msfd28/blob/master/install.ipynb)

を参照せよ. 前者は古く, 後者の方が新しい.

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
$

<!-- #region {"toc": true} -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Mellin変換とその逆変換" data-toc-modified-id="Mellin変換とその逆変換-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Mellin変換とその逆変換</a></span><ul class="toc-item"><li><span><a href="#Mellin変換" data-toc-modified-id="Mellin変換-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>Mellin変換</a></span><ul class="toc-item"><li><span><a href="#exp(-x²)のMellin変換" data-toc-modified-id="exp(-x²)のMellin変換-1.1.1"><span class="toc-item-num">1.1.1&nbsp;&nbsp;</span>exp(-x²)のMellin変換</a></span></li><li><span><a href="#exp(-x)のMellin変換" data-toc-modified-id="exp(-x)のMellin変換-1.1.2"><span class="toc-item-num">1.1.2&nbsp;&nbsp;</span>exp(-x)のMellin変換</a></span></li><li><span><a href="#exp(-x^m)のMellin変換" data-toc-modified-id="exp(-x^m)のMellin変換-1.1.3"><span class="toc-item-num">1.1.3&nbsp;&nbsp;</span>exp(-x^m)のMellin変換</a></span></li><li><span><a href="#急減少函数のMellin変換" data-toc-modified-id="急減少函数のMellin変換-1.1.4"><span class="toc-item-num">1.1.4&nbsp;&nbsp;</span>急減少函数のMellin変換</a></span></li></ul></li><li><span><a href="#逆Mellin変換" data-toc-modified-id="逆Mellin変換-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>逆Mellin変換</a></span></li></ul></li><li><span><a href="#Dirichlet級数の滑らかなカットオフ" data-toc-modified-id="Dirichlet級数の滑らかなカットオフ-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Dirichlet級数の滑らかなカットオフ</a></span><ul class="toc-item"><li><span><a href="#滑らかなカットオフの定義" data-toc-modified-id="滑らかなカットオフの定義-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>滑らかなカットオフの定義</a></span></li><li><span><a href="#滑らかなカットオフの逆Mellin変換表示" data-toc-modified-id="滑らかなカットオフの逆Mellin変換表示-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>滑らかなカットオフの逆Mellin変換表示</a></span></li><li><span><a href="#Re-s-<-0-および-s-=-0-での様子" data-toc-modified-id="Re-s-<-0-および-s-=-0-での様子-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>Re s &lt; 0 および s = 0 での様子</a></span></li></ul></li><li><span><a href="#滑らかなカットオフの例" data-toc-modified-id="滑らかなカットオフの例-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>滑らかなカットオフの例</a></span><ul class="toc-item"><li><span><a href="#ζ(s)の場合" data-toc-modified-id="ζ(s)の場合-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>ζ(s)の場合</a></span></li><li><span><a href="#極を持たない交代Dirichlet級数の場合" data-toc-modified-id="極を持たない交代Dirichlet級数の場合-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>極を持たない交代Dirichlet級数の場合</a></span><ul class="toc-item"><li><span><a href="#η(x)-=-exp(-x^2)-の場合" data-toc-modified-id="η(x)-=-exp(-x^2)-の場合-3.2.1"><span class="toc-item-num">3.2.1&nbsp;&nbsp;</span>η(x) = exp(-x^2) の場合</a></span></li><li><span><a href="#η(x)-=-exp(-x)-の場合" data-toc-modified-id="η(x)-=-exp(-x)-の場合-3.2.2"><span class="toc-item-num">3.2.2&nbsp;&nbsp;</span>η(x) = exp(-x) の場合</a></span></li></ul></li><li><span><a href="#-ζ'(s)の場合" data-toc-modified-id="-ζ'(s)の場合-3.3"><span class="toc-item-num">3.3&nbsp;&nbsp;</span>-ζ'(s)の場合</a></span></li><li><span><a href="#-(d/ds)log-ζ(s)の場合" data-toc-modified-id="-(d/ds)log-ζ(s)の場合-3.4"><span class="toc-item-num">3.4&nbsp;&nbsp;</span>-(d/ds)log ζ(s)の場合</a></span></li></ul></li><li><span><a href="#Riemannのゼータ函数の非自明な零点の分布" data-toc-modified-id="Riemannのゼータ函数の非自明な零点の分布-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>Riemannのゼータ函数の非自明な零点の分布</a></span></li></ul></div>
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
#pyplot(fmt=:svg)
default(bglegend=RGBA(1.0, 1.0, 1.0, 0.5))
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

using LaTeXStrings

using Primes
ENV["LINES"] = 100

using HTTP
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

## Mellin変換とその逆変換

以下, 収束性の詳細のこだわらずにラフに説明する.


### Mellin変換

函数 $f(x)$ に対して,

$$
F(s) = \int_0^\infty f(x) x^{s-1}\,dx
$$

を $f(x)$ の**Mellin変換**と呼ぶ.  $f(x)$ が $\R$ 上の急減少函数で $\real s>0$ ならば右辺の積分は絶対収束する. $f(x)$ が $\R$ 上の急減少函数で $\real s>0$ のとき, 部分積分によって,

$$
F(s) = 
\left[f(x)\frac{x^s}{s}\right]_0^\infty - 
\frac{1}{s}\int_0^\infty f'(x) x^s\,dx = -
\frac{1}{s}\int_0^\infty f'(x) x^s\,dx.
$$

さらに右辺の積分因子は $\real s>-1$ で絶対収束するので $F(s)$ は $\real s > -1$ に解析接続され, $s=0$ に極を持つ可能性がある. この操作を繰り返すことによって, 

$$
F(s) = \frac{(-1)^n}{s(s+1)\cdots(s+n-1)}\int_0^\infty f^{(n)}(x)x^{s+n-1}\,dx.
$$

これより, $F(s)$ は複素平面全体の有理型函数に解析接続され, その極は $s=0,-1,-2,\ldots$ に含まれる.

#### exp(-x²)のMellin変換

**例:** $f(x)=e^{-x^2}$ のとき, $x=y^{1/2}$ とおくと,

$$
F(s) = 
\int_0^\infty e^{-x^2}x^{s-1}\,dx =
\frac{1}{2}\int_0^\infty e^{-y}y^{s/2-1}\,dy =
\frac{1}{2}\Gamma\left(\frac{s}{2}\right)
$$

でガンマ函数 $\Gamma(s/2)$ の極の全体は $s/2=0,-1,-2,\ldots$ に一致するので, $F(s)$ の極の全体は $0$ 以下の偶数全体に一致する. $\QED$

#### exp(-x)のMellin変換

**例:** $x > 0$ のとき $f(x)=e^{-x}$ ならば

$$
F(s) = \int_0^\infty e^{-x} x^{s-1}\,dx = \Gamma(s)
$$

で $F(s)$ の極の全体は $s=0,-1,-2,\ldots$ に一致する. $\QED$

#### exp(-x^m)のMellin変換

**例:** $m>0$ であるとする. $x>0$ のとき $f(x) = e^{-x^m}$ のとき, $x=y^{1/m}$ とおくと, 

$$
F(s) = \int_0^\infty e^{-x^m} x^{s-1}\,dx = 
\frac{1}{m}\int_0^\infty e^{-y} y^{s/m-1}\,dy =
\frac{1}{m}\Gamma\left(\frac{s}{m}\right)
$$

となり, $F(s)$ の極の全体は $0$ 以下の $m$ の整数倍全体に一致する. $\QED$

#### 急減少函数のMellin変換

**例:** $f(x)$ は急減少函数でかつ $0<\delta<\delta_0$ であり, $|x|\leqq \delta_0$ ならば $f(x)=1$ であると仮定する. このとき, $\real s > 0$ ならば, 

$$
F(s) = -\frac{1}{s}\int_\delta^\infty f'(x) x^s\,dx.
$$

でかつ右辺の積分因子 $\ds \int_\delta^\infty f'(x) x^s\,dx$ は任意の複素数 $s$ について絶対収束しているので, $F(s)$ の極になっている可能性がある点は $s=0$ の1つだけである. $\QED$

```julia
eta(x) = exp(-x^2)
x = symbols("x", real=true)
s = symbols("s", positive=true)
integrate(eta(x)*x^(s-1), (x,0,oo))
```

```julia
eta(x,m) = exp(-x^m)
x = symbols("x", real=true)
s = symbols("s", positive=true)
m = symbols("m", positive=true)
integrate(eta(x,m)*x^(s-1), (x,0,oo))
```

$\ds\Gamma\left(1+\frac{s}{m}\right) = \frac{s}{m}\Gamma\left(\frac{s}{m}\right)$ に注意せよ. 


### 逆Mellin変換

$f(x)$ は急減少函数でかつ $F(s)$ はそのMellin変換であるとする:

$$
F(s) = \int_0^\infty f(x)x^{s-1}\,dx.
$$

$x = e^y$ とおくと, $dx/x = dy$ より,

$$
F(s) = \int_\R f(e^y)e^{sy}\,dy.
$$

$s = a+it$, $a>0$, $t\in\R$ とおくと,

$$
F(a+it) = 
\int_\R f(e^y)e^{(a+it)y}\,dy =
\int_\R f(e^y)e^{ay} e^{ity}\,dy.
$$

$y\to-\infty$ で $f(e^y)$ が有界で $e^{ay}$ が急減少し, $y\to\infty$ で $f(e^y)$ は(その導函数も含めて)どのような $e^{ky}$ よりも急速に $0$ に収束するので,  $y\in\R$ の函数として $f(e^y)e^{ay}$ は急減少函数になることに注意せよ. ゆえに, $t\in\R$ の函数としてそのFourier逆変換 $F(a+it)$ も急減少函数になる(急減少函数全体の空間はFourier変換および逆変換で閉じている). さらに, そのことから部分積分によって解析接続した結果の $F(s)$ も虚軸方向について急減少函数になることがわかる.

Fourier変換とその逆変換の理論より,

$$
f(e^y)e^{ay} = \frac{1}{2\pi}\int_\R F(a+it) e^{-ity}\,dt
$$

すなわち

$$
f(e^y) = \frac{1}{2\pi}\int_\R F(a+it) e^{-(a+it)y}\,dt.
$$

したがって, $s=a+it$ とおくと,

$$
f(e^y) = \frac{1}{2\pi i} \int_{a-i\infty}^{a+i\infty} F(s) e^{-sy}\,ds.
$$

さらに, $x=e^y$ とおくと,

$$
f(x) = \frac{1}{2\pi i} \int_{a-i\infty}^{a+i\infty} F(s) x^{-s}\,ds.
$$

右辺を $F(s)$ の**逆Mellin変換**と呼ぶ.

**注意:** $f(x)$ に不連続点がある場合には左辺の $f(x)$ を不連続点 $x$ での値を片側極限の平均 $\ds\frac{f(x-0)+f(x+0)}{2}$ に修正した $f_*(x)$ に置き換えなければいけない. $f_*(x)$ の正確な定義は以下の通り. 詳しくはFourier変換論における**Diniの条件**について確認せよ. $\QED$

**補足:** 以上の状況のもとで, $F(s)$ の $s=0$ での留数を $r$ と書き, $-1$ 以下の $F(s)$ の極で最大のものを $B$ とし, $B<b<0$ と仮定する. このとき, 留数定理より, $x>0$ ならば

$$
f(x) = 
r + \frac{1}{2\pi i}\int_{b-i\infty}^{b+i\infty} F(s)x^{-s}\,ds = r + O(x^{-b}) \qquad (x\to\infty).
$$

逆Mellin変換表示された函数についてこの議論はよく使われる. $\QED$


## Dirichlet級数の滑らかなカットオフ


### 滑らかなカットオフの定義

$\eta(x)$ は急減少函数で $\eta(0)=1$ を満たすものであると仮定し, Dirichlet級数

$$
Z(s) = \sum_{n=1}^\infty a_n n^{-s}
$$

は $\real s \geqq a$ で絶対収束しており, 複素平面全体に有理型函数として解析接続されると仮定する.

$N>0$ に対して,

$$
Z_\eta(N,s) = \sum_{n=1}^\infty a_n n^{-s}\eta\left(\frac{n}{N}\right)
$$

とおく. これを Dirichlet級数 $Z(s)$ の**カットオフ函数** $\eta(x)$ による**滑らかなカットオフ**と呼ぶ. $\real s \geqq a$ ならば $N\to\infty$ のとき $Z_\eta(N,s)\to Z(s)$ となる.


### 滑らかなカットオフの逆Mellin変換表示

カットオフ函数 $\eta(x)$ のMellin変換を $H(s)$ と書く:

$$
H(s) = \int_0^\infty \eta(x)x^{s-1}\,dx.
$$

$H(s)$ は虚軸方向に急減少する函数になり, $a>0$, $x>0$ ならば

$$
\eta(x) = 
\frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} H(t)x^{-t}\,dt.
$$

十分大きな $a>0$ について, 

$$
\begin{aligned}
Z_\eta(N,s) &= 
\sum_{n=1}^\infty a_n n^{-s}\eta\left(\frac{n}{N}\right) =
\sum_{n=1}^\infty a_n n^{-s}
\frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} H(t)\left(\frac{n}{N}\right)^{-t}\,dt 
\\ &=
\frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} H(t)N^t\,Z(s+t)\,dt.
\end{aligned}
$$

**注意:** $\eta(0)=1$ より, $\real s > -1$ のとき, 

$$
H(s) = -\frac{1}{s}\int_0^\infty \eta'(x)x^s\,dx, 
\quad -\int_0^\infty \eta'(x)\,dx = \eta(0) = 1
$$

より, $H(s)$ の $s=0$ での留数は $1$ である. $\QED$


### Re s < 0 および s = 0 での様子

$a>0$ は十分大きいと仮定し, $b<0$ であると仮定する. $b\leqq\real t<a$ における $H(t)$ の極は $t=0$ だけであり, $t$ の函数としての $Z(s+t)$ の同じ範囲内の極の全体は $s+t=\rho_1,\rho_2,\ldots$ であり, どれも1位の極であり, それぞれの留数は $r_1,r_2,\ldots$ であると仮定する. このとき, 

$$
\begin{aligned}
&
Z_\eta(N,s) = 
\frac{1}{2\pi i}\int_{a-i\infty}^{a+i\infty} H(t)N^t\,Z(s+t)\,dt, 
\\ &
\frac{1}{2\pi i}\int_{b-i\infty}^{b+i\infty} H(t)N^t\,Z(s+t)\,dt = O(N^b) \quad(N\to\infty)
\end{aligned}
$$

と留数定理より,

$$
Z_\eta(N,s) =
\sum_{n=1}^\infty a_n n^{-s}\eta\left(\frac{n}{N}\right) = 
Z(s) + \sum_j r_j H(\rho_j-s)N^{\rho_j-s} + O(N^b).
$$

$b<0$ なので $O(N^b)$ の項は $N\to 0$ で $0$ に収束する. $N$ に関する定数項は $Z(s)$ になり, カットオフ函数の取り方によらない.  特に $s=0$ のとき, 

$$
Z_\eta(N,0) = \sum_{n=1}^\infty a_n \eta\left(\frac{n}{N}\right) = 
Z(0) + \sum_j r_j H(\rho_j)N^{\rho_j} + O(N^b). 
$$


## 滑らかなカットオフの例


### ζ(s)の場合

$\ds Z(s) = \zeta(s)=\sum_{n=1}^\infty n^{-s}$ のとき, $t$ の函数としての $\zeta(s+t)$ の極は $s+t=1$ だけであり, そこでの留数は $1$ なので, ある $b<0$ が存在して, 

$$
\sum_{n=1}^\infty n^{-s}\eta\left(\frac{n}{N}\right) = 
\zeta(s) + H(1-s)N^{1-s} + O(N^b).
$$

このように, 滑らかなカットオフは $N\to\infty$ における定数項 $\zeta(s)$ と発散項 $H(1-s)N^{1-s}$ と $0$ に収束する項に分解される. 

例えば $\eta(x)=e^{-x^2}$ のとき $\ds H(s) = \frac{1}{2}\Gamma\left(\frac{s}{2}\right)$ であり, $H(s)$ の極は0以下の偶数にしかないので, $b=-1$ に取れる:

$$
\sum_{n=1}^\infty n^{-s}\exp\left(-\frac{n^2}{N^2}\right) = 
\zeta(s) + \frac{1}{2}\Gamma\left(\frac{1-s}{2}\right)N^{1-s} + O(N^{-1}).
$$

例えば, $\eta(x)$ が $x=0$ の近傍で $1$ になるならば(すなわち, ある $\delta>0$ が存在して $|x|\leqq\delta$ ならば $\eta(x)=1$ となるならば), $b$ は幾らでも小さく取れて, $0$ に収束する項 $O(N^b)$ の部分は $N$ について急減少することになる. 

```julia
# 実軸上のプロット

eta(x) = exp(-x^2)
H(s) = gamma(s/2)/2
CutoffZeta(s; N=30, L=10^5) = (ss = float(s); sum(n-> n^(-ss)*eta(n/N), 1:L))
DivergentTerm(s; N=30) = (ss = float(s); H(1-ss)*N^(1-ss))
CutoffZeta0(s; N=30, L=10^5) = CutoffZeta(s; N=N, L=L) - DivergentTerm(s; N=N)

PP = []
for s in [-6.2:0.1:-1.6, -2.0:0.05:0.95, 1.3:0.07:4.0, 3.8:0.07:10]
    P = plot(title="N = 30", titlefontsize=10, xlabel="s")
    plot!(s, zeta.(s), label="\$\\zeta(s)\$")
    plot!(s, CutoffZeta0.(s), label="\$\\sum\\,n^{-s}\\eta(n/N) - H(1-s)N^{1-s}\$", ls=:dash)
    push!(PP, P)
end
plot(PP[1:2]..., size=(720, 260), legend=:bottomleft)
```

```julia
plot(PP[3:4]..., size=(720, 260), legend=:topright)
```

```julia
# ζ(0) = "1+1+1+…" = -1/2

Ns = 0.1:0.01:1.2
y = (N->CutoffZeta0(0.0, N=N)).(Ns)
plot(size=(350,200), xlabel="N", legendfontsize=9)
plot!(Ns, y, label="\$\\sum\\,\\eta(n/N) - H(1)N\$")
plot!(Ns, zeta(0.0)*fill(1,size(Ns)), label="\$\\zeta(0) = -1/2\$", ls=:dash)
plot!(ylims=(-0.55,-0.05))
```

```julia
# ζ(-1) = "1+2+3+…" = -1/12

Ns = 0.1:0.01:3.0
y = (N->CutoffZeta0(-1.0, N=N)).(Ns)
P1 = plot(size=(350,200), xlabel="N", legendfontsize=9)
plot!(Ns, y, label="\$\\sum\\,n\\eta(n/N) - H(2)N^2\$")
plot!(Ns, zeta(-1.0)*fill(1,size(Ns)), label="\$\\zeta(-1) = -1/12\$", ls=:dash)
plot!(ylims=(-0.13, 0))
```

```julia
# ζ(-2) = "1^2+2^2+3^2+…" = 0

Ns = 0.1:0.01:1.65
y = (N->CutoffZeta0(-2.0, N=N)).(Ns)
P1 = plot(size=(350,200), xlabel="N", legend=:bottomright, legendfontsize=9)
plot!(Ns, y, label="\$\\sum\\,n^2\\eta(n/N) - H(3)N^3\$")
plot!(Ns, zeta(-2.0)*fill(1,size(Ns)), label="\$\\zeta(-2) = 0\$", ls=:dash)
plot!(ylims=(-0.05, 0.002))
```

```julia
# ζ(-3) = "1^3+2^3+3^3+…" = 1/120

Ns = 0.1:0.01:4
y = (N->CutoffZeta0(-3.0, N=N)).(Ns)
P1 = plot(size=(350,200), xlabel="N", legendfontsize=9)
plot!(Ns, y, label="\$\\sum\\,n^3\\eta(n/N) - H(4)N^4\$")
plot!(Ns, zeta(-3.0)*fill(1,size(Ns)), label="\$\\zeta(-3) = 1/120\$", ls=:dash)
plot!(ylims=(-0.016, 0.023), legend=:bottomright)
```

```julia
# critical line 上のプロット

#pyplot()
t = 0.0:0.2:50.0
s = 0.5 .+ im .* t
@time z = CutoffZeta.(s)
w = z .- DivergentTerm.(s)
P = plot(size=(500, 300))
plot!(legend=:topright, xlabel="t")
plot!(title="s = 0.5 + it,   N = 30", titlefontsize=10, legendfontsize=10)
plot!(t, abs.(zeta.(s)), label="\$\\left|\\zeta(s)\\right|\$")
plot!(t, abs.(z), label="\$\\left|\\sum n^{-s}\\eta(n/N)\\right|\$", ls=:dashdot)
plot!(t, abs.(w), label="\$\\left|\\sum n^{-s}\\eta(n/N) - H(1-s)N^{1-s}\\right|\$", ls=:dash)
```

$H(s)$ (の絶対値)が虚軸方向に急減少するので, 発散項 $H(1-s)N^{1-s}$ は $s$ の虚部が大きいときに無視できる. そのおかげで, 発散項を引き去る前の滑らかなカットオフ $\ds \sum_{n=1}^\infty n^{-s}\eta\left(\frac{n}{N}\right)$ で $\real s=1/2$, $\imag s > 10$ における $\zeta(s)$ をよく近似できる.


### 極を持たない交代Dirichlet級数の場合

交代Drichlet級数 $Z(s)$ を

$$
Z(s) = \sum_{n=1}^\infty \frac{(-1)^{n-1}}{n^s} = (1-2^{1-s})\zeta(s)
$$

と定める(2つ目の等号を自分で確認してみよ). たとえば, 

$$
\zeta(0) = -\frac{1}{2}, \quad
\zeta(-1) = -\frac{1}{12}, \quad
\zeta(-2) = 0, \quad
\zeta(-3) = \frac{1}{120}, \quad
\ldots
$$

より,

$$
Z(0) = \frac{1}{2}, \quad
Z(-1) = \frac{1}{4}, \quad
Z(-2) = 0, \quad
Z(-3) = -\frac{1}{8}, \quad
\ldots
$$

$\zeta(s)$ の極は $s=1$ のみであり, $s=1$ は $1-2^{1-s}$ の零点なので $Z(s)$ は極を持たない. したがって, $\real s \leqq 1$ であっても,  $N\to \infty$ で

$$
Z_\eta(N,s) = \sum_{n=1}^\infty\frac{(-1)^{n-1}}{n^s}\eta\left(\frac{n}{N}\right) = Z(s) + O(N^{-1})
$$

が成立している. この場合には $N\to\infty$ における発散項はなくなる. これを数値的に確認しよう. 


#### η(x) = exp(-x^2) の場合

```julia
# 実軸上のプロット

Z(s) = (ss = float(s); (1 - 2^(1-ss)) * zeta(ss))
eta(x) = exp(-x^2)
H(s) = gamma(s/2)/2
CutoffZ(s; N=50, L=10^5) = (ss = float(s); sum(n->(-1)^(n-1)*n^(-ss)*eta(n/N), 1:L))

PP = []
for s in [-6.2:0.1:-1.6, -2.0:0.05:0.95, 1.3:0.07:4.0, 3.8:0.07:10]
    y = Z.(s)
    ymax, ymin = maximum(y), minimum(y)
    P = plot(title="N = 50", titlefontsize=10, xlabel="s", legend=true, legendfontsize=8)
    plot!(s, Z.(s), label="\$ Z(s) = \\mathrm{ana. conti. of}\\; \\sum (-1)^{n-1}n^{-s}\$", 
        lw=1.5, ylim=(ymin, ymax+0.55*(ymax-ymin)))
    plot!(s, CutoffZ.(s), label="\$\\sum\\,(-1)^{n-1}n^{-s}\\exp(-n^2/N^2)\$", ls=:dash)
    push!(PP, P)
end
plot(PP[1:2]..., size=(800, 320))
```

```julia
plot(PP[3:4]..., size=(800, 320))
```

```julia
# Z(0) = "1-1+1-1+…" = 1/2

Ns = 0.1:0.01:2.0
y = (N->CutoffZ(0, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}\\exp(-n^2/N^2)\$")
plot!(Ns, Z(0)*fill(1,size(Ns)), label="\$Z(0) = 1/2\$", ls=:dash)
plot!(ylim=(-0.05, 0.55), legend=:bottomright)
```

```julia
# Z(-1) = "1-2+3-4+…" = 1/4

Ns = 0.1:0.01:4.0
y = (N->CutoffZ(-1, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}n\\,\\exp(-n^2/N^2)\$")
plot!(Ns, Z(-1)*fill(1,size(Ns)), label="\$Z(-1) = 1/4\$", ls=:dash)
plot!(legend=:bottomright)
```

```julia
# Z(-2) = "1-4+9-16+…" = 0

Ns = 0.1:0.01:4
y = (N->CutoffZ(-2, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}n^2\\exp(-n^2/N^2)\$")
plot!(Ns, Z(-2)*fill(1,size(Ns)), label="\$Z(-2) = 0\$", ls=:dash)
plot!(ylim=(-0.05, 0.35))
```

```julia
# Z(-3) = "1-8+27-64+…" = -1/8

Ns = 0.1:0.01:4.0
y = (N->CutoffZ(-3, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}n^3\\exp(-n^2/N^2)\$")
plot!(Ns, Z(-3)*fill(1,size(Ns)), label="\$Z(-3) = -1/8\$", ls=:dash)
```

```julia
# critical line 上のプロット

t = 0.0:0.1:50.0
s = 0.5 .+ im .* t
@time z = CutoffZ.(s)
P = plot(size=(500, 300))
plot!(legend=:topright, xlabel="t")
plot!(title="s = 0.5 + it,   N = 30", titlefontsize=10)
plot!(t, abs.(Z.(s)), label="\$\\left|Z(s)\\right|=\\left|(1-2^{1-s})\\zeta(s)\\right|\$")
plot!(t, abs.(z),     label="\$\\left|\\sum (-1)^{n-1}n^{-s}\\exp(-n^2/N^2)\\right|\$", ls=:dashdot)
plot!(ylim=(-0.1, 6.2), legendfontsize=10)
```

#### η(x) = exp(-x) の場合

カットオフ函数が $\eta(x)=\exp(-x)$ のとき, $r = \exp(-1/N)$ とおくと, $0<r<1$ でかつ $N\to\infty$ のとき $r\to 1$ となり, $\eta(n/N)=r^n$ なので

$$
Z_\eta(s, N) = \sum_{n=1}^\infty\frac{(-1)^{n-1}}{n^s}\eta\left(\frac{n}{N}\right) =
\sum_{n=1}^\infty \frac{(-1)^{n-1}}{n^s} r^n
$$

となる. ゆえに $N\to\infty$ のときの $Z_\eta(s,N)$ の収束先は**Abel総和法**の意味での**Abel和**になる. したがって

$$
Z_\eta(N,s) = \sum_{n=1}^\infty\frac{(-1)^{n-1}}{n^s}\eta\left(\frac{n}{N}\right) = Z(s) + O(N^b), 
\quad -1<b<0
$$

はAbel和が $Z(s)$ に一致することを意味している.

```julia
# 実軸上のプロット

Z(s) = (ss = float(s); (1 - 2^(1-ss)) * zeta(ss))
eta(x) = exp(-x)
H(s) = gamma(s)
CutoffZ(s; N=200, L=10^5) = (ss = float(s); sum(n->(-1)^(n-1)*n^(-ss)*eta(n/N), 1:L))

PP = []
for (s, N) in [(-6.2:0.1:-1.6, 50), (-2.0:0.05:0.95, 100), (1.3:0.07:4.0, 500), (3.8:0.07:10, 1000)]
    P = plot(title="N = $N", titlefontsize=10, xlabel="s")
    y = Z.(s)
    ymax, ymin = maximum(y), minimum(y)
    z = CutoffZ.(s, N=N)
    zmax, zmin = maximum(z), minimum(z)
    yzmax, yzmin = max(ymax, zmax), min(ymin, zmin)
    plot!(s, y, label="\$ Z(s) = \\mathrm{ana. conti. of}\\; \\sum (-1)^{n-1}n^{-s}\$")
    plot!(s, z, label="\$\\sum\\,(-1)^{n-1}n^{-s}\\exp(-n/N)\$", ls=:dash)
    plot!(ylim=(yzmin, yzmax + 0.5*(yzmax-yzmin)))
    push!(PP, P)
end
plot(PP[1:2]..., size=(800, 320), legend=:topleft)
```

```julia
plot(PP[3:4]..., size=(800, 320), legend=:topleft)
```

```julia
# Z(0) = "1-1+1-1+…" = 1/2

Ns = 0.1:0.1:20.0
y = (N->CutoffZ(0, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}\\exp(-n/N)\$")
plot!(Ns, Z(0)*fill(1,size(Ns)), label="\$Z(0) = 1/2\$", ls=:dash)
plot!(ylim=(-0.05, 0.55), legend=:bottomright)
```

```julia
# Z(-1) = "1-2+3-4+…" = 1/4

Ns = 0.1:0.1:5.0
y = (N->CutoffZ(-1, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}n\\,\\exp(-n/N)\$")
plot!(Ns, Z(-1)*fill(1,size(Ns)), label="\$Z(-1) = 1/4\$", ls=:dash)
plot!(ylim=(-0.025, 0.275), legend=:bottomright)
```

```julia
# Z(-2) = "1-4+9-16+…" = 0

Ns = 0.1:0.1:50.0
y = (N->CutoffZ(-2, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}n^2\\exp(-n/N)\$")
plot!(Ns, Z(-2)*fill(1,size(Ns)), label="\$Z(-2) = 0\$", ls=:dash)
plot!(ylim=(-0.01, 0.11))
```

```julia
# Z(-3) = "1-8+27-64+…" = -1/8

Ns = 0.1:0.01:8.0
y = (N->CutoffZ(-3, N=N)).(Ns)
plot(size=(350,200), xlabel="N")
plot!(Ns, y, label="\$\\sum\\,(-1)^{n-1}n^3\\exp(-n/N)\$")
plot!(Ns, Z(-3)*fill(1,size(Ns)), label="\$Z(-3) = -1/8\$", ls=:dash)
```

```julia
# critical line 上のプロット

t = 0.0:0.1:50.0
s = 0.5 .+ im .* t
@time z = CutoffZ.(s)
P = plot(size=(500, 300))
plot!(legend=:topright, xlabel="t")
plot!(title="s = 0.5 + it,   N = 200", titlefontsize=10)
plot!(t, abs.(Z.(s)), label="\$\\left|Z(s)\\right|=\\left|(1-2^{1-s})\\zeta(s)\\right|\$")
plot!(t, abs.(z),     label="\$\\left|\\sum (-1)^{n-1}n^{-s}\\exp(-n/N)\\right|\$", ls=:dashdot)
plot!(ylim=(-0.1, 6.2), legendfontsize=10)
```

### -ζ'(s)の場合

$\ds Z(s) = -\zeta'(s) = -\sum_{n=1}^\infty n^{-s}\log n$ の場合を考える. $-\zeta'(s)$ は $s=1$ で

$$
-\zeta'(s)=\frac{1}{(s-1)^2}+\gamma_1+\gamma_2(s-1)+\cdots
$$

とLaurent展開される. $s=1$ で2位の極なので上で作った公式をそのまま使用することはできないが, 同様の議論によって次が得られる.  ある $b<0$ が存在して, 

$$
\sum_{n=1}^\infty n^{-s}\log n\cdot\eta\left(\frac{n}{N}\right) =
-\zeta'(s) + H(1-s)N^{1-s}\log N + H'(1-s)N^{1-s} + O(N^b).
$$

特に $s=0$ のとき,

$$
\sum_{n=1}^\infty \log n\cdot\eta\left(\frac{n}{N}\right) =
-\zeta'(0) + H(1)N\log N + H'(1)N + O(N^b).
$$

すなわち, 左辺から発散項を除いて残る定数項はカットオフ函数 $\eta(x)$ の取り方によらず $-\zeta'(0)$ であることがわかる.

$$
-\zeta'(0)=\log\sqrt{2\pi}
$$

であることがよく知られている.  以上の内容は本質的に階乗に関するStirlingの近似公式であるとも考えられる. 

```julia
H(s) = gamma(s/2)/2
s = symbols("s")
diff(H(s), s) |> display
H1(s) = gamma(s/2)*digamma(s/2)/4
H1(Sym(1)) |> display
@show H1(1);
```

```julia
eta(x) = exp(-x^2)
CutoffLogSum(N; L=10^6) = sum(n->log(n)*eta(n/N), 1:L)
ApproxSum(N) = log(√(2π)) + H(1)*N*log(N) + H1(1)*N

[(N, CutoffLogSum(N), ApproxSum(N)) for N in 1:10] |> display

PP = []
for Ns in [1:10, 1:100]
    P = plot(legend=:topleft, xlabel="N")
    y = CutoffLogSum.(Ns)
    ymin, ymax = extrema(y)
    plot!(Ns, y, label="\$\\sum\\log(n)\\eta(n/N)\$")
    plot!(Ns, ApproxSum.(Ns), label="\$\\log(\\sqrt{2\\pi}) + H(1)N\\log N + H'(1)N\$", ls=:dash)
    plot!(ylims=(ymin-0.05*(ymax-ymin), ymax+0.5*(ymax-ymin)), legendfontsize=9)
    push!(PP, P)
end
plot(PP..., size=(800,300))
```

### -(d/ds)log ζ(s)の場合

$\ds Z(s)=-\frac{d}{ds}\log\zeta(s)=-\frac{\zeta'(s)}{\zeta(s)}$ の場合を考えよう. $\zeta(s)$ はEuler積表示

$$
\zeta(s) = \prod_p (1-p^{-s})^{-1} \quad (\real s > 1)
$$

を持つ. ここで $p$ は素数全体を走る. これより, 非常によく使われる対数函数のTayloe展開

$$
-\log(1-x) = \sum_{k=1}^\infty\frac{x^k}{k} \quad (|x|<1)
$$

より

$$
\log\zeta(s) = -\sum_p\log(1-p^{-s}) =
\sum_p\sum_{k=1}^\infty \frac{p^{-ks}}{k} =
\sum_{n=1}^\infty \frac{\Lambda(n)}{\log n}n^{-s}.
$$

ここで $\Lambda(n)$ は所謂 von Mangoldt 函数である:

$$
\Lambda(n) = 
\begin{cases}
\log p & (n=p^k,\ \text{$p$ is prime},\ k\in\Z_{>0}), \\
0      & (\text{otherwise}).
\end{cases}
$$

ゆえに

$$
-\frac{d}{ds}\log\zeta(s) = -\frac{\zeta'(s)}{\zeta(s)} =
\sum_p\sum_{k=1}^\infty p^{-ks}\log p =
\sum_{n=1}^\infty \Lambda(n)n^{-s}.
$$

$\ds Z(s)=-\frac{d}{ds}\log\zeta(s)$ の極はすべて1位であり, それら全体は $\zeta(s)$ の極と零点の全体に一致する. ゆえに

$$
\zeta(0) = -\frac{1}{2}, \quad
-\zeta'(0) = \log\sqrt{2\pi}, \quad
-\frac{\zeta'(0)}{\zeta(0)} = -\log(2\pi)
$$

より, $a=2$, $b=-1$ とすると, 

$$
\sum_{n=1}^\infty \Lambda(n)\eta\left(\frac{n}{N}\right) = 
-\log(2\pi) + H(1) N - \sum_{j=1}^\infty H(\rho_j)N^{\rho_j} + O(N^{-1}). 
$$

ここで, $\rho_j$ は $0\leqq \real s\leqq 1$ における $\zeta(s)$ の零点の全体である. 

```julia
#  von Mangoldt 函数 Λ(n) のカットオフを入れた和の計算

eta(x) = exp(-x^2)
Eta(x) = gamma(x/2)/2

function vonMangoldtCutoffSum(N; L=10^6)
    c = 0.0
    for p in primes(L)
        for k in 1:floor(Int,log(p,L))
            c += log(p) * eta(p^k/N)
        end
    end
    c
end

ApproximateCutoffSum(N) = -log(2π) + Eta(1)*N
@show log(2π)
@show Eta(1)

[(N, vonMangoldtCutoffSum(N), ApproximateCutoffSum(N)) for N in 1:10] |> display

PP = []
for Ns in [1:10, 1:100]
    P = plot(legend=:topleft, xlabel="N")
    y1 = vonMangoldtCutoffSum.(Ns)
    y2 = ApproximateCutoffSum.(Ns)
    ymin, ymax = extrema([y1;y2])
    plot!(Ns, y1, label="\$\\sum\\,\\Lambda(n)\\eta(n/N)\$")
    plot!(Ns, y2, label="\$-\\log(2\\pi)+H(1)N\$", ls=:dash)
    plot!(ylims=(ymin-0.05*(ymax-ymin), ymax+0.3*(ymax-ymin)), legendfontsize=9)
    push!(PP, P)
end
plot(PP..., size=(800,300))
```

**注意:** $\rho_j$ を $0\leqq \real s\leqq 1$ における $\zeta(s)$ の虚部の絶対値が $j$ 番目に小さな零点であるとするとし, $\psi(x)$ を

$$
\psi(x) = \sum_{1\leqq n\leqq x} \Lambda(n), 
$$

と定め, $\psi(x)$ の不連続点 $x$ における値を $\ds\frac{\psi(x-0)+\psi(x-0)}{2}$ で訂正したものを $\psi_*(x)$ と書くと,

$$
\psi_*(x) = -\log(2π) + x - \sum_{j=1}^\infty\frac{x^{\rho_j}}{\rho_j} -\frac{1}{2}\log(1-x^2)
$$

が成立することがよく知られている(**von Mangoldtの明示公式**). こちらの明示公式の場合には $x^{\rho_j}$ の係数が $1/\rho_j$ であり, $\rho_j$ の虚部についてゆっくり減少する函数になってしまっている. そのため $\ds \sum_{j=1}^\infty\frac{x^{\rho_j}}{\rho_j}$ は足し上げる順序に依存する条件収束級数になっている.  それに対して, 上で示した

$$
\sum_{n=1}^\infty \Lambda(n)\eta\left(\frac{n}{N}\right) =
-\log(2\pi) + H(1)N - \sum_{j=1}^\infty H(\rho_j)N^{\rho_j} + O(N^{-1})
$$

における $H(s)$ は $s$ の虚部の函数として急減少函数になっている点が大きく違う. 

$0\leqq\real s\leqq 1$, $0\leqq \imag s\leqq T$ を満たす $\zeta(s)$ の(重複度を含めた)零点の個数 $N(T)$ については

$$
N(T) = \frac{T}{2\pi}\log\frac{T}{2\pi} - \frac{T}{2\pi} + O(\log T)
$$

が成立することが知られている. $\QED$

```julia
# ψ(x) = Σ_{n≦x} Λ(n) と -log(2π) + x をプロットして比較

function psi(x)
    c = 0.0
    for p in primes(floor(Int,x))
        for k in 1:floor(Int,log(p,x))
            c += log(p)
        end
    end
    c
end

approxpsi(x) = -log(2π) + x

[(N, psi(N), approxpsi(N)) for N in 1:10] |> display

PP = []
for x in [1:0.05:12, 1:0.5:120]
    P = plot(legend=:topleft, xlabel="x")
    plot!(x, psi.(x), label="\$\\psi(x)=\\sum_{n\\leq x}\\,\\Lambda(n)\$")
    plot!(x, approxpsi.(x), label="\$-\\log(2\\pi)+x\$", ls=:dash)
    push!(PP, P)
end
plot(PP..., size=(800,300))
```

**注意:** $x$ 以下の素数の個数を $\pi(x)$ と書くとき, $\ds\pi(x)\sim\frac{x}{\log x}$ が成立するという結果は**素数定理**と呼ばれている.  $\psi(x)\sim x$ と素数定理は同値である. 上にプロットした結果はさらに定数項が $-\log(2\pi)$ になることを意味している. $\QED$


## Riemannのゼータ函数の非自明な零点の分布

http://www.dtc.umn.edu/~odlyzko/zeta_tables/index.html にRiemannのゼータ函数の非自明な零点の虚部のリストが置いてある.

それを用いて, 虚部が $0$ 以上 $T$ 以下の非自明な零点の個数 $N(T)$ の個数をプロットしてみよう. 次の漸近挙動が知られている:

$$
N(T) \sim \frac{T}{2\pi} \log\frac{T}{2\pi} - \frac{T}{2\pi}.
$$

```julia
res = HTTP.get("http://www.dtc.umn.edu/~odlyzko/zeta_tables/zeros1", 
    readtimeout=60, retry=true, retries=20)
zetazeros = parse.(Float64, split(String(res.body), "\n")[1:end-1])
zetazeros[1:10]
```

```julia
# NZeros(T) = N(T) のプロット

NZeros(T) = count(zetazeros .≤ T)
AZeros(T) = T/(2π)*log(T/(2π)) - T/(2π)
maxT = maximum(zetazeros)

PP = []
for T in [0:100, 0:maxT/200:maxT]
    P = plot(legend=:topleft, xlabel="T")
    plot!(title="N(T)", titlefontsize=10)
    plot!(T, NZeros.(T), label="\$N(T)\$")
    plot!(T, AZeros.(T), label="\$\\frac{T}{2\\pi}\\log\\frac{T}{2\\pi} - \\frac{T}{2\\pi}\$", 
        ls=:dash)
    plot!(legendfontsize=10)
    push!(PP, P)
end
plot(PP..., size=(800,300))
```

```julia

```
