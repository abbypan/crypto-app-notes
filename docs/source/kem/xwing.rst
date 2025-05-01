X-Wing
=========

`X-Wing: general-purpose hybrid post-quantum KEM <https://datatracker.ietf.org/doc/draft-connolly-cfrg-xwing-kem/>`_ 

使用SHAKE256，基于SK派生 ML-KEM 的SK_M、X25519的SK_X。

X-Wing Encap时，复用ML-KEM的Encap，随机生成X25519的EK_X。

ct_X 为 EK_X 导出的X25519公钥。

最终返回 SHA3-256(concat( ss_M, ss_X, ct_X, pk_X, XWingLabel))


