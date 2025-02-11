cts : ciphertext stealing
==========================================================

分组加密模式适用。

通过密文处理，无需填充明文到分组大小。

cbc-cts
==========================================================

`CBC ciphertext stealing <https://en.wikipedia.org/wiki/Ciphertext_stealing#CBC_ciphertext_stealing_decryption_using_a_standard_CBC_interface>`_

加密：明文末尾补0 -> CBC正常加密 -> 对换倒数两个密文块 -> 密文末尾裁减到与明文等长。

解密：解密倒数第二个密文块 -> 从解密结果获得末尾裁减的部分，拼接到最后一个密文块末尾 -> 对换倒数两个密文块 -> 解密 -> 裁减明文。
