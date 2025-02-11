AES
#######

doc
==========================================================

- `RFC5084 Using AES-CCM and AES-GCM Authenticated Encryption in the Cryptographic Message Syntax (CMS) <https://tools.ietf.org/html/rfc5084>`_
- `RFC5116 Authenticated Encryption <https://tools.ietf.org/html/rfc5116>`_
- `Basic question regarding OpenSSL and AES-GCM <https://security.stackexchange.com/questions/128883/basic-question-regarding-openssl-and-aes-gcm>`_
- `How to choose between AES-CCM and AES-GCM for storage volume encryption <https://crypto.stackexchange.com/questions/6842/how-to-choose-between-aes-ccm-and-aes-gcm-for-storage-volume-encryption>`_
- `Evaluation of Some BlockcipherModes of Operation <https://web.cs.ucdavis.edu/~rogaway/papers/modes.pdf>`_
- `Properties of AEAD algorithms <https://datatracker.ietf.org/doc/draft-irtf-cfrg-aead-properties/>`_
- `aes fips 197 <http://csrc.nist.gov/publications/fips/fips197/fips-197.pdf>`_

block size, key length
==========================================================

AES 的 block length 固定为128 bits, key length 可以是 128, 192, 256。

Rijndael 的 block length & key length 是 32 bits 的倍数，最小 128 bits, 最大 256 bits。例如可以取block length = 160 bits, key length = 224 bits。

Blowfish 和 3DES 是8字节。

ciphertext length
==========================================================
 
假设 x = len(plaintext)，不考虑IV/Nonce：

cbc/ecb: 以16bytes对齐

ctr/ofb/cfb : x 

gcm : x + 16 (authtag)

ccm(ctr with cbc-mac) : x + 16 (authtag)

    openssl enc -aes-256-cbc -salt -in src.txt -out src.aes-256-cbc.enc -k somepasswd 
    openssl enc -aes-256-ctr -k somepasswd -in src.txt -out src.aes-256-ctr.enc

padding
==========================================================

`EVP_EncryptInit <https://www.openssl.org/docs/manmaster/man3/EVP_EncryptInit.html>`_

`What is the difference between PKCS#5 padding and PKCS#7 padding <https://crypto.stackexchange.com/questions/9043/what-is-the-difference-between-pkcs5-padding-and-pkcs7-padding>`_

`Padding (cryptography) <https://en.wikipedia.org/wiki/Padding_(cryptography)>`_

`What are the relative merits of padding algorithms pkcs7, iso7816, and x923? <https://crypto.stackexchange.com/questions/31372/what-are-the-relative-merits-of-padding-algorithms-pkcs7-iso7816-and-x923>`_

默认使用 PKCS#7 padding。

ANSI X.923 是在最末一位指示padding length，前面以全0做padding content。

PKCS#5 and PKCS#7 是在最末一位指示padding length，前面以padding length做padding content。

ISO/IEC 7816-4 是以80标识padding，后面以全0做padding content。

可见，ANSI X.923/PKCS#7 都不能支持256bit以上的block padding，而ISO/IEC 7816-4可以。





adiantum 
==========================================================

`Adiantum and HPolyC <https://github.com/google/adiantum>`_

AES-XTS: 没硬件加速会比较慢；明文单bit变化，密文最多变化16bytes。

adiantum采用hash(NH + Poly1305), 流密码(XChaCha12)都比较快。


eax
==========================================================

`The EAX Mode of Operation <https://cseweb.ucsd.edu/~mihir/papers/eax.pdf>`_

ocb
==========================================================

`ocb <https://web.cs.ucdavis.edu/~rogaway/ocb/ocb-faq.htm>`_

`rfc7253: The OCB Authenticated-Encryption Algorithm <https://datatracker.ietf.org/doc/html/rfc7253>`_



aegis  
==========================================================

[The AEGIS Family of Authenticated Encryption Algorithms](https://datatracker.ietf.org/doc/draft-irtf-cfrg-aegis-aead/)

two AES-based authenticated encryption algorithms designed for high-performance applications


ipsec esp
==========================================================

RFC3686: Using Advanced Encryption Standard (AES) Counter Mode With IPsec Encapsulating Security Payload (ESP)

stream示例

nonce per-session 32bits, IV per-packet 64bits, ONE=0x01 32bits

    CTRBLK := NONCE || IV || ONE

    FOR i := 1 to n-1 DO
        CT[i] := PT[i] XOR AES(CTRBLK)
        CTRBLK := CTRBLK + 1
    END

    CT[n] := PT[n] XOR TRUNC(AES(CTRBLK))
