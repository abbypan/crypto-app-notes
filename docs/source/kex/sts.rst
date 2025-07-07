
STS
==========================================================

basic STS
----------------------------------------------------

.. math::

    A -> B : g^x

    B -> A : g^y, B, { SIG_B (g^x, g^y) }_Ks

    A -> B : A , { SIG_A(g^y, g^x) }_Ks

主要问题是identity misbinding

maced-signature
----------------------------------------------------

.. math::

       b = SIG_A(g^y, g^x)

       c = MAC_Ks(b)

把加密替换成mac

Photuris 变种
----------------------------------------------------

.. math::

    A -> B : g^x

    B -> A : g^y, B,  SIG_B (g^x, g^y, g^{xy}) 

    A -> B : A , SIG_A(g^y, g^x, g^{xy})

去掉加密

