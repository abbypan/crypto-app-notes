Ascon
==========

https://csrc.nist.gov/projects/lightweight-cryptography

`NIST.IR.8454 <https://nvlpubs.nist.gov/nistpubs/ir/2023/NIST.IR.8454.pdf>`_

`NIST.SP.800-232 Ascon-Based Lightweight Cryptography Standards for Constrained Devices <https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-232.pdf>`_


Ascon-AEAD128, nonce-based (禁止重用), 128-bit security strength, tag不得低于32 bits

Ascon-Hash256, 256-bit output, 128-bit security strength, 不用于hmac

Ascon-XOF128, 128-bit security strength

Ascon-CXOF128, 可指定custom string，128-bit security strength
