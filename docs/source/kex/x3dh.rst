x3dh
#########

`The X3DH Key Agreement Protocol <https://signal.org/docs/specifications/x3dh/>`_

`KCI in X3DH <https://moderncrypto.org/mail-archive/messaging/2018/002505.html>`_

通信双方相互认证，forward secrecy and cryptographic deniability.

通信的一方，可能是离线状态。

key
======================================

ecc: curve25519/curve448

设计了4类keypair：
- identity key (IK)：long term
- signed prekey (SPK): periodically update，signed by long term identity key
- one-time prekey (OPK): only use one time
- ephemeral key (EK): temporary generate

dh
======================================

设计了4个DH值：

.. note::

    DH1: IK_A & SPK_B
    DH2: EK_A & IK_B
    DH3: EK_A & SPK_B
    DH4(optional): EK_A & OPK_B

    SK = KDF(DH1 || DH2 || DH3)
    SK = KDF(DH1 || DH2 || DH3 || DH4)


显然：
- DH1是A与B的绑定，DH2/DH3是临时内容与B的绑定，DH123在`SPK_B`的生命周期内无法保证前向安全性
- DH4可以保证一定的`伪`前向安全性，前提是server不作恶


.. note::

   associated data： 
   AD = Encode(IKA) || Encode(IKB)

attack
======================================

Protocol replay: 无DH4的场景是可能重放的，导致SK总是一模一样；应设法加参数派生变换。

