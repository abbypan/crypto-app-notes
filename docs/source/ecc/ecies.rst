ECIES
==========================================================

Elliptic Curve Integrated Encryption Scheme 

- `A Survey of the Elliptic Curve Integrated Encryption Scheme <https://pdfs.semanticscholar.org/9f5e/ec8cb6a8883498157e8e27723da52ae4c752.pdf>`_
- `Integrated Encryption Scheme <https://en.wikipedia.org/wiki/Integrated_Encryption_Scheme>`_
- `Elliptic Curve Integrated Encryption Scheme (ECIES) <https://www.youtube.com/watch?v=saZj0ZKRNl0>`_
- `A Comparison of the Standardized Versions of ECIES <http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.819.9345&rep=rep1&type=pdf>`_
- `libsodium doc <https://download.libsodium.org/doc/>`_
- `ECC Encryption / Decryption <https://cryptobook.nakov.com/asymmetric-key-ciphers/ecc-encryption-decryption>`_
- `Crypto In Action <https://github.com/longcpp/CryptoInAction>`_
- `eciespy <https://pypi.org/project/eciespy/>`_

使用KA(key agreement)协商一个密钥S，使用KDF基于S生成会话密钥，其实基本上就是ECDH。

会话密钥用于ENC对称加密，同时还有MAC函数, HASH函数。

因此plaintext长度基本不受限。

