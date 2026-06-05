Crescent
===========

https://github.com/microsoft/crescent-credentials/

https://eprint.iacr.org/2024/2013

use case
------------

selective disclosure


credential
----------------

原有issuer颁发的JWT格式的Credentials维持不变

原有issuer public key 的可信校验维持不变，prover & verifier 自行维持

zkp groth16
---------------

Crescent生成PK (Prepare Params)、VK， 提供的公开参数

PK约几百MB，VK在1KB左右

用于对Credentials做Groth16 zk-snarks, committed attr(基于sub-prover), selective disclose attr (revealed)

Crescent Service 独立于原有 credential 体系


sub provers for show
-------------------------

类似DLEQ的sub prover，可用于range proof 等

Σ-proof

device public key
-------------------

https://github.com/personaelabs/spartan-ecdsa

issuer public key ->  device public key

credential与device public key绑定


device public key -> message

device public key 隐藏

提取public key的x值，Q = q_0 + 2^128 * q_1，记为 Q = (q_0, q_1)

构造linking snark proof，结合spartan-T256形式的ecdsa作为message sig algorithm, 结合poseidon hashing
