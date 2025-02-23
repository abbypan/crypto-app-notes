Bulletproofs
##########################################################

`Bulletproofs: Short Proofs for Confidential Transactions and More <https://crypto.stanford.edu/bulletproofs/)>`_

`Bulletproofs17 <https://eprint.iacr.org/2017/1066.pdf>`_

`Bulletproofs18 <https://ieeexplore.ieee.org/document/8418611>`_

Improved Inner-Product Argument
==========================================================

与BCCGP思路类似，优化为每次折半，Recursive ARgument log(n) 次


.. math::

    g, h ∈ G^n
    u, P ∈ G
    a, b ∈ Zp^n

    n=1: 

    P->V: a, b
    c = a · b
    V check P == g^a · h^b · u^c

    n>1:

    n' = n/2
    (a1, a2) = a, (b1, b2) = b, (g1, g2) = g, (h1, h2) = h
    c = a · b = a1 · b1 + a2 · b2

    CL = a1 · b2  ∈ Zp
    CR = a2 · b1  ∈ Zp
    L = g2^a1 · h1^b2 · u^CL ∈ G
    R = g1^a2 · h2^b1 · u^CR ∈ G

    P->V: L, R
    V->P: x ∈ Zp*
    
    g' = g1^(x^(-1)) · g2^x ∈ G^n'
    h' = h1^x · h2^(x^(-1)) ∈ G^n'
    P' = L^(x^2) · P · R^(x^(-2)) ∈ G

    a' = a1*x + a2*x^(-1) ∈ Zp^n' 
    b' = b1*x^(-1) + b2*x ∈ Zp^n' 

    c' = a' · b'
       = a1 · b1 + (a1 · b2)*x^2 + (a2 · b1)*x^(-2) + a2 · b2

    P' = L^(x^2) · P · R^(x^(-2))

       = (g2^(a1*x^2) · h1^(b2*x^2) · u^(CL*x^2))
          · ((g1^a1 · g2^a2) · (h1^b1 · h2^b2) · u^(a · b))
          · ((g1^(a2*x^(-2)) · h2^(b1*x^(-2)) · u^(CR*x^(-2))

       = (g1^(a1+a2*x^(-2)) · g2^(a2+a1*x^2))
         · (h1^(b2*x^2+b1) · h2^(b2+b1*x^(-2)))
         · u^(CL*x^2+a · b+CR*x^(-2))

       =  (g1^(x^(-1)))^(a1*x+a2*x^(-1)) · (g2^x)^(a2*x^(-1)+a1*x)
         · (h1^x)^(b2*x+b1*x^(-1)) · (h2^(x^(-1)))^(b2*x+b1*x^(-1))
         · u^((a1 · b2)*x^2+a1 · b1 + a2 · b2+(a2 · b1)*x^(-2))

       = g'^a' · h'^b · u^c'

     get (g', h', u, P', a', b')

显然，g'/h'/a'/b' 的vector size折半

Inner-Product Range Proof
==========================================================


.. math::

    < aL - z * 1^n, y^n · (aR + z * 1^n) + z^2 * 2^n >  
    = < aL, y^n · (aR + z * 1^n) > 
      + < aL, z^2 * 2^n > 
      + < - z * 1^n, y^n · (aR + z * 1^n) >
      - z^3 * <1^n, 2^n>
    =  <aL · aR, y^n> + <aL,  y^n · z * 1^n>
      + z^2 * <aL, 2^n>
      - z * <1^n, y^n · aR>
      - z^2 * <1^n, y^n>
      - z^3 * <1^n, 2^n>
    =  0 + z * <aL, y^n>
      + z^2 * v
      - z * <aR, y^n>
      - z^2 * <1^n, y^n>
      - z^3 * <1^n, 2^n>
    = z * <aL -aR, y^n>   
      + z^2 * v 
      - z^2 * <1^n, y^n>
      - z^3 * <1^n, 2^n>
    = z^2 * v
      + (z - z^2) * <1^n, y^n>
      - z^3 * <1^n, 2^n>
    = z^2 * v 
      + δ(y, z)

隐藏aL，引入sL、sR

A为aL/aR的commitment, S为sL/sR的commitment

结合Inner-Product, aL/aR/sL/sR 构造 linear vector polynomials
    

.. math::

    l(X) = (aL - z * 1^n) + sL · X
    r(X) = y^n · (aR + z * 1^n + sR · X) + z^2 * 2^n 

    t(X) = <l(X), r(X)> = t0 + t1 · X + t2 · X^2
    显然，t0 = z^2 * v + δ(y, z)

再构造检查
- t1,t2的commitments: T1, T2
- l(x), r(x)的commitment: P
- t与`<l, r>`相等

Logarithmic Range Proof    
==========================================================

结合Inner-Product Argument, Inner-Product Range Proof，节省 l, r的传输，可优化为`2log2(n) + 2` elements

达到n的Logarithmic

Aggregating Logarithmic Proofs
==========================================================

把m个indivual proof 拼成`n*m`size的vector，同样经过Inner-Product优化，以及Logarithmic的约减，优化为`log2(n*m)+4`element

达到m的additive

Non-Interactive Proof through Fiat-Shamir
==========================================================

把 Inner-Product Range Proof 中的 y, z 生成方式改一下

mpc
==========================================================

看dealer

Inner-Product Proof for Arithmetic Circuits
==========================================================

注意t2可直接计算，因此仅构造t1, t3, t4, t5, t6的commitment，用于校验t(x)

:math::`h' = h^(y^(-n))`,  注意此处h', h为G^n element

再基于P校验l(x), r(x)

