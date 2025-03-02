Lattice-based Cryptography
##############################


RLWE (Ring Learning With Errors) Problem
===============================================

`Introduction to post-quantum cryptographyand learning with errors <https://summerschool-croatia.cs.ru.nl/2018/slides/Introduction%20to%20post-quantum%20cryptography%20and%20learning%20with%20errors.pdf>`_

`Equivalence of Search and Decisional (Ring-) LWE <https://slideplayer.com/slide/14640091/>`_

`Learning With Errors (LWE) and Ring LWE <https://medium.com/asecuritysite-when-bob-met-alice/learning-with-errors-lwe-and-ring-lwe-accf72f98c22>`_

`Python and Crypto: Learning With Errors (LWE) and Ring LWE <https://asecuritysite.com/public/lwe_ring.pdf>`_

`The Learning with Errors Problem <https://cims.nyu.edu/~regev/papers/lwesurvey.pdf>`_

在 :math:`F_q` 有限域上的多项式 p(x) 环

    b_i(x) = a_i(x) * s(x) + e_i(x)

search LWE problem: 已知(b\_i(x), a\_i(x))求解s(x)是困难的

Decision LWE problem: 已知(b\_i(x), a\_i(x))，检查是否可以求解s(x)，还是随机pair

RLWE-KEX
======================================

`Ring Learning With Errors for Key Exchange (RLWE-KEX) <https://medium.com/asecuritysite-when-bob-met-alice/ring-learning-with-errors-for-key-exchange-rlwe-kex-5dc0ce37e207>`_

b_A(x) = A * s_A(x) + e_A(x)

b_B(x) = A * s_B(x) + e_B(x)

两边交换b_A(x), b_B(x)

share_A = s_A(x) * b_B(x)  / p(x) = s_A(x) * (A * s_B(x) + e_B(x)) / p(x)

share_B = b_A(x) * s_B(x) / p(x)  = s_B(x) * (A * s_A(x) + e_A(x)) / p(x)

LWE encryption
======================================

`Directions in Practical Lattice Cryptography Vadim Lyubashevsky IBM Research – Zurich. <https://slideplayer.com/slide/10426928/>`_

`On Ideal Lattices andLearning With Errors Over Rings <https://web.eecs.umich.edu/~cpeikert/pubs/slides-ideal-lwe.pdf>`_

`Lattice-Based Cryptography <https://slideplayer.com/slide/16130291/>`_

公钥为 (a, t)

a*s + e = t

随机生成(r, e1)

r*a + e1 = u

r*t + e2 + m = v

明文为m, 密文为 (u, v)

解密

.. math::

    v - u*s = r*t + e2 + m - (r*a + e1)*s  

            = r*(a*s + e) + e2 + m - (r*a + e1)*s

            = m + r*e + e2 - e1*s


LWE signature
======================================

.. math::

    t = a*s + e 

    c = H( a*u + v, m )

    z = s*c + u

    a*z - t*c = a*(s*c + u) - (a*s + e)*c = a*u - e*c


BLISS (Bimodal Lattice Signature Scheme)
======================================

`Lattice Signatures and Bimodal Gaussians <https://link.springer.com/content/pdf/10.1007/978-3-642-40041-4_3.pdf>`_

`Post-quantum Signature Bliss <https://medium.com/@billatnapier/post-quantum-signature-bliss-632b3904c9e9>`_

基础
--------

私钥S, 公钥(T, A)

    T = A*S mod q

message digest μ

    c = H( A*y mod q, μ ) 

    z = S*c + y

签名 (z, c)

校验  c = H( A*z − T*c mod q, μ) = H( A*S*c + A*y − T*c mod q, μ)

BLISS
---------

    A*S = q*I_n mod 2q

签名
    y 为随机数
    c = H( A*y mod 2q, μ )
    b 为随机选取的0/1
    z = y + (−1)^b*S*c

校验 c = H( A*z + q*c mod 2q, μ) = H( A*y + (-1)^b*A*S*c + q*c mod 2q, μ)  

NTRU 
======================================

`ntru prime <https://ntruprime.cr.yp.to/nist/ntruprime-20190330.pdf>`_

`Quantum technology and its impact on security in mobile networks <https://www.ericsson.com/en/reports-and-papers/ericsson-technology-review/articles/ensuring-security-in-mobile-networks-post-quantum>`_

falcon
======================================

`falcon <https://csrc.nist.gov/CSRC/media/Presentations/Falcon/images-media/Falcon-April2018.pdf>`_

dilithium
======================================

`dilithium <https://slideplayer.com/slide/14252214/>`_


参考资料
======================================

- `Lattice-based Cryptography <https://pqcrypto2016.jp/data/Nguyen-pdf-PQC-LatticeCrypto.pdf>`_
- `The Quantum Menace <https://reidbix.github.io/QuantumMenace/TheQuantumMenacePresentation.pdf>`_
- `Practical Aspects of Modern Cryptography <https://slideplayer.com/slide/14252306/>`_
- `A Decade of Lattice Cryptography <https://web.eecs.umich.edu/~cpeikert/pubs/lattice-survey.pdf>`_
- `Summer School on real-world crypto and privacy <https://summerschool-croatia.cs.ru.nl/2018/program.shtml>`_
- `NIST Cryptographic Standards and Guidelines <https://csrc.nist.gov/Projects/Cryptographic-Standards-and-Guidelines>`_
- `Lattice-based Cryptography <https://pqcrypto2016.jp/data/Nguyen-pdf-PQC-LatticeCrypto.pdf>`_
- `ETSI Quantum Safe Cryptography and Security <https://www.etsi.org/images/files/ETSIWhitePapers/QuantumSafeWhitepaper.pdf>`_
- `ETSI Implementation Security of Quantum Cryptography <https://www.etsi.org/images/files/ETSIWhitePapers/etsi_wp27_qkd_imp_sec_FINAL.pdf>`_
- `Lattice Based Cryptography for Beginners <https://eprint.iacr.org/2015/938.pdf>`_
- `2019 Status Report on the First Round of the NIST Post-Quantum Cryptography Standardization Process <https://nvlpubs.nist.gov/nistpubs/ir/2019/NIST.IR.8240.pdf>`_
- `Post-quantum cryptography: Lattice-based cryptography <https://www.redhat.com/en/blog/post-quantum-cryptography-lattice-based-cryptography>`_
