KZG10
#######

`KZG10: Constant-Size Commitments to Polynomials and Their Applications <https://www.iacr.org/archive/asiacrypt2010/6477178/6477178.pdf>`_

Pairing
==========================================================

universal polynomial φ(x)的取值作为g的幂次，同等变换为，polynomial φ(x)的coffients做为PK里的g^(α^i)的幂次

再结合pairing求解

.. math::

    Trusted Setup:
    bilinear pairing group GG = 〈e, G, Gt〉

    SK = Random α, PK =〈GG, g, g^α, . . . , g^(α^t) 〉

    polynomial φ(x) ∈ Zp[x] 
    φ(x) = ∑ φj * x^j,  0<= j <=deg(φ), deg(φ) <= t

    Commit(PK, φ(x)): 

    commit C  = g^φ(α) 
              = g^(∑ φj * x^j) 
              = ∏ g^(φj * α^j)
              = ∏ (g^(a^j))^φj

    Open(PK, C, φ(x)): φ(x)

    VerifyPoly(PK, C, φ(x)):
    verify C == ∏ (g^(a^j))^φj

    CreateWitness(PK, φ(x), i):

    ψi(x) = (φ(x)−φ(i))/(x−i), 
    proof wi = g^ψi(α)
    output 〈i, φ(i), wi〉

    VerifyEval(PK, C, i, φ(i), wi):

    verify e(C, g) == e(wi, g^α/g^i) * e(g, g)^φ(i)

    e(wi, g^α/g^i) * e(g, g)^φ(i)
    = e(g^ψi(α) , g^(α-i)) * e(g^φ(i), g)
    = e(g^(ψi(α) * (α-i)), g) * e(g^φ(i), g)
    = e(g^(φ(α)−φ(i)), g) * e(g^φ(i), g)
    = e(g^φ(α), g)
    = e(C, g)

Batch Opening 针对多个i聚合运算
==========================================================

.. math::

    CreateWitnessBatch(PK, φ(x), B): 〈B, r(x), wB 〉
    B ⊂ Zp
    r(x) = φ(x) % ∏ (x-i), i∈B
    ψB(x) = (φ(x)−r(x))/ ∏ (x-i), i∈B
    wB = g^ψB(α)

    VerifyEvalBatch(PK, C, B, r(x), wB ): i∈B
    verify e(C, g) == e(g^∏ (α−i), wB ) * e(g, g^r(α))
    verify r(i) == φ(i)

