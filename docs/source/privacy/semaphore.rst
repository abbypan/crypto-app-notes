Semaphore
=============

merkle tree + Groth16 + Poseidon

use case
------------

anonymous group identity and voting

在不泄露自身ID的前提下，匿名的证明自身属于某个group，且message未经篡改。

message内容公开，禁止重放/多发。

v3
--------------

tree
#########

Incremental Merkle Tree (IMT)

reg
##################

本地随机生成IdentityTrapdoor, IdentityNullifier

        secret = Poseidon(IdentityTrapdoor, IdentityNullifier)

        Identity Commitment = Poseidon(secret)

在merkle tree 登记 Identity Commitment 

Groth16
###################

externalNullifier 用于标识业务

NullifierHash = Poseidon(externalNullifier, IdentityNullifier) 用于绑定单用户，防重放/多发

注意signalHash无需签名


private input: secret, merkle treeSiblings & treeIndices

public input: signalHash, externalNullifier

public output: merkleRoot, NullifierHash



v4
-----------------

tree
#######

Lean Incremental Merkle Tree (LeanIMT)

reg
#########


本地随机生成eddsa keypair,私钥为32byte secret 

基于secret派生s，参考RFC8032；也可直接将secret置为s

PK = s * G

Identity Commitment = Poseidon(PK_x, PK_y)

在merkle tree 登记 Identity Commitment 


Groth16
###################

Scope 用于标识业务

Nullifier = Poseidon(Scope, secret) 用于绑定单用户，防重放/多发

注意message无需签名


private input: secret, merkle treeSiblings & treeIndices

public input: message, Scope

public output: merkleRoot, Nullifier
