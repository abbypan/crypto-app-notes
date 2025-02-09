Multivariate Cryptography
#############################

overview 
======================================

`public key cryptography Multivariate crypto <https://www.slideserve.com/birch/summary>`_

`Current State of Multivariate Cryptography <https://www.researchgate.net/publication/319170467_Current_State_of_Multivariate_Cryptography>`_

S : m -> m 的映射

F : n -> m 的映射，一元二次方程组

T : n -> n 的映射

公钥 P = S . F . T : n -> m 的映射

UOV : Unbalanced Oil and Vinegar signature scheme
============================================================

n = o + v

V =  { 1, ..., v }

O = { v+1, ..., n }

如果 o = v ，称为 Balanced Oil and Vinegar

如果 v > o，称为 Unbalanced Oil and Vinegar

center map , F : n -> o

注意多项式中的 :math:`x_i*x_j` 项，要么是 i, j均属于V，要么是i属于V且j属于O。

仅有T，没有S。

公钥 P = F . T

:math:`F^(-1)` 求解时，例如已知 :math:`y ( y_1, ..., y_o)` ，随机选择 :math:`x_1, ..., x_v` ，可代入求解得到 :math:`x_{v+1} , ..., x_n`

签名计算：hash(message) = w, 通过w求 :math:`F^(-1)` 的逆x，再求解 :math:`T^(-1)` 的逆z，z即为签名。

签名校验：w' = P(z)，比较w = w'

Rainbow Signature
======================================

分多个Vinegar + Oil层

.. math::

    0 < v_1 < v_2 < ... < v_{u+1} = n

    V_1 : { 1, ..., v_1 },  O_1: { v_1 + 1, ..., v_2 }

    V_2 : { 1, ..., v_2 }, O_2: { v_2 + 1, ..., v_3 }

可见 :math:`V_2` 迭代覆盖 :math:`V_1 + O_1`

.. math::

    m = n - v_1

    l = 1, ..., u

每个l对应的 :math:`V_l + O_l` 都对应一个方程

迭代求解时，例如已知 :math:`y (y_1, ..., y_m)` ， 随机选择 :math:`x_1, ..., x_{v1}`

根据 :math:`y_1, ..., y_{v2-v1}` 求得 :math:`x_{v1+1}, ..., x_{v2}`

根据 :math:`y_{v2-v1+1}, ..., y_{v3-v1}` 求得 :math:`x_{v2+1}, ..., x_{v3}`

其他与UOV一致


HFE: Hidden Fields Equations
======================================

- `Solving Systems of Quadratic Equation <https://www.slideserve.com/niveditha/solving-systems-of-quadratic-equations>`_
- `Hidden Field Equations(HFE) and Isomorphisms of Polynomials(IP): two new Families of Asymmetric Algorithms <http://www.minrank.org/hfe.pdf>`_
- `Winter school-pq2016v2 <https://www.slideshare.net/LudovicPerret/winter-schoolpq2016v2>`_

F 为有限域多项式


SimpleMatrix (ABC) Encryption
======================================

`Simple Matrix Scheme for Encryption <https://www.researchgate.net/publication/268028336_Simple_Matrix_Scheme_for_Encryption>`_

`Simple Matrix – A Multivariate Public Key Cryptosystem (MPKC) for Encryption <https://www.researchgate.net/publication/279636226_Simple_Matrix_-_A_Multivariate_Public_Key_Cryptosystem_MPKC_for_Encryption>`_

n = s^2

m = 2*n

A, B, C 为3个 s x s 的矩阵

E1 = A . C, E2 = B . C

central map F : 由E1, E2拼成，m个方程

公钥 P = S . F . T  ， n -> m

加密：w = P(z) 

解密：

.. math::

    x = S^{-1} (w)

    F(y) = x

将 :math:`x = (x_1, ..., x_m)` 拆入 E1, E2，其中， :math:`x_1, ..., x_n` 为E1的对角线， :math:`x_{n+1}, ..., x_m` 为E2的对角线

根据E1, E2, A的可逆状况，分类型求解。

例如，如果A可逆，

    求解 :math:`A^-1 . E1 - B = 0, A^-1 . E2 - C = 0`

    得到 :math:`A^-1` 的 :math:`r_1, ..., r_n`

    :math:`r1, ..., r_n, y_1, ..., y_n` 结合得到m元，运用高斯消元，恢复 :math:`y_1, ..., y_n`


问题
----------

E1, E2, A 如果都不可逆，解密就会失败


