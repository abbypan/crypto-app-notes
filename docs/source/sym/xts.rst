xts
==========================================================

IEEE 1619

`NIST SP 800-38E <https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38e.pdf>`_

`XEX-based tweaked-codebook mode with ciphertext stealing (XTS) <https://en.wikipedia.org/wiki/Disk_encryption_theory#XEX-based_tweaked-codebook_mode_with_ciphertext_stealing_XTS>`_

`You don't want xts <https://sockpuppet.org/blog/2014/04/30/you-dont-want-xts/>`_

倒数第二个块的密文截取，拼到最后一块的明文后面，做为最后一个分组加密的输入。

解密时，先解密最后一个分组密文，获得最后一块明文+截取的倒数第二个块的密文，再进行倒数第二个块的密文解密。

注意，用到了两个key。key1作用于分组明文，key2随分组id处理。除了倒数第二个分组，每个分组都可以独立解密。
