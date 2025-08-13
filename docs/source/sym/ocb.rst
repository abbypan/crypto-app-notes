ocb
========

RFC7253

block cipher mode: offset code book

aead

speed
---------

enc & dec 同速

比gcm快

nonce
----------

不要求random nonce，可用counter

nonce 96 bits


A: Associated data
------------------------

使用L_* = ENCIPHER(K, zeros(128))为起点，构造L_i

拆分128-bit blocks，构造A_i

以 Offset_0 = zeros(128) 为起点，计算

.. math::

        Offset_i = Offset_{i-1} xor L_{ntz(i)}

        Sum_i = Sum_{i-1} xor ENCIPHER(K, A_i xor Offset_i)

        如果最后一个 A_*<128 bits，则再构造异或一个block

        返回最终的Sum

tag
------

tag 可 trunc
