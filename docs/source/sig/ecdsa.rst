ecdsa
#########


malleability
==========================================================

(r, s) , (r, -s) 均可校验成功，签名构造。

low-s value 可修复该漏洞。

Verification Without Hash Pre-Image
==========================================================

可根据public key 任意构造(r, s, h)

因此，校验必须带原message，而非只校验h=hash(message)


Deterministic ECDSA
==========================================================

RFC6979: Deterministic Usage of the Digital Signature Algorithm (DSA) and Elliptic Curve Digital Signature Algorithm (ECDSA)

sony ps3 的案例是没有按算法要求生成随机数k。

RFC6979 的处理是，用SP800-90A的HMAC_DRBG随机数生成器生成k，其中，熵值用私钥(x)，nonce用hash(message)。

RFC8032 EdDSA的做法也类似。

Deterministic ECDSA/EdDSA + Randomness
==========================================================


完全确定的签名，又容易被side-channel and fault injection attacks，又把random加回去

思路是 random + private key + message 三者结合，搞出随机数，或者签名参数


doc
=======
- `ECDSA Malleability <http://coders-errand.com/malleability-ecdsa-signatures/>`_
- `The Exact Security of ECDSA <http://cacr.uwaterloo.ca/techreports/2000/corr2000-54.ps>`_
- `Elliptic Curve Cryptography: ECDH and ECDSA <https://andrea.corbellini.name/2015/05/30/elliptic-curve-cryptography-ecdh-and-ecdsa/>`_
- `How Not to Use ECDSA <https://yondon.blog/2019/01/01/how-not-to-use-ecdsa/>`_
- `FIPS PUB 186-3 Digital Signature Standard (DSS)  <https://csrc.nist.gov/csrc/media/publications/fips/186/3/archive/2009-06-25/documents/fips_186-3.pdf>`_
- `Deterministic ECDSA and EdDSA Signatures with Additional Randomness <https://www.ietf.org/archive/id/draft-mattsson-cfrg-det-sigs-with-noise-03.html>`_
- `ECDSA: Handle with Care <https://blog.trailofbits.com/2020/06/11/ecdsa-handle-with-care/>`_
- `BreakingECDSAwithLLL <https://github.com/daedalus/BreakingECDSAwithLLL>`_
