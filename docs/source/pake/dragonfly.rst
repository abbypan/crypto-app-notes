Dragonfly Key Exchange
#########################

`RFC7664 <https://www.rfc-editor.org/rfc/rfc7664.html>`_

Derivation of the Password Element
======================================

1. 基于双方id、password、counter计算hash，得到base

    :math:`base = H(max(Alice,Bob) | min(Alice,Bob) | password | counter)`

#. 基于base，派生 :math:`seed ( 1 <= seed < p)`

.. math::

    temp = KDF-n(base, "Dragonfly Hunting and Pecking")

    seed = (temp mod (p - 1)) + 1

#. 如果seed二次剩余，则作为x；否则counter++，重新计算base。

#. 基于计算E公式，选取较小的y值，得到 :math:`PE = (x, y)`

Commit Exchange
=================

q为阶

.. math::

    scalar = (private + mask) mod q

    Element = - mask * PE


双方互相发送 (scalar, Element)

.. math::

    share = private * (peer-Element + peer-scalar * PE) 

          = private * peer-private * PE

    ss = F(share)

    kck | mk = KDF-n(ss, "Dragonfly Key Derivation")


Confirm Exchange
===================

校验confirm，确认可用，则以mk做为master key建立会话

.. math::

    confirm = H(kck | scalar | peer-scalar | Element | Peer-Element | <sender-id>)


security
==========

resistant to active attack, passive attack, and offline dictionary attack

不同client区分会话密钥

side-channel attack

small subgroup

