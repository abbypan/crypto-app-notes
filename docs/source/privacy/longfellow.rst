longfellow
===========

https://github.com/google/longfellow-zk/tree/main

use case
-------------

selective disclosure

pk 200 KB, VK 200KB, proof 300KB

setup 5s, prov 500ms, verify 250ms

summary
-------------

ecdsa + sha256

real-time prover, small proof (400K), 端侧计算，无需改造issuer

credential自身的device-bound private key由SE保护，device-bound public key受issuer public key保护

device-bound public key 通过ZK隐藏

ZK在AP侧计算，无需改造SE


detail
----------

用 sumcheck 构建 m x k matrix M，末尾几行为random

用 Reed-Solomon encodes 将 matrix M 扩为 m x N ， FFT

基于 matrix M 各columns构建merkle tree


通过Reed-Solomon encodes扩大error，抵御伪造，提升错误识别概率，非100%


.. code-block::

        prover -> verifier :  merkle tree root hash
        prover <- verifier :  challenge r 
        prover -> verifier :  z = r * M
        prover <- verifier :  random column index list [j_1, ..., j_40]
        prover -> verifier :  open columns, merkle path
        verifer:  verify merkle path, verify z, verify sumcheck constraints



