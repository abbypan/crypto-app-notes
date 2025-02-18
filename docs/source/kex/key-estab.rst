Key-Establishment
######################

overview
======================================

`SP 800-56A Rev. 3 Recommendation for Pair-Wise Key-Establishment Schemes Using Discrete Logarithm Cryptography <https://csrc.nist.gov/publications/detail/sp/800-56a/rev-3/final>`_

`SP 800-56B Rev. 2 Recommendation for Pair-Wise Key-Establishment Using Integer Factorization Cryptography <https://csrc.nist.gov/publications/detail/sp/800-56b/rev-2/final>`_

`SP 800-56C Rev. 2 Recommendation for Key-Derivation Methods in Key-Establishment Schemes <https://csrc.nist.gov/publications/detail/sp/800-56c/rev-2/final>`_

trusted third party(TTP)

mac: hmac, aes-cmac, kmac

random  bit  generator  (RBG): SP 800-90

mqv 漏洞太多

IEEE P1363 

800-56c
======================================

Z: shared secret

OtherInput: salt, L, {IV}, FixedInfo

FixedInfo: Label, Context

DerivedKeyingMaterial = KDM(Z, OtherInput)

one-step kdf
---------------

shared secret -> Key-Derivation key

- hash(x), fips 180, fips 202
- hmac(key=salt, message=x), fips 198
- kmac(key=salt, message=x, H_outputBits, "KDF"), SP 800-185

可以基于 H(counter || Z || fixedInfo)，counter递增，迭代拼接

two-step kdf
---------------

extraction-then-expansion,  shared secret -> Key-Derivation key -> keying material

randomness extraction => KDK
- hmac(key=salt, message = Z)
- aes-cmac(key=salt, message=Z), sp 800-38B

key expansion => DerivedKeyingMaterial
- KDF(KDK, L, {IV}, FixedInfo)

如果PRF-based KDF是feedback mode，则需要IV。

如果PRF-based KDF是Counter mode / Double-Pipeline Iteration Mode，则不需要IV。
