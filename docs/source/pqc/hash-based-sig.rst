hash based signature
#######################


Lamport
============

准备：私钥->HASH->公钥； 

签名：Message->按bit的0,1映射到对应的私钥； 

校验：Message->按bit的0,1映射到对应的签名bit，计算hash，检查是否与公钥匹配； 


Winternitz OTS (WOTS)
==============================

准备： 

把HASH函数目标长度s，随机选择w，计算 t

随机选私钥 :math:`X = (X_1, ..., X_t)` ，均为s bits

计算 :math:`Y_i = H^(2^w-1)(X_i)` ，公钥为 :math:`Y=H(Y_1 || ... || Y_t)`

 
签名： 

将长度为s的消息M按w长分块，并计算checksum,并将checksum值也按w长分块，获得 :math:`b_1, ..., b_t` 。 

.. math::

    sig_i = H^(b_i)(X_i) 
    sig = (sig_1 || ... || sig_t) 


校验： 


.. math::

    sig_i' = H^(2^w-1-bi)(sig_i) = H^(2^w-1-b_i)(H^(b_i)(X_i))= Y_i 
    H(sig_1' || … || sig_t') = Y 


Winternitz OTS+ (WOTS+)
==============================


指定了 :math:`r_i` 与x做xor， :math:`f_k` 做迭代。

:math:`r_1 ... r_i` 迭代计算，随机生成的 r

:math:`f_k` 是此次选择的函数

对应 :math:`(sk_1 , ... , sk_l)` 是 sk = secret key

其他内容与WOTS基本一致

Merkle-Signature Scheme(MSS) 
=================================

希望复用public key 

选择一个n, 建一棵层为n+1的二叉树，最底层(n=0)叶子为 :math:`H(Y_i) , i = 0, ... , 2^(n-1)` . 

:math:`(X_i, Y_i)` 为一个keypair, :math:`Y_i` 为公钥。 

:math:`An = a_(n, 0)` 为第n+1层的最左边的节点，即公钥。 

sig' 是此次选用 :math:`(X_i, Y_i)` 执行单次签名的结果。 

.. math::

    sig = (sig' || auth_0 || ... || auth_{n-1}) 

其中，:math:`auth_0` 到 :math:`auth_{n-1}` 表示从底层 :math:`H(Y_i)` 叶子到顶层public key的路径上的其他节点的值。 

 
校验： 

首先校验sig'。 

再根据 :math:`H(Y_i)` 结合 :math:`auth_0 ... auth_{n-1}` 计算An是否与公钥完全一致。 

xmss
=======

xmss的每个节点与邻居一起hash之前，自己会先xor一个bitmask。

xmss tree的叶子节点存的不是公钥的hash，而是一个L-tree的根节点。

每个L-tree的叶子节点存的是WOTS+的public key。

L-tree节点与领居一起hash之前，也会先xor一个bitmask。注意L-tree的mask与xmss tree的mask不同；所有L-tree共用相同的mask配置。


参考资料
==========

- `Hash-based Signatures <http://www.pqsignatures.org/index/hbs.html>`_
- `Hash-Based Signatures Part I: One-Time Signatures (OTS) <https://cryptoservices.github.io/quantum/2015/12/04/one-time-signatures.html>`_
- `Merkle Signature Schemes, Merkle Trees and Their Cryptanalysis <https://www.emsec.ruhr-uni-bochum.de/media/crypto/attachments/files/2011/04/becker_1.pdf>`_
- `RFC8391: XMSS: eXtended Merkle Signature Scheme <https://datatracker.ietf.org/doc/rfc8391/>`_
- `RFC8554: Leighton-Micali Hash-Based Signatures <https://datatracker.ietf.org/doc/rfc8554/>`_
- `SPHINCS+ <https://csrc.nist.gov/CSRC/media/Presentations/SPHINCS/images-media/SPHINCS-Plus-April2018.pdf>`_
- `WOTS <https://www.di-mgt.com.au/pqc-03-winternitz.html>`_
- `W-OTS+– Shorter Signatures for Hash-BasedSignature Schemes <https://huelsing.net/wordpress/wp-content/uploads/2013/05/wotsspr.pdf>`_
- `SPHINCS+ Example <https://www.di-mgt.com.au/pqc-08-sphincs-example.html>`_
- `Hash-Based Signatures Part IV: XMSS and SPHINCS <https://cryptoservices.github.io/quantum/2015/12/08/XMSS-and-SPHINCS.html>`_
- `XMSS – A Practical Forward Secure Signature Scheme based on Minimal Security Assumptions <https://eprint.iacr.org/2011/484.pdf>`_
- `XMSS - A Practical Forward Secure Signature Scheme <https://slideplayer.com/slide/6080497/>`_
- `SPHINCS+ - Step by Step <https://er4hn.info/blog/2023.12.16-sphincs_plus-step-by-step/>`_
