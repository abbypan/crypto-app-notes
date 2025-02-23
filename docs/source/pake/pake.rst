PAKE
#######

`pake selection <https://www.ietf.org/proceedings/104/slides/slides-104-cfrg-pake-selection-01.pdf>`_

`pake selection <https://github.com/cfrg/pake-selection>`_

RFC8125: Requirements for Password-Authenticated Key Agreement (PAKE) Schemes

Password Authenticated Key Exchange (PAKE) protocols 指的是通信双方基于共享的password安全生成session key。

balanced PAKE是指通信双方保存相同的password，augumented PAKE是指其中一方保存password的变换值。

PFS & wPFS 
==========================================================

`Forward secrecy <https://en.wikipedia.org/wiki/Forward_secrecy>`_

`HMQV: A High-Performance Secure Diffie-Hellman Protocol <https://eprint.iacr.org/2005/176.pdf>`_

`Session-Key Generation using Human Passwords Only <https://iacr.org/archive/crypto2001/21390406.pdf>`_

PFS (perfect forward secrecy) 指即使long term key泄漏，也不会影响之前的session key

wPFS (weak perfect forward secrecy) 指即使long term key泄漏，也不会影响之前passive attacker监听下的session key（但是 actively interfered attacker的session key可能受影响)


公钥的传输方式
==========================================================

Encrypted Key Exchange(EKE)：以Password派生的kek，加密ephemeral public key

J-PAKE：交换ephemeral public key，以及基于password派生的value

Strong Password-Only Authenticated Key Exchange (SPEKE)：基于Password派生generator，交换派生的public key

PACE：加密传输nonce，以nonce派生common base。

是否多人参与
==========================================================

Group PAKEs

安全性
==========================================================

尤其是对dictionary attack的抗性

用途
==========================================================

基于password做key distribution

是否整合long term public key

用户标识的隐私保护可考虑数字信封


形态
================

balanced PAKE

augumented PAKE



