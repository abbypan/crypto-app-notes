vega
========

https://eprint.iacr.org/2025/2094

zkp
--------


拆分成2个zkp，利用 Spartan & NeutronNova 处理不变量 & 变量

Vega_sc 的 pk、vk 压到 6.5M 左右，proof 91KB, setup & precompute 1s, prove 150ms, verify 40ms

Vega_mc 的 pk、vk 压到 500KB 以内, proof 108KB, setup & precompute 100 ms, prove 90ms, verify 20ms


optimization
-----------------

不变量的circuit不用重算

优化 sha256 circuit

优化 ecdsa circuit

把cbor parser 改造为 lookup table，优化 circuit
