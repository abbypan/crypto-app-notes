PKCS#3 DH
##############

Key Agreement 
==========================================================

RFC2631 PKCS#3 Diffie-Hellman Key Agreement Method

a与b通过DH协商，得到共同的ZZ。

其中，ya/xa为a的key pair，yb/xb为b的key pair

.. math::

    ZZ = g ^ (xb * xa) mod p = (yb ^ xa)  mod p  = (ya ^ xb)  mod p

    ya = g ^ xa mod p

    yb = g ^ xb mod p


KEK : key-encryption key
==========================================================

    KM = H(ZZ || OtherInfo)，其中OtherInfo包含counter

根据需要计算拼接出所需长度的KEK

    KM(counter=1) || KM(counter=2) || ...

KEK 用于加密 CEK

CEK : content-encryption key
==========================================================

CEK用于加密实际content
