---


---

<h1 id="simple-introduction-to-compressive-sensing-and-dictionary-learning-on-python">Simple Introduction to Compressive Sensing and Dictionary Learning on Python</h1>
<p>An interesting Theory that recently I got to study is Compressive Sensing and Dictionary Learning, and here I will try to share some intuition that I had during my studies trying to keep thinghs easy and practical.</p>
<h2 id="what-is-compressive-sensing">What is Compressive Sensing?</h2>
<p>Compressive Sensing is a new Theory that after the First paper  <a href="http://statweb.stanford.edu/~candes/papers/ExactRecovery.pdf">Robust Uncertainty Principles: Exact Signal Reconstruction from Highly Incomplete Frequency Information</a> (June 2004) written by the noted <strong>Emmanuel Candes</strong>, <strong>Justin Romberg</strong> and <strong>Terence Tao</strong>, where they gived the proof that is possible to exactly recover an object from an incoplete frequency samples, by a convex optimization problem.</p>
<p>While the Second paper of Compressive Sensing history is <a href="http://statweb.stanford.edu/~donoho/Reports/2004/l1l0approx.pdf">For Most Large Underdetermined Systems of Equations, the Minimal ` 1 -norm Near-Solution Approximates the Sparsest Near-Solution</a> (August 2004) written by David L. Donoho; here we have the most famous approach to the problem, that is the one that use the inexact linear equation <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi>y</mi><mo>≈</mo><mi mathvariant="normal">Φ</mi><mi>α</mi></mrow><annotation encoding="application/x-tex">y \approx\Phi\alpha</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.68333em;"></span><span class="strut bottom" style="height: 0.87777em; vertical-align: -0.19444em;"></span><span class="base"><span class="mord mathit" style="margin-right: 0.03588em;">y</span><span class="mrel">≈</span><span class="mord mathrm">Φ</span><span class="mord mathit" style="margin-right: 0.0037em;">α</span></span></span></span></span> where <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi mathvariant="normal">Φ</mi></mrow><annotation encoding="application/x-tex">\Phi</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.68333em;"></span><span class="strut bottom" style="height: 0.68333em; vertical-align: 0em;"></span><span class="base"><span class="mord mathrm">Φ</span></span></span></span></span> is a given <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi>n</mi></mrow><annotation encoding="application/x-tex">n</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.43056em;"></span><span class="strut bottom" style="height: 0.43056em; vertical-align: 0em;"></span><span class="base"><span class="mord mathit">n</span></span></span></span></span> by <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi>m</mi></mrow><annotation encoding="application/x-tex">m</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.43056em;"></span><span class="strut bottom" style="height: 0.43056em; vertical-align: 0em;"></span><span class="base"><span class="mord mathit">m</span></span></span></span></span>  and <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi>α</mi></mrow><annotation encoding="application/x-tex">\alpha</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.43056em;"></span><span class="strut bottom" style="height: 0.43056em; vertical-align: 0em;"></span><span class="base"><span class="mord mathit" style="margin-right: 0.0037em;">α</span></span></span></span></span> is a sparse vector.<br>
In general this problem requires combinatorial optimization and so is considered intractable. But his convex-relaxation using <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><msub><mi>l</mi><mn>1</mn></msub></mrow><annotation encoding="application/x-tex">l_1</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.69444em;"></span><span class="strut bottom" style="height: 0.84444em; vertical-align: -0.15em;"></span><span class="base"><span class="mord"><span class="mord mathit" style="margin-right: 0.01968em;">l</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.301108em;"><span class="" style="top: -2.55em; margin-left: -0.01968em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mathrm mtight">1</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"></span></span></span></span></span></span></span></span></span> minimization is convex, and is totally tractable.</p>
<p><span class="katex--display"><span class="katex-display"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mtable><mtr><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow></mrow></mstyle></mtd><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow><mrow></mrow><msub><mo><mtext>minimize</mtext></mo><mi>α</mi></msub></mrow></mstyle></mtd><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow></mrow></mstyle></mtd><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow><mrow></mrow><mi mathvariant="normal">∣</mi><mi mathvariant="normal">∣</mi><mi>α</mi><mi mathvariant="normal">∣</mi><msub><mi mathvariant="normal">∣</mi><mn>1</mn></msub></mrow></mstyle></mtd></mtr><mtr><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow></mrow></mstyle></mtd><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow><mrow></mrow><mtext>subject&nbsp;to</mtext></mrow></mstyle></mtd><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow></mrow></mstyle></mtd><mtd><mstyle scriptlevel="0" displaystyle="true"><mrow><mrow></mrow><mi mathvariant="normal">∣</mi><mi mathvariant="normal">∣</mi><mi>y</mi><mo>−</mo><mi mathvariant="normal">Φ</mi><mo>⋅</mo><mi>α</mi><mi mathvariant="normal">∣</mi><msub><mi mathvariant="normal">∣</mi><mn>2</mn></msub><mo>≤</mo><mi>ϵ</mi></mrow></mstyle></mtd></mtr></mtable></mrow><annotation encoding="application/x-tex">\begin{aligned}
&amp; \underset{\alpha}{\text{minimize}}
&amp; &amp; ||\alpha||_1 \\
&amp; \text{subject to}
&amp; &amp; ||y - \Phi \cdot \alpha||_2 \leq \epsilon
\end{aligned}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 1.92em;"></span><span class="strut bottom" style="height: 3.34em; vertical-align: -1.42em;"></span><span class="base"><span class="mord"><span class="mtable"><span class="col-align-r"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.92em;"><span class="" style="top: -3.92em;"><span class="pstrut" style="height: 2.84em;"></span><span class="mord"></span></span><span class="" style="top: -2.08em;"><span class="pstrut" style="height: 2.84em;"></span><span class="mord"></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 1.42em;"></span></span></span></span><span class="col-align-l"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.92em;"><span class="" style="top: -4.08em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord"></span><span class="mop op-limits"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.66786em;"><span class="" style="top: -2.4em; margin-left: 0em;"><span class="pstrut" style="height: 3em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord mathit mtight" style="margin-right: 0.0037em;">α</span></span></span></span><span class="" style="top: -3em;"><span class="pstrut" style="height: 3em;"></span><span class=""><span class="mop"><span class="mord text"><span class="mord mathrm">minimize</span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.7em;"></span></span></span></span></span></span><span class="" style="top: -2.24em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord"></span><span class="mord text"><span class="mord mathrm">subject&nbsp;to</span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 1.42em;"></span></span></span></span><span class="arraycolsep" style="width: 1em;"></span><span class="col-align-r"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.92em;"><span class="" style="top: -3.92em;"><span class="pstrut" style="height: 2.84em;"></span><span class="mord"></span></span><span class="" style="top: -2.08em;"><span class="pstrut" style="height: 2.84em;"></span><span class="mord"></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 1.42em;"></span></span></span></span><span class="col-align-l"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.92em;"><span class="" style="top: -4.08em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord"></span><span class="mord mathrm">∣</span><span class="mord mathrm">∣</span><span class="mord mathit" style="margin-right: 0.0037em;">α</span><span class="mord mathrm">∣</span><span class="mord"><span class="mord mathrm">∣</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.301108em;"><span class="" style="top: -2.55em; margin-left: 0em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mathrm mtight">1</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"></span></span></span></span></span></span></span><span class="" style="top: -2.24em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord"></span><span class="mord mathrm">∣</span><span class="mord mathrm">∣</span><span class="mord mathit" style="margin-right: 0.03588em;">y</span><span class="mbin">−</span><span class="mord mathrm">Φ</span><span class="mbin">⋅</span><span class="mord mathit" style="margin-right: 0.0037em;">α</span><span class="mord mathrm">∣</span><span class="mord"><span class="mord mathrm">∣</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.301108em;"><span class="" style="top: -2.55em; margin-left: 0em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mathrm mtight">2</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"></span></span></span></span></span><span class="mrel">≤</span><span class="mord mathit">ϵ</span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 1.42em;"></span></span></span></span></span></span></span></span></span></span></span></p>
<p>If you want more go <a href="https://www.quora.com/What-are-the-seminal-papers-on-compressed-sensing">here </a></p>
<h2 id="lets-have-some-intuition-of-what-is-going-on....">Let’s have some intuition of what is going on…</h2>
<p>Let’s start to think what it means that <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi>y</mi><mo>=</mo><mi mathvariant="normal">Φ</mi><mo>⋅</mo><mi>α</mi></mrow><annotation encoding="application/x-tex">y = \Phi \cdot \alpha</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.68333em;"></span><span class="strut bottom" style="height: 0.87777em; vertical-align: -0.19444em;"></span><span class="base"><span class="mord mathit" style="margin-right: 0.03588em;">y</span><span class="mrel">=</span><span class="mord mathrm">Φ</span><span class="mbin">⋅</span><span class="mord mathit" style="margin-right: 0.0037em;">α</span></span></span></span></span>,<br>
if <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi mathvariant="normal">Φ</mi></mrow><annotation encoding="application/x-tex">\Phi</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.68333em;"></span><span class="strut bottom" style="height: 0.68333em; vertical-align: 0em;"></span><span class="base"><span class="mord mathrm">Φ</span></span></span></span></span> is n by n matrix is means that we have n hyper-planes of dimension n each one that, if they are linearly indipendent, they will end to identify a single point.<br>
Then if we have that <span class="katex--inline"><span class="katex"><span class="katex-mathml"><math><semantics><mrow><mi mathvariant="normal">Φ</mi></mrow><annotation encoding="application/x-tex">\Phi</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="strut" style="height: 0.68333em;"></span><span class="strut bottom" style="height: 0.68333em; vertical-align: 0em;"></span><span class="base"><span class="mord mathrm">Φ</span></span></span></span></span> is n-1 by n this system of equation will end on a line, and so on…<br>
More dimentions we remove from the row then more dimentions we give to the hiper-plane that is solution of the equations!!!<br>
So we could think that it seems that there is no single solution… but we should not forget that we want the solution to be the most sparse possible, and for this reason the solution exist and is only one.<br>
<img src="https://drive.google.com/open?id=1K3skSWwfqoVf40ysE0BpHp3afiwioEd-" alt="Sparse space"></p>
<p>At least since 0-norm is a NP-problem we transform the problem to convex using 1-norm</p>
<p><img src="https://drive.google.com/open?id=17xo6mZxv7uNcv5JbJVHxw2o5N9gwFPhI" alt="Norm p with p < 1"></p>

