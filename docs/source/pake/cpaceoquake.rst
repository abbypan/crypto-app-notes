CPaceOQUAKE
==============

`Hybrid Post-Quantum Password Authenticated Key Exchange <https://datatracker.ietf.org/doc/draft-vos-cfrg-pqpake/>`_


cpace 实现 passowrd hiding，oquake 实现pqc保护，cpaceoquake通过sequential composition顺序结合。

派生session key时参数包含tr1, tr2, sk1, sk2。


CPaceOQUAKE+
==============

注册
---------

client pwd，salt，client id, server id 派生 verifier，seed

seed 派生 pk, sk

server 记录 salt, verifier, pk, client id, server id


password confirm
--------------------

client 与 server 以 verifier 建立 CPaceOQUAKE 信道，派生SK1

server在SK1的保护下，结合已登记的client pk，返回k

client以client sk解密k

双方基于SK1，k等信息派生client_key, server_key
