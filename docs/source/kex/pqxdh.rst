pqxdh
========

`The PQXDH Key Agreement Protocol <https://signal.org/docs/specifications/pqxdh/>`_

Revision 3 沿用 `x3dh </2020/11/20/x3dh>`_ 的框架，增加了 PQSPK，PQOPK 的PQ KEM key， CRYSTALS-KYBER-1024。

PQKEM encapsulate symmetric secret (ss) 用于 session key 的派生。

::

    (CT, SS) = PQKEM-ENC(PQPKB)
    SK = KDF(DH1 || DH2 || DH3 || SS)
    SK = KDF(DH1 || DH2 || DH3 || DH4 || SS)


PQSPK, PQOPK 仍由 IK sign，没上PQ SIG。

