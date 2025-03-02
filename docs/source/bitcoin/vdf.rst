Verifiable Delay Functions
#############################

doc
==========================================================

`VDF research <https://vdfresearch.org/>`_

`A Survey of Two Verifiable Delay Functions <https://eprint.iacr.org/2018/712.pdf>`_

`On Verifiable Delay Functions <https://speakerdeck.com/asanso/on-verifiable-delay-functions-vdf-how-to-slow-burning-down-the-planet-verifiably>`_

计算耗时，校验简单，并行计算不能明显提速

Proofs of sequential work (PoSW)

randomness beacon (unpredictable), multiparty random (not bias), consensus from proof of resources(blockchain, elect/vote)

.. math::

    Setup pp = (ek, vk)
    (y, 𝜋) = Eval(ek, x)
    Verify(vk, x, y, 𝜋)

Correctness, Soundness, Sequentiality, Decodability, Incremental

Proof of Work
==========================================================

blockchain 求解小于某个t值的 H(nonce, x)


hidden order groups, RSA
==========================================================

euler，n = p*q，ϕ(n)=(p-1)*(q-1)

unknown order, 则为Time-lock puzzles

trusted setup, small subgroup attack

Wesolowski
----------------------------------------------------

`Wesolowski: Efficient verifiable delay functions <https://eprint.iacr.org/2018/623.pdf>`_

verfier随机选定l, 也可改造为nizk，例如置`l=next_prime(hash(x, y, T))`

.. math::

    r = 2^T mod l 
    2^T = m*l + r

    𝜋 = x^m 
    y = x^(2^T)

    Verify  y = (𝜋^l) * (x^r)

Pietrzak
----------------------------------------------------

Simple Verifiable Delay Functions](https://eprint.iacr.org/2018/627.pdf)
==========================================================

QRN是Zn 上的二次剩余的|x|绝对值集合，因此是1/4的Zn。

选取的cyclic group QRN+ 与 QRN 同构，x值在{−(n − 1)/2, . . . , (n − 1)/2}以内。


.. math::

    P: μ = x^(2^(T/2)), y=μ^(2^(T/2))
    P->V: y

    P->V: μ
    V->P: r
    P: (x', y') = (x^r * μ, μ^r * y)
    P -> V: (x', y'), T/2
    V: verify y' = x'^(2^(T/2))

De Feo
==========================================================

`De Feo: Verifiable Delay Functions from Supersingular Isogenies and Pairings <https://eprint.iacr.org/2019/166.pdf>`_

trusted setup, attack在于curve内部结构

例如bls signature选用的pairing curve，是在embedding degree k, characteristic p的 :math::`F_(p^k)` 上order为N的subgroup

X1, X2, Y1, Y2, G 的order为N

φ, ˆφ 为 E, E' 之间 degree l 的同态映射


.. math::

    φ : X1 → Y1 
    eY : Y1 × Y2 → G
    X1 × Y2  -> Y1 × Y2 -> G

    ˆφ : Y2 → X2
    eX : X1 × X2 → G
    X1 × Y2 -> X1 x X2 -> G

    eX (P, ˆφ(Q)) = eY (φ(P ), Q)

P为X1的生成元

.. math::

    pp = (N, X1, X2, Y1, Y2, G, eX , eY , P, φ(P ))

supersingular curves over Fp
----------------------------------------------------

类似CSIDH

supersingular curve E/Fp, order N

E,  ̃E 在 Fp2 上 quadratic twist 同构，分别对应X2, X1


.. math::

    u ∈ Fp^2 \ Fp, u^2 ∈ Fp

    υ : E →  ̃E
        (x, y) → (u^2 * x, u^3 * y)

    X2 = E[N] ∩  E(Fp).
    X1 = υ^−1 (  ̃E[N] ∩   ̃E(Fp) )

    Y2 = E′[N] ∩ E′(Fp)
    Y1 = υ^−1 (  ̃E′[N] ∩  ̃E′(Fp))

    isogeny φ : E → E′ of degree l^T , 对应 ˆφ;

    P为X1生成元

    (ek, vk) = (ˆφ, (E, E′, P, φ(P)))

    Eval( ˆφ, Q ∈ Y2) = ˆφ(Q)

    Verify(E, E′, P, Q, φ(P), ˆφ(Q))
    ˆφ(Q) ∈ X2
    eN (P, ˆφ(Q)) = eN (φ(P), Q).

supersingular curves over Fp2
----------------------------------------------------

Let π be the Frobenius endomorphism of E/Fp, the trace map on E/Fp2 is the map


.. math::

    Tr: E/Fp2 → E/Fp,
         P → P + π(P).

     eN (P, Tr(R)) = eN (P, (1+π)(R)) 
                   = eN ((1−π)(P), R) 
                   = eN ([2]P, R) 
                   = eN (P, R) ^2

     f : E′[N] → X2,
         Q → (Tr ◦ ˆφ)(Q);

     Eval( ˆφ, Q ∈ E′[N]) = (Tr ◦ ˆφ)(Q)

     Verify(E, E′, P, Q, φ(P ), (Tr ◦ ˆφ)(Q))
     (Tr ◦ ˆφ)(Q) ∈ X2
     eN (P, (Tr ◦ ˆφ)(Q)) = eN (φ(P), Q) ^2

Univariate permutation polynomials
==========================================================

`Verifiable Delay Functions <https://iacr.org/cryptodb/data/paper.php?pubkey=28858>`_

:math::`Y ⊆ Fq^n to X ⊆ Fq^m` 的injective rational map F = (f1 , ...., fm)


.. math::

    fi( ̄y) = xi for i = 1, ..., m
    fi( ̄y) = g( ̄y)/h( ̄y) = xi
    zi( ̄y) := g( ̄y)−xi*h( ̄y) = 0

Rational functions on finite fields
----------------------------------------------------

有限域上求根


.. math::

    F(X) = g(X)/h(X)
    GCD(X^q − X, g(X) − c · h(X))
    outputs X − s for the unique s such that F(s) = c

Rational maps on elliptic curves
----------------------------------------------------

曲线上求公共点


.. math::

    E(y, x) = y^2 − x^3 − ax − b
    R = Res_y (z1 , z2) is a univariate polynomial in x of degree deg(z1) · deg(z2) such that R(x) = 0
    R ′= Res_y (R, E)

weaker VDF
----------------------------------------------------

基于GCD/Res求解的困难度


.. math::

    Setup(λ, t): choose a (q, F, X , Y) ∈ F specified by λ and t, and output pp := ((q, F ), (q, F )).

    Eval((q, F),  ̄x): 
    for an output  ̄x ∈ X ⊆ Fq^m
    compute  ̄y ∈ Y such that F ( ̄y) =  ̄x; 
    The proof π is empty.

    Verify((q, F ),  ̄x,  ̄y, π) 
    outputs Yes if F ( ̄y) =  ̄x.
