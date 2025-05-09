VDAF
###############################################


Verifiable Distributed Aggregation Functions
====================================================

`vdaf <https://datatracker.ietf.org/doc/draft-irtf-cfrg-vdaf/>`_

主要针对smc的场景，相对于DP引入noise的模糊统计，vdaf是通过 **拆分提交-独立计算-合并求值** 处理。

    client -> aggregator -> collector

client随机机拆分上报n个share给n个aggregator，aggregator整合计算后提交到collector，collector合并计算。

如果client提交share, proof给到aggregator，则为vdaf。


prio
=========

`prio <https://crypto.stanford.edu/prio/>`_

zkp: secret-shared non-interactive proofs (SNIPs)

proof通过 :math:`f(r), r*g(r), r*h(r)` 构造 :math:`r*(f(r)*g(r) - h(r))` 的validate进行聚合校验。


PLASMA
========

- `PLASMA: Private, Lightweight Aggregated Statistics against Malicious Adversaries with Full Security <https://eprint.iacr.org/2023/80>`_
- `slides-116-cfrg-plasma <https://datatracker.ietf.org/meeting/116/materials/slides-116-cfrg-plasma-00.pdf>`_
