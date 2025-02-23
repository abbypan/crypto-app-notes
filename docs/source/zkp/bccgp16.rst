BCCGP16
##########################################################

`BCCGP16: Eﬃcient Zero-Knowledge Arguments for Arithmetic Circuits in the Discrete Log Setting <https://eprint.iacr.org/2016/263.pdf>`_

homomorphic commitment
==========================================================
 

.. math::

     Comck(m0; r0) · Comck(m1; r1) = Comck(m0 + m1; r0 + r1)

Pedersen commitment 
==========================================================


.. math::

    g, h are group elements
    (m, r) ∈ Zp x Zp
    c = g^r * h^m

Recursive Argument for Inner Product Evaluation
==========================================================

假设g vector size 为 n


.. math::

    n = ∏ m_i , 1 <= i <= µ

按因子顺序做reduce, 例如

取首个m = m_µ，拆分g = (g_1, ..., g_m), 相当于拆分成 m 个 vector size 为 n/m 的vectors g_i。h, a, b 的拆分与g类似。


计算


.. math::

    A_k = ∏ g_i^a_(i+k),  min(m,m−k) <=i<=max(1,1−k), k = 1 − m, . . . , m − 1
    A = g^a = ∏ g_i^a_i,  1<=i<=m


g_i^a_j的matrix斜线元素相乘，即为A_(j-i)，显然，A = A_0。B与A类似。


.. math::

    (G, p, g, A, h, B, z, m_µ = m, m_µ−1 = m', . . . , m_1)

计算


.. math::

    random challenge x
    g' = ∏ (g_i)^(x^−i),  1<=i<=m
    A' = ∏ (A_k)^(x^k),   1-m<=k<=m-1
    a' = ∑ a_i*(x^i), 1<=i<=m
    A' = g'^a'

    random challenge x^-1
    h' = ∏ (h_i)^(x^i),  1<=i<=m
    B' = ∏ (B_k)^(x^-k),   1-m<=k<=m-1
    b' = ∑ b_i*(x^-i), 1<=i<=m
    B' = h'^b'

    z_k = ∑ a_i · b_(i+k),  min(m,m−k) <= i <= max(1,1−k), 1-m<=k<=m-1
    z_0 = z = ∑ a_i · b_i,  1 <= i <= m
    z'  = ∑ z_k * x^(-k), 1-m<=k<=m-1

    a' · b' = ∑ a_i*(x^i) · ∑ b_j*(x^-j)
            = ∑ a_i*(x^i) · b_j*(x^-j)
            = ∑ (a_i · b_j)*(x^(i-j))
            = ∑ (a_i · b_(i+k))*(x^(-k)) , permutation, let j = i+k
            = ∑ z_k * x^(-k)
            = z'

获得(G, p, g', A', h', B', z', m_µ−1, . . . , m_1)，显然，g' vector size为n/m，可进一步拆分为m' = m_µ−1个子vector size。

Recursive Argument, 直至m_1。

