Dual_EC_DRBG 
###################

background
==========================================================

http://blog.0xbadc0de.be/archives/155

随机数生成算法 NSA后门

这个问题在于dQ=P，而 :math:`i_2 .. i_n, o_1 .. o_n` 可以交错链式生成（没有其他伪随机参数加入计算）。

因此直接回溯至 :math:`i_1Q = A` ，而A不超过32 bytes，可以直接进行暴力猜解。

指定x解椭圆方程求出可行的y，再计算比对 :math:`o_1` 。。。

从这点看，结尾说的没错，确实疯狂。  


detail
==========================================================

`Dual EC: A Standardized Back Door <https://eprint.iacr.org/2015/767>`_

如果通过output可以推测PRNG的某些内部状态，进而可以推测后续的output，则PRNG是有问题的。

密码算法：设计，分析，标准化，选择，实现，部署

.. math::

    state-based PRNG, with f and g
        s_1 = f(s_0)
        r_1 = g(s_1)

        s_2 = f(s_2)
        r_2 = g(s_2) 

显然，f and g 应该是one way function, 且如果是

Basic Dual EC
----------------------------------------------------

.. math::

    s_1 = x(s_0 * P)，r_1 = x(s_1 * Q) ，显然，如果知道P=d*Q，则整个 s_i, r_i 的交错序列就可以递推出来。

    假设 r = (r_1, y_r_1) = s_1*Q

    d*r = d*s_1*Q = s_1*d*Q = s_1*P
    s_2 = x(d*r) = x(s_1*P)  //可见，s_2 内部状态可通过 r_1 ，结合d推导出来


Dual EC 2006 with additional input
---------------------------------------

.. math::

    t_0 = s_0 xor H(adin0)

    s_1 = x(t_0 * P)
    r_1 = x(s_1 * Q)
    t_1 = s_1 xor H(adin1)

    s_2 = x(t_1 * P)  // 可见，此时 s_2 与 r_1 之间没有递推关系
    r_2 = x(s_2 * Q)

    s_3 = x(s_2 * P)  // s_3 = x(d * r_2) = x(d * s_2 * Q) = x(s_2 * P)，s_3与s_2/r_2之间。。。
    r_3 = x(s_3 * Q)


Dual EC 2007 with additional input
--------------------------------------

.. math::

    t_0 = s_0 xor H(adin_0)

    s_1 = x(t_0 * P)
    r_1 = x(s_1 * Q)

    s_2 = x(s_1 * P)
    t_2 = s_2 xor H(adin_2)
    
    s_3 = x(t_2 * P)
    r_3 = x(s_3 * Q) 

    s_4 = x(s_3 * P) // 可见，s_2 与 s_1/r_1、s_4 与 s_3/r_3 ，s_5 与 s_4/r_4 之间。。。
    r_4 = x(s_4 * Q)

    s_5 = x(s_4 * P)


