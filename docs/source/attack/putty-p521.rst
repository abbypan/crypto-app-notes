PuTTY vulnerability vuln-p521-bias
=====================================

`PuTTY vulnerability vuln-p521-bias <https://www.chiark.greenend.org.uk/~sgtatham/putty/wishlist/vuln-p521-bias.html>`_

`CVE-2024-31497: Secret Key Recovery of NIST P-521 Private Keys <https://www.openwall.com/lists/oss-security/2024/04/15/6>`_

ecdsa 的 random nonce (k) 的随机性问题，512 bits < 521 bits ，导致的bias。

与sony ps2案例类似。

RFC6979 Deterministic ECDSA 实现是个参考，此外还有side channel问题。
