HPKE
########

overview
======================================


IND-CCA2 secure ( chosen ciphertext attack )

方案比较简单，主要包含三部分：
- Key Encapsulation Mechanism (KEM)
- KDF
- AEAD 

key pair
======================================

skE, pkE : 发送方临时key pair

skS, pkS : 发送方长期key pair

skR, pkR : 接收方长期key pair

kem
======================================

base模式 Encap: 先基于skE&pkR计算dh，再用hkdf结合dh、kem的上下文信息（dh双方的公钥）派生share_secret；接收方必须获得enc(即pkE)

auth模式 AuthEncap: 先基于skE&pkR、skS&pkR分别计算dh并拼接，再用hkdf结合dh、kem的上下文信息（参与dh计算的三个公钥）派生`share_secret`; 接收方必须获得enc(pkE)

psk
======================================

基于share_secret，结合psk、info，派生key、`base_nonce`、`exporter_secret`

注意，如果指定了psk，则对应psk模式、`auth_psk`模式

aead
======================================

key, base_nonce用于aead

aes-gcm, chacha20-poly1305

aead nonce misuse 
--------------------------------------

management of AEAD counters: Rolling Sequence Window

Deterministic Authenticated Encryption (DAE): RFC5297 AES-SIV 

exporter
======================================

exporter_secret用法与tls1.3的exporter类似。基于exporter_secret，结合上下文信息，派生出指定长度的密钥。

detail
======================================

Compact Representation
-------------------------

public key compact representation

DeriveKeyPair(ikm)
---------------------

基于ikm派生key pair

nist curve显然要搞个for循环尝试

x25519/x448由于是扩张域，直接hkdf搞定

security
======================================

hpke不提供forward secrecy，这个主要是因为单向处理

vulnerable to key compromise impersonation attack，主要问题是skR、psk的安全性

此外，并未使用skS对(enc, ct)打签名

不搞algorithm agility就不会downgrade attack

有限的replay protection——密文流解密按顺序；其他不保证

psk_id, pkS都可能由于可标识，造成用户隐私泄漏。一种方法是使用base模式先加密一次`psk_id+pkS`获得(enc2, ciphertext2)，再正常加密message获得(enc, ciphertext)。最终传递(enc2, ciphertext2, enc, ciphertext)。。。


doc
====

- `An Analysis of Hybrid Public Key Encryption <https://eprint.iacr.org/2020/243.pdf>`_
- `HPKE: Standardizing public-key encryption <https://blog.cloudflare.com/hybrid-public-key-encryption/>`_
- `RFC 9180 Hybrid Public Key Encryption <https://www.rfc-editor.org/rfc/rfc9180.html>`_
- `Deterministic Nonce-less Hybrid Public Key Encryption <https://datatracker.ietf.org/doc/draft-irtf-cfrg-dnhpke/>`_
- `ML-KEM for HPKE <https://datatracker.ietf.org/meeting/119/materials/slides-119-cfrg-ml-kem-for-hpke-03.pdf>`_
