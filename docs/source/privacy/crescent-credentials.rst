Crescent
===========

https://github.com/microsoft/crescent-credentials/

https://eprint.iacr.org/2024/2013

Crescent针对原有issuer颁发的JWT格式的Credentials做变换，并对Credentials中的device public key做zkp， 支持selective disclose attr

message sig仍沿用原有ecdsa

issuer自身的public key，以及Crescent提供的参数做为trust anchor

Crescent 主动兼容原有issuer

device public key
-------------------

提取public key的x值，Q = q_0 + 2^128 * q_1，记为 Q = (q_0, q_1)


