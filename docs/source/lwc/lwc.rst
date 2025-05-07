lwc 
========

light weight cryptography


factor
----------------------------------------------------------

指标： block size, key length, gate area, technology value, round, latency, throughput

design complexity is determined by the gate value  => 逻辑门。。。

ge (gate equivalent)

gate area : energy consumption relate to chip area => 大小。。。

cmos technology node: 密度。。。

security enhancement, decreasing latency, reducing energy consumption, lowering power consumption, chip area reducion

algorithm
----------------------------------------------------------

key size, block size, round, structure type

LWBC: Lightweight Block Cipher
--------------------------------
- FN: feistel network, 加解密电路相同(chaos-based prng, present, ...)
- SPN: subtitution permutation network，没有key schedule(lwhc, qarma, lcc, ...)
- SPN consume more resource than FN

LWSC: Lightweight Stream Cipher 
-------------------------------------
lsc, hybrid symmetric, a4, llsc, ...

- LSFR: linear feedback shift registers
- NLFSRs: nonlinear feedback shift registers
- rc4, salsa20, trivium, chacha

ecc(modified ecc, iecc)

trade off
----------------------------------------------------------

security, cost, performance

doc
------

- https://www.latacora.com/blog/2018/04/03/cryptographic-right-answers/
- `On the NIST Lightweight Cryptography Standardization <https://csrc.nist.gov/CSRC/media/Presentations/on-the-nist-lwc-standardization/images-media/Talk-Elliptic-Curve-Crypto-Meltem_Dec2019.pdf>`_
- `libhydrogen <https://github.com/jedisct1/libhydrogen>`_
