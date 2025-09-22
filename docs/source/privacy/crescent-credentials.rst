Crescent
===========

https://github.com/microsoft/crescent-credentials/

https://eprint.iacr.org/2024/2013

Crescent针对原有issuer颁发的JWT格式的Credentials做变换

issuer自身的public key，以及Crescent提供的参数做为trust anchor

Crescent 主动兼容原有issuer

zkp
---------------

对Credentials做Groth16 zk-snarks, committed attr(基于sub-prover), selective disclose attr (revealed)

sub provers for show
-------------------------

类似DLEQ的sub prover，可用于range proof 等

Σ-proof

device public key
-------------------

https://github.com/personaelabs/spartan-ecdsa

与device public key绑定的credential，需要结合device private key的signature证明自身

因此，device public key自身的privacy需要保护，构造zkp

提取public key的x值，Q = q_0 + 2^128 * q_1，记为 Q = (q_0, q_1)

构造linking snark proof，结合spartan-T256形式的ecdsa作为message sig algorithm, 结合poseidon hashing
