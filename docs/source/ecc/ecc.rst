ECC
==========================================================

elliptic curve cryptography

intro
----------------------------------------------------------

- `Technical Guideline TR-03111 Elliptic Curve Cryptography <https://www.bsi.bund.de/SharedDocs/Downloads/EN/BSI/Publications/TechGuidelines/TR03111/BSI-TR-03111_V-2-0_pdf.pdf?__blob=publicationFile&v=2>`_
- `Elliptic Curve Cryptography: ECDH and ECDSA <https://andrea.corbellini.name/2015/05/30/elliptic-curve-cryptography-ecdh-and-ecdsa/>`_
- `Cryptography - The ElGamal Public key System (Public key encryption from Diffie Hellman) <https://www.youtube.com/watch?v=fUSN7z0UquU>`_
- `Elliptic Curve Public Key Cryptography <http://gauss.ececs.uc.edu/Courses/c653/lectures/PDF/elliptic.pdf>`_
- `CPSC 467:  Cryptography and Computer Security <https://zoo.cs.yale.edu/classes/cs467/2017f/lectures/ln13.pdf>`_
- `Elliptic Curves <https://crypto.stanford.edu/pbc/notes/elliptic/>`_
- `An Introduction to the Theory of Elliptic Curves <https://www.math.brown.edu/johsilve/Presentations/WyomingEllipticCurve.pdf>`_
- `Cofactor Explained <https://loup-vaillant.fr/tutorials/cofactor>`_


ec point
----------------------------------------------------------

`Elliptic Curves <https://www.cs.purdue.edu/homes/ssw/cs655/ec.pdf>`_

如果是ec point变换，则受限于ec point的个数。本原根。

`RFC7748: Elliptic Curves for Security <https://www.rfc-editor.org/rfc/rfc7748.html>`_

`SafeCurves: choosing safe curves for elliptic-curve cryptography <https://safecurves.cr.yp.to/index.html>`_

`ECCHacks: A gentle introduction to elliptic-curve cryptography <http://ecchacks.cr.yp.to/>`_

`Explicit-Formulas Database <https://hyperelliptic.org/EFD/>`_


Edwards curve 
------------------

.. math::

    x^2 + y^2 = 1 + d*x^2*y^2 , p = 3 mod 4

twisted Edwards curve 
-------------------------

.. math::

    a*x^2 + y^2 = 1 + d*x^2*y^2 , p = 1 mod 4

short Weierstrass curve
----------------------------

.. math::

    y^2 = x^3 + a*x + b


Montgomery
----------------------------------------------------

`Montgomery curves and the Montgomery ladder <https://eprint.iacr.org/2017/293.pdf>`_

Montgomery ladder就是scalar multiples of points的快速算法, n*P算得比较快

Weierstrass curve:

.. math::

    a0*y^2 + a1*x*y + a3y = x^3 + a2*x^2 + a4*x + a6

Montgomery curve: 

.. math::

    B*y^2 = x^3 + A*x^2 + x

可见，Montgomery curve 属于Weierstrass curve

Montgomery curve可以跟 (twisted) Edwards curve 互相转换

Montgomery curve 的 x(2*P) 可以直接从 x(P) 算得，无需代入y(P)

Montgomery curve 的 x(P + Q) 可以从Montgomery ladder的(X : Y : Z)快速求得

Elligator
----------------------------------------------------

`Elligator <https://elligator.cr.yp.to/index.html>`_

`Elliptic curve points indistinguishable from random strings <https://elligator.cr.yp.to/poster.pdf>`_

Elligator: 本质上是如何隐藏信息。

通过string <-> ecc point的映射，使得交互时仅传输string，而非ecc point结构体。

进而使得中间人难以观测到实质上传输的是一个ecc point。

Elligator1 要求：q为prime, q = 3 mod 4, E 为 complete Edwards curve。

curve1174 适用 Elligator1。

Elligator2 要求： y^2 = x^3 + Ax^2 + Bx，其中 AB(A^2 - 4B)不为0。

curve25519 适用 Elligator2。

curve25519 vs curve448
----------------------------------------------------

`Elliptic curve ed25519 vs ed448 - Differences <https://crypto.stackexchange.com/questions/67457/elliptic-curve-ed25519-vs-ed448-differences>`_

`curve25519 <https://en.wikipedia.org/wiki/Curve25519)>`_
约2^`125, curve448 约 2^224, 
`Curve41417 <https://csrc.nist.gov/csrc/media/events/workshop-on-elliptic-curve-cryptography-standards/documents/presentations/session7-chuengsatiansup.pdf>`_
约 2^200

X25519  vs Ed25519
----------------------------------------------------

`Why Curve25519 for encryption but Ed25519 for signatures? <https://crypto.stackexchange.com/questions/27866/why-curve25519-for-encryption-but-ed25519-for-signatures>`_

椭圆曲线计算无非是标量乘法:
- Fixed-base: 预知固定标量，例如签名计算时的基点乘法
- variable-base: 动态变换的标量，例如ECDH
- double-base：做两次标量乘法，然后相加，例如签名校验

X25519针对variable-base计算做了优化，其实就是Montgomery Curve，利用 `Montgomery ladder <https://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication#Montgomery_ladder>`_
搞x轴。

Ed25519针对fixed-base & double-base计算做了优化，其实就是twisted Edwards curve，利用complete twisted Edwards addition law。

因此，默认不用X25519来做Ed25519的签名，但是，也可以结合Montgomery ladder应用，参考 `qDSA <https://eprint.iacr.org/2017/518>`_
。

RFC7748给出了X25519 (u, v)与Ed25519 (x, y)的转换公式。

`How do Ed25519 keys work? <https://blog.mozilla.org/warner/2011/11/29/ed25519-keys/>`_
，图画的挺好。

`Using Ed25519 signing keys for encryption <https://blog.filippo.io/using-ed25519-keys-for-encryption/>`_
，加密时直接用公式把公钥的point中的y转换为u，解密时从ed25519的key做hash提取前32byte的私钥。

`use an X25519 key for EdDSA <https://signal.org/docs/specifications/xeddsa/#elliptic-curve-conversions>`_
，定义一个从Montgomery私钥k到Edwards曲线的(A, a)的转换，再定义xeddsa、vxeddsa两种签名机制。

`the Reference Implementation of Ed25519 <https://eiken.dev/blog/2020/11/code-spotlight-the-reference-implementation-of-ed25519-part-1/>`_

`Why EdDSA held up better than ECDSA against Minerva <https://blog.cr.yp.to/20191024-eddsa.html>`_

