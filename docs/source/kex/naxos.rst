naxos
==========================================================

`Stronger Security of Authenticated Key Exchange <https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/strongake-submitted.pdf>`_

authentic key exchange (AKE) protocols 也挺多的，naxos是一个。。。

naxos是基于long-term private key + ephemeral secret key 的hash派生临时公钥，再执行3次DH，省掉Sig操作。标识保护欠一点。

noise的思路也类似。

noise的兼容性更好, naxos则通过派生临时公钥省掉了long-term key的ss DH。

