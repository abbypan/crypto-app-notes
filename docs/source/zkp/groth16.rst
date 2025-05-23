Groth16
########

`Groth16: On the Size of Pairing-based Non-interactive Arguments <https://eprint.iacr.org/2016/260.pdf>`_

`Groth16 zkSNARK: R1CS and QAP - From Zero to Hero with Finite Fields & sagemath <https://risencrypto.github.io/R1CSQAP/>`_

`Groth16 <http://www.zeroknowledgeblog.com/index.php/groth16>`_

`The Mathematical Mechanics Behind the Groth16 Zero-knowledge Proving Protocol <https://kayleegeorge.github.io/math110_WIM.pdf>`_

R1CS: rank-1 constraint system
==========================================================

- 假设共m个Gate。
- 将Gate运算的variable按顺序排列，S中的每一列对应每个variable在某个输入的取值statement x。
- 将Gate运算公式转换为约束matrix：A中的每一行为left input，B中的每一行为right input，C中的每一行为output。

QAP
==========================================================

- 对A/B/C的每一列，以(row_i, value)为point计算lagrange多项式，获得转置后的lagrange系数多项式matrix，对应ui(x), vi(x), wi(x)。
- S 与 上述lagrange matrix点乘，获得A/B/C对应的多项式A(x), B(x), C(x)，计算

.. math::

    T(x) = A(x) * B(x) - C(x)
    Z(x) = (x - 1) * ... * (x - m)
    H(x) = T(x) / Z(x)

显然，T(x) = H(x) * Z(x)

NIZK
==========================================================

见 3.2 NIZK arguments for quadratic arithmetic programs

pairing-friendly elliptic curves


.. math::

    a0 = 1
    public statements (a1, ..., al)
    secret witnesses (al+1, ..., am)

    ∑ ai*ui(x) · ∑ ai*vi(x) = ∑ ai*wi(X) + h(X)t(X),  i = 0, ..., m

    Setup(R): 
    τ = (α, β, γ, δ, x)
    σ1 = ( α, β, δ, 
        {x^i} 0<=i<=n−1, 
        { (β*ui(x)+α*vi(x)+wi(x))/γ } 0<=i<=l, 
        { (β*ui(x)+α*vi(x)+wi(x))/δ } l+1<=i<=m, 
        { (x^i * t(x))/δ } 0<=i<=n-2
    )
    σ2 = ( β, γ, δ, 
        {x^i} 0<=i<=n-1
    )
    σ = ([σ1]1, [σ2]2)

    Prove:
    π ← Prove(R, σ, a1, . . . , am)
    r, s ← Zp
    A = α + ∑ ai*ui(x) + rδ,  0<=i<=m
    B = β + ∑ ai*vi(x) + sδ,  0<=i<=m
    C = ( ∑ ai*( β*ui(x) + α*vi(x) + wi(x)) + h(x)t(x) )/δ + As + Br − rsδ
    π = ([A]1, [C]1, [B]2)

    Verify:
    Vfy(R, σ, a1, . . . , al, π)
    [A]1 · [B]2 = [α]1 · [β]2 + ∑ ai*( (β*ui(x) + α*vi(x) + wi(x))/γ ) · [γ]2 + [C]1 · [δ]2,   0<=i<=l

    Sim:
    π ← Sim(R, τ, a1, . . . , al)
    C = ( AB − αβ − ∑ ai*(β*ui(x) + α*vi(x) + wi(x)) )/δ, i= 0, ..., l  

精简的σV，只需l+2个G1 element, 3个G2 element，1个GT element


.. math::

    σV = ( p, G1, G2, GT , e, [1]1, { [( β*ui(x) + α*vi(x) + wi(x))/γ ]1 }  0<=i<=l, [1]2, [γ]2, [δ]2, [αβ]T )

security
==========================================================

显然容易继承pairing curve的malleable问题，基本参考校验原文，或者防重放等。

如果关联identity，同时涉及identity的trust。

