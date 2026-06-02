CL
====

Camenisch–Lysyanskaya signature

.. math::

        A^e = Z * S^v * \prod_{i=1}^{l} R_{i}^{m_i} (mod n)


n = p * q，私钥为(p, q)

公钥为 (Z, S, R_i)

sig 为 (A, e, v)

p, q 用于 计算A时的1/e求根


selective disclosure
-------------------------

假设仅暴露 m_1, 其余 m_2, ... , m_l 保持隐藏


re-randomize sig

.. math::

        A' = A * S^r

        v' = v + e*r


计算 U，做为 hidden attributes commitment

.. math::

        U = S^{v'} * \prod_{i=2}^{l} R_{i}^{m_i} (mod n)


生成 ZK proof: (A', v', e, U,  m_1)


verifier 校验

.. math::
        
        A'^e = Z * R_{1}^{m_1} * U mod n
        


range proof
------------------

Pedersen commitment

假设 x 为某个 m_i

.. math::

        y = x - k

        C = g^y * h^r

        y = \sum{b_i * 2^i}

        C_i = g^{b_i} * h^{r_i}

        C = \prod C_{i}^{2^i}



