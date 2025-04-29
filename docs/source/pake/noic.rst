NoIC
==========

`NoIC: PAKE from KEM without Ideal Ciphers <https://eprint.iacr.org/2025/231.pdf>`_

2-Feistel 方案
-------------------

Fig.1  

通过H1, H2 结合 k 的运算变换，安全传输 M。

NoIC 方案
---------------

Fig.4

A : 生成临时密钥对 (sk, pk)

A -> B: 基于 2-Feistel 方案传输临时公钥pk

B -> A: 计算 (ct, K) = KEM.encap(pk)，并基于K计算tag，传输(ct, tag)

A : 通过KEM.decap(sk, ct)恢复K，并基于K校验tag

A, B : 基于K派生session key


