Nova
==========================================================

`Nova: Recursive Zero-Knowledge Arguments from Folding Schemes <https://eprint.iacr.org/2021/370.pdf>`_

通过incrementally veriﬁable computation (IVC)构造recursive zk ARgument, 实现folding

Relaxed R1CS 将W, E均构造为witness；folding时，二者对应的commitment W', E'也进行对应的变换

Relaxed R1CS:

.. math::

    A, B, C ∈ F^(m x m)

    public scalar u ∈ F
    public x ∈ F^l
    witness W ∈ F^(m−l−1)
    Z = (W, x, u)

    Folding:
        E ← E1 + r · (AZ1 ◦ BZ2 + AZ2 ◦ BZ1 − u1CZ2 − u2CZ1) + r2 · E2
        E ∈ F^m

        AZ ◦ BZ = (u1 + r · u2) · C(Z1 + rZ2) + E
            =  uCZ + E

        T = AZ1 ◦ BZ2 + AZ2 ◦ BZ1 − u1CZ2 − u2CZ1

        ...


Construction 1:

.. math::

    E' = Com(ppE , E, rE )
    W' = Com(ppW , W, rW )
    T' = Com(ppE , T, rT )
    将2个Relaxed R1CS instance/witness进行folding，得到新的instance/witness
    instance (E', u, W', x)
    witness (E, rE , W, rW )

