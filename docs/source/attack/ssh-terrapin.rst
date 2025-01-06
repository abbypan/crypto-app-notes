SSH: TerrapinAttack
=====================

`Terrapin Attack <https://terrapin-attack.com/>`_

`Terrapin Attack: Breaking SSH Channel Integrity By Sequence Number Manipulation <https://terrapin-attack.com/TerrapinAttack.pdf>`_

mitm条件下，操纵handshake sequence计数，drop关键extinfo，实施downgrade attack。

symmetric cipher mode影响, something synced is influnenced by seq number。

核心还是full transcript hash，参考sequence number reset, handshake与record互不影响；end-of-communication message，标识h结束。
