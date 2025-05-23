PKCS#1 RSA
#############

`RSA Key Formats <https://www.cryptosys.net/pki/rsakeyformats.html>`_

`RFC8017 PKCS#1 RSA <https://tools.ietf.org/html/rfc8017>`_

Key Types 
======================================

注意，RSA2048，其中的2048指的是modulus的length

.. math::

    p = r_1,  q = r_2
    q * qInv == 1 (mod p)
    e * d_i == 1 (mod (r_i - 1))
    R_i * t_i == 1 (mod r_i)
    R_i = r_1 * r_2 * ... * r_(i-1)

length
======================================

`Private key length bytes <https://stackoverflow.com/questions/5403808/private-key-length-bytes>`_

以 rsa2048 为例，

n 2048 bits

e 一般是 3~4 bytes，经常取65537 = 0x10001

d 约 2048 bits

p 128 bytes

q 128 bytes

Encryption Schemes
======================================

RSAES-OAEP 加入了seed参与运算, 优于 RSAES-PKCS1-v1_5。

Signature Scheme
======================================

参考 `RSA-PSS – Provable secure RSA Signatures <https://rsapss.hboeck.de/rsapss.pdf>`_

PSS: Probabilistic Signature Scheme 

RSASSA-PSS 加入了salt参与运算，每次运算得到的signature值各自不同，优于 RSASSA-PKCS1-v1_5

Encoding Methods for Signatures with Appendix
====================================================

EMSA-PSS 优于 EMSA-PKCS1-v1_5，注意这是hash型的单向变换

关键参数包含 hash function, mgf (mask generation function), salt length

Mask Generation Functions 掩码生成，默认MGF1使用的hash函数为sha1。

PSS的实现代码可以参考 `Crypt::RSA::SS::PSS <https://metacpan.org/source/Crypt::RSA::SS::PSS>`_ ，不过需要注意，源码中的 $params{Message} || $params{Plaintext} 其实相当于EMSA-PSS 中的mHash，而不是M。

ASN.1 Syntax
======================================

RSA Key Representation 各项参数说明，n/p/q/e/d/...

.. note::

    RSAPublicKey ::= SEQUENCE {
                modulus           INTEGER,  -- n
                publicExponent    INTEGER   -- e
    }

    RSAPrivateKey ::= SEQUENCE {
                 version           Version,
                 modulus           INTEGER,  -- n
                 publicExponent    INTEGER,  -- e
                 privateExponent   INTEGER,  -- d
                 prime1            INTEGER,  -- p
                 prime2            INTEGER,  -- q
                 exponent1         INTEGER,  -- d mod (p-1)
                 exponent2         INTEGER,  -- d mod (q-1)
                 coefficient       INTEGER,  -- (inverse of q) mod p
                 otherPrimeInfos   OtherPrimeInfos OPTIONAL
             }

    OtherPrimeInfos ::= SEQUENCE SIZE(1..MAX) OF OtherPrimeInfo

    OtherPrimeInfo ::= SEQUENCE {
                prime             INTEGER,  -- ri
                exponent          INTEGER,  -- di
                coefficient       INTEGER   -- ti
    }
