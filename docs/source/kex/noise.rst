Noise
#########


overview
===========

其实就是 DH + HASH + KDF + AEAD 的组合。


ACCE : Authenticated and Confidential Channel Establishment

KEM: Key Encapsulation Mechanism

ORKE: One-Round-Key Exchange

AKE: Authenticated Key Exchange

MSKE: Multi-Stage Key Exchange

相对STS，优势明显。

- Authenticated and integrity
- Key compromise impersonation (KCI) resistance
- Forward-secrecy
- Resistance against replay attack
- Resistance against reveals executions' random coins

role
=======

initiator

responder

pattern
=========

N: 对端没有long-term public key

I: handshake的时候对端明文传输long-term public key

X: handshake的时候对端加密传输long-term public key

K: 预先知道了对端的long-term public key

例如 pattern = XK，表示 initiator 预先知道 responder 的long-term public key，initiator加密自身的long-term public key 给到 responder

.. note::

    KEM-ACCE: initiator -> responder: g^a, c0 = enc( key = KDF(g^aB), aad = g^a, plaintext = m0 )
    ORKE-ACCE: responder -> initiator: g^b, c1 = enc( key = KDF(g^aB, g^ab), aad = g^a || c0 || g^b, plaintext = m1)
    AKE-ACCE:  initiator -> responder: c2 = enc( key = KDF(g^aB, g^ab), aad = g^a || c0 || g^b || c1, plaintext = g^A) 
                                       c3 = enc( key = KDF(g^aB, g^ab, g^Ab), aad = g^a || c0 || g^b || c1 || c2, plaintext = m3) 
 
key
=======

long-term key pair

ephemeral key pair

symmetric key material
========================

ck: chainning key, 主要用于key更新

h:  hash variable, 主要用于channel binding

k:  handshake message 加解密；

n:  nonce，递增

state
=========

三个state:
- CipherState：主要是对称加密的k, n。用于加密消息。
- SymmetricState: 主要是轮换ck，h。其中ck主要用于派生k；h主要用于channel binding的aad。
- HandshakeState: 主要是握手过程中(s, e), (rs, re)的状态更新，并基于这些DH轮换ck, k。

握手结束后，将ck拆分为两个key ( :math:`k_i` 、:math:`k_r` )，类似TLS，用于communication message两个方向的加解密。

prologue
===========

prologue 双方预知的一些上下文信息。mixhash。

## pre-message pattern 

初始化 双方预先知道的信息。

例如预先知道对方的s，可以调用mixhash更新h。

message pattern
==================

握手，交互信息，一方write，对端read

WriteMessage: 
- e: 明文，mixhash
- s: 加密，mixhash
- ee/es/se/ss: DH值要mixkey 

ReadMessage:
- e: 相当于收到re
- s: 对密文解密获得rs，然后对密文mixhash
- ee/es/se/ss: DH值要mixkey

psk
=====

psk0，表示放在第一个message的最前面

pski，表示放在第i个message的最后面

mixkeyandhash(psk)

psk模式下，message pattern里的e在mixhash后要再mixkey

参考资料
============

- `Noise Protocol Framework <http://www.noiseprotocol.org/>`_
- `Noise Explorer: Fully Automated Modeling and Verification for Arbitrary Noise Protocols <https://eprint.iacr.org/2018/766.pdf>`_
- `Flexible Authenticated and Confidential Channel Establishment (fACCE): Analyzing the Noise Protocol Framework <https://eprint.iacr.org/2019/436.pdf>`_
- `An Introduction to the Noise Protocol Framework <https://duo.com/labs/tech-notes/noise-protocol-framework-intro>`_
- `Noise-Go Library <https://github.com/flynn/noise>`_
