whatsapp
##############

`Security Principles for Private Messaging <https://engineering.fb.com/wp-content/uploads/2022/07/Meta-Security-Principles-for-Private-Messaging-White-Paper-July-2022-2.pdf>`_

`whatsapp security whitepaper <https://www.whatsapp.com/security/WhatsApp-Security-Whitepaper.pdf>`_

`Messenger End-to-End Encryption Overview <https://engineering.fb.com/wp-content/uploads/2023/12/MessengerEnd-to-EndEncryptionOverview_12-6-2023.pdf>`_

`The Labyrinth Encrypted Message Storage Protocol <https://engineering.fb.com/wp-content/uploads/2023/12/TheLabyrinthEncryptedMessageStorageProtocol_12-6-2023.pdf>`_

comm
======================================

基于signal，注意FS, KCI。

calling: initiator提供 S/I/O, responder提供I/E。

franking: 依赖双层hmac，NF由sender随M加密传输给receiver，TF由sender提交给server。KF属于server, RF由server计算，返回给sender&receiver。receiver举报时，向server提供M, NF, TF, RF, context。

.. note::

    TF = HMAC-SHA256(NF, M)
    RF = HMAC-SHA256(KF,TF || context)

group message: group key是by sender的，因此有 n*(n-1) 的key传输(message)。成员加入/退出，换key。key type包含Encryption (sym), Authentication (asym)。

media: media key传输(message)。media密文直接提交给meta。下载密文后，再以接收到的media key解密。

restore
======================================

AES-GCM-Extended: 32-byte key and a 28-byte nonce

.. note::

    subkey := hchacha20(nonce[0:16], key)
    subnonce := nonce[16:28]

用的hpke的AuthPSK

每个device，对应的key bundle:
- deviceKeyPriv, deviceKeyPub: asymmetric signing key pair
- epochStorageKeyPriv, epochStorageKeyPub: asymmetric encryption key pair
- epochStorageAuthKeyPriv, epochStorageAuthKeyPub

server存储上述keypub，以及deviceKeyPriv对2个Storage Key Pub的signature。

epoch内由epochRootKey派生epochChainingKey, epochDistributionPreSharedKey，与rachet类似。root key轮转由newEpochEntropy驱动, entropy通过hpke AuthPSK加密传输，此处PSK即epochDistributionPreSharedKey。

同一epoach内的各device共享相同的epochRootKey，换entropy要全通知到(or延迟离线通知)。

每个device有client state，以及对应的server state，注意 client state + server state最后map的id相同，主要是一个blind factor的处理。

root key还用于派生DataStorageKey, DeviceMacKey（key没区分device，仅mac区分device）。

.. note::

    epochDeviceMac := mac(epochDeviceMacKey, deviceKeyPub)

DataStorageKey加密存储前一个previousEpochRootKey，用于回溯。

backup
--------------------------------------

backup时，在当前device虚拟生成一个vdevice。

初始化vdevice的key bundle。

recovery code包含2部分：entropy, code(纠错码)。用基础的finite field矩阵乘法。

基于当前device的clientState，blind factor生成vdevice的vclientState。

基于entropy派生vdeviceId, vdeviceDecryptionKey。vdeviceDecryptionKey用于加解密该vdevice的key bundle, vclientState, epochRootKey等。

当前device提交vdeviceId, evolveToken, epochDeviceMac到server，显然，server将生成vserverState。

add new device
----------------------------------------------------

初始化new device的key bundle。

基于recovery code获得vdevice的相关信息。以hpke AuthPSK解密epochEntropyEncrypted进行forward chaining。

基于vclientState，blind factor生成new device的clientState。

new device提交new device id, evolveToken, epochDeviceMac到server，显然，server将生成new device的serverState。

rovoke device
----------------------------------------------------

切entropy，删device登记的id, mac以及对应的serverState。

PIN
----------------------------------------------------

PIN码用于authenticate，从backup key vault中获取recovery code。10次机会。

device keychain
----------------------------------------------------

本地recovery code，存于device keychain。用于user在本机上uninstall-reinstall的快速恢复。

one-time code
----------------------------------------------------

device help device, 用的6-digit code，CPACE。

近场device帮new device做blind factor生成clientState，提供epoach root key，帮忙向server提交evolveToken。
