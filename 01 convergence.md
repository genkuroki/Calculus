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

# 01 収束

黒木玄

2018-04-18, 2023-06-22

* Copyright 2018, 2023 Gen Kuroki
* License: MIT https://opensource.org/licenses/MIT
* Repository: https://github.com/genkuroki/Calculus

このファイルは次の場所できれいに閲覧できる:

* http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/01%20convergence.ipynb

* https://genkuroki.github.io/documents/Calculus/01%20convergence.pdf

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
\newcommand\Q{{\mathbb Q}}
\newcommand\R{{\mathbb R}}
\newcommand\C{{\mathbb C}}
\newcommand\QED{\text{□}}
\newcommand\root{\sqrt}
$

<!-- #region toc=true -->
<h1>目次<span class="tocSkip"></span></h1>
<div class="toc"><ul class="toc-item"><li><span><a href="#収束の定義" data-toc-modified-id="収束の定義-1"><span class="toc-item-num">1&nbsp;&nbsp;</span>収束の定義</a></span><ul class="toc-item"><li><span><a href="#数列の収束の定義" data-toc-modified-id="数列の収束の定義-1.1"><span class="toc-item-num">1.1&nbsp;&nbsp;</span>数列の収束の定義</a></span></li><li><span><a href="#級数の収束の定義" data-toc-modified-id="級数の収束の定義-1.2"><span class="toc-item-num">1.2&nbsp;&nbsp;</span>級数の収束の定義</a></span></li><li><span><a href="#連続的な極限" data-toc-modified-id="連続的な極限-1.3"><span class="toc-item-num">1.3&nbsp;&nbsp;</span>連続的な極限</a></span></li></ul></li><li><span><a href="#収束の判定法の基本" data-toc-modified-id="収束の判定法の基本-2"><span class="toc-item-num">2&nbsp;&nbsp;</span>収束の判定法の基本</a></span><ul class="toc-item"><li><span><a href="#挟撃法-(挟み撃ち)" data-toc-modified-id="挟撃法-(挟み撃ち)-2.1"><span class="toc-item-num">2.1&nbsp;&nbsp;</span>挟撃法 (挟み撃ち)</a></span></li><li><span><a href="#二項定理の復習" data-toc-modified-id="二項定理の復習-2.2"><span class="toc-item-num">2.2&nbsp;&nbsp;</span>二項定理の復習</a></span></li><li><span><a href="#多項式函数より指数函数の方が速く増加すること" data-toc-modified-id="多項式函数より指数函数の方が速く増加すること-2.3"><span class="toc-item-num">2.3&nbsp;&nbsp;</span>多項式函数より指数函数の方が速く増加すること</a></span></li><li><span><a href="#指数函数より階乗の方が速く増加すること" data-toc-modified-id="指数函数より階乗の方が速く増加すること-2.4"><span class="toc-item-num">2.4&nbsp;&nbsp;</span>指数函数より階乗の方が速く増加すること</a></span></li><li><span><a href="#まとめ" data-toc-modified-id="まとめ-2.5"><span class="toc-item-num">2.5&nbsp;&nbsp;</span>まとめ</a></span></li></ul></li><li><span><a href="#上限と下限と上極限と下極限" data-toc-modified-id="上限と下限と上極限と下極限-3"><span class="toc-item-num">3&nbsp;&nbsp;</span>上限と下限と上極限と下極限</a></span><ul class="toc-item"><li><span><a href="#上限と下限" data-toc-modified-id="上限と下限-3.1"><span class="toc-item-num">3.1&nbsp;&nbsp;</span>上限と下限</a></span></li><li><span><a href="#上極限と下極限" data-toc-modified-id="上極限と下極限-3.2"><span class="toc-item-num">3.2&nbsp;&nbsp;</span>上極限と下極限</a></span></li></ul></li><li><span><a href="#実数の連続性の帰結" data-toc-modified-id="実数の連続性の帰結-4"><span class="toc-item-num">4&nbsp;&nbsp;</span>実数の連続性の帰結</a></span><ul class="toc-item"><li><span><a href="#有界単調実数列の収束" data-toc-modified-id="有界単調実数列の収束-4.1"><span class="toc-item-num">4.1&nbsp;&nbsp;</span>有界単調実数列の収束</a></span></li><li><span><a href="#Bolzano–Weierstrassの定理" data-toc-modified-id="Bolzano–Weierstrassの定理-4.2"><span class="toc-item-num">4.2&nbsp;&nbsp;</span>Bolzano–Weierstrassの定理</a></span></li><li><span><a href="#実数のCauchy列の収束" data-toc-modified-id="実数のCauchy列の収束-4.3"><span class="toc-item-num">4.3&nbsp;&nbsp;</span>実数のCauchy列の収束</a></span></li><li><span><a href="#閉区間に関するHeine-Borelの被覆定理" data-toc-modified-id="閉区間に関するHeine-Borelの被覆定理-4.4"><span class="toc-item-num">4.4&nbsp;&nbsp;</span>閉区間に関するHeine-Borelの被覆定理</a></span></li><li><span><a href="#閉区間上の実数値連続函数が最大最小を持つこと" data-toc-modified-id="閉区間上の実数値連続函数が最大最小を持つこと-4.5"><span class="toc-item-num">4.5&nbsp;&nbsp;</span>閉区間上の実数値連続函数が最大最小を持つこと</a></span></li></ul></li></ul></div>
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
default(fmt=:png)
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

## 収束の定義


### 数列の収束の定義

**定義:** 数列 $a_n$ が $n\to\infty$ で $\alpha$ に**収束する**とは, 任意の $\eps>0$ に対して, ある番号 $N$ が存在して, $N$ 以降のすべての番号 $n$ について $|a_n - \alpha| < \eps$ が成立することである. $\QED$

$a_n$ が $\alpha$ に収束するとき, $a_n$ の極限の値を $\displaystyle \lim_{n\to\infty} a_n = \alpha$ と定義する.  収束しないときには極限の値は定義されない.

$a_n$ が $\alpha$ に収束することを $a_n\to \alpha$ と書くことにする.

上の定義に基けば, 数列 $a_n$ が $n\to\infty$ で $\alpha$ に**収束しない**ことは, ある $\eps>0$ が存在して, 任意の番号 $N$ について, ある $n\geqq N$ で $|a_n-\alpha|\geqq\eps$ を満たすものが存在することだと言い換えられる. 

どのような数にも収束しない数列は**発散**しているという. 


以上は所謂 $\eps$-$N$ 論法による数列の収束の定義である.

**勉強の仕方について1:** このノートから始まる一連のノート群では $\eps$-$N$ 論法や $\eps$-$\delta$ 論法による詳しい証明は**多くの場合に扱わない予定**である.  

しかし, 以下では数列の収束について $\eps$-$N$ について詳しく説明しておくことにする. $\eps$-$N$ および $\eps$-$\delta$ の方が易しく扱える場合は結構多いので教養として身に付けておいて損はないと思われる. 

特に解析学の知識を信頼できる数値計算の遂行に役立てたいと思っている人は, どこかの段階で ε-N と ε-δ の考え方をマスターしておくべきである. なぜならば,  **ε-N と ε-δ は「誤差の評価をまじめに行うこと」そのもの**だからである. 単に数学を応用するだけならば,  ε-N と ε-δ は必要ないという考え方は誤りである.

初心者のために, 下の方には ε-N を使わない解答例1と ε-N を使った解答例2の両方を解説した場合を複数書いておいた. それらの解答を比較することは, 収束に関する直観と論理の整備に役に立つだろう.

**しかし, 無理はいけない.** 

初心者のうちは ε-N および ε-δ を使った証明を無理してフォローする必要はない.

**すぐに理解できそうもないならば, 「適当に証明を飛ばしながら先に進み, 必要に応じて何度でも前に戻ることを繰り返しながら膨大な時間をかければいつかは理解できる」のように考えるのがよいと思う.** $\QED$


**勉強の仕方について2:** 筆者の個人的な経験では, 初心者のうちは, $\eps$-$N$ や $\eps$-$\delta$ の議論を書き下すときには, $\forall$, $\exists$ という記号を**使わない方がよい**と思う. より正確に言えば, $\forall$, $\exists$ という記号を使わずに説明できないと感じたら, $\forall$, $\exists$ という記号を使わない方がよい. 記号に頼る議論は非常によろしくない.  頼るべきなのは記号ではなく, 健全な直観である.

特に解析学の議論を $\forall$, $\exists$ という記号を使った形式的な記号計算として理解しようとするのはやめた方がよい. 解析学のポイントは「近似の誤差にあたるものが何にどのように依存しているか」を見極めることにある場合が多い. 

「〇〇の大きさが△△に◇◇のように依存して決まっており, 大雑把には□□と評価される」というような考え方を滑らかにできるようになることを目標にするのがよい. $\QED$


**数列の収束の正確な定義の1つの解釈:** 上の定義は数列 $a_n$ で $\alpha$ の値を近似計算したい場合には次のような意味を持つ.  

(1) 計算可能な数列 $a_n$ は直接正確な値を計算できない数値 $\alpha$ に収束することがわかっているとする.

(2) $\eps>0$ を任意に与え, 数列 $a_n$ の計算によって数値 $\alpha$ を誤差 $\eps$ 未満で計算したいとする.

(3) もしも数列 $a_n$ が $\alpha$ に収束しているならば, ある番号 $N$ が存在して, その $n\geqq N$ とすれば $a_n$ と $\alpha$ の差は $\eps$ 未満になる.

(4) そのとき, もしも $\eps$ から $N$ を具体的に求めることができれば, $N$ 以上の $n$ に対する $a_n$ を実際に計算することによって, $\alpha$ の値を誤差 $\eps$ 未満で求めることができる. $\QED$


**例:** $\alpha=\sqrt{2}$ に対して, $a_n$ を $\sqrt{2}$ を小数点以下第 $n$ 桁まで計算した結果だとする. 例えば, $a_1=1.4$, $a_2=1.41$, $a_3=1.414$, $\ldots$. このとき, $|a_n-\alpha|<10^{-n}$ なので, $\alpha=\sqrt{2}$ を誤差 $10^{-N}$ 未満で求めるためには $n\geqq N$ に対する $a_n$ を求めればよい. $\QED$


**数列が α に収束しないことの正確な定義の1つの解釈:** 数列 $a_n$ が $\alpha$ に収束しないことは次のように言い換えられる: ある $\eps>0$ が存在して, どんなに番号 $N$ を大きくしても, ある $n\geqq N$ で $|a_n-\alpha|\geqq\eps$ を満たすものが存在する.

これは, 以下のような意味を持っている.

(1) 数列 $a_n$ で数値 $\alpha$ を近似計算したいとする.

(2) しかし, $a_n$ は $\alpha$ に収束していないとする.

(3) その場合には, 許される誤差 $\eps>0$ を小さく取り過ぎると, どんなに $N$ を大きくしても, $N$ 以上の $n$ の中に $a_n$ による $\alpha$ の近似計算の誤差が許される誤差 $\eps$ 以上になるものが存在する.

もしも $a_n$ が $\alpha$ に収束しているならば, どんなに許される誤差 $\eps>0$ を小さくしても, 十分に $N$ を大きくすると, $N$ 以上の $n$ については常に $a_n$ による $\alpha$ の近似計算の誤差が許される誤差 $\eps$ 未満になる. すなわち, あるところから先の $a_n$ と $\alpha$ の距離は $\eps$ 未満になる.

$a_n$ が $\alpha$ に収束していなければ, 許される誤差 $\eps>0$ を小さくし過ぎると, どんなに先の $a_N$ 以降の $a_N,a_{N+1},a_{N+2},\ldots$ の中に誤差が $\eps$ 以上になるものが含まれてしまう. $\QED$


**例:** $a_n$ は上の例と同様に $\alpha=\sqrt{2}$ を小数点以下 $n$ 桁まで計算した結果であるとし, $b_n=a_n-(0.005+(-1)^n\times 0.005)$ とおく. このとき, $b_{2k+1}=a_{2k+1}$ かつ $b_{2k}=a_n-0.01$ となる. だから, 奇数の $n$ についてのみ $b_n$ を考えれば $\alpha=\sqrt{2}$ に収束するが, 偶数の $n$ に対する $b_n$ は $\sqrt{2}$ よりも $0.01$ 以上小さくなる. だから, どんなに番号 $N$ を大きくしても $b_N,b_{N+1},b_{N+2},\ldots$ の中に $\alpha=\sqrt{2}$ との差が $0.01$ 以上になるものが存在する. これは $b_n$ が $\alpha=\sqrt{2}$ に収束しないということを意味している. $\QED$


**問題:** 数列 $a_n$ が $\alpha$ に収束することと, 次の条件が同値であることを示せ:

(☆) 任意の $\eps>0$ に対して, $a_n$ と $\alpha$ の距離が $\eps$ 以上になる $n$ 達の個数は有限個しかない.

数列 $a_n$ が $\alpha$ に収束しないことと, 次の条件が同値であることを示せ:

(★) ある $\eps>0$ で, $a_n$ と $\alpha$ からの距離が $\eps$ 以上になる $n$ 達の個数が無限個になるものが存在する.

**注意:** 同値なのでこれらの条件を収束する, しないの定義として採用してもよい.

**解答例:** $a_n\to\alpha$ と仮定し, 任意に $\eps>0$ を取る. 収束の定義より、ある番号 $N$ が存在して, $n\geqq N$ ならば $|a_n-\alpha|<\eps$ となる. そのとき, $|a_n-\alpha|\geqq\eps$ ならば $n<N$ となるので, $\alpha$ からの距離が $\eps$ 以上の $a_n$ の個数は $N$ 未満の有限個になる. これで(☆)が示された.

条件(☆)を仮定し, $\eps>0$ であるとする. $\alpha$ から距離が $\eps$ 以上の $a_n$ 達の個数は有限個なので, そのような $n$ の最大値が存在する. その最大値を $N-1$ と書くと, $n\geqq N$ のとき $a_n$ と $\alpha$ の距離は $\eps$ 未満になる.  これで $a_n\to\alpha$ が示された.

条件(★)は条件(☆)の否定そのものであり, 条件(☆)は $a_n$ が $\alpha$ に収束することの定義と同値だったので, 条件(★)は $a_n$ が $\alpha$ に収束しないことと同値になる. 

これで示すべきことがすべて示されたが, 条件(★)と「ある $\eps>0$ が存在して, 任意の番号 $N$ に対して, ある $n\geqq N$ で $|a_n-\alpha|\geqq\eps$ を満たすものが存在する」が同値であることも示しておこう.

ある $\eps>0$ が存在して, 任意の番号 $N$ に対して, ある $n\geqq N$ で $|a_n-\alpha|\geqq\eps$ を満たすものが存在すると仮定する. もしも $|a_n-\alpha|\geqq\eps$ を満たす $n$ 達が有限個しかないとすると, そのような $n$ の最大値が存在し, そのとき $n$ がその最大値より大きければ常に $|a_n-\alpha|<\eps$ となってしまい仮定に反する。ゆえに $|a_n-\alpha|\geqq\eps$ を満たす $n$ 達は無限個存在する. これで条件(★)が示された.

条件(★)を仮定する. すなわち, ある $\eps>0$ で, $a_n$ と $\alpha$ からの距離が $\eps$ 以上になる $n$ 達の個数が無限個になるものが存在すると仮定する.  任意に $N$ を取る. もしも $n\geqq N$ ならば常に $|a_n-\alpha|<\eps$ となるならば, $|a_n-\alpha|\geqq\eps$ となる $n$ 達の個数が $N-1$ 以下になってしまうので, 仮定に反する. ゆえに, ある $n\geqq N$ で $|a_n-\alpha|\geqq\eps$ となるものが存在する. これで, 「ある $\eps>0$ が存在して, 任意の番号 $N$ に対して, ある $n\geqq N$ で $|a_n-\alpha|\geqq\eps$ を満たすものが存在する」という条件が示された. $\QED$


**問題(三角不等式):** 実数 $A,B$ について

$$
|A\pm B|\leqq |A|+|B|, \quad |A\pm B|\geqq |A|-|B|, \quad |A\pm B|\geqq |B|-|A|
$$

となることを示せ. 

**解答例:**
$$
\begin{aligned}
&
(|A|+|B|)^2 - |A\pm B|^2 =
|A|^2+2|A||B|+|B|^2 - (A^2\pm 2AB+B^2) =
2(|A||B| \mp AB)\geqq 0,
\end{aligned}
$$

より, $|A\pm B|\leqq |A|+|B|$ が得られる. その不等式で $A$ を $-(A\pm B)$ で置き換えると, $|A|\leqq |A\pm B|+|B|$ が得られるので, $|A\pm B|\geqq |A|-|B|$ が得られる. その不等式で $A$ と $B$ の立場を取り換えると, $|A\pm B|\geqq |B|-|A|$ が得られる. $\QED$

**三角不等式のよくある使い方:** $|A-B|$ が小さいことを示すために, 三角不等式を

$$
|A-B| = |A-C+C-B|\leqq |A-C|+|C-B|
$$

の形で用い, $|A-C|$ と $|C-B|$ が小さいことを示すという手段が非常によく使われる. ここで使われる $C$ は初等幾何の問題における補助線のようなものであり, うまい $C$ を見付けることが証明のポイントになることが多い. $\QED$

**三角不等式は空気のごとく多用される.**


**問題:** 数列 $a_n$ が $\alpha$ と $\beta$ に収束しているならば, $\alpha=\beta$ となることを示せ. (これは同一の数列の収束先は唯一であることを意味している.)

**解答例1:** $n\to\infty$ とすると,

$$
|\beta-\alpha|=|\beta-a_n+a_n-\alpha|\leqq |\beta-a_n|+|a_n-\alpha|\to 0.
$$

ゆえに $|\beta-\alpha|=0$ となるので, $\beta=\alpha$. $\QED$

**解答例2:** 任意に $\eps>0$ を取る. $a_n$ は $\alpha$ に収束しているので, ある番号 $N'$ が存在して, $n\geqq N$ ならば $|a_n-\alpha|<\eps/2$ となる. $a_n$ は $\beta$ にも収束しているので, ある番号 $N''$ が存在して, $n\geqq N''$ ならば $|a_n-\beta|<\eps/2$ となる. ゆえに, $N=\max\{N',N''\}$ とおくと, $n\geqq N$ のとき,

$$
|\beta-\alpha| =
|(a_n-\alpha)-(a_n-\beta)| \leqq
|a_n-\alpha| + |a_n-\beta|
< \frac{\eps}{2}+\frac{\eps}{2} = \eps.
$$

ここで $\leqq$ では三角不等式を使った. $\eps>0$ はいくらでも小さくできるので, $\alpha=\beta$ となる. $\QED$

**解説:** 以上の解答例は等式 $\beta=\alpha$ を示すために不等式を使っている. 等式を不等式を使って証明することは解析学における典型的な議論の仕方だと言ってよい.  $\alpha$ と $\beta$ が等しいことを示すためには, それらの差がどんな正の実数よりも小さいことを示せよばよい. $\alpha$ と $\beta$ の差は三角不等式によって, 

$$
|\beta-\alpha| =
|(a_n-\alpha)-(a_n-\beta)| \leqq
|a_n-\alpha| + |a_n-\beta|
$$

と評価される. これは $a_n$ と $\alpha$ の差と $a_n$ と $\beta$ の差が同時に小さくなれば $\alpha$ と $\beta$ の差も小さくなることを意味している. $a_n$ が $\alpha$ と $\beta$ に収束するならば, $a_n$ と $\alpha$ の差も $a_n$ と $\beta$ の差も同時に小さくできるので, 示したいことが示される. $\QED$


**問題:** 収束する実数列 $a_n,b_n$ が全ての番号 $n$ について $a_n \leqq b_n$ を満たしているならば, $\ds\lim_{n\to\infty} a_n\leqq\lim_{n\to\infty}b_n$ が成立することを示せ.

**解答例1:** $a_n,b_n$ の収束先をそれぞれ $\alpha,\beta$ と書くことにする. 一般に実数 $A$ について $-|A|\leqq A\leqq |A|$ が成立しているので,

$$
a_n = a_n-\alpha+\alpha \geqq -|a_n-\alpha|+\alpha, \quad
b_n = b_n-\beta+\beta \leqq |b_n-\beta|+\beta
$$

なので $a_n\leqq b_n$ なので

$$
-|a_n-\alpha|+\alpha \leqq a_n\leqq b_n\leqq |b_n-\beta|+\beta.
$$

ゆえに

$$
\alpha\leqq \beta + |a_n-\alpha| + |b_n-\beta|.
$$

$n\to\infty$ とすると, $|a_n-\alpha|\to 0$, $|b_n-\beta|\to 0$ となるので, $\alpha\leqq\beta$ を得る. $\QED$

**注意:** 上の証明中で, 全ての番号 $n$ について $a_n<b_n$ が成立していれば

$$
-|a_n-\alpha|+\alpha \leqq a_n< b_n\leqq |b_n-\beta|+\beta
$$

となるので,

$$
\alpha < \beta + |a_n-\alpha| + |b_n-\beta|.
$$

となるが, この不等式で $n\to\infty$ としても, $\alpha<\beta$ となるとは限らないことに注意せよ. 実際には $\alpha=\beta$ だが $|a_n-\alpha| + |b_n-\beta|$ が常に正であるおかげで, $\alpha < \beta + |a_n-\alpha| + |b_n-\beta|$ となっているかもしれないからである. 例えば $\ds a_n=-\frac{1}{n}$, $\ds b_n=\frac{1}{n}$ のとき, $a_n<b_n$ となっているが, $a_n\to 0$, $b_n\to 0$ となっている. $\QED$

**解答例2:** $a_n,b_n$ の収束先をそれぞれ $\alpha,\beta$ と書くことにする. 任意に $\eps>0$ を取る. $a_n$ は $\alpha$ に収束するので, ある番号 $N$ が存在して, $n\geqq N$ ならば $|a_n-\alpha|<\eps$, 特に $\alpha + \eps < a_n$ が成立している. $b_n$ は $\beta$ に収束するので, ある番号 $N'$ が存在して, $n\geqq N'$ ならば $|b_n-\beta|<\eps$, 特に $b_n < \beta + \eps$ が成立している. そのとき, $n\geqq\max\{N,N'\}$ ならば

$$
\alpha-\eps < a_n \leqq b_n < \beta + \eps
$$

となるので

$$
\alpha < \beta + 2\eps
$$

となる.

$\eps>0$ は幾らでも小さくできるので, $\alpha\leqq\beta$ が成立している. (もしも $\alpha>\beta$ ならば $\eps = (\alpha-\beta)/2 > 0$ のとき $\alpha < \beta+2\eps=\alpha$ となって矛盾する.) $\QED$

**注意:** 収束する実数列 $a_n,b_n$ が全ての番号 $n$ について $a_n < b_n$ を満たしていても, $\ds\lim_{n\to\infty} a_n < \lim_{n\to\infty}b_n$ が成立するとは限らない. 例えば $a_n=-1/n$, $b_n=1/n$ は $a_n<b_n$ を満たしているが, どちらも $0$ に収束する. 

このように $\leqq$ の型の不等号は極限で保たれるが, $<$ の型の不等号は保たれるとは限らない. この意味で証明中で極限操作を多用する場合には $\leqq$ 型の不等号を多用した方が便利な場合がある. $\QED$


**問題:** 数列 $a_n,b_n$ がそれぞれ $\alpha,\beta$ に収束するとき, 数列 $a_n+b_n$ が $\alpha+\beta$ に収束することを証明せよ.

**解答例1:** 三角不等式より, $n\to\infty$ とすると,

$$
|(a_n+b_n)-(\alpha+\beta)| = 
|(a_n-\alpha)+(b_n-\beta)| \leqq
|a_n-\alpha|+|b_n-\alpha| \to 0.
$$

ゆえに $|(a_n+b_n)-(\alpha+\beta)|\to 0$. すなわち $a_n+b_n\to\alpha+\beta$. $\QED$

**解答例2:** 任意に $\eps>0$ を取る. $a_n$ は $\alpha$ に収束するので, ある番号 $N'$ が存在して, $n\geqq N'$ ならば $|a_n-\alpha|<\eps/2$ となる. $b_n$ は $\beta$ に収束するので, ある番号 $N''$ が存在して, $n\geqq N''$ ならば $|b_n-\beta|<\eps/2$ となる. ゆえに, $N=\max\{N',N''\}$ とおくと, $n\geqq N$ ならば,

$$
\begin{aligned}
|(a_n+b_n)-(\alpha+\beta)| &= 
|(a_n-\alpha)+(b_n-\beta)| 
\\ &\leqq
|a_n-\alpha|+|b_n-\alpha| < 
\frac{\eps}{2}+\frac{\eps}{2} = \eps.
\end{aligned}
$$

ここで $\leqq$ では三角不等式を使った. これで $a_n+b_n$ が $\alpha+\beta$ に収束することが示された. $\QED$

**解説:** $a_n+b_n$ が $\alpha+\beta$ に収束することを示すためにはそれらの差が適当な条件のもとで適切な意味で小さくなることを示せばよい. それらの差は三角不等式によって

$$
|(a_n+b_n)-(\alpha+\beta)| = 
|(a_n-\alpha)+(b_n-\beta)| \leqq
|a_n-\alpha|+|b_n-\alpha| 
$$

を満たしている. これは $a_n$ と $\alpha$ の差と $b_n$ と $\beta$ の差が両方小さくなれば, $a_n+b_n$ と $\alpha+\beta$ の差も小さくなることを意味している. こういう当たり前の議論を $\eps$-$N$ 論法の言葉で書き直せば上の問題の解答が得られる. $\QED$


**問題:** $a_n,b_n$ がそれぞれ $\alpha,\beta$ に収束するならば $a_n-b_n$ が $\alpha-\beta$ に収束することを示せ. $\QED$

上の問題と本質的に同じ証明が可能なので解答は略す.


**問題:** 数列 $a_n$ がある値に収束しているならば, ある正の実数 $M$ で $|a_n|\leqq M$ を満たすものが存在することを示せ. (この事実を**収束する数列は有界である**という.)

**解答例:** $a_n$ の収束先は $\alpha$ であるとする. ある番号 $N$ が存在して, $n\geqq N$ ならば $|a_n-\alpha|\leqq 1$ となるので, 

$$
|a_n|=|a_n-\alpha+\alpha|\leqq|a_n-\alpha|+|\alpha| \leqq 1+|\alpha|.
$$

ゆえに $M=\max\{|a_1|,\ldots,|a_{N-1}|,1+|\alpha|\}$ とおくと, すべての番号 $n$ について $|a_n|\leqq M$ となる. $\QED$


**例:** $\ds a_n=10+\frac{1000}{n}$ である場合に上の解答例の議論を適用してみよう. そのとき, $n\geqq 1000$ とすると, $\ds|a_n-10|=\frac{1000}{n}\leqq \frac{10}{1000}\leqq 1$ となり, $|a_n|\leqq 1+10=11$ となる. $a_1=1010$, $a_2=510$, $\ldots$, $a_{999}=10+1000/999$, $1+10=11$ の最大値は $1010$ である. ゆえにすべての番号 $n$ について $|a_n|\leqq 1010$. $\QED$ 


**問題:** 数列 $a_n,b_n$ がそれぞれ $\alpha,\beta$ に収束するとき, 数列 $a_n b_n$ が $\alpha\beta$ に収束することを示せ.

**解答例1:** 収束する数列は有界なので, ある正の実数 $M$ ですべての番号 $n$ について $|a_n|\leqq M$ を満たすものが存在する. 三角不等式より, $n\to\infty$ のとき

$$
\begin{aligned}
|a_n b_n - \alpha\beta| &= 
|a_n b_n - a_n\beta + a_n\beta - \alpha\beta| =
|a_n(b_n - \beta) + (a_n-\alpha)\beta|
\\ &\leqq 
|a_n||b_n-\beta|+|a_n-\alpha||\beta| \leqq 
M|b_n-\beta|+|a_n-\alpha||\beta| \to 0
\end{aligned}
$$

となるので, $a_n b_n \to \alpha\beta$ となる. $\QED$

**解答例2:** $a_n b_n$ と $\alpha\beta$ の差は三角不等式によって

$$
\begin{aligned}
|a_n b_n - \alpha\beta| &= 
|a_n b_n - a_n\beta + a_n\beta - \alpha\beta| =
|a_n(b_n - \beta) + (a_n-\alpha)\beta|
\\ &\leqq 
|a_n||b_n-\beta|+|a_n-\alpha||\beta|
\end{aligned}
$$

と上から評価される. 上の問題の結果より, ある正の実数 $M$ が存在して, すべての番号 $n$ について $|a_n|\leqq M$ となる. $b_n$ は $\beta$ に収束するので, ある番号 $N''$ が存在して, $n\geqq N''$ ならば $|b_n-\beta|<\eps/(2M)$ となる. $a_n$ は $\alpha$ に収束するので, ある番号 $N'$ が存在して, $n\geqq N'$ ならば $|a_n-\alpha|<\eps/(2|\beta|+1)$ となる. そのとき, $n\geqq\max\{N',N''\}$ ならば, 上の評価式より, 

$$
|a_n b_n - \alpha\beta| < M\frac{\eps}{2M}+\frac{\eps}{2|\beta|+1}|\beta| \leqq
\frac{\eps}{2}+\frac{\eps}{2} = \eps.
$$

これで $a_n b_n$ が $\alpha\beta$ に収束することが示された. $\QED$


**問題:** $a_n$ が $\alpha\ne 0$ に収束するならば, $r<|\alpha|$ を満たす任意の実数 $r$ に対して, ある番号 $N$ が存在して, $n\geqq N$ ならば $|a_n|\geqq r$ となることを示せ.

**解答例:** $r<|\alpha|$ より $\eps=|\alpha|-r$ とおくと, $\eps>0$ となり, $a_n$ は $\alpha$ に収束するので, ある番号 $N$ が存在して, $n\geqq N$ ならば, $|a_n-\alpha|<\eps=|\alpha|-r$ となり, 

$$
|a_n| = |a_n-\alpha+\alpha| \geqq |\alpha|-|a_n-\alpha| > r
$$

となる. $\QED$


**例:** $\ds a_n = -1 + (-1)^n \frac{100}{n}$ と $r=0.9$ の場合に上の解答例の議論を適用してみよう. $n\geqq 1001$ とすると $\ds|a_n-(-1)|=\frac{100}{n}\leqq \frac{100}{1001} < 0.1$ となるので,  $\ds |a_n|\geqq|-1| - |a_n-(-1)| > 1-0.1=0.9=r$ となる. $\QED$


**問題:** $a_n$ が $\alpha\ne 0$ に収束しているならば $1/a_n$ は $1/\alpha$ に収束することを示せ.

**解答例1:** $a_n\to\alpha\ne 0$ より, 十分に $n$ を大きくすると $\ds|a_n|\geqq\frac{|\alpha|}{2}$ となる. 三角不等式より, $n\to\infty$ のとき, 

$$
\left|\frac{1}{a_n}-\frac{1}{\alpha}\right| =
\left|\frac{\alpha-a_n}{a_n \alpha}\right| =
\frac{|a_n-\alpha|}{|a_n||\alpha|}\leqq
\frac{|a_n-\alpha|}{(|\alpha|/2)|\alpha|}\to 0.
$$

ゆえに $\ds\frac{1}{a_n}\to\frac{1}{\alpha}$ となる. $\QED$

**解答例2:** $1/a_n$ と $1/\alpha$ の差は

$$
\left|\frac{1}{a_n}-\frac{1}{\alpha}\right| =
\left|\frac{\alpha-a_n}{a_n \alpha}\right| =
\frac{|a_n-\alpha|}{|\alpha|}
$$

をみたしている. 上の問題の結果より, ある番号 $N'$ が存在して, $n\geqq N'$ ならば $|a_n|\geqq |\alpha|/2$ となる. $a_n$ は $\alpha$ に収束しているので, ある番号 $N''$ が存在して, $n\geqq N''$ ならば $|a_n-\alpha|<|\alpha|^2\eps/2$ となる. そのとき, $N=\max\{N',N''\}$ とおくと, $n\geqq N$ ならば

$$
\left|\frac{1}{a_n}-\frac{1}{\alpha}\right| =
\frac{|a_n-\alpha|}{|a_n||\alpha|} < \frac{|\alpha|^2\eps/2}{(|\alpha|/2)|\alpha|} = \eps.
$$

これで $1/a_n$ が $\alpha$ に収束することが示された. $\QED$


**問題:** 数列 $a_n$ が $\alpha$ に収束しているならば

$$
b_n = \frac{a_1+\cdots+a_n}{n}
$$

も $\alpha$ に収束することを示せ.

**ヒント:** $a_n$ を $a_n-\alpha$ で置き換えることによって, $a_n$ が $0$ で収束するならば $b_n$ も $0$ に収束することを示せば十分であることがわかる. $a_n$ が $0$ に収束するならば, 十分大きな $N$ について $a_n$ はほとんど $0$ になるので, $n\geqq N$ のとき加法平均 $(a_N+a_{N+1}+\cdots+a_n)/(n-N+1)$ もほとんど $0$ になるだろう. $N$ を固定したまま $n\geqq N$ を大きくすれば $(a_1+\cdots+a_{N-1})/n$ は $0$ に収束する. $\QED$

**注意:** 上の問題は $\eps$-$N$ 論法を使った方が「易しい」例として有名である.  $\eps$-$N$ 論法を理解しているかどうかのチェックにもよく使われる. 以下の解答例を読まずに自力で証明を考えた方がよいと思われる. $\QED$

**解答例1:** $A_n=a_n-\alpha$ とおくと, 

$$
b_n - \alpha = 
\frac{a_1+\cdots+a_n}{n} -\alpha =
\frac{(a_1-\alpha)+\cdots+(a_n-\alpha)}{n} =
\frac{A_1+\cdots+A_n}{n}.
$$

これの右辺を $B_n$ を書くことにする. これより, $A_n\to 0$ から $B_n\to 0$ を示せば十分であることがわかる. 以下では $A_n$, $B_n$ をそれぞれ $a_n$, $b_n$ と書く. $a_n\to 0$ という仮定のもとで $b_n\to 0$ を示せばよい.

任意に $\eps>0$ を取る. $a_n\to 0$ より, ある番号 $N$ が存在して, $|a_N|,|a_{N+1}|,|a_{N+2}|,\ldots$ がすべて $\ds\frac{\eps}{2}$ 未満になるので, $n\geqq N$ ならば

$$
\begin{aligned}
|b_n|&\leqq \frac{|a_1|+\cdots+|a_n|}{n} 
\\ &=
\frac{|a_1|+\cdots+|a_{N-1}|}{n} + \frac{|a_N|+|a_{N+1}|+\cdots+|a_n|}{n}
\\ &\leqq
\frac{|a_1|+\cdots+|a_{N-1}|}{n} + \frac{|a_N|+|a_{N+1}|+\cdots+|a_n|}{n-N+1}
\\ &<
\frac{|a_1|+\cdots+|a_{N-1}|}{n} + \frac{\eps}{2}.
\end{aligned}
$$

$N'$ は $2(|a_1|+\cdots+|a_{N-1}|)/\eps$ より大きな正の整数であるとする. そのとき, $n\geqq N'$ ならば

$$
\frac{|a_1|+\cdots+|a_{N-1}|}{n} < \frac{|a_1|+\cdots+|a_{N-1}|}{2(|a_1|+\cdots+|a_{N-1}|)/\eps} = 
\frac{\eps}{2}.
$$

以上より, $n\geqq \max\{N,N'\}$ ならば

$$
|b_n| < \frac{\eps}{2}+\frac{\eps}{2} = \eps.
$$

これで $b_n$ が $0$ に収束することが示された. $\QED$


**注意:** 上の問題の逆は成立しない. すなわち $\ds b_n = \frac{a_1+\cdots+a_n}{n}$ が収束していても, $a_n$ が収束しない場合がある. 例えば, $a_n=(-1)^n$ のとき, $a_n$ は振動して収束しないが, $b_n$ は $0$ に収束する. $\QED$


**問題:** 数列 $a_n$ が収束するならば $a_{n+1}-a_n$ も $0$ に収束することを示せ.

**解答例1:** 数列 $a_n$ は $\alpha$ に収束していると仮定する. 三角不等式より, $n\to\infty$ のとき

$$
|a_{n+1}-a_n| = |a_{n+1}-\alpha+\alpha-a_n|\leqq
|a_{n+1}-\alpha|+|\alpha-a_n| \to 0.
$$

これで, $a_{n+1}-a_n$ が $0$ に収束することが示された. $\QED$

**解答例2:** 数列 $a_n$ は $\alpha$ に収束すると仮定し, $\eps>0$ を任意に取る. $a_n$ が $\alpha$ に収束することから, ある番号 $N$ が存在して, $n\geqq N$ ならば $|a_n-\alpha|<\eps/2$ となる. そのとき, $n\geqq N$ ならば

$$
|a_{n+1}-a_n| = |a_{n+1}-\alpha+\alpha-a_n|\leqq
|a_{n+1}-\alpha|+|\alpha-a_n| < \frac{\eps}{2}+\frac{\eps}{2} = \eps.
$$

これで $a_{n+1}-a_n$ が $0$ に収束することが示された. $\QED$


**注意:** 上の問題の逆は成立しない. すなわち $a_{n+1}-a_n\to 0$ であっても $a_n$ が収束しない場合がある. 例えば, $\ds a_n=\frac{1}{1}+\frac{1}{2}+\cdots+\frac{1}{n}$ のとき, $\ds a_{n+1}-a_n=\frac{1}{n+1}$ は $0$ に収束するが, $a_n$ は無限大に発散する. $\QED$


**問題(Cauchy列=基本列):** 数列 $a_n$ が**Cauchy列**もしくは**基本列**であるとは, 任意の $\eps>0$ に対して, ある番号 $N$ が存在して, $m,n\geqq N$ ならば $|a_m-a_n|\leqq \eps$ となることだと定める. 収束する数列はCauchy列であることを示せ.

**解答例:** 本質的に上の問題の解答例と同じ. 数列 $a_n$ は $\alpha$ に収束すると仮定し, $\eps>0$ を任意に取る. $a_n$ が $\alpha$ に収束することから, ある番号 $N$ が存在して, $n\geqq N$ ならば $|a_n-\alpha|<\eps/2$ となる. そのとき, $m,n\geqq N$ ならば

$$
|a_m-a_n| = |a_m-\alpha+\alpha-a_n|\leqq
|a_m-\alpha|+|\alpha-a_n| < \frac{\eps}{2}+\frac{\eps}{2} = \eps.
$$

これで示すべきことが全て示された. $\QED$

**注意1:** 数列 $a_n$ がCauchy列であることを, 「$m,n\to\infty$ のとき $|a_m-a_n|\to 0$ になる」と言うこともある.  その代わりに「$m, n\geqq N$ で $N\to\infty$ のとき $|a_m-a_n|\to 0$ となる」と言う方がよいかもしれない. $\QED$

**注意2(完備性の定義):** $a_n$ が収束する数列ならば $a_n$ はCauchy列になる. 実数や複素数の範囲内ではこれの逆が成立している. すなわち, $a_n$ が実数列(もしくは複素数列)でかつCauchy列ならば, ある実数 $\alpha$ (もしくは複素数 $\alpha$)が存在して $a_n$ は $\alpha$ に収束する.  この結果を「実数(もしくは複素数)全体の集合は**完備**である」という.

実数全体の集合は有理数全体の集合の通常の絶対値による距離に関する**完備化**として定義されていると思ってよい. その意味で実数全体の集合の定義の中には完備性が含まれていると思ってよい. $\QED$


### 級数の収束の定義

(無限)級数 $\ds\sum_{n=1}^\infty a_n$ の収束は数列 $\ds s_n = \sum_{k=1}^n a_k$ の収束で定義される. 収束するとき $\ds \sum_{n=1}^\infty a_n = \lim_{n\to\infty} \sum_{k=1}^n a_k$ と書く.


**例(等比級数):** $|q|<1$, $a_n=q^{n-1}$ のとき

$$
s_n = \sum_{k=1}^n a_k = 1+q+q^2+\cdots+q^{n-1} = \frac{1-q^n}{1-q}
$$

であり, $n\to\infty$ で $q^n\to 0$ なので, $s_n\to 1/(1-q)$ となる. すなわち,

$$
\sum_{m=0}^\infty q^m = \sum_{n=1}^\infty q^{n-1} = \frac{1}{1-q}.
\qquad \QED
$$

**注意:** $\ds1+q+\cdots+q^{n-1}=\frac{1-q^n}{1-q}$ は $q\to 1$ で $n$ に収束する. これを $q$ 数 ($q$-number)と呼び, $(n)_q$ のように書くことがある:

$$
(n)_q = \frac{1-q^n}{1-q}.
$$

さらに $q$ 階乗 ($q$-factorial)や $q$ 二項係数が

$$
(n)_q! = (1)_q(2)_q\cdots(n)_q, \quad
\binom{n}{k}_q = \frac{(n)_q!}{(k)_q!\,(n-k)_q!}
$$

と定義される. このようにして, 数学的対象にパラメーター $q$ を入れて解析を行うことを $q$ 解析と呼ぶ. 20世紀の終わり頃に量子群が発見され, $q$ 解析と深い繋がりがあることが判明した.  $\QED$


**問題:** 級数 $\ds\sum_{n=1}^\infty a_n$ が収束しているならば, $n\to\infty$ で $a_n\to 0$ となることを示せ.

**解答例1:** 級数 $\ds\sum_{n=1}^\infty a_n$ は $\alpha$ に収束していると仮定する. すなわち, $\ds \sum_{k=1}^n a_k$ は $n\to\infty$ で $\alpha$ に収束していると仮定する. このとき, $n\to\infty$ のとき,

$$
|a_n| = \left|\sum_{k=1}^n a_k - \sum_{k=1}^{n-1}a_k\right| =
\left|\sum_{k=1}^n a_k-\alpha\right|+\left|\sum_{k=1}^{n-1}a_k-\alpha\right| \to 0.
$$

これで $a_n\to 0$ であることが示された. $\QED$

**解答例2:** 級数 $\ds\sum_{n=1}^\infty a_n$ は $\alpha$ に収束していると仮定し, $\eps >0$ を任意に取る. このとき, ある番号 $N$ が存在して, $n\geqq N$ ならば $\ds\left|\sum_{k=1}^n a_k - \alpha\right|<\frac{\eps}{2}$ となる. ゆえに $n-1\geqq N$ のとき, 

$$
|a_n| = \left|\sum_{k=1}^n a_k - \sum_{k=1}^{n-1}a_k\right| =
\left|\sum_{k=1}^n a_k-\alpha\right|+\left|\sum_{k=1}^{n-1}a_k-\alpha\right| <
\frac{\eps}{2} + \frac{\eps}{2} = \eps.
$$

これで, $a_n\to 0$ となることが示された. $\QED$


**問題:** 上の問題の逆が成立しないことを示せ. すなわち, $a_n\to 0$ であっても, $\ds\sum_{n=1}^\infty a_n$ が収束するとは限らないことを示せ.

**解答例:** $a_n=1/n$ とおくと, $a_n\to 0$ である. しかし, $n\leqq x\leqq n+1$ のとき $1/x\leqq 1/n$ なので, 

$$
\log(k+1)-\log k = \int_k^{k+1}\frac{dx}{x} \leqq \int_k^{k+1}\frac{1}{k}\,dx = \frac{1}{k} = a_k
$$

となるので, 両辺を $k=1,\ldots,n$ について足し上げてると,  

$$
\log(n+1) \leqq \sum_{k=1}^n \frac{1}{k} = \sum_{k=1}^n a_k.
$$

これより, $n\to\infty$ で $\ds \sum_{k=1}^n a_k\to\infty$ となることがわかる. $\QED$

```julia
# 1/1+1/2+...+1/n と log n の比較
# nを大きくするとそれらの差はEuler定数 γ = 0.5772… に近付く
# ゆえに, 1/1+1/2+...+1/n は log n + γ でよく近似される.
# 以下のプロットでもそれらのグラフはほとんど一致している.

f(x) = sum(k->1/k, 1:floor(Int, x))
n = 1:100
x = 1:0.1:100
plot(size=(500,350), legend=:bottomright)
plot!(n, f.(n), label="1/1+1/2+...+1/n", lw=2.5)
plot!(x, log.(x), label="log(x)", ls=:dashdot, color=:orange)
plot!(x, log.(x.+1), label="log(x+1)", ls=:dashdot, color=:darkgreen)
plot!(x, log.(x).+eulergamma, label="log(x) + Euler's gamma", lw=2, ls=:dash, color=:red)
```

```julia
?eulergamma
```

### 連続的な極限

函数の $x$ における値 $f(x)$ が $x\to a$ で $\alpha$ に**収束する**とは, 任意の $\eps>0$ に対して, ある $\delta > 0$ が存在して, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|<\eps$ が成立することである.

$f(x)$ が $x\to a$ で $\alpha$ に収束するとき, その極限の値を $\displaystyle \lim_{x\to a} f(x) = \alpha$ と定義する.  収束しないときには極限の値は定義されない.


**問題:** 上の定義における $|f(x)-\alpha|<\eps$ を $|f(x)-\alpha|\leqq\eps$ に置き換えても同値な条件になることを示せ. $\QED$

**解答例:** 以下の2条件が互いに同値であることを示そう.

(1) 任意の $\eps>0$ に対して, ある $\delta > 0$ が存在して, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|<\eps$ が成立する.

(2) 任意の $\eps>0$ に対して, ある $\delta > 0$ が存在して, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|\leqq \eps$ が成立する.

ついでに任意の正の実数 $M$ について以下も同値であることも示してしまおう.

(3) 任意の $\eps>0$ に対して, ある $\delta > 0$ が存在して, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|\leqq M\eps$ が成立する.

(1) $\implies$ (2): (1)を仮定して, 任意に $\eps>0$ を取る. (1)より, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|< \eps$ が成立し, そのとき $|f(x)-\alpha|\leqq\eps$ が自明に成立している. これで(1)から(2)が自明に導かれることがわかった.

(2) $\implies$ (3): (2)を仮定して, 任意に $\eps>0$ を取る. (2)の $\eps$ として $M\eps$ を採用すると, ある $\delta > 0$ が存在して, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|\leqq M\eps$ が成立する. これで(2)から(3)も自明に導かれることがわかった.

(3) $\implies$ (1): (3)を仮定して, 任意に $\eps>0$ を取る. (3)の $\eps$ として $\eps/(2M)$ を採用すると, ある ある $\delta > 0$ が存在して, $0<|x-a|<\delta$ を満たすすべての $x$ について $|f(x)-\alpha|\leqq M(\eps/(2M))=\eps/2<\eps$ が成立する. これで(3)から(1)が導かれることがわかった.

以上によって(1),(2),(3)が互いに同値であることがわかった. $\QED$


**問題:** さらに $<\delta$ の部分を $\leqq\delta$ に変更しても同値であることを示せ. \QED

上の解答例と完全に同じ考え方で証明できるので解答略.


## 収束の判定法の基本


### 挟撃法 (挟み撃ち)

**定理:** $A_n \leqq a_n \leqq B_n$ でかつ $A_n$ と $B_n$ が $\alpha$ に収束するならば $a_n$ も $\alpha$ に収束する. 

**証明:** $\eps>0$ を任意に取る. $A_n\to\alpha$ より, ある番号 $N'$ が存在して, $n\geqq N'$ ならば $\ds|A_n-\alpha|<\eps$, 特に $\ds\alpha-\eps<A_n$ となる. $B_n\to\alpha$ より, ある番号 $N''$ が存在して, $n\geqq N''$ ならば $\ds|A_n-\alpha|<\eps$, 特に $\ds B_n<\alpha+\eps$ となる. ゆえに $N=\max\{N',N''\}$ とおくと, $n\geqq N$ のとき,

$$
\alpha - \eps < A_n \leqq a_n \leqq B_n < \alpha+\eps
$$

なので $|a_n-\alpha|<\eps$ となる. これで $a_n\to\alpha$ が示された. $\QED$


**定理:** $A_n \leqq a_n$ でかつ $A_n\to\infty$ ならば $a_n\to\infty$ となる. 

**証明:** $M$ は任意の実数であるとする. $A_n\to\infty$ より, ある番号 $N$ が存在して, $n\geqq N$ ならば $A_n\geqq M$ となり, $a_n\geqq A_n\geqq M$ となる. これで $a_n\to\infty$ を示せた. $\QED$


**注意:** 挟撃法は「$|a_n|\leqq A_n$ かつ $A_n\to 0$ ならば $a_n\to0$ となる」の形式で使われることが非常に多い. したがって, 挟撃法を便利に利用するためには $0$ に収束する $A_n$ の例をたくさん知っておくことが必要である. $\QED$


**例:** $n\to\infty$ のとき $a_n\to 0$ かつ $b_n\to\beta$ となっているならば, $a_n b_n\to 0$ となる. 同様に, $x\to 0$ のとき, $f(x)\to 0$ かつ $g(x)\to 0$ となっているならば, $f(x)g(x)\to 0$ となる. この場合は簡単である. 難しいのは $a_n\to 0$ かつ $b_n\to \infty$ となっているときの, $a_n b_n$ の $n\to\infty$ での様子や, $x\to 0$ のとき $f(x)\to 0$ かつ $g(x)\to\infty$ となっているときの, $f(x)g(x)$ の $x\to 0$ での様子を調べることである. $\QED$


**上の例の補足の注意:** $a_n\to\infty$ の定義は, 任意の実数 $M$ に対して, ある番号 $N$ が存在して, $n\geqq N$ ならば $a_n\geqq M$ となることである. $x\to 0$ のとき $f(x)\to\infty$ となることの定義は, 任意の実数 ＄Ｍ＄に対して, ある $\delta>0$ が存在して, $0<|x|<\delta$ ならば $f(x)\geqq M$ となることである. $\QED$


### 二項定理の復習

以下の節では $0$ への収束の判定を主に扱うが, そのための準備として二項定理について復習しておく.

二項係数を

$$
\binom{n}{k} = \frac{n(n-1)\cdots(n-k+1)}{k!}
$$

と定義する. 例えば

$$
\binom{n}{0} = 1, \quad
\binom{n}{1} = n, \quad
\binom{n}{2} = \frac{n(n-1)}{2}, \quad
\binom{n}{3} = \frac{n(n-1)(n-2)}{6}.
$$

$n$ も $k$ も非負の整数のとき $\binom{n}{k}$ は $n$ 個から $k$ 個選び出すときの場合の数に一致している. 高校数学では ${}_nC_k$ のように書いているようだが, 一般には $\binom{n}{k}$ と書くことの方が多いように感じられるので, $\binom{n}{k}$ の方を使用する.  (上の定義に従えば, $k$ が非負の整数でありさえすれば, $n$ が整数でなくても $\binom{n}{k}$ が定義されていることに注意せよ. この事実は二項展開を考えるときに必要になる.)


以上の記号法のもとで, 二項定理は

$$
(x+y)^n = 
\sum_{k=0}^n \binom{n}{k} x^k y^{n-k} =
\sum_{k=0}^n \binom{n}{k} x^{n-k}y^k
\tag{$*$}
$$

と書ける. ２つ目の等号は自明である.

高校数学で証明を習っているはずだが, 忘れている人は二項定理を証明しておくこと. 

**考え方:** 解析学では不等式を扱いたい. しかし, 二項定理のような**良い等式は良い不等式を生み出す**ために非常に役に立つ. 解析学では非自明な等式は非自明な不等式を作る材料になる. 次の節の証明を見よ!


**二項定理の証明1:** $(x+y)^n$ を展開したときに得られる $x^k y^{n-k}$ の係数は $k$ 個の $x$ と $n-k$ 個の $y$ の並べ方の個数に等しい. その個数は全部で $n$ 個並べるうち $k$ 個の $x$ の場所の選び方の個数に等しい. それは

$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}
$$

に等しい. $\QED$


**二項定理の証明2:** $n$ に関する帰納法を使う. $n=0$ の場合に($*$)は成立している. ($*$)が $n$ について成立していると仮定する. そのとき, $k$ が負の整数のとき $\binom{n}{k}=0$ と約束しておくと, 

$$
\begin{aligned}
\binom{n}{k-1} + \binom{n}{k} &= 
\frac{n(n-1)\cdots(n-k+2)}{(k-1)!} + \frac{n(n-1)\cdots(n-k+2)(n-k+1)}{k!}
\\ &=
\frac{n(n-1)\cdots(n-k+2)}{k!}\times(k+(n-k+1))
\\ &=
\frac{n(n-1)\cdots(n-k+2)}{k!}\times(n+1)
\\ &=
\binom{n+1}{k}.
\end{aligned}
$$

ゆえに $n$ に関する帰納法の仮定($*$)を使うと, 

$$
\begin{aligned}
(x+y)^{n+1} &= (x+y)(x+y)^n =
(x+y)\sum_{k=0}^n \binom{n}{k}x^k y^{n-k}
\\ &=
\sum_{k=0}^n \binom{n}{k}x^{k+1}y^{n-k} +
\sum_{k=0}^n \binom{n}{k}x^{k}y^{n+1-k}
\\ &=
\sum_{k=0}^{n+1} \binom{n}{k-1}x^{k}y^{n+1-k} +
\sum_{k=0}^{n+1} \binom{n}{k}  x^{k}y^{n+1-k}
\\ &=
\sum_{k=0}^{n+1} \left(\binom{n}{k-1}+\binom{n}{k}\right)x^{k}y^{n+1-k}
\\ &=
\sum_{k=0}^{n+1} \binom{n+1}{k}x^{k}y^{n+1-k}.
\end{aligned}
$$

これで $n$ を $n+1$ で置き換えた場合の条件($*$)も成立することがわかった. これで示すべきことが示された. $\QED$


**注意:** 二項定理は形式べき級数としての $\ds f(x)=\sum_{k=0}^\infty\frac{x^k}{k!}$ が

$$
f(x+y) = f(x)f(y)
$$

を満たしていることと同値である. なぜならば,

$$
\begin{aligned}
&
f(x+y) = \sum_{n=0}^\infty\frac{1}{n!}(x+y)^n,
\\ &
f(x)f(y) = 
\sum_{k,l=0}^\infty \frac{x^k y^l}{k!l!} =
\sum_{n=0}^\infty \sum_{k=0}^{n-k} \frac{x^k y^{n-k}}{k!(n-k)!} =
\sum_{n=0}^\infty \frac{1}{n!}\sum_{k=0}^{n-k} \binom{n}{k} x^k y^{n-k}.
\end{aligned}
$$

$f(x)$ は $e^{x+y}=e^x e^y$ を満たす指数函数 $e^x$ の $x=0$ におけるTaylor展開に等しい. そのことを使うことを許せば, 二項定理を経由せずに $f(x+y)=f(x)f(y)$ を示せる.  このように, 二項定理のような組み合わせ論的な内容を持つ定理と $e^{x+y}=e^x e^y$ のような特別な函数が満たす特別な関係式が同値になっていることはよくある. $\QED$


### 多項式函数より指数函数の方が速く増加すること

**定理:** $a>1$ ならば $n\to\infty$ のとき $\ds\frac{n^l}{a^n}\to 0$. $\QED$

**証明:** 二項定理を使う.  

$L$ は $l$ より大きな正の整数であるとし, $n$ は $L$ 以上であると仮定する. 

そのとき特に $n\to\infty$ のとき $n^l/n^L\to 0$ が成立している.

$a>1$ なので $a=1+\alpha$, $\alpha>0$ と書ける.  二項定理より

$$
\begin{aligned}
a^n &= (1+\alpha)^n = \sum_{k=0}^n \binom{n}{k}\alpha^k =
\sum_{k=0}^n \frac{n(n-1)\cdots(n-k+1)}{k!}\alpha^2 
\\ &\geqq
\frac{n(n-1)\cdots(n-L+1)}{L!}\alpha^L
= n^L\frac{(1-1/n)\cdots(1-(L-1)/n)\alpha^L}{L!}.
\end{aligned}
$$

ゆえに, $n\to\infty$ のとき

$$
0\leqq\frac{n^l}{a^n} \leqq 
\frac{n^l}{n^L}\times\frac{L!}{(1-1/n)\cdots(1-(L-1)/n)\alpha^L} \to
0\times\frac{L!}{\alpha^L} = 0.
\qquad\QED
$$


**問題:** $a=1.01$, $l=100$ のとき, $n^l$ と $a^n$ の大きさを比較するためのグラフを描いてみよ.

**解答例:** 直接 $n^l$ と $a^n$ を扱うと数値が巨大になり過ぎるので, それらの対数を比較することにする.

```julia
a = 1.01
l = 100
n = 1:10^3:2*10^5
f(n) = l*log(n)
g(n) = n*log(a)
plot(size=(500, 350), legend=:topleft, xlabel="n")
plot!(n, f.(n), label="$l*log(n)")
plot!(n, g.(n), label="n*log($a)")
```

**問題:** $a>1$ ならば, $x>0$ が連続的に幾らでも大きくなるとき $\ds\frac{x^l}{a^x}\to 0$ となることを示せ.

**証明:** 正の整数 $n$ について $n\leqq x \leqq n+1$ のとき, $x^l$ は $n^l$ と $(n+1)^l$ のあいだにあり, $a^x$ は $a^n$ 以上になる. ゆえに, $x\to\infty$ のとき, $n\to\infty$ となり, 

$$
0\leqq
\frac{x^l}{a^x} \leqq \frac{\max\{n^l, (n+1)^l\}}{a^n} \leqq 
\max\left\{ \frac{n^l}{a^n},\; a\frac{(n+1)^l}{a^{n+1}}\right\} \to 0.
$$

最後に上の方の定理を使った. これで示すべきことが示された. $\QED$


**問題:** $x>0$ について, $x\to 0$ のとき $x\log x\to 0$ となることを示せ.

**証明:** $x>0$ なので $x=e^{-t}$, $t\in\R$ と書ける. $x\to 0$ のとき $t\to\infty$ となるので,

$$
x\log x = e^{-t}\log e^{-t} = -\frac{t}{e^t} \to 0.
$$

最後の上の問題の結果を使った. $\QED$


**注意:** 統計学に出て来る計算では $0\log 0 = 0$ と約束しておくことが適切な場合が多い. $\QED$

```julia
f(x) = x*log(x)
x = 0.001:0.001:1
plot(x, f.(x), xlim=(-0.1,1), ylim=(-0.4,0), label="y = x log x", legend=:top)
```

### 指数函数より階乗の方が速く増加すること


**定理:** $a>0$ と仮定する. そのとき $n\to\infty$ ならば $\ds\frac{a^n}{n!}\to 0$ が成立する.

**証明:** $N$ を十分大きくすると $\ds\frac{a}{N+1}<1$ となる. そして, $n>N$ とすると, 

$$
\frac{a^n}{n!} = \frac{a^N}{N!}\frac{a}{N+1}\frac{a}{N+2}\cdots\frac{a}{n}
\leqq \frac{a^N}{N!}\left(\frac{a}{N+1}\right)^{n-N} \to 0 \quad(n\to\infty).
\quad \QED
$$



**問題:** $a>1$ と仮定する. そのとき $n\to\infty$ ならば $\ds\frac{n!}{a^{n^2}}\to 0$ が成立することを示せ.

**解答例:** 指数函数は多項式函数より速く増加するので $\ds\frac{n}{a^n}\to 0$.  ゆえにある番号 $N$ が存在して $n>N$ ならば $\frac{n}{a^n} \geqq \frac{1}{2}$ となる. したがって, $n>N$ のとき, 

$$
\frac{n!}{a^{n^2}} = \frac{n!}{(a^n)n} =
\frac{N!}{(a^n)^N}\frac{N+1}{a^n}\frac{N+2}{a^n}\cdots\frac{n}{a^n} \leqq
\frac{N!}{(a^N)^n}\left(\frac{1}{2}\right)^{n-N}
\to 0 \quad (n\to\infty).
\quad \QED
$$


### まとめ

$n\to\infty$ のとき $\ds\frac{a_n}{b_n}\to 0$ となることを $a_n\prec b_n$ と書くことにする. $a>1$ のとき

$$
1 \prec \cdots \prec \log\log n \prec \log n \prec 
n \prec n^2 \prec\cdots\prec 
a^n \prec n! \prec a^{n^2}\prec\cdots
$$

右に行くほど $n\to\infty$ で速く増加する. このように増大度に階層性があるという認識は解析学における基本になる.


**例:** Stirlingの近似公式より, 

$$
n! = n^n \frac{1}{e^n} \sqrt{n}\;\sqrt{2\pi}\;(1+\eps_n) \quad (\eps_n\to 0)
$$

が成立することが知られている(この一連のノート群の中でも複数回証明される)). 右辺に登場する $n^n$, $e^n$, $\sqrt{n}$, $\sqrt{2\pi}$ は左のものほど $n\to\infty$ で速く増加する.

$\delta_n=\log(1+\eps_n)$ とおき, Stirlingの近似公式の両辺の対数を取ると, 

$$
\log n! = n\log n - n + \frac{1}{2}\log n + \frac{1}{2}\log 2\pi + \delta_n
\quad (\delta_n\to 0).
$$

右辺に登場する $n\log n$, $n$, $\ds\frac{1}{2}\log n$, $\ds\frac{1}{2}\log\sqrt{2\pi}$, $\delta_n$ は左のものほど $n\to\infty$ で速く増加する. $\QED$

次のセルで $\log n!$ がどのように近似されるかをプロットしてみよう. $\QED$

```julia
lfact(n) = lgamma(n+1) # = log n!
f1(n) = n*log(n)
f2(n) = n*log(n) - n
f3(n) = n*log(n) - n + 0.5*log(n)
f4(n) = n*log(n) - n + 0.5*log(n) + 0.5*log(2π)

function plot_logfactorial(n)
    plot(legend=:topleft)
    plot!(n, lfact.(n), label="log n!", lw=1.5)
    plot!(n, f1.(n), label="n log n", ls=:dash)
    plot!(n, f2.(n), label="n log n - n", ls=:dash)
    plot!(n, f3.(n), label="n log n - n + 0.5 log n", ls=:dash)
    plot!(n, f4.(n), label="n log n - n + (1/2)log n + (1/2)log 2pi", color=:red, ls=:dash)
end
```

```julia
plot_logfactorial(1:20)
```

```julia
plot_logfactorial(10:10:1000)
```

```julia
plot_logfactorial(10^16:10^16:10^18)
```

すぐ上のプロットを見れば, $n$ がこのように極めて大きな値であれば $\log n!$ の $\log n^n = n\log n$ による近似もそう悪くないことがわかる.


## 上限と下限と上極限と下極限


### 上限と下限

$Y$ は空でない実数の集合であると仮定する.

実数 $M$ で任意の $y\in Y$ について $y\leqq M$ を満たすものを実数の集合 $Y$ の**上界**(upper bound)と呼ぶ. 実数 $m$ で任意の $y\in Y$ について $m\leqq y$ を満たすものを $Y$ の**下界**(lower bound)と呼ぶ.

$Y$ の上界が1つ以上存在するとき, $Y$ は**上に有界**(bounded from above)であると言い, $Y$ の下界が1つ以上存在するとき, $Y$ は**下に有界**(bounded from below)であると言う.  $Y$ が上に有界かつ下に有界であるとき, $Y$ は**有界**(bounded)であると言う.

次の結果は認めて使うことにする.

**実数の連続性:** 実数の空でない集合 $Y$ が上に有界ならば $Y$ の上界全体の集合の中に最小値が存在する. $\QED$

$Y$ が上に有界なとき, $Y$ の上界全体の集合の最小値を $Y$ の**最小上界**(least upper bound)もしくは**上限**(supremum)と呼び.  同様に, $Y$ が下に有界ならば $Y$ の下界全体の集合の中に最大値が存在する. その最大値を $Y$ の**最大下界**(greatest lower bound)または**下限**(infimum)と呼ぶ. $Y$ の上限と下限をそれぞれ

$$
\sup Y, \quad \inf Y
$$

と書く. 集合 $X$ 上の実数値函数 $f(x)$ による集合 $X$ の像 $f(X)=\{\,f(x)\mid x\in X\,\}$ の上限と下限をそれぞれ

$$
\sup f(X) = \sup_{x\in X}f(x), \quad \inf f(X) = \inf_{x\in X}f(x)
$$

と書く. $f(X)$ が上に(もしくは下)に有界なとき, 函数 $f$ は $X$ において上に有界(もしくは下)に有界であるという.

$Y$ が上に有界でないときには $Y$ は(有限な)上限を持たないが, そのとき $\sup Y = \infty$ と書くことがある. 同様に $Y$ が下に有界でないときには $Y$ は(有限な)下限を持たないが, そのとき $\inf Y=-\infty$ と書くことがある.

$Y$ に最大値 $\max Y$ が存在するとき, $\sup Y = \max Y$ となる. 同様に $Y$ に最小値 $\min Y$ が存在するとき, $\inf Y = \min Y$ となる.

**上限と下限の便利なところ:** $X$ に最大値や最小値が存在しなくても, 上限と下限は値として $\pm\infty$ を許せば常に存在する. 上限と下限は, 最大値や最小値が存在しないときに, それらに代わるものとして用いることができる場合が結構ある. $\QED$


**例:** 開区間 $Y = (0,1)$ には最小値も最大値も存在しない. $Y=(0,1)$ のとき $\inf Y = 0$, $\sup Y = 1$ となる. $\QED$

**例:** $Y=\{1,1/2,1/3,\ldots\}$ には最大値 $1$ は存在するが, 最小値は存在しない. このとき, 

$$
\sup Y = \sup_{n=1,2,3,\ldots}\frac{1}{n} = \max_{n=1,2,3,\ldots}\frac{1}{n} = 1, \quad
\inf Y = \inf_{n=1,2,3,\ldots}\frac{1}{n} = 0.
\qquad \QED
$$

**例:** $Y = \{1,2,3,\ldots\}$ は上に有界でないので有限の上限を持たない. 下限は最小値の $1$ になる:

$$
\sup\{1,2,3,\ldots\} = \infty, \quad
\inf\{1,2,3,\ldots\} = \min\{1,2,3,\ldots\} = 1.
$$


### 上極限と下極限

この節はすぐに読む必要はない. 上極限と下極限が必要になったときに読めば十分である.

実数の集合 $X$ とその部分集合 $Y$ を考える. $X$ の上界は $Y$ の上界にもなるので, $X$ の上界の最小値は $Y$ の上界の最小値以上になる. すなわち, $\sup X\geqq \sup Y$ となる(これは図を描いて考えれば明らかだろう).  部分集合の上限はもとの集合の上限以下になる.  同様に部分集合の下限はもとの集合の下限以上になる. 

したがって, 実数列 $a_n$ に対して, 数列 $\ds\alpha_n = \sup_{k\geqq n} a_k$ は単調減少し, 数列 $\ds\beta_n = \inf_{k\geqq n} a_k$ は単調増加する. ゆえに $\pm\infty$ も収束先に含めれば, それらの数列は常に収束することになる.  それぞれの収束先を次のように書く:

$$
\begin{aligned}
&
\limsup_{n\to\infty} a_n = \lim_{n\to\infty}\sup_{k\geqq n} a_k = \inf_{n\geqq 1}\sup_{k\geqq n} a_k,
\\ &
\liminf_{n\to\infty} a_n = \lim_{n\to\infty}\inf_{k\geqq n} a_k = \sup_{n\geqq 1}\inf_{k\geqq n} a_k.
\end{aligned}
$$

これらをそれぞれ**上極限**(limit superior), **下極限**(limit inferior)と呼ぶ. 

それぞれを $\liminf a_n$, $\limsup a_n$ と略して書くこともある.

自明に $\ds\inf_{k\geqq n} a_k\leqq \sup_{k\geqq n} a_k$ が成立しているので, 

$$
\liminf_{n\to\infty} a_n \leqq \limsup_{n\to\infty} a_n
$$

が成立している.


**定理:** 実数列 $a_n$ について

$$
-\infty < \liminf_{n\to\infty} a_n = \limsup_{n\to\infty} a_n < \infty
$$

が成立しているならば, 実数列 $a_n$ は収束して,

$$
\lim_{n\to\infty}a_n = \liminf_{n\to\infty} a_n = \limsup_{n\to\infty} a_n.
$$

**証明:** $\alpha = \liminf_{n\to\infty} a_n = \limsup_{n\to\infty} a_n$ とおき, 任意に $\eps>0$ と取って固定する.  $\alpha = \liminf_{n\to\infty} a_n$ より, $\inf_{j\geqq n} a_j$ が $n$ について単調増加することに注意すれば, ある番号 $N'$ が存在して, 

$$
\alpha - \eps \leqq \inf_{j\geqq n} a_j \leqq \alpha \quad (n\geqq N').
$$

同様に, $\sup_{j\geqq n} a_j$ が $n$ について単調減少することに注意すれば, ある番号 $N''$ が存在して, 

$$
\alpha \leqq \sup_{j\geqq n} a_j \leqq \alpha+\eps \quad (n\geqq N'').
$$

ゆえに $n=\max\{N',N''\}$ のとき,  $k\geqq n$ ならば

$$
\alpha - \eps \leqq \inf_{j\geqq n} a_k \leqq a_k\leqq \sup_{j\geqq n} a_j \leqq \alpha+\eps.
$$

これは $k\geqq n$ のとき, $|a_k - \alpha|\leqq\eps$ が成立することを意味する. したがって, 数列の収束の定義より, 数列 $a_n$ は $\alpha$ に収束する. これで示すべきことが示された. $\QED$


**上極限と下極限の便利な点:** $\lim a_n$ と違って, $\liminf a_n$ と $\limsup a_n$ は収束先として $\pm\infty$ を含めれば常に収束しているので収束性を確認せずに気軽に使うことができる.  そして上の定理より, $\liminf a_n$ と $\limsup a_n$ が一致していれば, $a_n$ は収束していて収束先はそれらと同じになる.  

だから, 以下のような使われ方をすることが多い. 実数列 $a_n$ に対して, 任意の $\eps>0$ に対して, それに依存して決まる別の数列 $A_n^{(\eps)}$ と $B_n^{(\eps)}$ で同じ値 $\alpha$ に収束するものが存在して, さらに

$$
A_n^{(\eps)} - \eps \leqq a_n \leqq B_n^{(\eps)} + \eps
$$

が成立していると仮定する. このとき, $n\to\infty$ とすると,

$$
\alpha-\eps \leqq\liminf_{n\to\infty}a_n\leqq\limsup_{n\to\infty}a_n\leqq \alpha+\eps.
$$

$\eps>0$ は幾らでも小さくできるので,

$$
\liminf_{n\to\infty}a_n = \limsup_{n\to\infty}a_n = \alpha
$$

となる. ゆえに数列 $a_n$ は $\alpha$ に収束する:

$$
\lim_{n\to\infty} a_n = \alpha.
$$

「$n\to\infty$ とすると」の段階では数列 $a_n$ が収束するかどうかわかっていないので, $\lim$ ではなく, $\liminf$ と $\limsup$ を使わなければいけない. $\QED$


**問題:** 以下の実数列の上極限を求めよ.

(1) $a_n = 10/n + (-1)^n$.

(2) $b_n = 10/n + \sin(4n)$. 

$\pi$ が無理数であることを認めて使ってよい.

**解答例:** (1) $\limsup a_n = \limsup(10/n + (-1)^n) = 1$.

(2) $\pi$ は無理数なので $a = 4/(2\pi)$ も無理数である. そのとき, $\sin(4n)=\sin(2\pi na)$. 次の問題より, 任意の $k>0$ に対して, ある正の整数 $n_k$ で $n_k a$ の小数点以下の部分と $1/4$ の距離が $1/k$ 以下になるものが存在する. そのとき, $n_ka$ の小数点以下の部分は $1/4$ に収束するので, $k\to\infty$ で $\sin(2\pi n_k a)\to \sin(2\pi(1/4))=1$ となる. これより, $\limsup b_n = 1$ となることがわかる. $\QED$


**問題:** 実数 $x$ に対して $x$ 以下の最大の整数を $\lfloor x\rfloor$ と書き, $f(x)=x-\lfloor x\rfloor$ とおく. $f(x)$ は $x$ の小数点以下の部分になっている. $a$ は無理数であるとする. そのとき, 任意の $\alpha\in[0,1)$ と $\eps>0$ に対して, ある正の整数 $n$ で $f(na)$ と $\alpha|$ の距離が $\eps$ 以下になるものが存在することを示せ.

**解答例:** $a$ は無理数なので $a,2a,3a,\ldots$ は決して整数にはならない. そして, $f(a), f(2a), f(3a),\ldots$ の中の任意の2つは決して一致しない. なぜならば, もしも $f(ma)=f(na)$, $m<n$ ならば $na-ma=(n-m)a$ が整数になって矛盾するからである.) だから, $\{f(a), f(2a), f(3a),\ldots\}$ は有限半開区間 $[0,1)$ の無限部分集合になる. ゆえにある正の整数の組 $m<n$ で $f(ma)$ と $f(na)$ の距離が $\eps$ 以下になるものが存在する. そのとき, $(n-m)a$ とそれに最も近い整数のあいだの距離は $\eps$ 以下になる. このとき, $f(k(n-m))$, $k=1,2,3,\ldots$ は $[0,1)$ の中に刻み幅が $\eps$ 以下の等差数列を作るので, 正の整数 $k$ をうまく取れば $f(k(n-m)a)$ と $\alpha$ の距離を $\eps$ 以下にできる. $\QED$

```julia
nmax = 100
n = 1:nmax
plot(n, @.(10/n + (-1)^n), label="a_n", ylims=(-1.5,10), marker=:o, markersize=3, markerstrokealpha=0)
plot!(n, fill(1, size(n)), label="1")
```

```julia
nmax = 200
n = 1:nmax
plot(n, @.(10/n + sin(4*n)), label="b_n", ylims=(-1.5, 10), marker=:o, markersize=2, markerstrokealpha=0)
plot!(n, fill(1, size(n)), label="1")
```

**問題(上極限の特徴付け):** $a_n$ は実数列であるとし, $\ds\alpha = \limsup_{n\to\infty}a_n$ とおく. 以下を示せ.

(1) $\alpha > A$ のとき, 任意の番号 $n$ に対して, ある $k\geqq n$ で $a_k \geqq A$ を満たすものが存在する($a_k\geqq A$ を満たす $k$ が無限個存在する).

(2) $\alpha < B$ のとき, ある番号 $N$ が存在して, $k\geqq N$ ならば $a_k\leqq B$ となる(ある番号から先のすべての $k$ について $a_k\leqq B$ が成立している).

逆に(1),(2)を $\alpha$ が満たしていれば, $\ds\alpha = \limsup_{n\to\infty}a_n$ となることも示せ.

**解答例:** (1)を示そう. $\ds \alpha=\inf_{n\geqq 1}\sup_{k\geqq n} a_k$ より, 任意の番号 $n$ について $\ds \sup_{k\geqq n}a_k\geqq\alpha$ となる. ゆえに, $\alpha > A$ という仮定より, 各 $n$ ごとにある $k\geqq n$ で $a_k\geqq A$ を満たすものが存在する. 

(2)を示そう. $\alpha < B$ と $\ds\sup_{k\geqq n}\to \alpha$ より, ある番号 $N$ が存在して, $n\geqq N$ ならば $\ds\sup_{k\geqq n}a_k\leqq B$ すなわち $a_k\leqq B$ ($k\geqq n$) となる. ゆえに $k\geqq N$ ならば $a_k\leqq B$ となる.

(1),(2)を仮定して, $\ds\alpha = \limsup_{n\to\infty}a_n$ を示そう. $A < \alpha < B$ と仮定する. (1)より, 任意の番号 $n$ に対してある $k\geqq n$ で $a_k\geqq A$ を満たすものが存在するので, $\ds\sup_{k\geqq n}a_k\geqq A$ となる.  ゆえに $\ds\limsup_{n\to\infty}a_n\geqq A$ となる. (2)より, ある番号 $N$ が存在して, $k\geqq N$ ならば $a_k\leqq B$ となる.  すなわち, $\ds\sup_{k\geqq N}\leqq B$ となる. 
ゆえに, $\ds\limsup_{n\to\infty}a_n\leqq B$ となる. これで

$$
A\leqq \limsup_{n\to\infty} a_n\leqq B
$$

が示された.  $A,B$ は $\alpha$ に幾らででも近くに取れるので $\ds\alpha = \limsup_{n\to\infty}a_n$ となる. $\QED$


**問題:** 実数列 $a_n$ が収束しているならば

$$
\limsup_{n\to\infty} a_n = \liminf_{n\to\infty} a_n = \lim_{n\to\infty}a_n
$$

となっていることを示せ.

**解答例:** 実数列 $a_n$ は $\alpha$ に収束していると仮定する. $\ds\limsup_{n\to\infty} a_n=\alpha$ のみを示そう.  $A<\alpha<B$ と仮定する. $a_n\to\alpha$ なので, ある番号 $N$ が存在して $k\geqq N$ ならば $A\leqq a_k\leqq B$ となる. ゆえに, 上の問題の条件(1),(2)を $\alpha$ は満たしている. したがって, $\ds\limsup_{n\to\infty} a_n=\alpha$ となる. $\QED$


## 実数の連続性の帰結

**この節を読むときの注意:** この節の内容は初心者にとっては難しいので, 解析学の議論に慣れてから読んだ方がよい. 初心者であるにもかかわらず, どうしても読みたい場合には, まず証明には目を通さずに, 各主張の内容を図に描いたり, 図には描けない理想的な様子をイメージしてみたりする方がよいと思う. 十分に健全な直観があれば(ない場合にはそのような健全な直観を心の中に育成することが大事), 実数の連続性もその帰結もどれも当然成り立ってほしい結果であることがわかるはずである. 少なくとも, 驚くべきような仮定をしているわけでもないし, 驚くべきような結論が導かれているわけではない. $\QED$

**補足:** 数学における仮定や結論の多くは通常の直観のもとでは当然そうなって欲しいと感じられるような事柄に過ぎない.  当然そうなって欲しいことを論理的に適切にかつ厳密に記述するためにテクニカルな(技術的な)要素が必要になる点が難しいだけで, 直観的には驚くべきことが含まれていない場合は相当に多い. そういう場合について, 直観的に何か非常に以外なことをやっているかのように誤解するのはまずい. それとは対照的に通常の直観では想像もできないような驚くべき主張をしている数学的命題も存在する. そのような場合に素直に驚きを感じて感動すればよい. 普段から通常の直観で「明らか」だと感じられ事柄を明らかだと感じられるような直観を身に付けておいた方が, 想定外の定義や結論への感動は大きくなると思われる. $\QED$


この節では次の実数の連続性を認めて使って, 幾つかの帰結を導く.

**実数の連続性:** 実数の空でない集合 $Y$ が上に有界ならば $Y$ の上界全体の集合の中に最小値が存在する. $\QED$

$Y$ を $-Y=\{\,-y\mid y\in Y\,\}$ に置き換えることによって, 実数の空でない集合 $Y$ が下に有界ならば $Y$ の下界全体の集合の中に最大値が存在することを示せる. 


### 有界単調実数列の収束

**有界単調数列の収束:** 上に有界な単調増加実数列はある実数に収束する. 

**証明:** $a_n$ は上に有界な単調増加実数列であるとする. 実数の連続性より, $a_n$ 達の最小上界 $\alpha$ が存在する. 任意に $\eps>0$ を取る.  もしもすべての番号 $n$ について $a_n\leqq \alpha-\eps$ となっているならば,$\alpha-\eps$ も $a_n$ 達の上界になり, $\alpha$ が最小の上界であることに反するので, ある番号 $N$ が存在して $a_N > \alpha-\eps$ となる. $a_n$ は単調増加数列なので $n\geqq N$ のとき $a_n>\alpha-\eps$ となる. $\alpha$ は $a_n$ たちの上界なので $a_n\leqq \alpha$ でもある. ゆえに $n\geqq N$ ならば, $\alpha-\eps<a_n\leqq\alpha$ なので特に $|a_n-\alpha|<\eps$ となる. これで $a_n$ が $\alpha$ に収束することが示された. $\QED$

**注意:** 数列をその $-1$ 倍で置き換えることによって, 下に有界な単調減少実数列はある実数に収束することも得られる. $\QED$


### Bolzano–Weierstrassの定理

数列 $a_n$ に対して, $1\leqq k_1<k_2<k_3<\cdots$ が定める数列 $b_n=a_{k_n}$ を $a_n$ の**部分列**と呼ぶ. 例えば, 数列 $1,4,9,16,25,\ldots$ は数列 $1,2,3,4,5,\ldots$ の部分列である.

**Bolzano–Weierstrassの定理:** 有界な実数列は収束する部分列を持つ. 

**証明:** $a_n$ は有界な実数列であるとする. 有界なのである実数 $A,B$ ですべての番号 $n$ について $A\leqq a_n\leqq B$ が成立するものが存在する.  $A_1=A$, $B_1=B$ とおくと, $a_k\in[A_1,B_1]$ を満たす $k$ は(当然)無限個存在する.  $A_n,B_n$ で $a_k\in [A_n,B_n]$ を満たす $k$ が無限個存在するものを帰納的に以下のように定める. $C_n$ を $A_n,B_n$ の中点とする. $a_k\in [A_n,C_n]$ を満たす $k$ が無限個あるならば $A_{n+1}=A_n$, $B_{n+1}=C_n$ とおき, そうでないならば $a_k\in[C_n,B_n]$ を満たす $k$ は無限個あるので $A_{n+1}=C_n$, $B_{n+1}=B_n$ とおく. このとき, $A_n\leqq A_{n+1}\leqq B_{n+1}\leqq B_n$ なので $A_n$ は上に有界な単調増加数列になり, $B_n$ は下に有界な単調減少数列になる. ゆえに有界単調実数列の収束性より, $A_n$, $B_n$ は収束する. それぞれの収束先を $\alpha,\beta$ と書く. $\ds|B_n-A_n|=\frac{|B_1-A_1|}{2^{n-1}}$ なので $\alpha=\beta$ となる. 各 $n$ について,  $a_k\in[A_n,B_n]$ を満たす $k$ は無限個存在するので, $1\leqq k_1<k_2<\cdots$ ですべての番号 $n$ について $a_{k_n}\in[A_n,B_n]$ を満たすものが存在する. このとき, $a_n$ の部分列 $a_{k_n}$ は $A_n\leqq s_{k_n}\leqq B_n$ を満たしているので $\alpha=\beta$ に収束する. $\QED$


### 実数のCauchy列の収束

数列 $a_n$ がCauchy列(基本列)であるとは, $m,n\geqq N\to\infty$ のとき $|a_m-a_n|\to 0$ となることであると定める. そり正確に言えば, 任意の $\eps>0$ に対して, ある番号 $N$ が存在して, $m,n\geqq N$ ならば $|a_m-a_n|<\eps$ が存在することであると定める.

**実数のCauchy列の収束:** 実数のCauchy列はある実数に収束する.

**証明:** $a_n$ は実数のCauchy列であるとする. そのとき, ある番号 $N$ が存在して $n\geqq N$ のとき $|a_n-a_N|< 1$ となるので, $|a_n|< |a_N|+1$ となる. ゆえに $M=\max\{|a_1|,\ldots,|a_{N-1}|, |a_N|+1\}$ とおくと, すべての番号 $n$ について $|a_n|\leqq M$ となり, 数列 $a_n$ が有界であることがわかる. 

Bolzano-Weierstrass の定理より, $a_n$ のある部分列 $a_{k_n}$ である実数 $\alpha$ に収束するものが存在する.  $\eps>0$ を任意に取る.  $a_n$ はCauchy列なので, ある番号 $N'$ が存在して $m,n\geqq N'$ ならば $\ds|a_n-a_m|<\frac{\eps}{2}$ となる.  部分列 $a_{k_n}$ は $\alpha$ に収束するので, ある番号 $N''$ が存在して $m\geqq N''$ ならば $\ds|a_{k_m}-\alpha|<\frac{\eps}{2}$ となる. $k_m\geqq N'$ となるような $m\geqq N''$ が存在する. そのとき, $n\geqq N'$ ならば

$$
|a_n-\alpha|\leqq|a_n-a_{k_m}|+|a_{k_m}-\alpha|<\frac{\eps}{2}+\frac{\eps}{2}=\eps.
$$

これで, $a_n$ が $\alpha$ に収束することが示された. $\QED$


### 閉区間に関するHeine-Borelの被覆定理

**閉区間に関するHeine-Borelの被覆定理:** $a\leqq b$ であるとし, 閉区間 $I=[a,b]$ を考える. 開区間 $U_\lambda=(a_\lambda,,b_\lambda)$, $a_\lambda<b_\lambda$, $\lambda\in\Lambda$ 達によって $I$ が覆われていると仮定する: $\ds I\subset\bigcup_{\lambda\in\Lambda}U_\lambda$. このとき有限個の $U_\lambda$ 達で $I$ を覆うことができる. すなわち有限個の $\lambda_1,\ldots,\lambda_r\in\Lambda$ で $\ds I\subset\bigcup_{i=1}^r U_{\lambda_i}$ を満たすものが存在する.

**証明:** $a=b$ の場合は自明なので $a<b$ であると仮定する. この定理の結論が成立していないと仮定して矛盾を導こう. すなわち, 有限個の $U_\lambda$ 達で $I=[a,b]$ を覆うことができないと仮定する. $a_1=a$, $b_1=b$ とおくと, $[a_1,b_1]$ は有限個の $U_\lambda$ 達で覆うことはできない. $a_n<b_n$ で $[a_n,b_n]$ が有限個の $U_\lambda$ 達で覆えないものを以下のように帰納的に定めることができる. $[a_n,b_n]$ が有限個の $U_\lambda$ 達で覆うことができないならば, $c_n$ を $a_n,b_n$ の中点とするとき, $[a_n,c_n]$ または $[c_n,b_n]$ のどちらかは有限個の $U_\lambda$ で覆うことはできない. $[a_n,c_n]$ の方がそうならば $a_{n+1}=a_n$, $b_{n+1}=c_n$ とおき, それ以外の場合には $[c_n,b_n]$ の方がそうなっているので $a_{n+1}=c_n$, $b_{n+1}=b_n$ とおく.  このとき, $[a_{n+1},b_{n+1}]$ も有限個の $U_\lambda$ 達で覆うことはできない. このとき, $a_n\leqq a_{n+1}\leqq b_{n+1}\leqq b_n$ でかつ $\ds|b_n-a_n|\leqq\frac{b-a}{2^{n-1}}$ なので, $a_n$ と $b_n$ はCauchy列になり, 同一の $\alpha\in I$ に収束する. $U_\lambda$ 達は $I$ を覆っているので, ある $\lambda_0\in\Lambda$ が存在して $\alpha\in U_{\lambda_0}$ となる. $a_n,b_n$ はともに $\alpha$ に収束するので, 十分に $n$ を大きくすると, $[a_n,b_n]\subset U_{\lambda_0}$ となる. これは $[a_n,b_n]$ が有限個の $U_\lambda$ 達で覆えないことに反する.  これで矛盾が出た.  ゆえに上の定理の結論は成立する. $\QED$


### 閉区間上の実数値連続函数が最大最小を持つこと

ノート「<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/04%20continous%20functions.ipynb">連続函数</a>」の<a href="http://nbviewer.jupyter.org/github/genkuroki/Calculus/blob/master/04%20continous%20functions.ipynb#閉区間上の実数値連続函数が最大最小を持つこと">対応する節</a>を参照せよ.
