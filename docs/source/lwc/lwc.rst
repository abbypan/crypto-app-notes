lwc 
========

light weight cryptography

`Current Lightweight Cryptography Protocols in Smart City IoT Networks: A Survey <https://arxiv.org/abs/2010.00852>`_

factor
==========================================================

指标： block size, key length, gate area, technology value, round, latency, throughput

design complexity is determined by the gate value  => 逻辑门。。。

ge (gate equivalent)

gate area : energy consumption relate to chip area => 大小。。。

cmos technology node: 密度。。。

security enhancement, decreasing latency, reducing energy consumption, lowering power consumption, chip area reducion

algorithm
==========================================================

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
==========================================================

security, cost, performance

