aead
==========================================================

Authenticated encryption with associated data (AEAD)，例如ccm (CBC counter mode), gcm (Galois counter mode)。

gcm
----------------------------------------------------

`The Galois/Counter Mode of Operation (GCM) <https://pdfs.semanticscholar.org/b4c4/66e7158c158fb513b729d6302521017d72fa.pdf>`_

`Nonce-Disrespecting Adversaries <https://github.com/nonce-disrespect/nonce-disrespect>`_

nonce不能重用。


xgcm
----------------------------------------------------

`XGCM <https://soatok.blog/2022/12/21/extending-the-aes-gcm-nonce-without-nightmare-fuel/>`_

`XAES-256-GCM/11 <https://words.filippo.io/dispatches/xaes-256-gcm-11/>`_


dndk-gcm
----------------------------------------------------

[Double Nonce Derive Key AES-GCM (DNDK-GCM)](https://datatracker.ietf.org/doc/draft-gueron-cfrg-dndkgcm/)

为gcm增加key commitment，避免不同的 (k1, m1), (k2, m2) 加密得到相同的 (c, t)

K, NONCE(要求24 bytes，要求random) -> KC, DK

KC用于key commitment用于校验

DK, 全0的NONCE用于加密
