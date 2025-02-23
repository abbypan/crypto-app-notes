HMAC 
==========================================================

RFC2104

    authentication key K

    ipad = the byte 0x36 repeated B times

    opad = the byte 0x5C repeated B times.

To compute HMAC over the data "text" we perform

    HMAC = H(K XOR opad, H(K XOR ipad, text))

也就是用K对text做处理

H函数可选SHA1, MD5, RIPEMD, 等等

HMAC-SHA-256
==========================================================

RFC4868

   Block size:  the size of the data block the underlying hash algorithm
      operates upon.  For SHA-256, this is 512 bits, for SHA-384 and
      SHA-512, this is 1024 bits.

   Output length:  the size of the hash value produced by the underlying
      hash algorithm.  For SHA-256, this is 256 bits, for SHA-384 this
      is 384 bits, and for SHA-512, this is 512 bits.

   Authenticator length:  the size of the "authenticator" in bits.  This
      only applies to authentication/integrity related algorithms, and
      refers to the bit length remaining after truncation.  In this
      specification, this is always half the output length of the
      underlying hash algorithm.

注意参看2.7.2节的示例

Block size可以padding对齐，Output length长度固定，Authenticator length取Output的前一半

doc
===========

NIST 800-38

FIPS 198
