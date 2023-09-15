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
    display_name: Julia 1.9.3
    language: julia
    name: julia-1.9
---

# 06 Taylorの定理

黒木玄

2018-04-20～2019-04-03, 2020-08-18, 2023-06-02～2023-09-15

* Copyright 2018, 2019, 2020, 2023 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

* http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/06%20Taylor%27s%20theorems.ipynb

* https://genkuroki.github.io/documents/Calculus/06%20Taylor%27s%20theorems.pdf

<!-- このファイルは <a href="https://juliabox.com">Julia Box</a> で利用できる. -->

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
\newcommand\arctanh{\operatorname{arctanh}}
\newcommand\real{\operatorname{Re}}
\newcommand\imag{\operatorname{Im}}
\newcommand\Li{\operatorname{Li}}
\newcommand\PROD{\mathop{\coprod\kern-1.35em\prod}}
$

<!-- #region toc=true -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#Taylorの定理" data-toc-modified-id="Taylorの定理-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>Taylorの定理</a></span><ul class="toc-item"><li><span><a href="#積分型剰余項版Taylorの定理" data-toc-modified-id="積分型剰余項版Taylorの定理-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>積分型剰余項版Taylorの定理</a></span><ul class="toc-item"><li><span><a href="#積分型剰余項の絶対値の上からの評価" data-toc-modified-id="積分型剰余項の絶対値の上からの評価-1.1.1"><span class="toc-item-num">1.1.1&nbsp;&nbsp;</span>積分型剰余項の絶対値の上からの評価</a></span></li><li><span><a href="#積分型剰余項の書き直し:-1重積分表示" data-toc-modified-id="積分型剰余項の書き直し:-1重積分表示-1.1.2"><span class="toc-item-num">1.1.2&nbsp;&nbsp;</span>積分型剰余項の書き直し: 1重積分表示</a></span></li><li><span><a href="#積分型剰余項の書き直し:-Lagrangeの剰余項" data-toc-modified-id="積分型剰余項の書き直し:-Lagrangeの剰余項-1.1.3"><span class="toc-item-num">1.1.3&nbsp;&nbsp;</span>積分型剰余項の書き直し: Lagrangeの剰余項</a></span></li><li><span><a href="#積分型剰余項の書き直し:-Cauchyの剰余項" data-toc-modified-id="積分型剰余項の書き直し:-Cauchyの剰余項-1.1.4"><span class="toc-item-num">1.1.4&nbsp;&nbsp;</span>積分型剰余項の書き直し: Cauchyの剰余項</a></span></li><li><span><a href="#Landau記号を用いたTaylorの定理" data-toc-modified-id="Landau記号を用いたTaylorの定理-1.1.5"><span class="toc-item-num">1.1.5&nbsp;&nbsp;</span>Landau記号を用いたTaylorの定理</a></span></li></ul></li><li><span><a href="#$n$-回微分可能性だけを仮定した場合のTaylorの定理について" data-toc-modified-id="$n$-回微分可能性だけを仮定した場合のTaylorの定理について-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>$n$ 回微分可能性だけを仮定した場合のTaylorの定理について</a></span></li></ul></li><li><span><a href="#Taylor展開" data-toc-modified-id="Taylor展開-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>Taylor展開</a></span><ul class="toc-item"><li><span><a href="#基本的なTaylor展開の例" data-toc-modified-id="基本的なTaylor展開の例-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>基本的なTaylor展開の例</a></span></li><li><span><a href="#コンピューターを用いた計算例" data-toc-modified-id="コンピューターを用いた計算例-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>コンピューターを用いた計算例</a></span></li></ul></li><li><span><a href="#Bernoulli数とEuler数を用いたTaylor展開" data-toc-modified-id="Bernoulli数とEuler数を用いたTaylor展開-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>Bernoulli数とEuler数を用いたTaylor展開</a></span><ul class="toc-item"><li><span><a href="#Bernoulli数とEuler数の定義" data-toc-modified-id="Bernoulli数とEuler数の定義-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>Bernoulli数とEuler数の定義</a></span></li><li><span><a href="#tan,-cot,-sec,-cosec-の展開" data-toc-modified-id="tan,-cot,-sec,-cosec-の展開-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>tan, cot, sec, cosec の展開</a></span></li></ul></li><li><span><a href="#べき級数の収束" data-toc-modified-id="べき級数の収束-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>べき級数の収束</a></span><ul class="toc-item"><li><span><a href="#べき級数の定義" data-toc-modified-id="べき級数の定義-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>べき級数の定義</a></span></li><li><span><a href="#べき級数の収束半径" data-toc-modified-id="べき級数の収束半径-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>べき級数の収束半径</a></span><ul class="toc-item"><li><span><a href="#d'Alembert-の判定法" data-toc-modified-id="d'Alembert-の判定法-4.2.1"><span class="toc-item-num">4.2.1&nbsp;&nbsp;</span>d'Alembert の判定法</a></span></li><li><span><a href="#Cauchy-Hadamard定理" data-toc-modified-id="Cauchy-Hadamard定理-4.2.2"><span class="toc-item-num">4.2.2&nbsp;&nbsp;</span>Cauchy-Hadamard定理</a></span></li></ul></li><li><span><a href="#収束円盤の境界上でのべき級数の収束" data-toc-modified-id="収束円盤の境界上でのべき級数の収束-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>収束円盤の境界上でのべき級数の収束</a></span><ul class="toc-item"><li><span><a href="#準備:-Cesàro総和可能性" data-toc-modified-id="準備:-Cesàro総和可能性-4.3.1"><span class="toc-item-num">4.3.1&nbsp;&nbsp;</span>準備: Cesàro総和可能性</a></span></li><li><span><a href="#Abel総和可能性" data-toc-modified-id="Abel総和可能性-4.3.2"><span class="toc-item-num">4.3.2&nbsp;&nbsp;</span>Abel総和可能性</a></span></li><li><span><a href="#準備:-Vitaliの定理" data-toc-modified-id="準備:-Vitaliの定理-4.3.3"><span class="toc-item-num">4.3.3&nbsp;&nbsp;</span>準備: Vitaliの定理</a></span></li><li><span><a href="#M.-Riesz-の補題とその応用" data-toc-modified-id="M.-Riesz-の補題とその応用-4.3.4"><span class="toc-item-num">4.3.4&nbsp;&nbsp;</span>M. Riesz の補題とその応用</a></span></li></ul></li></ul></li><li><span><a href="#超幾何級数" data-toc-modified-id="超幾何級数-5"><span class="toc-item-num">5&nbsp;&nbsp;</span>超幾何級数</a></span></li></ul></div>
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
default(fmt = :png)
#gr(); ENV["PLOTS_TEST"] = "true"
#clibrary(:colorcet)
#clibrary(:misc)

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

## Taylorの定理

$$
f(x) = f(a) + f'(a)(x-a) + \frac{1}{2}f''(a)(x-a)^2 + \cdots + \frac{1}{k!}f^{(k)}(a)(x-a)^k + \cdots
$$

の最後の $+\cdots$ の部分を $f$ の $k+1$ 階の導函数の式で書いた結果を**Taylorの定理**と呼ぶことにする.  その表示の仕方には様々な形がある.


### 積分型剰余項版Taylorの定理


$f$ は $C^n$ 級函数であると仮定する.  

まず, $n=4$ の場合を例として扱う. そのとき

$$
f'''(x) = f'''(a) + \int_a^x f^{(4)}(x_4)\,dx_4.
$$

ゆえに

$$
\begin{aligned}
f''(x) &= 
f''(a) + \int_a^x f'''(x_3)\,dx_3
\\ & =
f''(a) + f'''(a)(x-a) + \int_a^x\left[\int_a^{x_3} f^{(4)}(x_4)\,dx_4\right]\,dx_3.
\end{aligned}
$$

括弧が増えると書くのが面倒になるので

$$
\int_a^x dx_3\int_a^{x_3} dx_4\; f^{(4)}(x_4) :=
\int_a^x\left[\int_a^{x_3} f^{(4)}(x_4)\,dx_4\right]\,dx_3
$$

と書くことにする. 積分が3重以上でも同様に書くことにする.  そのように書くと, 

$$
f''(x) = 
f''(a) + f'''(a)(x-a) + \int_a^x dx_3\int_a^{x_3} dx_4\; f^{(4)}(x_4).
$$

ゆえに

$$
\begin{aligned}
f'(x) &= f'(a) + \int_a^x dx_2\,f''(x_2)
\\ & =
f'(a) + f''(a)(x-a) + \frac{1}{2}f'''(a)(x-a)^2 + 
\int_a^x dx_2\int_a^{x_2}dx_3\int_a^{x_3} dx_4\; f^{(4)}(x_4).
\end{aligned}
$$

さらに, 

$$
\begin{aligned}
f(x) &= f(a) + \int_a^x dx_1\,f'(x_1)
\\ & =
f(a) + f'(a)(x-a) + \frac{1}{2}f''(a)(x-a)^2 + \frac{1}{3!}f'''(a)(x-a)^3 +
\int_a^x dx_1 \int_a^{x_1} dx_2\int_a^{x_2}dx_3\int_a^{x_3} dx_4\; f^{(4)}(x_4).
\end{aligned}
$$

以上をまとめると, 

$$
\begin{aligned}
&
f(x) = f(a) + f'(a)(x-a) + \frac{1}{2}f''(a)(x-a)^2 + \frac{1}{3!}f'''(a)(x-a)^3 + R_4,
\\ &
R_4 = \int_a^x dx_1 \int_a^{x_1} dx_2\int_a^{x_2}dx_3\int_a^{x_3} dx_4\; f^{(4)}(x_4).
\end{aligned}
$$


上と同様にして次が成立することを示せる.

**Taylorの定理:** $f$ は $C^n$ 級函数であると仮定する. そのとき, 
$$
\begin{aligned}
&
f(x) = f(a) + f'(a)(x-a) + \frac{1}{2}f''(a)(x-a)^2 + \cdots + 
\frac{1}{(n-1)!}f^{(n-1)}(a)(x-a)^{n-1} + R_n,
\\ &
R_n = \int_a^x dx_1 \int_a^{x_1} dx_2\cdots\int_a^{x_{n-1}} dx_n\; f^{(n)}(x_n).
\qquad 
\end{aligned}
$$

$R_n$ を**剰余項**と呼ぶ. $\QED$

この形のTaylorの定理は $f^{(n)}(x)$ を何度も積分するだけで得られ, 技巧的な証明の工夫をする必要が一切ない.

**問題:** 上の形のTaylorの定理の証明を完成せよ. $\QED$


#### 積分型剰余項の絶対値の上からの評価

以下, $a$ と $x$ のあいだの $t$ について $|f^{(n)}(t)|\leqq M$ が成立しているとき, 

$$
|R_n| \leqq \left|\int_a^x dx_1 \int_a^{x_1} dx_2\cdots\int_a^{x_{n-1}} dx_n\; M \right| = 
\frac{1}{n!}M |x-a|^n.
$$

ここで, 

$$
\int_a^x dx_1 \int_a^{x_1} dx_2\cdots\int_a^{x_{n-1}} dx_n\;1 = \frac{1}{n!}(x-a)^n
$$

となることを使った. $1$ を $a$ から $x$ まで積分する操作を $n$ 回繰り返すとそうなることは容易に確かめられる.

$$
|R_n| \leqq \frac{M |x-a|^n}{n!}
$$

の形の不等式はよく使われる.

<!-- #region -->
#### 積分型剰余項の書き直し: 1重積分表示

**定理:**
$$
R_n = \int_a^x f^{(n)}(x_n) \frac{(x-x_n)^{n-1}}{(n-1)!}\,dx_n.
$$

もしくは $x_n$ を $t$ と書き直すことによって, 

$$
R_n = \int_a^x f^{(n)}(t) \frac{(x-t)^{n-1}}{(n-1)!}\,dt.
$$

**証明:** 簡単のため $a<x$ と仮定する($a>x$ の場合も同様である).  そのとき, 積分

$$
R_n = \int_a^x dx_1 \int_a^{x_1} dx_2\cdots\int_a^{x_{n-1}} dx_n\; f^{(n)}(x_n)
$$

の中の $x_i$ たちは $a\leqq x_n \leqq x_{n-1}\leqq\cdots x_2 \leqq x_1 \leqq x$ を動く. ゆえに, この積分は

$$
R_n = \int_a^x\left[\,
f^{(n)}(x_n)
\int_{x_n}^x dx_1\int_{x_n}^{x_1}dx_2\cdots\int_{x_n}^{x_{n-2}}dx_{n-1}\; 1
\right]\,dx_n
$$


と書き直される. そして, 

$$
\int_{x_n}^x dx_1\int_{x_n}^{x_1}dx_2\cdots\int_{x_n}^{x_{n-2}}dx_{n-1}\; 1 = 
\frac{(x-x_n)^{n-1}}{(n-1)!}
$$

なので上の定理が得られる. $\QED$
<!-- #endregion -->

__系:__
$$
R_n = (x-a)^n \int_0^1 f^{(n)}(a + (x-a)t) \frac{(1-t)^{n-1}}{(n-1)!}\,dt.
$$

__証明:__ 上の定理で得た公式中の積分変数 $x_n$ を $x_n = a + (x-a)t = (1-t)a+tx$ によって $t$ に変換すると, $x_n$ を $a$ から $x$ まで動かすことは $t$ を $0$ から $1$ まで動かすことに対応し, $dx_n = (x-a)\,dt$, $x - x_n = (1-t)(x-a)$ なので, 

$$
\begin{aligned}
R_n
&= \int_0^1 f^{(n)}(a + (x-a)t) \frac{(1-t)^{n-1}(x-a)^{n-1}}{(n-1)!}\,(x-a)\,dt
\\
&= (x-a)^n \int_0^1 f^{(n)}(a + (x-a)t) \frac{(1-t)^{n-1}}{(n-1)!}\,dt.
\qquad \QED
\end{aligned}
$$


**注意:** $f$ が $C^{n+1}$ 級函数ならば, 上の定理の $n$ の場合の結果から, 部分積分によって

$$
R_n = f^{(n)}(a)\frac{(x-a)^n}{n!} + R_{n+1}
$$

が得られる(問題: 示してみよ). ゆえに, 部分積分を用いた $n$ に関する帰納法によっても上の定理を示せる. 部分積分を用いる計算法はよく使われる. $\QED$


**注意:** $f$ は $C^n$ 級であるとする. 剰余項 $R_n$ の1重積分を以下のようにしてシンプルに求めることもできる. $x$, $a$ を固定し, 函数 $R_n(t)$ を次のように定める:

$$
R_n(t) = f(x) - \sum_{k=0}^{n-1}f^{(k)}(t)\frac{(x-t)^k}{k!}.
$$

このとき, 

$$
\begin{aligned}
R_n'(t) &= -
\sum_{k=0}^{n-1}f^{(k+1)}(t)\frac{(x-t)^k}{k!} +
\sum_{k=1}^{n-1}f^{(k)}(t)\frac{(x-t)^{k-1}}{(k-1)!}
\\ &= -
\sum_{k=1}^{n}f^{(k)}(t)\frac{(x-t)^{k-1}}{(k-1)!} +
\sum_{k=1}^{n-1}f^{(k)}(t)\frac{(x-t)^{k-1}}{(k-1)!}
\\ &= -
f^{(n)}(t)\frac{(x-t)^{n-1}}{(n-1)!},
\\
R_n(x) &= 0, 
\\
R_n(a) &= R_n
\end{aligned}
$$

なので

$$
R_n = R_n(a) =
\int_x^a\left(-f^{(n)}(t)\frac{(x-t)^{n-1}}{(k-1)!}\right)\,dt =
\int_a^x f^{(n)}(t)\frac{(x-t)^{n-1}}{(k-1)!}\,dt.
$$

おそらく, Taylorの定理を証明する最も簡単な方法は $R_n(t)$ を上のように定義して, $t$ で微分することである. この方法の欠点は $R_n(t)$ の定義を天下り的に与えなければいけないことである.  $\QED$


#### 積分型剰余項の書き直し: Lagrangeの剰余項

一時的に簡単のため $a<x$ であると仮定する.

$f$ は $C^n$ 級函数であるとする. そのとき $f^{(n)}$ は連続函数になる. $f^{(n)}(t)$ の $a\leqq t\leqq x$ における最小値と最大値はそれぞれ $f^{(n)}(\alpha)$, $f^{(n)}(\beta)$ であるとする. そのとき,

$$
f^{(n)}(\alpha) \frac{(x-a)^n}{n!}\leqq  R_n \leqq f^{(n)}(\beta) \frac{(x-a)^n}{n!}
\tag{$*$}
$$

が成立する. なぜならば $R_n$ の1重積分表示

$$
R_n = \int_a^x f^{(n)}(t) \frac{(x-t)^{n-1}}{(n-1)!}\,dt
$$

の中の $f^{(n)}(t)$ を定数 $C=f(\alpha),f(\beta)$ で置き換えて得られる積分は

$$
\int_a^x C \frac{(x-t)^{n-1}}{(n-1)!}\,dt = C\frac{(x-a)^n}{n!}.
$$

となるからである. 中間値の定理より, ある実数 $\xi$ で

$$
R_n = f^{(n)}(\xi)\frac{(x-a)^n}{n!}, \quad a<\xi<x 
$$

を満たすものが存在する.  

$a<x$ の仮定を $a>x$ で置き換えると, $a<\xi<x$ は $a>\xi>x$ に変わる.

以上で導出した剰余項の

$$
R_n = f^{(n)}(\xi)\frac{(x-a)^n}{n!}, \quad a{< \atop >}\xi{<\atop >}x 
$$

の形もよく使われる. この形の剰余項を**Lagrangeの剰余項**と呼ぶらしい.


__例:__ $f(x)=\sqrt{x}$, $x=10$, $a=9$, $n=2$ の場合.  $n=2$ の場合のTaylorの公式は

$$
f(x) = f(a) + f'(a)(x-a) + R_2.
$$

そして, 上の($*$)より, $f''(t)$ の $a\leqq t\leqq x$ での最小値と最大値をそれぞれ $f''(\alpha)$, $f''(\beta)$ と書くと,

$$
f''(\alpha)\frac{(x-a)^2}{2} \leqq R_2 \leqq f''(\beta)\frac{(x-a)^2}{2}.
$$

$f(x)=\sqrt{x}$, $0<a<x$ のとき,

$$
f'(t) = \dfrac{1}{2\sqrt{t}}, \quad
f''(t)=-\frac{1}{4t\sqrt{t}}
$$

なので, 特に $f''(t)$ は単調増加函数になり, $\alpha=a$, $\beta=x$ となるので, 

$$
-\frac{(x-a)^2}{8a\sqrt{a}} \leqq R_2 \leqq -\frac{(x-a)^2}{8x\sqrt{x}}.
$$

これの全体に $f(a)+f'(a)(x-a)=\sqrt{a} + \dfrac{x-a}{2\sqrt{a}}$ を足すと,

$$
\sqrt{a} + \frac{x-a}{2\sqrt{a}} - \frac{(x-a)^2}{8a\sqrt{a}}
\leqq \sqrt{x} \leqq
\sqrt{a} + \frac{x-a}{2\sqrt{a}} - \frac{(x-a)^2}{8x\sqrt{x}}.
$$

$a=9$, $x=10$ のとき, $\sqrt{9}=3$, $\sqrt{10}<\sqrt{16}=4$ なので,

$$
3 + \frac{1}{2\cdot 3} - \frac{1}{8\cdot 9\cdot 3}
\leqq \sqrt{10} <
3 + \frac{1}{2\cdot 3} - \frac{1}{8\cdot 10\cdot 4}.
$$

ゆえに,

$$
3.162 < \sqrt{10} < 3.164.
$$

このようにして $\sqrt{10} \approx 3.1622776601683795$ の値を小数点以下第2桁まで求めることができる. $\QED$

```julia
@show 3 + 1/(2*3)
@show 3 + 1/(2*3) - 1/(8*9*3)
@show 3 + 1/(2*3) - 1/(8*10*4)
@show √10;
```

#### 積分型剰余項の書き直し: Cauchyの剰余項

一時的に簡単のため $a<x$ であると仮定する.

$f$ は $C^n$ 級函数であるとする. そのとき $f^{(n)}$ は連続函数になる. $f^{(n)}(t)(x-t)^{n-1}/(n-1)!$ の $a\leqq t\leqq x$ における最小値と最大値はそれぞれ $f^{(n)}(\alpha)(x-\alpha)^{n-1}/(n-1)!$, $f^{(n)}(\beta)(x-\beta)^{n-1}/(n-1)!$ であるとする. そのとき,

$$
f^{(n)}(\alpha) \frac{(x-\alpha)^{n-1}(x-a)}{(n-1)!}\leqq 
R_n \leqq
f^{(n)}(\beta) \frac{(x-\beta)^{n-1}(x-a)}{(n-1)!}
$$

が成立する. なぜならば $R_n$ の1重積分表示

$$
R_n = \int_a^x f^{(n)}(t) \frac{(x-t)^{n-1}}{(n-1)!}\,dt
$$

の中の $f^{(n)}(t)(x-t)^{n-1}/(n-1)!$ を定数 $C=f^{(n)}(\alpha)(x-\alpha)^{n-1}/(n-1)!, f^{(n)}(\beta)(x-\beta)^{n-1}/(n-1)!$ で置き換えて得られる積分は

$$
\int_a^x C \,dt = C(x-a)
$$

となるからである. 中間値の定理より, ある実数 $\xi$ で

$$
R_n = f^{(n)}(\xi)\frac{(x-\xi)^{n-1}(x-a)}{(n-1)!}, \quad a<\xi<x 
$$

を満たすものが存在する.  

$a<x$ の仮定を $a>x$ で置き換えると, $a<\xi<x$ は $a>\xi>x$ に変わる.

以上で導出した剰余項の

$$
R_n = f^{(n)}(\xi)\frac{(x-\xi)^{n-1}(x-a)}{(n-1)!}, \quad a{< \atop >}\xi{<\atop >}x 
$$

の形もよく使われる. この形の剰余項を**Cauchyの剰余項**と呼ぶらしい.


#### Landau記号を用いたTaylorの定理

$C^n$ 級函数 $f$ について $a$ と $x$ のあいだのある実数 $\xi$ で

$$
R_n = f^{(n)}(\xi)\frac{(x-a)^n}{n!}
$$

を満たすものが存在することを上で示した. $f^{(n)}$ は連続なので $x\to a$ のとき $f^{(n)}(\xi)\to f^{(n)}(a)$ となる. ゆえに

$$
R_n - f^{(n)}(a)\frac{(x-a)^n}{n!} = (f^{(n)}(\xi) - f^{(n)}(a))\frac{(x-a)^n}{n!} = o((x-a)^n)
\quad (x\to a).
$$

これで次が成立することがわかった:

$$
f(x) = f(a) + f'(a)(x-a) + \frac{1}{2}f''(a)(x-a)^2 + \cdots + 
\frac{1}{n!}f^{(n)}(a)(x-a)^n + o((x-a)^n)
\quad (x\to a).
$$

Taylorの定理はこの形でもよく使われる.

さらに $f$ が $C^{n+1}$ 級ならば, $n+1$ 次の剰余項は

$$
R_{n+1} = \frac{1}{(n+1)!}f^{(n+1)}(\xi)(x-a)^{n+1} = O((x-a)^{n+1}), \quad
\text{($\xi$ は $a$ と $x$ のあいだにある)}
$$

の形をしているので, 

$$
f(x) = f(a) + f'(a)(x-a) + \frac{1}{2}f''(a)(x-a)^2 + \cdots + 
\frac{1}{n!}f^{(n)}(a)(x-a)^n + O((x-a)^{n+1})
\quad (x\to a).
$$

も成立していることがわかる.


### $n$ 回微分可能性だけを仮定した場合のTaylorの定理について

以上では $C^n$ 級の仮定のもとでTaylorの定理を証明したが, $n$ 回微分可能の仮定のもとでもTaylorの定理を導くことができる. 例えば, 

* 高木貞治『解析概論』改定第三版

の第25節のpp.61-63に詳しい説明がある.

以下ではその議論をさらに整理したものを簡単に解説する.


$f$ は $n$ 回微分可能な函数であると仮定し, $x\ne a$ を固定し, $t$ の函数 $R_n(t)$ と剰余項 $R_n$ を

$$
R_n(t) = f(x) - \sum_{k=0}^{n-1}f^{(k)}(t)\frac{(x-t)^k}{k!}, \quad R_n = R_n(a)
$$

と定める.  $f$ が $n$ 回微分可能であることより, $R_n(t)$ は微分可能になり, 

$$
\begin{aligned}
R_n'(t) &= -
\sum_{k=0}^{n-1}f^{(k+1)}(t)\frac{(x-t)^k}{k!} +
\sum_{k=1}^{n-1}f^{(k)}(t)\frac{(x-t)^{k-1}}{(k-1)!}
\\ &= -
\sum_{k=1}^{n}f^{(k)}(t)\frac{(x-t)^{k-1}}{(k-1)!} +
\sum_{k=1}^{n-1}f^{(k)}(t)\frac{(x-t)^{k-1}}{(k-1)!}
\\ &= -
f^{(n)}(t)\frac{(x-t)^{n-1}}{(n-1)!},
\\
R_n(x) &= 0, 
\\
R_n(a) &= R_n
\end{aligned}
$$

が成立している. ゆえに $R_n(t)$ と $G(t)=(x-t)^p$ ($p=1,\ldots,n$) にCauchyの平均値の定理を適用すると, $a$ と $x$ のあいだにある実数 $\xi$ で次を満たすものが存在することがわかる:

$$
\frac{R_n}{(x-a)^p} =
\frac{R_n(a)-R_n(x)}{G(a)-G(x)} =
\frac{R_n'(\xi)}{G'(\xi)} =
f^{(n)}(\xi)\frac{(x-\xi)^{n-p}}{p(n-1)!}.
$$

このとき

$$
R_n = f^{(n)}(\xi)\frac{(x-\xi)^{n-p}(x-a)^p}{p(n-1)!}.
$$

これの $p=1$ の場合がCauchyの剰余項で, $p=n$ の場合がLagrangeの剰余項である.

以上は議論は, $f^{(n-1)}$ が $C^1$ 級の場合の積分を使った議論を, $f^{(n-1)}$ が微分可能なだけで使えるCauchyの平均値の定理に置き換えただけであるとも言える. 


## Taylor展開

$C^\infty$ 函数 $f$ について, ある $r>0$ が存在して, 

$$
f(x) = \sum_{k=0}^\infty \frac{1}{k!}f^{(k)}(a)(x-a)^k
$$

が $|x-a|<r$ で収束するとき $f$ は $x=a$ で**Taylor展開**可能であるという. そのような $r$ の最大値をTaylor展開の**収束半径**と呼ぶ. Taylor展開は**べき級数展開**と呼ばれることも多い.

$x=0$ におけるTaylor展開は**Maclaurin展開**と呼ばれることがある.


### 基本的なTaylor展開の例

以下のTaylor展開はよく使われる.

$$
\begin{aligned}
&
(1+x)^a = \sum_{k=0}^\infty\binom{a}{k}x^k \qquad(|x|<1), 
\qquad \binom{a}{k} = \frac{a(a-1)\cdots(a-k+1)}{k!}.
\\ &
(1-x)^{-a} = \sum_{k=0}^\infty\frac{(a)_k}{k!} x^k \qquad(|x|<1), 
\qquad (a)_k = a(a+1)\cdots(a+k-1).
\\ &
e^x = \sum_{k=0}^\infty \frac{x^k}{k!} \qquad(|x|<\infty).
\\ &
\cos x = \sum_{k=0}^\infty \frac{(-1)^k x^{2k}}{(2k)!} \qquad(|x|<\infty).
\\ &
\sin x = \sum_{k=0}^\infty \frac{(-1)^k x^{2k+1}}{(2k+1)!} \qquad(|x|<\infty).
\\ &
\cosh x = \frac{e^x+e^{-x}}{2} = \sum_{k=0}^\infty \frac{x^{2k}}{(2k)!} \qquad(|x|<\infty).
\\ &
\sinh x = \frac{e^x-e^{-x}}{2} = \sum_{k=0}^\infty \frac{x^{2k+1}}{(2k+1)!} \qquad(|x|<\infty).
\\ &
\log(1+x) = \sum_{k=1}^\infty \frac{(-1)^{k-1}x^k}{k}, \qquad (|x|<1).
\\ &
-\log(1-x) = \sum_{k=1}^\infty \frac{x^k}{k} \qquad (|x|<1).
\\ &
\arcsin x = \sum_{k=0}^\infty (-1)^k\binom{-1/2}{k}\frac{x^{2k+1}}{2k+1} =
\sum_{k=0}^\infty \frac{1}{2^{2k}}\frac{(2k)!}{(k!)^2}\frac{x^{2k+1}}{2k+1}
\qquad (|x|<1).
\\ &
\arctan x = \sum_{k=0}^\infty (-1)^k\frac{x^{2k+1}}{2k+1} 
\qquad (|x|<1).
\end{aligned}
$$

**問題:** 以上の公式をすべて示せ. $\QED$

解答は略す. まずは自分で $f(0)$, $f'(0)$, $f''(0)$, $f'''(0)$ を計算してみよう. そして, 個人的には最初の段階ではTaylor展開が収束するか否かにはあまり神経を使わなくてもよいと思う. 

他人が書いた解説に単に従うだけだと数学は決して理解できない. 自分で何を計算するべきであるかを考え, 自分が持っている力でその計算を実行してみて, その計算結果を見て次に何を計算するべきであるかを考え, また計算を繰り返す. 試行錯誤抜きに数学をまともに理解できるようになることはありえない.

しかし, 以下の説明も読んでおくこと.


**復習:** $\ds\binom{a}{k}$ は二項係数を表すのであった:

$$
\binom{a}{k} = \frac{a(a-1)\cdots(a-k+1)}{k!}.
$$

例えば

$$
\binom{a}{0}=1,\quad
\binom{a}{1}=a,\quad
\binom{a}{2}=\frac{a(a-1)}{2},\quad
\binom{a}{3}=\frac{a(a-1)(a-2)}{6}.
$$


$(a)_k = a(a+1)\cdots(a+k-1)$ もよく使われる記号法である. 例えば

$$
(a)_0 = 1, \quad
(a)_1 = a, \quad
(a)_2 = a(a+1), \quad
(a)_3 = a(a+1)(a+2).
$$


**参考:** 収束半径については複素解析によって以下が知られている. $x=a$ でTaylor展開可能な函数 $f(x)$ を $|z-a|<r$ を満たす複素数 $z$ の正則函数に拡張できるとき, そのTaylor展開の収束半径は $r$ 以上になる. だから, $f(x)$ を可能な限り解析接続したものを $f(z)$ と書くとき, $f$ の $x=a$ でのTaylor展開の収束半径は $a$ から $f(z)$ の正則ではない点までの最短距離に等しい.

例えば, $a$ が非負の整数以外であるとき, $f(x)=(1+x)^a = \exp(a\log(1+x))$ を複素平面に解析接続によって拡張すると, $x=0$ に最も近い $f$ の正則ではない点は $x=-1$ なので収束半径は $1$ になる. 例えば $a=-1$ のとき

$$
(1+x)^{-1} = \frac{1}{1+x} = 1 - x + x^2 - x^3 + \cdots
$$

と公比 $-x$ の等比級数が出て来るが, これが成立するのは $|x|<1$ においてである. $x=0$ から最も近い函数 $f(x)=1/(1+x)$ の特異点は $x=-1$ である.

同様の理由で $f(x)=\log(1+x)$ や $f(x)=-\log(1-x)$ の場合も $x=0$ でのTaylor展開の収束半径は $1$ になる.

例えば, $\exp x$, $\sin x$, $\cos x$ は複素平面全体上の正則函数に拡張できることと, それらのTaylor展開の収束半径が $\infty$ になることは同値になり, 実際にどちらも成立している.


**逆三角函数に関するヒント:** $\arcsin x$ と $\arctan x$ については

$$
\arcsin x = \int_0^x \frac{dt}{\sqrt{1-t^2}}, \quad
\arctan x = \int_0^x \frac{dt}{1+t^2}
$$

を用いてよい. さらに $|t|<1$ で

$$
\frac{1}{\sqrt{1-t^2}} = (1-t^2)^{-1/2} = \sum_{k=0}^\infty (-1)^k\binom{-1/2}{k}t^{2k}, \qquad
\frac{1}{1+t^2} = \sum_{k=0}^\infty (-1)^k t^{2k}.
$$

これを項別に積分すれば欲しい結果が得られる.

二項係数 $\ds\binom{-1/2}{k}$ の部分は以下のように処理できる:

$$
\begin{aligned}
(-1)^k\binom{-1/2}{k} &=
(-1)^k\frac{-\frac{1}{2}\left(-\frac{1}{2}-1\right)\cdots\left(-\frac{1}{2}-k+1\right)}{k!}
\\ &=
\frac{\frac{1}{2}\left(\frac{1}{2}+1\right)\cdots\left(\frac{1}{2}+k-1\right)}{k!} =
\frac{1\cdot 3\cdots(2k-1)}{2^k\;k!} 
\\ &=
\frac{1\cdot 3\cdots(2k-1)}{2^k\;k!} \frac{2\cdot 4\cdots(2k)}{2^k\;k!}
= \frac{1}{2^{2k}}\frac{(2k)!}{(k!)^2}.
\qquad\QED
\end{aligned}
$$


**問題:** $e^x$ と $\cos x$ と $\sin x$ の $x=0$ におけるTaylor展開が $x$ が複素数である場合にも成立していると仮定して, 

$$
e^{ix} = \cos x + i \sin x
$$

が成立していることを示せ. $\QED$

**解答例:**
$$
\begin{aligned}
e^{ix} &=
\sum_{n=0}^\infty \frac{i^n x^n}{n!} =
\sum_{k=0}^\infty \frac{i^{2k}x^{2k}}{(2k)!} + \sum_{k=0}^\infty \frac{i^{2k+1}x^{2k+1}}{(2k+1)!}
\\ &=
\sum_{k=0}^\infty \frac{(-1)^k x^{2k}}{(2k)!} + i\sum_{k=0}^\infty \frac{(-1)^k x^{2k+1}}{(2k+1)!} =
\cos x + i\sin x.
\end{aligned}
$$

1つ目の等号で $e^z$ の $z=0$ におけるTaylor展開を $z=ix$ に適用した. 2つ目の等号では和を $n$ が偶数の場合と奇数の場合に分けた. 3つ目の等号では $i^{2k}=(-1)^k$ を使った. 4つ目の等号では $\cos z$, $\sin z$ の $z=0$ におけるTaylor展開を $z=x$ に適用した.  $\QED$


**問題:** $\sqrt{10}$ を

$$
(1+x)^{1/2} = \sum_{k=0}^\infty\binom{1/2}{k}x^k =
1 + \frac{x}{2} - \frac{x^8}{8} + \frac{x^3}{16} + O(x^4)
\qquad (|x|<1)
$$

を用いて小数点以下第2桁まで求めよ.

**解答例:**

$$
\sqrt{10} = (9+1)^{1/2} = 3(1+1/9)^{1/2} =
3 + 3\frac{1}{2}\frac{1}{9} - 3\frac{1}{8}\frac{1}{9^2} + 3\frac{1}{16}\frac{1}{9^3} + \cdots.
$$

そして, 

$$
3\frac{1}{2}\frac{1}{9} = \frac{1}{6} = 0.166666\cdots, \quad
-3\frac{1}{8}\frac{1}{9^2} = -\frac{1}{216} = -0.004629\cdots, \quad
3\frac{1}{16}\frac{1}{9^3} = 0.000257\cdots, \quad
$$

より

$$
3 + 3\frac{1}{2}\frac{1}{9} - 3\frac{1}{8}\frac{1}{9^2} = 3.162\cdots
$$

$\sqrt{10}=3.16\cdots$ である. $\QED$


**注意:** 上では誤差項をきちんと評価せずに計算したがきちんと評価するには次のようにすればよい. 剰余項付きのTaylorの定理より, $0<x<1$ のとき ある実数 $\xi$ が存在して,

$$
\begin{aligned}
&
(1+x)^{1/2} = 1 + \frac{x}{2} - \frac{x}{8} + \frac{x^3}{16(1+\xi)^{5/2}}, \quad 0<\xi<x,
\\ &
0<\frac{x^3}{16(1+\xi)^{5/2}}<\frac{x^3}{16}.
\end{aligned}
$$

ゆえに, $x=1/9$ のとき, $\sqrt{10}$ と $3+1/6-1/216=3.162\cdots$ の差は

$$
3\frac{1}{16}\frac{1}{9^3} = 0.000257\cdots
$$

より小さい. このことに注意すれば本当に小数点以下第2桁まで正確に計算できているという数学的な証明が得られる. $\QED$

```julia
x = symbols("x")
series((1+x)^(Sym(1)/2), n=5)
```

```julia
3/2/9, -3/8/9^2, 3/16/9^3
```

```julia
3 + 3/2/9 -3/8/9^2, 3/16/9^3
```

√ は \sqrt TAB で入力できる.

https://docs.julialang.org/en/stable/manual/unicode-input/

```julia
√(10)
```

```julia
factorial(10)
```

**問題:** $\tan x$ の $x=0$ でのTaylor展開 $5$ 次の項まで求めよ.

**解答例:** $f(x)=\tan x$ とおくと $f'= 1 + \tan^2 x = 1 + f^2$ となので, 

$$
\begin{aligned}
&
f'' = 2ff' = 2f + 2f^3,
\\ &
f''' =  2f' + 6f^2f' = 2 + 8f^2 + 6f^4,
\\ &
f^{(4)} = 16ff' + 24f^3f' = 16f + 40f^3 + 24f^5,
\\ &
f^{(5)} = 16f' + 120f^2f' + 120f^4f' = 16 + 136f^2 + 240f^4 + 120f^6.
\end{aligned}
$$

ゆえに

$$
f(0)=f''(0)=f^{(4)}(0)=0, \quad
f'(0)=1, \quad f'''(0)=2, \quad f^{(5)}(0)=16. 
$$

したがって, 

$$
\tan x = f(x)
= x + \frac{2}{3!}x^3 + \frac{16}{5!}x^5 + \cdots
= x + \frac{1}{3}x^3 + \frac{2}{15}x^5 + \cdots.
\qquad\QED
$$

**別の解答例:** $\sin x$ と $\cos x$ のMaclaurin展開から $\tan x$ のMaclaurin展開を求めよう:

$$
\begin{aligned}
\tan x &= \frac{\sin x}{\cos x} = \frac{\sin x}{1-(1-\cos(x))}
\\ &=
\sin x\;(1 + (1-\cos x) + (1-\cos x)^2 + \cdots)
\\ &=
\left(x - \frac{x^3}{6} + \frac{x^5}{120} + O(x^7)\right)
\left(
1 + 
\left(\frac{x^2}{2} - \frac{x^4}{24}\right) +
\left(\frac{x^2}{2} - \frac{x^4}{24}\right)^2 + O(x^6)
\right)
\\ &=
\left(x - \frac{x^3}{6} + \frac{x^5}{120} + O(x^7)\right)
\left(
1 + \frac{x^2}{2} + \left(-\frac{1}{24}+\frac{1}{4}\right)x^4 + O(x^6)
\right)
\\ &=
\left(x - \frac{x^3}{6} + \frac{x^5}{120} + O(x^7)\right)
\left(
1 + \frac{x^2}{2} + \frac{5}{24}x^4 + O(x^6)
\right)
\\ &=
x + \left(\frac{1}{2}-\frac{1}{6}\right)x^3 + \left(\frac{5}{24}-\frac{1}{12}+\frac{1}{120}\right)x^5 + O(x^7)
\\ &=
x + \frac{1}{3}x^3 + \frac{2}{15}x^5 + O(x^7).
\qquad\QED
\end{aligned}
$$

```julia
x = symbols("x")
sympy.init_printing(order="rev-lex")
for k in 0:5
    expand(diff(tan(x), x, k)) |> display
end
sympy.init_printing(order="lex") # default

x = symbols("x")
series(tan(x), n=10)
```

```julia
s = series(sin(x), n=7)
cinv = series(1/cos(x), n=6)
display(s)
display(cinv)
expand(s * cinv)
```

**問題:** $\tanh x = (\sinh x)/(\cosh x)$ の $x=0$ でのTaylor展開 $5$ 次の項まで求めよ.

**解答例:** $f(x)=\tanh x$ とおくと $f'= 1 - \tanh^2 x = 1 - f^2$ となので, 

$$
\begin{aligned}
&
f'' = -2ff' = -2f + 2f^3,
\\ &
f''' = -2f' + 6f^2f' = -2 + 8f^2 - 6f^4,
\\ &
f^{(4)} = 16ff' - 24f^3f' = 16f - 40f^3 + 24f^5,
\\ &
f^{(5)} = 16f' - 120f^2f' + 120f^4f' = 16 - 136f^2 + 240f^4 - 120f^6.
\end{aligned}
$$

ゆえに

$$
f(0)=f''(0)=f^{(4)}(0)=0, \quad
f'(0)=1, \quad f'''(0)=-2, \quad f^{(5)}(0)=16. 
$$

したがって, 

$$
\tanh x = f(x)
= x - \frac{2}{3!}x^3 + \frac{16}{5!}x^5 + \cdots
= x - \frac{1}{3}x^3 + \frac{2}{15}x^5 + \cdots.
\qquad\QED
$$

```julia
x = symbols("x")
sympy.init_printing(order="rev-lex")
for k in 0:5
    expand(diff(tanh(x), x, k)) |> display
end
sympy.init_printing(order="lex") # default

x = symbols("x")
series(tanh(x), n=10)
```

```julia
s = series(sinh(x), n=7)
cinv = series(1/cosh(x), n=6)
display(s)
display(cinv)
expand(s * cinv)
```

**問題:** $e=e^1$ の値を $e^x$ のMaclaurin展開を用いて誤差100万の1以下で正確に計算するためには $\ds\sum_{k=0}^9 \frac{1}{k!}$ を計算すれば十分であることを示せ. $e<3$ と $10!=3628800$ を認めて使ってよい.

**解答例:** Taylorの定理より, $0$ と $1$ のあいだにある実数 $\xi$ で次を満たすものが存在する.

$$
e^1 = \sum_{k=0}^9\frac{1}{k!} + \frac{e^\xi}{10!}, \quad
\frac{e^\xi}{10!} < \frac{e}{10!} < \frac{3}{3000000} = \frac{1}{10^6}.
$$

不等式は $\xi<1$ と $e<3$, $10!>3000000$ より得られる. これで示すべきことが示された. $\QED$

Taylorの定理を使えば誤差項の大きさを上から評価できる.

```julia
sum(1/factorial(k) for k in 0:9), exp(1)
```

次の問題の解答例のように, Taylorの定理の剰余項を用いるのではなく, Taylor展開の残りの部分の総和を上から評価することによって誤差項を評価することもできる.

**問題:** $\log 2$ を小数点以下第2桁まで求めよ.

**解答例:** $|x|<1$ とし, 

$$
f(x) = \log \frac{1+x}{1-x} = \log(1+x)-\log(1-x) =
2\left(x + \frac{x^3}{3} + \frac{x^5}{5} + \cdots\right)
$$

とおくと, $0\leqq x<1$ のとき

$$
\begin{aligned}
&
f(x) = 2\sum_{k=0}^{n-1} \frac{x^{2k+1}}{2k+1} + R_{2n+1},
\\ &
0 \leqq
R_{2n+1} =
2\sum_{k=n}^\infty \frac{x^{2k+1}}{2k+1} =
\frac{2x^{2n+1}}{2n+1}\sum_{k=n}^\infty \frac{2n+1}{2k+1} x^{2k+1-(2n+1)}
\\ & \;\;\leqq
\frac{2x^{2n+1}}{2n+1}\sum_{j=0}^\infty x^{2j} =
\frac{2x^{2n+1}}{2n+1}\frac{1}{1-x^2}.
\end{aligned}
$$

ゆえに, $n=2$, $\ds x=\frac{1}{3}$ のとき, 

$$
\begin{aligned}
&
\log 2 = f\left(\frac{1}{3}\right) = 
f(x) = \alpha + R_5, 
\\ &
\alpha =  2\left(\frac{1}{3} + \frac{(1/3)^3}{3}\right) = 0.69135802\cdots, 
\\ &
0\leqq R_5\leqq \frac{9}{4}\frac{(1/3)^5}{5} = \frac{1}{540} = 0.00185185\cdots
\end{aligned}
$$

これより, $\log 2 = 0.69\cdots$ であることがわかる. $\QED$

**注意:** $\ds y = f(x) = \log\frac{1+x}{1-x}$ は $\ds x = \frac{e^y-1}{e^y+1} = \frac{e^{y/2}-e^{-y/2}}{e^{y/2}+e^{-y/2}} = \tanh\frac{y}{2}$ の逆函数である. すなわち

$$
\arctanh x =  \frac{1}{2}\log\frac{1+x}{1-x} = \sum_{k=0}^\infty \frac{x^{2k+1}}{2k+1}.
\qquad \QED
$$

**注意(70％ルール):** $\log 2 = 0.69314718\cdots$ が $0.7$ に近いことはよく以下のように使われる. 年利 $r$ パーセントの金利で資産を運用するとき, 約 $70/r$ 年後に資産は倍になる:

$$
\left(1+\frac{r}{100}\right)^{70/r} \approx 2.
$$

例えば $r$ が $5$ パーセントのとき $1.05^{70/5}=1.97993\cdots$. $r$ が $7$ パーセントのとき $1.07^{70/7}=1.96715\cdots$. 確かにほぼ $2$ になっている. このような形で $100\log 2\approx 70$ は我々の金融社会の中で便利に利用されている. $\QED$

**問題:** 70％ルールの概算法がうまく行く理由を説明せよ.

**解答例:** $\log 2 \approx 0.7$ と近似すると, $\ds\left(1+\frac{r}{100}\right)^x \approx e^{rx/100} = 2$ を満たす $x$ は $\ds x = \frac{100\log 2}{r} \approx \frac{70}{r}$ で概算可能である.  $\QED$

```julia
f(x) = log((1+x)/(1-x))
g(n,x) = 2*sum(k->x^(2k+1)/(2k+1), 0:n-1)
R(n,x) = 2*x^(2n+1)/(2n+1)/(1-x^2)

sympy.init_printing(order="rev-lex")
x = symbols("x", positive=true)
g(2, x) |> display
sympy.init_printing(order="lex") # default

n = 2
y = 2
x = (y-1)/(y+1)
log(y), f(x), g(n,x), f(x) - g(n,x), R(n,x)
```

```julia
f(x) = log((1+x)/(1-x))
g(n,x) = 2*sum(k->x^(2k+1)/(2k+1), 0:n-1)
R(n,x) = 2*x^(2n+1)/(2n+1)/(1-x^2)

n = 3
y = 2
x = (y-1)/(y+1)
log(y), f(x), g(n,x), f(x) - g(n,x), R(n,x)
```

**問題:** 少し上の方でやった計算と同様の方法で $\log 1.5$ を小数点以下第3桁まで計算せよ. $\QED$

```julia
f(x) = log((1+x)/(1-x))
g(n,x) = 2*sum(k->x^(2k+1)/(2k+1), 0:n-1)
R(n,x) = 2*x^(2n+1)/(2n+1)/(1-x^2)

n = 2
y = 1.5
x = (y-1)/(y+1)
log(y), f(x), g(n,x), f(x) - g(n,x), R(n,x)
```

```julia
f(x) = log((1+x)/(1-x))
g(n,x) = 2*sum(k->x^(2k+1)/(2k+1), 0:n-1)
R(n,x) = 2*x^(2n+1)/(2n+1)/(1-x^2)

n = 3
y = 1.5
x = (y-1)/(y+1)
log(y), f(x), g(n,x), f(x) - g(n,x), R(n,x)
```

**問題:** $\arcsin x$, $\arctan x$ の $x=0$ でのTaylor展開の $x\to 1$ の極限を取ることによって

$$
\frac{\pi}{2} = \sum_{k=0}^\infty\frac{1}{2^{2k}}\frac{(2k)!}{(k!)^2}\frac{1}{2k+1}, \quad
\frac{\pi}{4} = \sum_{k=0}^\infty\frac{(-1)^k}{2k+1}
$$

が得られることを確認せよ. $\QED$

解答略. 次の節におけるコンピューターによる計算例も参照せよ.


**問題:** $\R$ 上の $C^\infty$ 函数 $f(x)$ で, 任意の実数 $x$ について $\ds \sum_{n=0}^\infty \frac{1}{n!}f^{(n)}(0)x^n$ が収束しているのに, その収束先が $x\ne0$ のとき $f(x)$ と一致しないものが存在する. そのような $f(x)$ の例を1つ挙げよ.

**解答例:** $f(x)$ を

$$
f(x) = \begin{cases}
\exp(-1/|x|) & (x\ne 0) \\
0 & (x=0)
\end{cases}
$$

と定めると, $f$ は $C^\infty$ 函数でかつ $f^{(n)}(0)=0$ ($n=0,1,2,\ldots$) となることを示せる(自分で示してみよ). ゆえにそのとき, $\ds \sum_{n=0}^\infty \frac{1}{n!}f^{(n)}(0)x^n$ は常に $0$ に収束する. しかし, $x\ne 0$ のとき $f(x)>0$ なので収束先の $0$ は $f(x)$ に一致しない. $\QED$

```julia
f(x) = x == 0 ? zero(x) : exp(-1/abs(x))
x = -2:0.001:2
plot(x, f.(x), label="y = f(x)", size=(400,250), legend=:top)
```

### コンピューターを用いた計算例

私が個人的に知る限りの範囲内では, 手計算もきちんとできる人の方がコンピューターを上手に使っている場合が多い. おそらく, その人自身がコンピューターを頼らなくてもある程度の計算ができるおかげで, コンピューターを使った人間には不可能な計算の価値をよく理解できるからなのだろう.

```julia
x = symbols("x")
series(e^x, n=10)
```

```julia
series(cos(x), n=10)
```

```julia
series(sin(x), n=10)
```

```julia
series(cosh(x), n=10)
```

```julia
series(sinh(x), n=10)
```

```julia
series(log(1+x), n=10)
```

```julia
series(-log(1-x), n=10)
```

```julia
series(asin(x), n=10)
```

```julia
k = symbols("k", integer=true)
doit(sympy.Sum(1/2^(2k)*factorial(2k)/(factorial(k)^2)*x^(2k+1)/(2k+1), (k,0,4)))
```

```julia
series(atan(x), n=10)
```

```julia
series(atanh(x), n=10)
```

```julia
sympy.Sum((-1)^k/(2k+1), (k,0,oo))
```

```julia
doit(sympy.Sum((-1)^k/(2k+1), (k,0,oo)))
```

次のセルの f(n,k) は

$$
f(n,k) = \frac{1}{2^n}\binom{n}{k} = \frac{1}{2^n}\frac{n!}{k!(n-k)!}
$$

を意味している. これは $p=1/2$ に対する二項分布における確率である.

$$
\operatorname{lgamma}(n+1) = \log \Gamma(n+1) = \log n!
$$

であることに注意せよ. 

次のセルで一度全体の対数を取った式を計算してから exponential を取っている理由は階乗を単体で計算すると簡単にオーバーフローしてしまうからである. 階乗の対数ならばオーバーフローしない.  さらに, $\ds\binom{n}{k}$ と $1/2^n$ を別々に計算した後で掛け合わせる計算の仕方もよくない. 巨大な数を巨大な数で割る数値計算はオーバーフローやアンダーフローを起こしやすい. 対数を取って適度な大きさの数値にして差を取ってから, まとめて exponential するのがよい.

しかし, そのように注意深く書いたコードであっても, もとの級数の収束は極めて遅いので, 1億項を足し合わせても小数点以下第4桁までしか正確に計算できていない. 

このようなことは頻繁に起こるので, 数値計算により適した公式を見付けたり, 遅い収束を加速する方法(加速法)を開発することは大事な仕事である.

```julia
f(n,k) = exp(lgamma(n+1)-lgamma(k+1)-lgamma(n-k+1)-n*log(2))
@time sum(k->f(2k,k)/(2k+1), 0:10^8-1), π/2
```

## Bernoulli数とEuler数を用いたTaylor展開


### Bernoulli数とEuler数の定義

Bernoulli数 $B_n$ とEuler数 $E_n$ を次のように定める:

$$
\frac{z}{e^z-1} = \sum_{n=0}^\infty B_n \frac{z^n}{n!}, \quad
\frac{2}{e^z+e^{-z}} = \sum_{n=0}^\infty E_n \frac{z^n}{n!}.
$$


**注意:** これらの母函数はある確率分布のモーメント母函数分の1になっている:

$$
\int_0^1 e^{zx}\,dx = \frac{e^{z}-1}{z}, \quad
\int_\R e^{zx}\frac{\delta(x-1)+\delta(x+1)}{2}\,dx = \frac{e^{z}+e^{-z}}{2}.
$$

ここで $\delta(x-a)$ は<a href="https://www.google.co.jp/search?q=%E3%83%87%E3%83%AB%E3%82%BF%E5%87%BD%E6%95%B0">デルタ(超)函数</a>である:

$$
\int_\R \varphi(x)\delta(x-a)\,dx = \varphi(a).
$$

モーメント母函数は統計力学における分配函数に対応しており, モーメント母函数分の $e^{xz}$ はカノニカル分布の確率密度函数に対応している. このような視点からのBernoulli数およびBernoulli多項式の一般化については

* 黒木玄, <a href="https://genkuroki.github.io/documents/20170724EulerMaclaurin.pdf">Euler-Maclaurinの和公式の一般化</a>

を参照せよ.  $\QED$


**問題:** $B_0=1$, $B_1=-1/2$ であることを示せ.

**解答例:** 以下の計算から得られる:

$$
\frac{z}{e^z-1} = \frac{1}{1+z/2+O(z^2)} = 1-\frac{z}{2}+O(z^2).
\qquad\QED
$$


この問題の結果と,

$$
\frac{z}{e^z-1}+\frac{z}{2} = \frac{z}{2}\frac{e^z+1}{e^z-1} =
\frac{z}{2}\frac{e^{z/2}+e^{-z/2}}{e^{z/2}-e^{-z/2}}
$$

が偶函数になることより, $B_n$ は $n$ が3以上の奇数のとき $0$ になることがわかる.

$\ds\frac{2}{e^z+e^{-z}}$ が偶函数であることより, $n$ が奇数のとき $E_n=0$ となることがわかる.

後で次の公式を用いる:

$$
\frac{z}{2}\frac{e^{z/2}+e^{-z/2}}{e^{z/2}-e^{-z/2}} = 
\sum_{k=0}^\infty \frac{B_{2k}}{(2k)!}z^{2k},
\quad
\frac{2}{e^z+e^{-z}} =
\sum_{k=0}^\infty \frac{E_{2k}}{(2k)!}z^{2k}.
\tag{$*$}
$$


**問題:** Bernoulli数 $B_n$ とEuler数 $E_{2n}$ が次の漸化式を満たすことを示せ: $n=1,2,3,\ldots$ に対して,

$$
B_n = -\frac{1}{n+1}\sum_{k=0}^{n-1}\binom{n+1}{k}B_k, \quad
E_{2n} = -\sum_{k=0}^{n-1} \binom{2n}{2k}E_{2k}.
$$

**注意:** これらの漸化式と $B_0=E_0=1$ を用いて, Bernoulli数 $B_n$ とEuler数 $E_{2n}$ を順次求めることができる. $\QED$

**解答例:** $B_n$ の漸化式は $\ds\frac{z}{e^z-1}\frac{e^z-1}{z}=1$ から得られる.

$$
1 = \frac{z}{e^z-1}\frac{e^z-1}{z} =
\sum_{k,l=0}^\infty \frac{B_k}{k!(l+1)!}z^{k+l} =
\sum_{n=0}^\infty \left(\sum_{k=0}^n \frac{n!}{k!(n-k+1)!}B_k\right)\frac{z^n}{n!}.
$$

すなわち

$$
\delta_{n,0} =
\sum_{k=0}^n \frac{n!}{k!(n-k+1)!}B_k =
\frac{1}{n+1}\sum_{k=0}^n \binom{n+1}{k}B_k =
\frac{1}{n+1}\sum_{k=0}^{n-1} \binom{n+1}{k}B_k + B_n. 
$$

これより $B_n$ の漸化式が得られる. 

$E_{2n}$ の漸化式は $\ds\frac{2}{e^z-e^{-z}}\frac{e^z-e^{-z}}{2}=1$ から得られる.

$$
1 = \frac{2}{e^z+e^{-z}}\frac{e^z+e^{-z}}{2} =
\sum_{k,l=0}^\infty \frac{E_{2k}}{(2k)!(2l)!}z^{2k+2l} =
\sum_{n=0}^\infty \left(\sum_{k=0}^n \binom{2n}{2k}E_{2k}\right)\frac{z^{2n}}{(2n)!}.
$$

すなわち

$$
\delta_{n,0} = \sum_{k=0}^n \binom{2n}{2k}E_{2k} =
\sum_{k=0}^{n-1} \binom{2n}{2k}E_{2k} + E_{2n}.
$$

これより $E_{2n}$ の漸化式が得られる. $\QED$

```julia
# Bernoulli数 B_{2k} の例

BernoulliNumber(n) = sympy.bernoulli(n)
B = [BernoulliNumber(2k) for k in 0:8]
```

```julia
# Euler数 E_{2k} の例

EulerNumber(n) = sympy.euler(n)
E = [EulerNumber(2k) for k in 0:8]
```

### tan, cot, sec, cosec の展開

$B_{2k}$, $E_{2k}$ に関する公式($*$)を使うと, 

$$
\begin{aligned}
&
\frac{z}{2}\cot\frac{z}{2} = \frac{z}{2}\frac{\cos(z/2)}{\sin(z/2)} =
\frac{iz}{2}\frac{e^{iz/2}+e^{-iz/2}}{e^{iz/2}-e^{-iz/2}} =
\sum_{k=0}^\infty \frac{B_{2k}}{(2k)!}(iz)^{2k} =
\sum_{k=0}^\infty (-1)^k\frac{B_{2k}}{(2k)!}z^{2k},
\\ &
\cot z = \frac{\cos z}{\sin z} = \sum_{k=0}^\infty (-1)^k \frac{2^{2k}B_{2k}}{(2k)!}z^{2k-1},
\\ &
\tan z = \frac{\sin z}{\cos z} = \cot z - 2\cot(2z) = 
\sum_{k=1}^\infty(-1)^k\frac{2^{2k}(1-2^{2k})B_{2k}}{(2k)!}z^{2k-1},
\\ &
\cosec z = \frac{1}{\sin z} =
\cot\frac{z}{2} - \cot z =
\sum_{k=0}^\infty(-1)^k\frac{2^{2k}(2^{-(2k-1)}-1)B_{2k}}{(2k)!}z^{2k-1}
\\ & \qquad =
\sum_{k=0}^\infty(-1)^k\frac{2(1-2^{2k-1})B_{2k}}{(2k)!}z^{2k-1},
\\ &
\sec z = \frac{1}{\cos z} = \frac{2}{e^{iz}-e^{-iz}} = 
\sum_{k=0}^\infty \frac{E_{2k}}{(2k)!}(iz)^{2k} =
\sum_{k=0}^\infty (-1)^k\frac{E_{2k}}{(2k)!}z^{2k},
\\ &
\coth z = \frac{e^{z}+e^{-z}}{e^{z}-e^{-z}} = 
\sum_{k=0}^\infty \frac{2^{2k}B_{2k}}{(2k)!}z^{2k-1}
\\ &
\tanh z = 2\coth(2z) - \coth z =
\sum_{k=0}^\infty \frac{2^{2k}(2^{2k}-1)B_{2k}}{(2k)!} z^{2k-1}.
\end{aligned}
$$

__注意:__ $B_{2k}$ は本質的に $\ds\frac{z}{2}\cot\frac{z}{2}$ のMaclaurin展開の係数であり, $E_{2k}$ は $\sec z$ のMaclaurin展開の係数である.

__注意:__ $\tan z = \cot z - 2\cot(2z)$ は次のように示される:

$$
2\cot(2z) = \frac{2\cos 2z}{\sin 2z} =
\frac{2(\cos^2 z - \sin^2 z)}{2\cos z \sin z} = \cot z - \tan z.
$$

ゆえに $\tan z = \cot z - 2\cot(2z)$.  $\tanh z = 2\coth(2z) - \coth z$ は次のように示される(わざと少し計算の仕方を $\tan$ の場合から変える):

$$
2\coth(2z) = \frac{2(e^{2z} + e^{-2z})}{e^{2z} - e^{-2z}} =
\frac{(e^z+e^z)^2 + (e^z-e^{-z})^2}{(e^z+e^{-z})(e^z-e^{-z})} = \coth z + \tanh z.
$$

ゆえに, $\tanh z = 2\coth(2z) - \coth z$.

```julia
z = symbols("z")
series((z/2)*cot(z/2), z, n=10)
```

```julia
z = symbols("z")
series(cot(z), z, n=10)
```

```julia
z = symbols("z")
series(tan(z), z, n=10)
```

```julia
z = symbols("z")
series(csc(z), z, n=10)
```

```julia
z = symbols("z")
series(sec(z), z, n=10)
```

## べき級数の収束


### べき級数の定義

$\ds \sum_{k=0}^\infty a_k x^k$ の形式の級数を**べき級数**と呼ぶ.

$\ds \sum_{k=0}^\infty |a_k x^k|$ が収束するとき, べき級数 $\ds \sum_{k=0}^\infty a_k x^k$ は絶対収束するという.


### べき級数の収束半径


**準備:** 実数列 $\alpha_n$ が $n\to\infty$ のとき $\alpha_n\to\infty$ となることを, 任意の実数 $M$ に対して(どんなに大きな実数 $M$ に対しても), ある番号 $N$ が存在して $n\geqq N$ ならば $\alpha_n\geqq M$ となること(すなわち, どんなに大きな $M$ に対しても, ある番号以降のすべての $n$ について $\alpha_n\geqq M$ 成立すること)であると定める. $\QED$


####  d'Alembert の判定法

**定理( d'Alembert の判定法):** $k\to\infty$ のとき $\ds\frac{|a_k|}{|a_{k+1}|}\to\rho > 0$ が成立していると仮定する(上の準備のもとで $\rho=\infty$ であってもよいとする). このとき, べき級数 $\ds\sum_{k=0}^\infty a_k x^k$ は $|x|<\rho$ で絶対収束し, $|x|>\rho$ で収束しない.

**注意:** 実際には $0<r<\rho$ のとき $|x|\leqq r$ でべき級数 $\ds\sum_{k=0}^\infty a_k x^k$ が一様絶対収束することも示される.

**注意:** このような $\rho$ をべき級数 $\ds\sum_{k=0}^\infty a_k x^k$ の**収束半径**と呼ぶ.

**証明:** $0\leqq r<\rho$ と仮定する. $r$ は幾らでも $\rho$ に近付き得るので, $|x|<\rho$ のときべき級数が絶対収束することを示すためには, $|x|\leqq r$ のときべき級数が絶対収束することを示せば十分である. $r<R<\rho$ を満たす実数 $R$ が存在する.  $|x|\leqq r$ と仮定する. $|a_k|/|a_{k+1}|\to\rho>R>0$ より, ある番号 $N$ が存在して, $k\geqq N$ のとき,

$$
\frac{|a_k|}{|a_{k+1}|} \geqq R, \quad\text{i.e.}\quad \frac{|a_{k+1}|}{|a_k|}\leqq \frac{1}{R}.
$$

ゆえに, $n=0,1,2,\ldots$ に対して, 

$$
|a_{N+n} x^{N+n}| =
\frac{|a_{N+n}|}{|a_{N+n-1}|}\cdots\frac{|a_{N+2}|}{|a_{N+1}|}\frac{|a_{N+1}|}{|a_N|}|a_N||x|^{N+n} \leqq
\frac{1}{R^n} |a_N| r^{N+n} =
|a_N| r^N \left(\frac{r}{R}\right)^n.
$$

$r<R$ という仮定より, $r/R<1$ なので, 右辺の $n=0,1,2,\ldots$ に関する和は有限の値に収束する. ゆえに左辺の同様の和も有限の値に収束する. これで, べき級数 $\ds \sum_{k=0}^\infty a_k x^k = \sum_{k=0}^{N-1} a_k x^k + \sum_{n=0}^\infty a_{N+n}x^{N+n}$ が絶対収束することが示された. (**注意:** 以上の議論によって $|x|\leqq r$ で**一様**絶対収束することも示されている.)

$|x|>\rho$ であると仮定する. $\ds\sum_{k=0}^\infty a_k x^k$ が収束しないことを示したい. $|x|>\rho$ と $|a_k|/|a_{k+1}|\to\rho<|x|$ より, ある番号 $N$ が存在して, $k\geqq N$ ならば, $|a_k|>0$ かつ

$$
\frac{|a_k|}{|a_{k+1}|} \leqq |x|, \quad\text{i.e.}\quad \frac{|a_{k+1}|}{|a_k|} \geqq \frac{1}{|x|}.
$$

ゆえに, $n=0,1,2,\ldots$ に対して,

$$
|a_{N+n}x^{N+n}| = 
\frac{|a_{N+n}|}{|a_{N+n-1}|}\cdots\frac{|a_{N+2}|}{|a_{N+1}|}\frac{|a_{N+1}|}{|a_N|}|a_N||x|^{N+n} \geqq
\frac{1}{|x|^n}|a_N||x|^{N+n} =
|a_N||x|^N > 0.
$$

ゆえに $a_n x^n$ は $0$ に収束しない. これより, $\ds\sum_{k=0}^\infty a_k x^k$ が収束しないことがわかる. $\QED$


**例:** べき級数 $\ds \sum_{k=0}^\infty \frac{1}{k!}x^k$ について, $k\to\infty$ のとき, 

$$
\frac{1/k!}{1/(k+1)!} = k+1 \to \infty
$$

なので, そのべき級数は $|x|<\infty$ で絶対収束する. $\QED$


**例:** べき級数 $\ds\sum_{k=0}^\infty \frac{(-1)^k x^{2k}}{(2k)!}$ を $x^2$ のべき級数とみなす.

$$
\frac{1/(2k)!}{1/(2(k+1))!} = (2k+1)(2k+2)\to \infty \quad(k\to\infty)
$$

なので, そのべき級数は $|x|<\infty$ で絶対収束する.



**問題:** べき級数 $\ds\sum_{k=0}^\infty \frac{(-1)^k x^{2k+1}}{(2k+1)!}$ が $|x|<\infty$ で絶対収束することを示せ.

**ヒント:** このべき級数を $x^2$ のべき級数と $x$ の積とみなしてから, d'Alembertの判定法を適用せよ. $\QED$


d'Alembertの判定法だけで十分な場合が多いが, より精密なCauchy-Hadamard定理の定理もよく使われる.


#### Cauchy-Hadamard定理

**Cauchy-Hadamard定理:** $\rho\geqq 0$ を $\ds \frac{1}{\rho}=\limsup_{n\to\infty}|a_n|^{1/n}$ によって定める(右辺が $0$ のとき $\rho=\infty$ と定め, 右辺が $\infty$ のとき $\rho=0$ と定める.). このとき, べき級数 $\ds\sum_{k=0}^\infty a_k x^k$ は $|x|<\rho$ で絶対収束し, $|x|>\rho$ で収束しない.

**注意:** 実際には $0<r<\rho$ のとき $|x|\leqq r$ でべき級数 $\ds\sum_{k=0}^\infty a_k x^k$ が一様絶対収束することも示される.

**注意:** Cauchy-Hadamardの定理はべき級数 $\ds\sum_{k=0}^\infty a_k x^k$ の**収束半径** $\rho$ が $\ds \frac{1}{\rho}=\limsup_{n\to\infty}|a_n|^{1/n}$ で得られることを意味している.

**証明:** $0<r<\rho$ と仮定する. $r<R<\rho$ を満たす実数 $R$ が存在する. $\ds \limsup_{n\to\infty}|a_n|^{1/n}=\frac{1}{\rho}<\frac{1}{R}$ より, ある番号 $N$ が存在し, $n\geqq N$ ならば $\ds \sup_{k\geqq n}|a_k|^{1/k}\leqq \frac{1}{R}$ となる. 特に $k\geqq N$ ならば $\ds |a_k|^{1/k}\leqq \frac{1}{R}$ となる. したがって, $|x|\leqq r$ のとき, $k\geqq N$ ならば

$$
|a_k x^k| = \left(|a_k|^{1/k} |x|\right)^k\leqq \left(\frac{r}{R}\right)^k
$$

となる. $|r/R|<1$ なので, $|x|\leqq r$ で $\ds \sum_{k=0}^\infty a_k x^k$ が一様絶対収束することがわかった.

$|x|>\rho$ と仮定する. このとき, 

$$
\frac{1}{\rho} = \limsup_{n\to\infty}|a_n|^{1/n}=\inf_{n\geqq 0}\sup_{k\geqq n}|a_k|^{1/k}
$$

より, $n=0,1,2,\ldots$ に対して $\ds\sup_{k\geqq n}|a_k|^{1/k}\geqq\frac{1}{\rho}$ なので, $\ds \frac{1}{\rho} > \frac{1}{|x|}$ より, 各 $n$ ごとにある $k\geqq n$ で $\ds |a_k|^{1/k} \geqq \frac{1}{|x|}$ を満たすものが存在し, 

$$
|a_k x^k| = \left(|a_k|^{1/k}|x|\right)^k \geqq \left(\frac{1}{|x|}|x|\right)^k = 1
$$

となっている. 数列 $a_k x^k$ は $0$ に収束しないことがわかった. ゆえに級数 $\ds\sum_{k=0}^\infty a_k x^k$ も収束しない.  $\QED$

**注意:** Cauchy-Hadamardの定理の証明はシンプルなので上極限の使い方を学ぶために適切な題材になっている. $\QED$

**注意:**  d'Alembertの判定法もCauchy-Hadamardの定理も一般のべき級数を等比級数と比較することによって収束を判定している. 大雑把に言えば, d'Alembertの判定法は

$$
|a_k x^k| = |a_0| \prod_{j=1}^k \frac{|a_j||x|}{|a_{j-1}|}
$$

の $\ds \prod_{j=1}^k \frac{|a_j||x|}{|a_{j-1}|}$ の部分が $1$ 未満のある $r\geqq 0$ に対する $r^k$ 以下であれば収束すると考える判定法であり, Cauchy-Hadamardの定理は

$$
|a_k x^k| = \left(|a_k|^{1/k}|x|\right)^k
$$

が $1$ 未満のある $r\geqq 0$ に対する $r^k$ 以下であれば収束すると考える判定法である. $\QED$


**例:** べき級数

$$
1 - x + x^2 - x^3 + x^4 - x^5 + \cdots
$$

の収束半径は数列 $|-1|^{1/1}=1$, $|1|^{1/2}=1$, $|-1|^{1/3}=1$, $|1|^{1/4}=1$, $\ldots$ の上極限の $1$ であり, $|x|<1$ で $\ds\frac{1}{1+x}$ に収束するので, このべき級数は $x\nearrow 1$ で $1$ に収束する.

それではべき級数

$$
F(x) = \sum_{n=0}^\infty (-1)^n x^{2^n} = x - x^2 + x^4 - x^8 + x^{16} - x^{32} + \cdots
$$

についてはどうなるだろうか? これの収束半径も数列 $|1|^{1/1}=1$, $|-1|^{1/2}=1$, $|1|^{1/4}=1$, $|-1|^{1/8}=1$, $\ldots$ の上極限の $1$ になる. 

もしも $x\nearrow 1$ で $f(x)$ が $\alpha$ に収束するならば, $F(x^2) = x - F(x)$ なので, $x\nearrow 1$ の極限を取って $\alpha=1-\alpha$ すなわち $\ds\alpha=\frac{1}{2}$ となる. 

そこで実際に $x\nearrow 1$ のとき $F(x)\to 1/2$ となるかどうかを確認するために1000項足した結果を計算してみると, 例えば
    
    F(0.9) = 0.4677755990574414
    F(0.99) = 0.49409849522830906
    F(0.999) = 0.5001242215513184
    F(0.9999) = 0.5020251448564931
    F(0.99999) = 0.4973598550256996
    F(0.999999) = 0.5007394876031053

を得る. これらは確かに $\ds\frac{1}{2}=0.5$ にかなり近いように見える.

**しかし, べき級数の $F(x)$ の値は $x\nearrow 1$ で収束しない!!!** この事実のTauber型定理を使った証明については

* 黒木玄, <a href="https://genkuroki.github.io/documents/20160501StirlingFormula.pdf">ガンマ分布の中心極限定理とStirlingの公式</a>

の第10.4節を見よ. 数値計算結果を用いた初等的な証明を下の方で紹介する. $\QED$

```julia
# 上のセルのべき級数の数値計算例

F(x; N=10^3) = sum(n->(-1)^n*x^(2.0^n), 0:N)
@show F(0.9)
@show F(0.99)
@show F(0.999)
@show F(0.9999)
@show F(0.99999)
@show F(0.999999);
```

```julia
# 上の f(x) が x→1 で収束しないことの数値的確認

F(x; N=10^3) = sum(n->(-1)^n*x^(2.0^n), 0:N)
x = 0:0.001:0.999
P1 = plot(x, F.(x), label="F(x)", ylims=(0,0.5), xlims=(0,1))
x = 0.999:0.000001:0.999999
P2 = plot(x, F.(x), label="F(x)", ylims=(0.497, 0.503), xlims=(0.999,1))
plot(P1, P2, size=(600, 240), legend=:topleft)
```

### 収束円盤の境界上でのべき級数の収束


#### 準備: Cesàro総和可能性

Cesàro総和可能性から収束円盤の境界上での収束性(後で説明するところのAbel総和可能性)が導かれるので, Cesàro総和可能性について準備しておこう.

**定義:** 級数 $\ds\sum_{k=0}^\infty a_k$ が**Cesàro総和可能**(Cesàro summable)であるとは, その部分和 $\ds s_n = \sum_{k=0}^n a_k$ の加法平均

$$
\frac{s_0+s_1+\cdots+s_n}{n+1}
$$

が $n\to\infty$ で収束することであると定める. その収束先を**Cesàro和**(Cesàro sum)と呼ぶ. $\QED$

**問題:** $\ds s_n = \sum_{k=0}^n a_k$, $\ds t_n = \sum_{k=0}^n s_k$ とおく. 級数 $\ds\sum_{k=0}^\infty a_k$ がCesàro総和可能ならば $a_n=O(n)$, $s_n=O(n)$, $t_n=O(n)$ となることを示せ.

**解答例:** $\ds\sum_{k=0}^\infty a_k$ のCesàro総和可能性は $\ds\frac{t_n}{n+1}$ の収束性と同値である. ゆえに, 収束数列が有界であることより, ある $M>0$ が存在して, すべての番号 $n$ について

$$
|t_n|\leqq M(n+1)
$$

となることが導かれる. そのとき, 

$$
\begin{aligned}
&
|s_n|=|t_n-t_{n-1}|\leqq |t_n|+|t_{n-1}|\leqq M(n+1)+Mn = M(2n+1), \quad
\\ &
|a_n|=|s_n-s_{n-1}|\leqq |s_n|+|s_{n-1}|\leqq M(2n+1)+M(2n-1)=4Mn.
\end{aligned}
$$

これで $a_n=O(n)$, $s_n=O(n)$, $t_n=O(n)$ であることがわかった. $\QED$

**注意:** 特に級数 $\ds\sum_{k=0}^\infty a_k$ がCesàro総和可能ならばべき級数 $\ds\sum_{k=0}^\infty a_k z^k$, $\ds\sum_{k=0}^\infty s_k z^k$, $\ds\sum_{k=0}^\infty t_k z^k$ の収束半径は $1$ 以上になる. $\QED$


**問題:** 級数 $\ds\sum_{n=0}^\infty a_n$ が $\alpha$ に収束するとき, 級数 $\ds\sum_{n=0}^\infty a_n$ はCesàro総和可能でかつそのCesàro和は $\alpha$ に一致することを示せ.

**解答例:** $\ds s_n = \sum_{k=0}^n a_k$ とおく. $s_n\to\alpha$ ならば $\ds\frac{s_0+s_1+\cdots+s_n}{n+1}\to \alpha$ となることを示せばよい. しかし, その事実はノート「01 収束」ですでに示している. $\eps$-$N$ 論法の応用の典型例であり, $\eps$-$N$ 論法が使える人にとって証明は容易である. $\QED$

**問題:** 級数 $\ds\sum_{k=0}^\infty (-1)^k$ は発散するが, Cesàro総和可能であり, そのCesàro和は $\ds\frac{1}{2}$ になることを示せ.

**解答例:** 一般に級数 $\ds\sum_{k=0}^\infty a_k$ が収束していれば $a_n\to 0$ となるが, $(-1)^n$ は収束しないので $\ds\sum_{k=0}^\infty (-1)^k$ は収束していない(すなわち発散している). $\ds s_n=\sum_{k=0}^n a_k$ とおくと, $n$ が偶数ならば $s_n=1$ となり, 奇数ならば $s_n=0$ となる. このことから, $\ds\frac{1}{n+1}\sum_{k=0}^n s_k$ は $n$ 以下の偶数の割合に一致するので, $n\to\infty$ で $\ds\frac{1}{2}$ に収束する. $\QED$

**問題:** $\quad\ds\sum_{j=1}^n (-1)^{j-1}j^2 = (-1)^{n-1}\frac{n(n+1)}{2}$ を示せ.

**解答例:** 数学的帰納法. $n=0$ のとき両辺は $0$ になり等しい. $n\geqq 0$ でその等式が成立していると仮定すると,

$$
\begin{aligned}
\sum_{j=1}^{n+1} (-1)^{j-1}j^2 &= 
(-1)^{n-1}\frac{n(n+1)}{2} + (-1)^n (n+1)^2 
\\ &=
(-1)^n\frac{n+1}{2}(-n+2(n+1)) =
(-1)^n\frac{(n+1)(n+2)}{2}.
\end{aligned}
$$

ゆえに数学的帰納法より任意の $n\geqq 0$ について示したい等式は成立している. $\QED$

```julia
j, n = symbols("j n", integer=true)
doit(sympy.Sum((-1)^(j-1)*j^2, (j,1,n)))
```

**問題:** $n$ が平方数ならば $a_n=1$ でそれ以外ならば $a_n=0$ であるとする. このとき, 級数 $\ds\sum_{k=0}^\infty (-1)^k a_k$ は発散するが, Cesàro総和可能であり, そのCesàro和は $\ds\frac{1}{2}$ になることを示せ.

**解答例:** $\ds s_n = \sum_{k=0}^n (-1)^k a_k$ とおく. 整数 $j$ について $(-1)^{j^2}=(-1)^j$ なので $\ds s_n = \sum_{0\leqq j\leqq\sqrt{n}} (-1)^j$ となる. これより, $s_n$ は $\sqrt{n}$ 以下の最大の整数が偶数であれば $1$ になり, 奇数ならば $0$ になる. ゆえに $s_n$ 自身は収束しない. $\sqrt{n}$ 以下の最大の整数が $m$ になることと $m^2\leqq n< (m+1)^2$ は同値である. $\ds \sigma_n = \frac{1}{n+1}\sum_{k=0}^n s_k$ とおく. $\ds\sigma_n\to \frac{1}{2}$ を示せばよい. 偶数 $m$ に対する $m^2\leqq n<(m+1)^2$ で $\sigma_{n-1}<\sigma_n$ となり, 奇数 $m$ に対する $m^2\leqq n<(m+1)^2$ で $\sigma_{n-1}<\sigma_n$ となる. ゆえに, $\sigma_n$ は偶数 $m$ に対する $n=(m+1)^2-1$ で極大値

$$
\sigma_n = \frac{1}{(m+1)^2}\sum_{j=1}^{m+1}(-1)^{j-1}j^2 = \frac{1}{2}\frac{m+2}{m+1}
$$

になり, 奇数 $m$ に対する $n=(m+1)^2-1$ で極小値

$$
\sigma_n = \frac{1}{(m+1)^2}\sum_{j=1}^{m}(-1)^{j-1}j^2 = \frac{1}{2}\frac{m}{m+1}
$$

になることがわかる. これより, $\ds \sigma_n\to\frac{1}{2}$ であることがわかる. $\QED$

```julia
aa(n) = n == floor(typeof(n), √n)^2 ? one(n) : zero(n)
ss(n) = sum(k->(-1)^k*aa(k), 0:n)
tt(n) = sum(k->ss(k), 0:n)
sigma(n) = tt(n)/(n+1)
N = 1000
n = 0:N
plot(size=(640, 180), xlim=(-0.005*N, 1.005*N))
@time plot!(n, sigma.(n), label="sigma_n")
hline!([1/2], label="1/2", ls=:dash)
```

**問題:** $n$ が $2$ のべきで表わされる整数ならば $a_n=1$ でそれ以外ならば $a_n=0$ であるとする. このとき, 級数

$$
\sum_{k=0}^\infty (-1)^{\log_2 k} a_k =
0+1-1+0+1+0+0+0+0+0+0+0+0-1+\cdots
$$

はCesàro総和**不可能**であることを示せ.

**解答例:** $\ds s_n=\sum_{k=0}^n (-1)^{\log_2 k} a_k$, $\ds \sigma_n=\frac{1}{n+1}\sum_{k=0}^n s_k$ とおく. $s_n$ は $2^m\leqq n < 2^{m+1}-1$ のとき $m$ が偶数なら $1$ になり, 奇数なら $0$ になる. ゆえに $\sigma_n$ は偶数 $m$ に対する $n=2^{m+1}-1$ で極大値

$$
\sigma_n = \frac{1}{2^{m+1}}\sum_{k=0}^{m+1}(-1)^{k-1}2^k = \frac{2^{m+2}-1}{3\cdot 2^{m+1}}
$$

になり, 奇数 $m$ に対する $n=2^{m+1}-1$ で極小値

$$
\sigma_n = \frac{1}{2^{m+1}}\sum_{k=0}^{m}(-1)^{k-1}2^k = \frac{2^{m-2}-1}{3\cdot 2^{m+1}}
$$

になる. これより, $m$ を偶数のまま $m\to\infty$ とすると $\ds\sigma_{2^{m+1}-1}\to\frac{2}{3}$ となり, $m$ を奇数のまま $m\to\infty$ とすると $\ds\sigma_{2^{m+1}-1}\to\frac{1}{3}$ となる. これより, $\sigma_n$ が収束しないことがわかる. これで級数 $\sum_{k=0}^\infty (-1)^{\log_2 k} a_k$ はCesàro総和不可能であることが分かった.  $\QED$


#### Abel総和可能性

**定義:** 級数 $\ds\sum_{k=0}^\infty a_k$ が**Abel総和可能**(Abel summable)であるとは

$$
f(x) = \sum_{k=0}^\infty a_k x^k
$$

の右辺のべき級数の収束半径が $1$ 以上でかつ $x\nearrow 0$ のとき $f(x)$ が収束することであると定める. $f(x)$ の $x\nearrow 1$ での収束先を**Abel和**(Abel sum)と呼ぶ. $\QED$

**問題:** 級数 $\ds\sum_{n=0}^\infty a_n$ が $\alpha$ に収束するとき, 級数 $\ds\sum_{n=0}^\infty a_n$ はAbel総和可能でかつそのAbel和は $\alpha$ に一致することを示せ.

**解答例:** $\ds\sum_{n=0}^\infty a_n$ が収束することより, 特に $a_n\to 0$ なので, べき級数 $\ds\sum_{k=0}^\infty a_k x^k$ の収束半径は $1$ 以上であることがわかる. $|x|<1$ を満たす複素数 $z$ に対して $f(z) = \ds\sum_{k=0}^\infty a_k z^k$ とおく. $\ds s_n=\sum_{k=0}^n a_k$, $s_{-1}=0$ とおく. $s_n$ は収束するので特に有界である. このとき, $a_k=s_k-s_{k-1}$ を代入すると, 

$$
\sum_{k=0}^n a_k z^k = s_n z^n + (1-z)\sum_{k=0}^{n-1} s_k z^k.
$$

$|z|<1$ のとき $s_n$ の有界性より, $s_n z^n\to 0$ となるので

$$
f(z) = (1-z)\sum_{k=0}^\infty s_k z^k.
$$

これと

$$
\alpha = (1-z)\frac{\alpha}{1-z} = (1-z)\sum_{k=0}^\infty \alpha z^k
$$

の差は任意の番号 $N$ について

$$
f(z) - \alpha = (1-z)\sum_{k=0}^\infty (s_k-\alpha)z^k =
(1-z)\sum_{k=0}^{N-1} (s_k-\alpha)z^k + (1-z)\sum_{k=N}^\infty (s_k-\alpha)z^k
$$

となる. $R\geqq 1$, $\ds\frac{|1-z|}{1-|z|}\leqq R$ と仮定する. 任意に $\eps>0$ を取る. $s_k\to\alpha$ なのである番号 $N$ が存在して $k\geqq N$ ならば $\ds|s_k-\alpha|\leqq \frac{\eps}{2R}$ となる. ゆえにそのとき

$$
\left|(1-z)\sum_{k=N}^\infty (s_k-\alpha)z^k\right|\leqq
|1-z|\sum_{k=N}^\infty\eps|z|^k\leqq
\frac{\eps}{2R}\frac{|1-z|}{1-|z|} \leqq
\frac{\eps}{2}.
$$

$s_k-\alpha$ は有界なので $k$ によらない定数 $M>0$ が存在して $|s_k-\alpha|\leqq M$ となるので, $\ds\frac|1-z|\leqq\frac{\eps}{2MN}$ とすると,

$$
\left|(1-z)\sum_{k=0}^{N-1} (s_k-\alpha)z^k\right| = |1-z|MN\leqq \frac{\eps}{2}. 
$$

このとき,

$$
|f(z) - \alpha| \leqq  \frac{\eps}{2} + \frac{\eps}{2} = \eps.
$$

これで, ある $R\geqq 1$ について, $z$ が $|z|<1$ かつ $\ds\frac{|1-z|}{1-|z|}\leqq R$ を満たしながら $1$ に近付くとき, $f(z)$ が $\alpha$ に収束することが示された. 特に $x$ が区間 $[0,1)$ の上を動きながら $1$ に近付くならば $f(x)$ は $\alpha$ に収束する. これで, Abel総和可能性とAbel和が $\alpha$ に等しくなることが示された. $\QED$

**注意:** 上の問題の結果は**Abelの連続性定理**と呼ばれることがある. $|z|<1$ かつ $\ds\frac{|1-z|}{1-|z|}\leqq R$ を満たす $z$ 達の領域は**Stolz領域**のように呼ばれることがあるらしい. $\QED$

**問題:** 級数 $\ds\sum_{n=0}^\infty a_n$ がCesàro総和可能でかつそのCesàro和が $\alpha$ になるとき, 級数 $\ds\sum_{n=0}^\infty a_n$ はAbel総和可能でかつそのAbel和は $\alpha$ に一致することを示せ.

**証明:**  $\ds s_n=\sum_{k=0}^n a_k$, $s_{-1}=0$, $\ds t_n=\sum_{k=0}^n s_k$, $t_{-1}=0$ とおく. $\ds\sum_{n=0}^\infty a_n$ がCesàro総和可能であるとき, $a_n=O(n)$, $s_n=O(n)$, $t_n=O(n)$ となることは上の方ですでに示した. そのことより, べき級数 $\ds\sum_{k=0}^\infty a_k z^k$,  $\ds\sum_{k=0}^\infty s_k z^k$,  $\ds\sum_{k=0}^\infty t_k z^k$ の収束半径が $1$ 以上になることがわかる. $|z|<1$ を満たす複素数 $z$ に対して $\ds f(z)=\sum_{k=0}^\infty a_k z^k$ とおく. このとき, $|z|<1$ ならば

$$
f(z) = (1-z)\sum_{k=0}^\infty s_k z^k = (1-z)^2\sum_{k=0}^\infty t_k z^k =
(1-z)^2 \sum_{k=0}^\infty \frac{t_k}{k+1} (k+1) z^k.
$$

これと

$$
\alpha = (1-z)^2\frac{\alpha}{(1-z)^2} = (1-z)^2\sum_{k=0}^\infty \alpha (k+1)z^k
$$

の差は任意の番号 $N$ について

$$
f(z)-\alpha = 
(1-z)^2 \sum_{k=0}^{N-1}  \left(\frac{t_k}{k+1}-\alpha\right) (k+1) z^k +
(1-z)^2 \sum_{k=N}^\infty \left(\frac{t_k}{k+1}-\alpha\right) (k+1) z^k
$$

となる. $R\geqq 1$, $\ds\frac{|1-z|}{1-|z|}\leqq R$ と仮定する. 任意に $\eps>0$ を取る. 級数 $\ds\sum_{n=0}^\infty a_n$ のCesàro総和可能性は $\ds\frac{t_k}{k+1}\to\alpha$ を意味するので, ある番号 $N$ が存在して $k\geqq N$ ならば $\ds\left|\frac{t_k}{k+1}-\alpha\right|\leqq\frac{\eps}{2R^2}$ となる. そのとき,

$$
\left|(1-z)^2 \sum_{k=N}^\infty \left(\frac{t_k}{k+1}-\alpha\right) (k+1) z^k\right| \leqq
|1-z|^2\eps\sum_{k=N}^\infty(k+1)|z|^k \leqq
\frac{\eps}{2R^2}\frac{|1-z|^2}{(1-|z|)^2} \leqq 
\frac{\eps}{2}.
$$

$\ds\frac{t_k}{k+1}-\alpha$ は有界なので $k$ によらない定数 $M>0$ が存在して, $\ds \left|\frac{t_k}{k+1}-\alpha\right|\leqq M$ となるので, $\ds |1-z|\leqq\sqrt{\frac{\eps}{MN(N+1)}}$ ならば

$$
\left|(1-z)^2 \sum_{k=0}^{N-1}  \left(\frac{t_k}{k+1}-\alpha\right) (k+1) z^k\right| \leqq
|1-z|^2M\frac{N(N+1)}{2}\leqq \frac{\eps}{MN(N+1)}\frac{MN(N+1)}{2}= \frac{\eps}{2}.
$$

このとき,

$$
|f(z) - \alpha| \leqq  \frac{\eps}{2} + \frac{\eps}{2} = \eps.
$$

これで, ある $R\geqq 1$ について, $z$ が $|z|<1$ かつ $\ds\frac{|1-z|}{1-|z|}\leqq R$ を満たしながら $1$ に近付くとき, $f(z)$ が $\alpha$ に収束することが示された. 特に $x$ が区間 $[0,1)$ の上を動きながら $1$ に近付くならば $f(x)$ は $\alpha$ に収束する. これで, Abel総和可能性とAbel和が $\alpha$ に等しくなることが示された. $\QED$

**問題:** 収束半径が $1$ のべき級数 $\ds f(x)=\sum_{k=0}^\infty (-1)^k x^{k^2}$ について, $x\nearrow 1$ のとき $\ds f(x)\to \frac{1}{2}$ となることを示せ. 

**解答例:** $n$ が平方数のとき $a_n=1$ でそれ以外のとき $a_n=0$ とおくと, $\ds f(x) = \sum_{j=0}^\infty (-1)^j a_j x^j$ となる. 上の方で級数 $\ds\sum_{j=0}^\infty (-1)^j a_j$ がCesàro総和可能でかつそのCesàro和が $\ds\frac{1}{2}$ になることを示した. ゆえに, その級数はAbel総和可能でかつそのAbel和も $\ds\frac{1}{2}$ になる. すなわち, $x\nearrow 1$ のとき $\ds f(x)\to \frac{1}{2}$ となる. $\QED$

**問題:** Cesàro総和不可能だが, Abel総和可能な級数の例を挙げよ.

**解答例:** $a_k = (-1)^k (k+1)$, $\ds s_n = \sum_{k=0}^n a_k = 1-2+3-4+\cdots+(-1)^n(n+1)$, $t_n=\sum_{k=0}^n s_k$ とおくと,

$$
\begin{aligned}
&
s_0 = 1, \quad 
s_1 = -1, \quad 
s_2 = 2, \quad 
s_3 = -2, \quad 
s_4 = 3, \quad 
s_5 = -3, \quad
\ldots,
\\ &
t_0 = 1, \quad
t_1 = 0, \quad
t_2 = 2, \quad
t_3 = 0, \quad
t_4 = 3, \quad
t_5 = 0, \quad
\ldots
\end{aligned}
$$

すなわち, $t_{2k}=k+1$, $s_{2k+1}=0$ なので $\ds\lim_{k\to\infty}\frac{t_{2k}}{2k+1}=\frac{1}{2}$, $\ds\lim_{k\to\infty}\frac{s_{2k+1}}{2k+2}=0$ となるので, $\ds\frac{t_n}{n+1}$ は収束しない. すなわち, 級数 $\ds\sum_{k=0}^\infty a_k = 1 - 2 + 3 - 4 + \cdots$ はCesàro総和不可能である. しかし, 

$$
f(x) = \sum_{k=0}^\infty a_k x^k = \sum_{k=0}^\infty(-1)^k(k+1)x^k = 
\sum_{k=0}^\infty \binom{-2}{k}(-x)^k = \frac{1}{(1+x)^2}
$$

は $x\nearrow 1$ で $\ds\frac{1}{4}$ に収束する. すなわち, 級数 $\ds\sum_{k=0}^\infty a_k = 1 - 2 + 3 - 4 + \cdots$ はAbel総和可能であり, そのAbel和は $\ds\frac{1}{4}$ になる. $\QED$

次の問題とその解答例は次のスライドのp.25にある.

* Peter Duren, <a href="http://matematicas.uam.es/~dragan.vukotic/respub/Duren_Tauberian_Talk_2013-10_UAM.pdf">Sums for Divergent Series: A Tauberian Adventure</a>, Slide 2013-10

**問題:** $n$ が $2$ のべきで表わされる整数ならば $a_n=1$ でそれ以外ならば $a_n=0$ であるとする. このとき, 級数

$$
\sum_{k=0}^\infty (-1)^{\log_2 k} a_k =
0+1-1+0+1+0+0+0+0+0+0+0+0-1+\cdots
$$

はAbel総和**不可能**であることを示せ. ただし, $|x|<1$ のとき

$$
f(x) = \sum_{k=0}^\infty (-1)^{\log_2 k} a_k x^k =
\sum_{j=0}^\infty (-1)^j x^{2^j} =
x - x^2 + x^4 - x^8 + x^{16} - x^{32} + \cdots
$$

とおくと, 数値計算の結果 $f(0.995)=0.50088\cdots$ が正しいことを認めて使ってよいものとする.

**解答例:** $0<x<1$ であると仮定する. もしも $x\nearrow 1$ で $f(x)$ が $\alpha$ に収束するならば, $f(x)=x-f(x^2)$ より $\alpha=1-\alpha$ となるので, $\ds\alpha=\frac{1}{2}$ でなければいけない. しかし, $f(x)=x-x^2+f(x^4) > f(x^4)$ より, $f(x)<f(x^{1/4})<f(x^{1/4^2})<\cdots$ となることと, 数値計算の結果より $x=0.995$ のとき $f(x)>0.5$ となることから, もしも $x\nearrow 1$ で $f(x)$ が収束するならばその収束先 $\alpha$ は $0.5$ より大きくなる. これで矛盾が導かれた. $x\nearrow 1$ で $f(x)$ は収束しない. $\QED$

```julia
f(x; N=2^5) = sum(j->(-1)^j*x^(typeof(x)(2)^j), 0:N)
@time [f(big"0.995"; N=2^m) for m in 0:5]
```

**Stolz領域の形:** $R\geqq 1$ に対して, Stolz領域 $K_R$ を

$$
K_R = \left\{\,z\in\C \,\left|\, |z|<1,\; \frac{|1-z|}{1-|z|}\leqq R \right.\right\}
$$

と定める. $K_R$ の形については次のセルのプロットを見よ. $\QED$

```julia
x = -1:0.005:1
y = -1:0.005:1
z = x' .+ im.*y
f(z, R) = (abs2(z)<1 && abs(1-z) ≤ R*(1-abs(z))) ? 0.0 : 1.0
t = 0:0.01:2π

PP = []
for RR in [1.0, 2.0, 5.0, 10.0]
    P = plot(aspectratio=1)
    plot!(xlim=(-1,1), ylim=(-1,1))
    plot!(title="R = $RR", titlefontsize=10)
    heatmap!(x, y, f.(z, RR), colorbar=false, color=:plasma)
    plot!(cos.(t), sin.(t), label="")
    push!(PP, P)
end

plot(PP..., size=(750, 170), layout=@layout([a b c d]))
```

**問題:** $a_k\geqq 0$ かつ $\ds\sum_{k=0}^\infty a_k=\infty$ ならば,  $x\nearrow 1$ のとき $\ds \sum_{k=1}^\infty a_k x^k\to\infty$ となることを示せ.

**解答例:** $a_k\geqq 0$ かつ $\ds\sum_{k=0}^\infty a_k=\infty$ と仮定し, $M>0$ であるとする. ある番号 $N$ が存在して $n\geqq N$ ならば $\ds\sum_{k=0}^n a_k\geqq 2M$ となる.  ゆえに $\ds\frac{1}{2^{1/N}}\leqq x<1$ とすると, 

$$
\sum_{k=0}^\infty a_k x^k \geqq
\sum_{k=0}^N a_k x^k \geqq
x^N \sum_{k=0}^N a_k \geqq
\frac{1}{2}2M = M.
$$

これで $x\nearrow 1$ のとき $\ds \sum_{k=1}^\infty a_k x^k\to\infty$ となることが示された. $\QED$

**例:** $\ds f(z) = \sum_{k=0}^\infty\frac{z^{2\cdot 3^k}-z^{3^k}}{k}$ とおく. 右辺のべき級数の収束半径は $1$ であり, $z=1$ でも収束しており, $f(1)=0$ となっている. ゆえに, Abelの連続性定理より, ある $R\geqq 1$ について, $z$ が $\ds\frac{|1-z|}{1-|z|}\leqq R$ を満たしながら $1$ に近付くと(特に $z\nearrow 1$ のとき) $f(z)$ は $f(0)=0$ に近付く.

しかし, $\ds z_m=e^{\pi i/3^m}$ とおくと, $m\to\infty$ で $z_m\to 1$ であり, さらに $k\geqq m$, $0\leqq r\leqq 1$, $z=r z_m$ ならば $z^{2\cdot 3^k}-z^{3^k}=r^{2\cdot3^k}+r^{3^k}$ となり, $r$ のべき級数 $\ds\sum_{k=m}^\infty\frac{r^{2\cdot3^k}+r^{3^k}}{k}$ は $r=1$ のとき $\infty$ になるので, 上の問題の結果より, $r\nearrow 1$ で $\ds\sum_{k=m}^\infty\frac{r^{2\cdot3^k}+r^{3^k}}{k}\to\infty$ となる.  このことから, $|z|<1$ を満たす $z$ を $1$ に自由に近付けてよいことにすると, $f(z)$ は $0$ に収束しなくなることがわかる. $\QED$ 

```julia
f(z; N=39) = sum(k->(z^(2*3^k)-z^(3^k))/k, 1:N)
@show f(1)
@show f(0.9*e^(π*im/3^1))
@show f(0.9999*e^(π*im/3^2))
@show f(0.9999999*e^(π*im/3^3))
@show f(0.9999999999*e^(π*im/3^4))
@show f(0.9999999999999*e^(π*im/3^5));
```

#### 準備: Vitaliの定理

複素函数論を使うことができれば, べき級数の収束円盤の境界の一部分までべき級数で定義された函数が解析接続されているというAbelの連続性定理の仮定も強い仮定のもとで, Abelの定理よりも強い結論を得ることができる. そういう結果を示すための準備として, 次のVitaliの定理を引用しておく.

**Vitaliの定理:** $D$ は$\C$ の連結開集合であり, $f_n$ は $D$ 上の正則函数列であるとする. もしも $f_n$ が $D$ 上で広義一様有界でかつ複素数列 $f_n(z)$ が収束するような $z\in D$ の全体が集積点を持つならば, 正則函数列 $f_n$ は $D$ 上で広義一様収束する. $\QED$

ここで, $f_n$ が広義一様有界であるとは, $D$ の任意のコンパクト部分集合 $K$ に対してある定数 $M>0$ が存在して, $K$ 上ですべての番号 $n$ について $|f_n|\leqq M$ となることであり, $f_n$ が $D$ 上で広義一様収束するとは, $D$ の任意のコンパクト部分集合 $K$ 上で $f_n$ が一様収束することである.  正則函数列の広義一様収束先の函数も正則函数になる. 

Vitaliの定理の証明はこのノートで扱わないが, その定理は極めて有用である. Vitaliの定理を使えば, 領域 $D$ 内のほんの一部分での収束さえ証明すれば, 領域 $D$ 全体での収束が証明できてしまうことがある. 実際に次の節でVitaliの定理がそのような使われ方をする.

Vitaliの証明については以下の文献を参照せよ:

* 大島利雄, <a href="https://web.archive.org/web/20180202163724/http://akagi.ms.u-tokyo.ac.jp/CAMPUS/ca.pdf">関数論</a>, 2003年

* 斎藤恭司述, 松本佳彦記, <a href="http://www.ms.u-tokyo.ac.jp/publication/documents/saito-lectures.pdf">複素解析学特論</a> (<a href="https://web.archive.org/web/20130626083906/http://www.ms.u-tokyo.ac.jp/publication/documents/saito-lectures.pdf">archive</a>), 1978年, 2009年


#### M. Riesz の補題とその応用

この節の内容については

* Remmert, Reinhold.  Classical topics in complex function theory.  Graduate Texts in Mathematics 172, Springer, 1998

の第11章 "Boundary Behavior of Power Series" の§1 "Convergent on the Boundary" (pp.244-249)を参照した.  この文献のこの章は収束円盤の境界上でのべき級数の振る舞いに関する結果やアイデアに関する歴史的覚え書きについても非常に詳しい. 

基本は次の補題である. $g_n$ の定義の仕方が絶妙で本質を突いている.

**M. Rieszの補題:** $\ds\sum_{k=0}^\infty a_k z^k$ は収束半径が1の複素べき級数であるとし, 複素数列 $a_n$ は有界であると仮定する. 

$$
a < b\leqq a+2\pi, \quad
c > 1, \quad
K=K_{a,b,c}=\{\, z\in\C \mid a\leqq\arg(z)\leqq b,\; |z|\leqq c \,\}
$$

であるとし, $|z|<1$ における正則函数 $\ds\sum_{k=0}^\infty a_k z^k$ は $K$ を含む $\C$ の開集合上の正則函数 $f$ に拡張されると仮定する. $K$ 上の函数列 $g_n$ を次のように定める:

$$
g_n(z) = \frac{f(z) - s_n(z)}{z^{n+1}} (z-\alpha)(z-\beta),
\quad s_n(z) = \sum_{k=0}^n a_k z^k,
\quad \alpha = e^{ia}, 
\quad \beta = e^{ib}.
$$

このとき, 函数列 $g_n$ は $K$ 上で一様有界である. $\QED$

**証明:** $\kappa = ce^{ia}$, $\lambda = ce^{ib}$ とおく.  最大値の原理より, $K$ の境界上で, すなわち, 線分 $[0,\kappa]$, $[0,\lambda]$ および円弧 $L = \{\,ce^{i\theta}\mid a\leqq\theta\leqq b\,\}$ の上で函数列 $g_n$ が一様有界であることを示せば十分である. 線分 $[0,\kappa]$ を $0$, $\alpha$, $(0,\alpha)$, $(\alpha,\lambda]$ に分割して考える. 

複素数列 $|a_k|$ の上界の一つを $A>0$ と書き, $|f|$ の $K$ 上での上界の一つを $M>0$ と書く.

$z=0$ において, $|g_n(0)| = |a_{n+1}||0-\alpha||0-\beta|=|a_{n+1}|\leqq A$.

$z=\alpha$ において, $|g_n(\alpha)|=0$.

$z\in(0,\alpha)$ は $z = re^{ia}$, $0< r < 1$ と表わされる. そのとき,

$$
\begin{aligned}
&
|f(z)-s_n(z)| = \left|\sum_{k=n+1}^\infty a_k z^k\right| \leqq 
A\sum_{k=n+1}^\infty r^k = \frac{A r^{n+1}}{1-r},
\\ &
|z-\alpha|=1-r,  \quad |z-\beta|\leqq 2
\end{aligned}
$$

なので, 

$$
|g_n(z)|\leqq
\frac{A r^{n+1}}{1-r}\frac{1}{r^{n+1}}(1-r)2 = 2A.
$$

$z\in(\alpha,\kappa]$ は $z=re^{ia}$, $1<r\leqq c$ と表わされる. そのとき,

$$
\begin{aligned}
&
|f(z)-s_n(z)| \leqq 
|f(z)|+A\sum_{k=0}^n r^k \leqq
M + \frac{A(r^{n+1}-1)}{r-1} \leqq
M + A\frac{r^{n+1}}{r-1},
\\ &
|z-\alpha| = r-1, \quad |z-\beta|\leqq c+1,
\\ &
r^{n+1} \geqq r \geqq r-1
\end{aligned}
$$

なでの,

$$
|g_n(z)|\leqq 
\left(M + A\frac{r^{n+1}}{r-1}\right)\frac{1}{r^{n+1}}(r-1)(c+1) \leqq
(M+A)(c+1).
$$

$z\in[0,\lambda]$ 上での $|g_n|$ の上からの評価は以上における $z\in[0,\lambda]$ の場合と同様である.

$z\in L$ は $z=ce^{i\theta}$, $a\leqq\theta\leqq b$ と表わされる. そのとき, $|z|=c>1$ なので,

$$
\begin{aligned}
&
|f(z)-s_n(z)| \leqq 
|f(z)|+A\sum_{k=0}^n c^k \leqq
M + \frac{A(c^{n+1}-1)}{c-1} \leqq
M + A\frac{c^{n+1}}{c-1},\\ &
|z-\alpha|\leqq c+1, \quad |z-\beta|\leqq c+1,
\end{aligned}
$$

なので,

$$
|g_n(z)|\leqq
\left(M + A\frac{c^{n+1}}{c-1}\right)\frac{1}{c^{n+1}}(c+1)(c+1) \leqq
\left(M + \frac{A}{c-1}\right)(c+1)^2.
$$

以上によって, $|g_n|$ が $K$ の境界上で一様有界であることが示されたので, 最大値の原理から $K$ 全体で一様有界であることも導かれる. $\QED$.

**M. Rieszの有界性定理:** $\ds\sum_{k=0}^\infty a_k z^k$ は収束半径が1の複素べき級数であるとし, 複素数列 $a_n$ は有界であると仮定する. 

$$
S=\{\, z\in\C \mid A<\arg(z)<B,\; |z| < C\,\}, \quad
A < B \leqq A+2\pi, \quad
C > 1
$$

であるとし, $|z|<1$ における正則函数 $\ds\sum_{k=0}^\infty a_k z^k$ は $S$ 上の正則函数 $f$ に拡張されると仮定する. このとき, 部分和の函数列 $s_n(z) = \ds\sum_{k=0}^n a_k z^k$ は

$$
S_{|z|\leqq 1} = \{\,z\in\C\mid A<\arg(z)<B,\; |z|\leqq 1\,\}
$$

上で広義一様有界である. $\QED$

**証明:** $S_{|z|\leqq 1}$ の任意のコンパクト部分集合 $Z$ は M. Rieszの補題におけるある $K=K_{a,b,c}$ で $A<a<b<B$, $1<c<C$ を満たすものの内部に含まれる. 以下では M. Rieszの補題とその証明における記号をそのまま用いる. $Z$ から $K$ の境界への最短距離を $d>0$ と書く. M. Rieszの補題より, ある定数 $G>0$ が存在して, すべての番号 $n$ について $K$ 上で $|g_n|\leqq G$ となる. ゆえに, $z\in Z$ のとき, $|z|\leqq 1$ でもあるので,

$$
|s_n(z)| = \left|f(z) - \frac{g_n(z)z^{n+1}}{(z-\alpha)(z-\beta)}\right| \leqq
|f(z)| + \frac{|g_n(z)||z^{n+1}|}{|z-\alpha||z-\beta|} \leqq
M + \frac{G}{d^2}.
$$

これで $s_n(z)$ が $Z$ 上で一様有界であることが確かめられた. $\QED$


**FatouとM. Rieszの収束定理:** M. Rieszの有界性定理の状況において, さらに複素数列 $a_n$ が $0$ に収束していると仮定すると, べき級数 $\ds\sum_{k=0}^\infty a_k z^k$ は $S_{|z|\leqq 1}$ 上で $f$ に広義一様収束する. $\QED$

**証明:** $S_{|z|\leqq 1}$ の任意のコンパクト部分集合 $Z$ は M. Rieszの補題におけるある $K=K_{a,b,c}$ で $A<a<b<B$, $1<c<C$ を満たすものの内部に含まれる. 以下では M. Rieszの補題とその証明における記号をそのまま用いる. $Z$ から $K$ の境界への最短距離を $d>0$ と書く. このとき, $z\in Z$ ならば, $|z|\leqq 1$ でもあるので,

$$
|f(z)-s_n(z)| = \frac{|g_n(z)||z^{n+1}|}{|z-\alpha||z-\beta|} \leqq
\frac{|g_n(z)|}{d^2}.
$$

したがって, $s_n(z)$ が $f(z)$ に一様収束することを示すためには, $g_n(z)$ が $Z$ 上で $0$ に一様収束することを示せば十分である. さらに, そのためには, Vitaliも定理より, $0<r<1$ を満たす $r$ を任意に固定して, $z\in K$, $|z|=r$ のときに, $\ds \lim_{n\to\infty}g_n(z)=0$ となることを示せば十分である. 

$0<r<1$, $z\in K$, $|z|=r$ と仮定し, 任意に $\eps>0$ を取る. この場合には, $a_n$ が $0$ に収束すると仮定してあったので, ある番号 $N$ が存在して, $k\geqq N$ ならば $\ds |a_k|\leqq \eps\frac{1-r}{(r+1)^2}$ となる.  以下, $n\geqq N$ と仮定する. このとき,  $|z-\alpha|\leqq r+1$, $|z-\beta|\leqq r+1$, 

$$
|f(z)-s_n(z)|\leqq\sum_{k=n+1}^\infty |a_k|q^k \leqq 
\eps\frac{1-r}{(r+1)^2}\frac{r^{n+1}}{1-r}
$$

なので, 

$$
|g_n(z)|\leqq \eps\frac{1-r}{(r+1)^2}\frac{r^{n+1}}{1-r}\frac{1}{r^{n+1}}(r+1)^2 = \eps.
$$

これで $\ds\lim_{n\to\infty}g_n(z)=0$ が示された. $\QED$


**注意:** FatouとM. Rieszの収束定理より, 収束半径 $1$ のべき級数で定義された $|z|<1$ における正則函数 $\ds f(z)=\sum_{k=0}^\infty a_k z^k$ が $z=1$ まで解析接続されているとき, 複素数列 $a_n$ が $0$ に収束することと $\ds\sum_{k=0}^\infty a_k = \lim_{n\to\infty}\sum_{k=0}^n a_k$ が収束することが同値であることがわかる. 後者ならば前者は自明であるが, その逆向きはFatouとM. Rieszの収束定理の証明を読めばかなり非自明である. $\QED$


**例:** FatouとM. Rieszの収束定理(もしくはRieszの有界性定理)において, べき級数の係数が $0$ に収束する(もしくは有界である)という仮定を除くことはできないことは次の例を見ればわかる. 有理函数 $\ds f(z)=\frac{1}{(1+z)^2}$ は $z\ne-1$ 以外で正則であり, そのべき級数展開

$$
f(z) = \sum_{n=1}^\infty (-1)^{n-1} n z^n
$$

の収束半径は $1$ であり, $\ds f(1)=\frac{1}{4}$ である. しかし, 右辺のべき級数の係数 $(-1)^{n-1}n$ は $0$ に収束していないし(有界でさえない), そのべき級数は $z=1$ のとき

$$
\begin{aligned}
&
s_N = \sum_{n=1}^N (-1)^{n-1} n = 1-2+3-4+\cdots+(-1)^{N-1}N = 
(-1)^{N-1}\left\lfloor\frac{N+1}{2}\right\rfloor,
\\ &
s_1=1,\ s_2=-1,\ s_3=2,\ s_4=-2,\ s_5=3,\ s_6=-3,\ \ldots
\end{aligned}
$$

となって収束しない(有界でさえない). $\QED$

**例:** $\ds \log(1+z) = \int_0^z \frac{dt}{1+t}$ は $z\ne -1$ 以外では多価正則であり, そのべき級数展開

$$
\log(1+z) = \sum_{n=1}^\infty (-1)^{n-1}\frac{z^n}{n}
$$

の収束半径は $1$ であり, その係数 $(-1)^n/n$ は $0$ に収束している. ゆえに, FatouとM. Rieszの収束定理より, このべき級数は

$$
|z|\leqq 1, \quad -\pi < \arg(z) < \pi
$$

において $\log(1+z)$ に広義一様収束している. $\QED$

**例:** $\ds\Li_2(z)=\int_0^z\frac{-\log(1-t)}{t}\,dt$ は $z=1$ 以外では多価正則であり, そのべき級数展開

$$
\Li_2(z) = \sum_{n=1}^\infty \frac{z^n}{n^2}
$$

は $|z|\leqq 1$ において一様絶対収束している.  $\ds\sum_{n=1}^\infty\frac{1}{n^2}=\zeta(2)=\frac{\pi^2}{6}$ なので, Abelの定理より, $z\nearrow 1$ のとき, $\ds\sum_{n=1}^\infty\frac{z^n}{n^2}\to\zeta(2)$ となる. $\QED$


## 超幾何級数

次のべき級数を**Gaussの超幾何級数** (Gauss's hypergeometric series)と呼ぶ:

$$
{}_2F_1(a,b; c; x) = \sum_{k=0}^\infty \frac{(a)_k (b)_k}{(c)_k k!} x^k.
$$

ここで

$$
(\alpha)_k = \alpha(\alpha+1)\cdots(\alpha+k-1).
$$

より一般に

$$
{}_rF_s(a_1,\ldots,a_r;b_1,\ldots,b_s;x) =
\sum_{k=0}^\infty \frac{(a_1)_k\cdots(a_r)_k}{(b_1)_k\cdots(b_s)_k k!}x^k
$$

と定める.


**問題:** Gaussの超幾何級数 ${}_2F_1$ が $|x|<1$ で絶対収束することを示せ.

**解答例:** d'Alembertの判定法を使おう.

$$
\frac{(a)_k(b)_k/((c)_k k!)}{(a)_{k+1}(b)_{k+1}/((c)_{k+1}(k+1)!)} =
\frac{(c+k)(k+1)}{(a+k)(b+k)}\to 1 \qquad(k\to\infty)
$$

より, Gaussの超幾何級数は $|x|<1$ で絶対収束する. $\QED$


**問題:** 以下を示せ:

$$
(1)_k=k!, \quad (2)_k = (k+1)!, \quad
(1/2)_k k! = \frac{(2k)!}{4^k}, \quad
(2k+1)(1/2)_k = (3/2)_k.
$$

**ヒント:** 3つ目の公式を除けば易しい. 3つ目の公式は以下のようにして示される:

$$
(1/2)_k k! =
\frac{1}{2}\frac{3}{2}\cdots\frac{2k-1}{2} k! =
\frac{1\cdot3\cdots(2k-1)}{2^k}\frac{2\cdot4\cdots(2k)}{2^k} =
\frac{(2k)!}{4^k}. \quad \QED
$$


**例:** 初等函数を以下のように表すことができる:
$$
\begin{aligned}
&
{}_0F_0(;; x) = \sum_{k=0}^\infty\frac{1}{k!}x^k = e^x.
\\ &
{}_0F_1(;1/2;-x^2/4) = 
\sum_{k=0}^\infty \frac{(-x^2/4)^k}{(1/2)_k k!} =
\sum_{k=0}^\infty \frac{(-x^2)^k}{(2k)!} = \cos x.
\\ &
{}_1F_0(a;; x) = \sum_{k=0}^\infty\frac{(a)_k}{k!}x^k = (1-x)^{-a}.
\\ &
{}_2F_1(1,1;2;x) = \sum_{k=0}^\infty\frac{k!k!}{(k+1)!k!}x^k = 
\sum_{k=0}^\infty\frac{x^k}{k+1} = -\frac{\log(1-x)}{x}.
\\ &
x\;{}_2F_1(1/2,1;3/2;-x^2) =
x\sum_{k=0}^\infty\frac{(1/2)_k k!}{(3/2)_k k!}(-x^2)^k =
x\sum_{k=0}^\infty\frac{(-x^2)^k}{2k+1} = \arctan x.
\end{aligned}
$$

**問題:** 上の例の結果を確認せよ. $\QED$


**問題:** $\sin x$ を ${}_0F_1$ を使って表せ. $\QED$

**問題:** $\arcsin x$ を ${}_2F_1$ を使って表せ. $\QED$

**ヒント:** <a href="https://www.google.co.jp/search?q=%E8%B6%85%E5%B9%BE%E4%BD%95+%E5%88%9D%E7%AD%89%E5%87%BD%E6%95%B0">「超幾何 初等函数」を検索</a>.


**問題:** ガンマ函数とベータ函数を

$$
\Gamma(s) = \int_0^\infty e^{-x}x^{s-1}\,dx, \quad
B(p,q) = \int_0^1 t^{p-1}(1-t)^{q-1}\,dt
\quad(s,p,q > 0)
$$

と定めると, 

$$
(x)_k = \frac{\Gamma(x+k)}{\Gamma(x)}, \quad
B(p,q) = \frac{\Gamma(p)\Gamma(q)}{\Gamma(p+q)}
$$

が成立することを認めて, $c>b>0$ のとき

$$
{}_2F_1(a,b;c;x) = \frac{1}{B(b,c-b)}
\int_0^1 t^{b-1}(1-t)^{c-b-1}(1-xt)^{-a}\,dt
$$

が成立することを示せ. これを**Gaussの超幾何函数の積分表示**と呼ぶ.

**解答例:** 

$$
\begin{aligned}
\frac{(a)_k (b)_k}{(c)_k k!} x^k &=
\frac{\Gamma(b+k)\Gamma(c)}{\Gamma(b)\Gamma(x+k)} \frac{(a)_k}{k!}x^k =
\frac{B(b+k,c-b)}{B(b,c-b)}\frac{(a)_k}{k!}x^k
\\ &=
\frac{1}{B(b,c-b)}\int_0^1 t^{b-1}(1-t)^{c-b-1} \frac{(a)_k}{k!}(xt)^k\,dt.
\end{aligned}
$$

ゆえに, これを $k=0,1,2,\ldots$ について足し上げて目的の公式を得る. $\QED$

**注意:** 高校数学における積分の取り扱いにおいて

$$
I = \int_\alpha^\beta (x-\alpha)^A (\beta-x)^B (\gamma-x)^C \,dx
$$

の形の積分に出会っている人はその時点でGaussの超幾何函数にすでに出会っていると言える. この形の積分は $x = (1-t)\alpha+t\beta$, $z=(\beta-\alpha)/(\gamma-\alpha)$ とおくと

$$
I = (\beta-\alpha)^{A+B+1}(\gamma-\alpha)^C\int_0^1 t^A (1-t)^B (1-zt)^C \,dt
$$

と変形される. $\QED$


**問題:** 以下の公式を示せ: $c>b>0$ のとき,

$$
{}_1F_1(b;c;x) = \frac{1}{B(b,c-b)}\int_0^1 t^{b-1}(1-t)^{c-b-1}\,e^{xt}\,dt
$$

となることを示せ. これを**Kummerの合流型超幾何函数の積分表示**と呼ぶ.

**解答例:**

$$
\begin{aligned}
\frac{(b)_k}{(c)_k k!} x^k &=
\frac{\Gamma(b+k)\Gamma(c)}{\Gamma(b)\Gamma(x+k)} \frac{x^k}{k!} =
\frac{B(b+k,c-b)}{B(b,c-b)}\frac{x^k}{k!}
\\ &=
\frac{1}{B(b,c-b)}\int_0^1 t^{b-1}(1-t)^{c-b-1} \frac{(xt)^k}{k!}\,dt.
\end{aligned}
$$

ゆえに, これを $k=0,1,2,\ldots$ について足し上げて目的の公式を得る. $\QED$


**問題(Gaussの超幾何微分方程式):** $y={}_2F_1(a,b; c; x)$ が

$$
x(1-x)y'' + (c-(a+b+1)x)y' - aby = 0
$$

を満たしていることを示せ. これを**Gaussの超幾何微分方程式**と呼ぶ.

**解答例:** $\d=\frac{d}{dx}$ とおく. $x\d x^k = kx^k$ となる. ゆえに, $\ds{}_2F_1(a,b;c;x)=\sum_{k=0}^\infty \frac{(a)_k (b)_k}{(c)_k k!} x^k$ より,

$$
\begin{aligned}
&
(x\d+a)y = \sum_{k=0}^\infty \frac{(a)_k (b)_k}{(c)_k k!} (a+k)x^k =
\sum_{k=0}^\infty \frac{a(a+1)_k (b)_k}{(c)_k k!}x^k =
a\,{}_2F_1(a+1,b;c;x),
\\& \therefore\quad
(x\d+a)(x\d+b)y = ab\,{}_2F_1(a+1,b+1;c;x).
\\ &
\d(x\d+c-1)y = \sum_{k=1}^\infty \frac{(a)_k (b)_k}{(c)_k k!}(c+k-1)k x^{k-1} 
\\ &\qquad\qquad\quad =
\sum_{k=1}^\infty \frac{a(a+1)_{k-1} b(b+1)_{k-1}}{(c)_{k-1} (k-1)!} x^{k-1} =
ab\,{}_2F_1(a+1,b+1;c;x),
\\ & \therefore\quad
[\d(x\d+c-1)-(x\d+a)(x\d+b)] y = 0.
\end{aligned}
$$

そして,

$$
\begin{aligned}
\d(x\d+c-1)-(x\d+a)(x\d+b) &=
x\d^2+c\d - (x^2\d^2 + (a+b+1)x\d + ab) 
\\ &=
x(1-x)\d^2 +(c-(a+b+1)x)\d - ab.
\end{aligned}
$$

これで示したい公式が示された. $\QED$


**注意:** Gaussの超幾何函数に関する詳しい結果を用いて, Virasoro代数の対称性を持つBelavin-Polyakov-Zamolodchikovの共形場理論の $c=1$ の場合を詳しく分析すると, Painlevé VI 方程式の $\tau$ 函数を構成できるという面白い結果については, 次の集中講義のノートを参照せよ.

* 名古屋創, <a href="https://genkuroki.github.io/documents/201805NagoyaHajime/">共形場理論とPainlevé方程式</a>, 東北大学数学教室における集中講義, 2018年5月7日～10日

この集中講義の<a href="https://genkuroki.github.io/documents/201805NagoyaHajime/2018-05-09%20%E5%90%8D%E5%8F%A4%E5%B1%8B%E5%89%B5%20%E5%85%B1%E5%BD%A2%E5%A0%B4%E7%90%86%E8%AB%96%E3%81%A8%E3%83%8F%E3%82%9A%E3%83%B3%E3%83%AB%E3%82%A6%E3%82%99%E3%82%A7%E6%96%B9%E7%A8%8B%E5%BC%8F%202%20of%204.pdf">第2回目</a>はGaussの超幾何微分方程式の易しい入門的解説になっている. $\QED$


**問題(Kummerの合流型超幾何微分方程式):** $y={}_1F_1(b; c; x)$ が

$$
xy'' + (c-x)y' - by = 0
$$

を満たしていることを示せ. これを**Kummerの合流型超幾何微分方程式**と呼ぶ.

**解答例:**$\d=\frac{d}{dx}$ とおく. $x\d x^k = kx^k$ となる. ゆえに, $\ds{}_1F_1(b;c;x)=\sum_{k=0}^\infty \frac{(b)_k}{(c)_k k!} x^k$ より,

$$
\begin{aligned}
&
(x\d+b)y = \sum_{k=0}^\infty \frac{(b)_k}{(c)_k k!} (b+k)x^k =
\sum_{k=0}^\infty \frac{b(b+1)_k}{(c)_k k!}x^k =
b\,{}_2F_1(b+1;c;x),
\\ &
\d(x\d+c-1)y = \sum_{k=1}^\infty \frac{(b)_k}{(c)_k k!}(c+k-1)k x^{k-1} 
\\ &\qquad\qquad\quad =
\sum_{k=1}^\infty \frac{b(b+1)_{k-1}}{(c)_{k-1} (k-1)!} x^{k-1} =
b\,{}_1F_1(b+1;c;x),
\\ & \therefore\quad
[\d(x\d+c-1)-(x\d+b)] y = 0.
\end{aligned}
$$

そして,

$$
\begin{aligned}
\d(x\d+c-1)-(x\d+b) &=
x\d^2+c\d - (x\d+b)
\\ &=
x(1-x)\d^2 +(c-x)\d - b.
\end{aligned}
$$

これで示したい公式が示された. $\QED$
