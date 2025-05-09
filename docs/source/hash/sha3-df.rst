SHA-3 Derived Functions
#############################

`SP 800-185 SHA-3 Derived Functions: cSHAKE, KMAC, TupleHash, and ParallelHash <https://csrc.nist.gov/publications/detail/sp/800-185/final>`_

4个函数底层都基于KECCAK。

注意security strength同时在函数名里体现，例如cSHAKE128, cSHAKE256。

cSHAKE
========

cSHAKE是可指定目标输出长度的hash，底层实际调用KECCAK。

    cSHAKE(X, L, N, S) 
    X 为输入
    L 为目标长度
    N 为function-name bit string
    S 为customization bit string

当N与S均为空string时，    

.. math::

    cSHAKE(X, L, "", "") = SHAKE(X, L)
    
当N/S不全为空string时，

.. math::

    cSHAKE128(X, L, N, S) = KECCAK[256](bytepad(encode_string(N) || encode_string(S), 168) || X || 00, L)
    cSHAKE256(X, L, N, S) = KECCAK[512](bytepad(encode_string(N) || encode_string(S), 136) || X || 00, L)
    
注意128/256的区别仅在于bytepad对齐的rate值不同，为168/136 bits。

KMAC
======

.. math::

    KMAC(K, X, L, S)
        K 为key bit string，可以为空

    KMAC128(K, X, L, S)
        newX = bytepad(encode_string(K), 168) || X || right_encode(L)
        return cSHAKE(newX, L, "KMAC", S)

在XOF(extendable output Function)模式下，KMAC运算时并不确定目标长度right_encode(0)，仅在生成output之后，再truncate。

.. math::

    KMACXOF128(K, X, L, S)
        newX = bytepad(encode_string(K), 168) || X || right_encode(0)
        return cSHAKE(newX, L, "KMAC", S)

KMAC256 主要注意rate值不同。

TupleHash
============

TupleHash的核心是对一个序列计算Hash，通过对序列内每个元素的encode_string处理，避免不同序列得到重复的结果，例如("abc", "d")与("ab", "cd")是不同的。

encode_string处理后的序列，拼接后作为输入，再调用cSHAKE处理。

.. math::

    TupleHash128(X, L, S)
        z = encode_string(X[1]) || ... || encode_string(X[n])
        newX = z || right_encode(L)
        return cSHAKE128(newX, L, "TupleHash", S)

TupleHashXOF128 与 前面类似，主要是right_encode(0)

TupleHash256 主要注意rate值不同。

ParallelHash
===============

ParallelHash 的核心是对一个超长序列，分块进行并行计算，加速。

.. math::

    ParallelHash128(X, B, L, S)
           B为分块字节数。
           X按B字节分块后，得到n个Block。
           z = left_encode(B)
           z ||= cSHAKE128(Block[1], 256, "", "") || ... || cSHAKE128(Block[n], 256, "", "")
           newX = z || right_encode(n) || right_encode(L)
           return cSHAKE128(newX, L, "ParallelHash", S)

ParallelHashXOF128  与 前面类似，主要是right_encode(0)

ParallelHash256 主要注意rate值不同。

Hashing into a Range
======================

假设要生成一个[ 0, R ) 内的整数

可以计算 k = cell(lg(R)) + 128

hash计算得到k bits的输出Z

将Z转换为整数

security
=================

L, S 最长不超过 :math:`2^2040 - 1` bits。

K长度不应低于security strength。

MAC场景下，L长度不应低于32bits，尽量>=64bits。


doc
======

- `FIPS 202 SHA-3 Standardization <https://csrc.nist.gov/Projects/Hash-Functions/SHA-3-Project/SHA-3-Standardization)>`_
- `FIPS 180-4 Secure Hash Standard (SHS) <https://csrc.nist.gov/publications/detail/fips/180/4/final)>`_
- `NIST Policy on Hash Functions <https://csrc.nist.gov/Projects/Hash-Functions/NIST-Policy-on-Hash-Functions)>`_

