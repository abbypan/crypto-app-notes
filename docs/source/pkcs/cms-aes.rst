use AES-CCM/GCM in CMS
###########################

RFC5084: Using AES-CCM and AES-GCM Authenticated Encryption in the Cryptographic Message Syntax (CMS)

AES-CCM
==========================================================

输入: key, nonce, plaintext, optional additional authenticated data (AAD)

输出：ciphertext, authentication tag

AAD不会加密传输，只是用于辅助authenticated计算。因此，AAD不在AES-CCM的output内。

CMS authenticated-enveloped-data content type的authenticated属性中包含AAD。

AES-GCM
==========================================================

输入：key, IV, plaintext, optional additional authenticated data (AAD)

输出以及AAD细节与AES-CCM类似

推荐IV长度为12字节

Automated Key Management
==========================================================

需要key Management定期更新AES密钥

CMS authenticated-enveloped-data content type 支持4种key Management技术
- Key Transport: 以接收方的公钥加密content-authenticated-encryption key
- Key Agreement: 以接收方的公钥＋发送方的私钥生成pairwise symmetric key，再用pairwise symmetric key加密content-authenticated-encryption key
- Symmetric Key-Encryption Keys: 以预置的symmetric key-encryption key加密content-authenticated-encryption key
- Passwords: 以password或之前预置的保密信息生成key-encryption key，再用key-encryption key加密content-authenticated-encryption key
